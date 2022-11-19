---
title: Informazioni sulla cache in Dispatcher
description: Comprendi il funzionamento del modulo Dispatcher nella cache.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---


# Informazioni sulla memorizzazione in cache

[Sommario](./overview.md)

[&lt;- Precedente: Spiegazione dei file di configurazione](./explanation-config-files.md)

Questo documento illustra come avviene la memorizzazione in cache di Dispatcher e come può essere configurata

## Directory di memorizzazione nella cache

Utilizziamo le seguenti directory di cache predefinite nelle installazioni della linea di base

- Autore
   - `/mnt/var/www/author`
- Editore
   - `/mnt/var/www/html`

Quando ogni richiesta attraversa il Dispatcher, le richieste seguono le regole configurate per mantenere una versione cache locale per rispondere agli elementi idonei

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Manteniamo intenzionalmente il carico di lavoro pubblicato separato dal carico di lavoro dell&#39;autore perché quando Apache cerca un file nel DocumentRoot non sa da quale istanza AEM proviene. Pertanto, anche se la cache è disabilitata nella farm dell’autore, se DocumentRoot dell’autore è uguale a quella dell’editore, i file della cache vengono distribuiti quando presente. Ciò significa che distribuirai i file dell’autore per dalla cache pubblicata e produrrai un’esperienza di mix davvero terribile per i tuoi visitatori.

Anche mantenere directory DocumentRoot separate per contenuti pubblicati diversi è una pessima idea. Sarà necessario creare più elementi nella cache che non differiscono tra siti come clientlibs, nonché impostare un agente di flush di replica per ogni DocumentRoot configurato. Incremento della quantità di scaricamento in head con ogni attivazione di pagina. Utilizza lo spazio dei nomi dei file e i percorsi completi nella cache ed evita più DocumentRoot per i siti pubblicati.
</div>

## File di configurazione

Il Dispatcher controlla ciò che si qualifica come memorizzabile in cache nel `/cache {` di qualsiasi file farm. 
Nelle farm di configurazione della linea di base AMS, troverai i nostri &quot;include&quot; come mostrato di seguito:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Quando crei le regole per cosa memorizzare in cache o meno, fai riferimento alla documentazione [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Creazione di cache

Abbiamo visto molte implementazioni in cui le persone non memorizzano in cache i contenuti di authoring. 
Stanno perdendo un enorme miglioramento delle prestazioni e della reattività per i loro autori.

Parliamo della strategia adottata per configurare la farm degli autori in modo che venga memorizzata correttamente nella cache.

Di seguito è riportato un autore di base `/cache {` sezione del file farm dell’autore:


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    } 
    /invalidate { 
        /0000 { 
            /glob "*" 
            /type "allow" 
        } 
    } 
    /allowedClients { 
        /0000 { 
            /glob "*.*.*.*" 
            /type "deny" 
        } 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

Le cose importanti da notare qui sono che `/docroot` è impostato sulla directory cache per l’autore.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Assicurati che `DocumentRoot` nel `.vhost` il file corrisponde alle farm `/docroot` parameter
</div>

L’istruzione &quot;include&quot; delle regole di cache include il file `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` che contiene le seguenti regole:

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

In uno scenario di authoring, il contenuto cambia continuamente e di proposito. Desideri memorizzare nella cache solo gli elementi che non verranno modificati frequentemente.
Abbiamo delle regole da memorizzare nella cache `/libs` poiché fanno parte della linea di base AEM installare e cambierebbero fino a quando non avrai installato Service Pack, Cumulative Fix Pack, Upgrade o Hotfix. La memorizzazione in cache di questi elementi ha un senso e offre enormi vantaggi all&#39;esperienza di authoring degli utenti finali che utilizzano il sito.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tieni presente che anche queste regole memorizzano nella cache <b>`/apps`</b> è qui che vive il codice dell&#39;applicazione personalizzata. Se stai sviluppando il codice su questa istanza, allora si rivelerà molto confuso quando salvi il file e non vedi se riflette nell&#39;interfaccia utente perché serve una copia in cache. In questo caso, se distribuisci il codice in AEM, anche questo non è frequente e parte dei passaggi di distribuzione dovrebbe essere la cancellazione della cache dell’autore. Anche in questo caso il vantaggio è enorme, rendendo il codice memorizzabile nella cache più veloce per gli utenti finali.
</div>


## ServeOnStale (Servizio AKA su Stale / SOS)

Questa è una delle gemme di una funzione del Dispatcher. Se l’editore è sotto carico o non risponde, in genere genera un codice di risposta http 502 o 503. Se questo accade e questa funzione è abilitata, Dispatcher sarà istruito a continuare a servire il contenuto ancora presente nella cache come miglior sforzo, anche se non è una nuova copia. È meglio servire qualcosa se ce l&#39;hai invece di mostrare un messaggio di errore che non offre funzionalità.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tieni presente che se il renderer dell’editore dispone di un timeout del socket o di un messaggio di errore 500, questa funzione non verrà attivata. Se AEM non è raggiungibile, questa funzione non funziona
</div>

Questa impostazione può essere impostata in qualsiasi farm, ma ha senso applicarla solo ai file farm di pubblicazione. Ecco un esempio di sintassi della funzione abilitata in un file farm:

```
/cache { 
    /serveStaleOnError "1"
```

## Memorizzazione in cache di pagine con parametri di query/argomenti

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Uno dei comportamenti normali del modulo Dispatcher è che se una richiesta ha un parametro di query nell’URI (generalmente mostrato come `/content/page.html?myquery=value`) salterà la memorizzazione in cache del file e andrà direttamente all&#39;istanza AEM. Considera questa richiesta come una pagina dinamica e non dovrebbe essere memorizzata nella cache. Questo può causare effetti negativi sull’efficienza della cache.
</div>
<br/>

Vedi questo [articolo](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) mostrare quanto importanti parametri di query possono influenzare le prestazioni del sito.

Per impostazione predefinita, si desidera impostare il `ignoreUrlParams` regole per consentire `*`.  Ciò significa che tutti i parametri di query vengono ignorati e consentono la memorizzazione in cache di tutte le pagine indipendentemente dai parametri utilizzati.

Ecco un esempio in cui qualcuno ha creato un meccanismo di riferimento per i collegamenti profondi dei social media che utilizza il riferimento all&#39;argomento nell&#39;URI per sapere da dove proviene la persona.

<b>Esempio ignorabile:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

La pagina è memorizzabile nella cache al 100%, ma non memorizza in cache perché gli argomenti sono presenti. 
Configurazione della `ignoreUrlParams` come elenco consentiti aiuterà a risolvere questo problema:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Ora, quando il Dispatcher visualizza la richiesta, ignorerà il fatto che la richiesta ha il `query` parametro di `?` riferimento e memorizzazione in cache della pagina

<b>Esempio dinamico:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Tieni presente che se disponi di parametri di query che apportano una modifica alla pagina, viene eseguito il rendering dell’output, dovrai escluderli dall’elenco ignorato e rendere nuovamente la pagina non memorizzabile nella cache.  Ad esempio, una pagina di ricerca che utilizza un parametro di query modifica l’html non elaborato di cui è stato eseguito il rendering.

Ecco quindi la fonte HTML di ogni ricerca:

`/search.html?q=fruit`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

Se hai visitato `/search.html?q=fruit` in primo luogo, avrebbe messo in cache l&#39;html con i risultati che mostrano i frutti.

Poi visita `/search.html?q=vegetables` secondo, ma mostrerebbe i risultati della frutta.
Questo perché il parametro di query di `q` viene ignorato per quanto riguarda la memorizzazione in cache.  Per evitare questo problema, è necessario prendere nota delle pagine che eseguono il rendering di HTML diversi in base ai parametri di query e negare il caching per tali pagine.

Esempio:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Le pagine che utilizzano parametri di query tramite Javascript continueranno a funzionare completamente ignorando i parametri in questa impostazione.  Perché non cambiano il file html a riposo.  Utilizzano javascript per aggiornare i browser dom in tempo reale sul browser locale.  Ciò significa che se utilizzi i parametri di query con javascript è molto probabile che tu possa ignorare questo parametro per il caching delle pagine.  Consenti a quella pagina di memorizzare in cache e godere del guadagno di prestazioni!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenere traccia di queste pagine richiede un certo livello di manutenzione, ma ne vale la pena.  È possibile chiedere al CSE di eseguire un rapporto sul traffico dei siti web per ottenere un elenco di tutte le pagine che utilizzano parametri di query negli ultimi 90 giorni per poter analizzare e assicurarsi di sapere quali pagine esaminare e quali parametri di query non ignorare
</div>
<br/>

## Memorizzazione in cache delle intestazioni di risposta

È piuttosto ovvio che il Dispatcher memorizza in cache `.html` pagine e clientlibs (es. `.js`, `.css`), ma sapevi che può anche memorizzare nella cache intestazioni di risposta particolari lungo il lato del contenuto in un file con lo stesso nome ma un `.h` estensione file. Questo consente la risposta successiva non solo al contenuto, ma anche alle intestazioni di risposta che dovrebbero accompagnarlo dalla cache.

AEM può gestire più di una semplice codifica UTF-8

A volte gli elementi hanno intestazioni speciali che aiutano a controllare i dettagli di codifica del TTL della cache e gli ultimi timestamp modificati.

Questi valori quando vengono memorizzati nella cache per impostazione predefinita e il webserver Apache httpd eseguirà il proprio lavoro di elaborazione della risorsa con i suoi normali metodi di gestione dei file, che normalmente si limitano all’indovinazione dei tipi mime in base alle estensioni dei file.

Se disponi della cache di Dispatcher per la risorsa e le intestazioni desiderate, puoi esporre l’esperienza corretta e garantire che tutti i dettagli lo rendano al browser dei client.

Ecco un esempio di farm con le intestazioni da memorizzare nella cache specificate:

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


Nell’esempio che hanno configurato AEM per servire le intestazioni, la rete CDN cerca di sapere quando annullare la validità della cache. Ciò significa che ora AEM in modo appropriato determinare quali file vengono invalidati in base alle intestazioni.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tieni presente che non puoi utilizzare espressioni regolari o corrispondenze globali. È una lista letterale delle intestazioni da memorizzare in cache. Inserisci solo un elenco delle intestazioni letterali che desideri memorizzare in cache.
</div>


## Periodo di tolleranza per annullamento automatico validità

Nei sistemi AEM che svolgono molte attività da parte degli autori che eseguono molte attivazioni di pagina è possibile avere una race condition in cui si verificano ripetuti invalidamenti. Le richieste di scaricamento ripetute in modo significativo non sono necessarie e puoi generare una certa tolleranza per non ripetere uno scaricamento finché il periodo di tolleranza non viene cancellato.

### Esempio di funzionamento:

Se hai 5 richieste di annullamento della validità `/content/exampleco/en/` tutto avviene in un periodo di 3 secondi.

Con questa funzione disattivata, invalideresti la directory della cache `/content/exampleco/en/` 5 volte

Attivando questa funzione e impostandola su 5 secondi, invaliderebbe la directory della cache `/content/exampleco/en/` <b>una volta</b>

Ecco un esempio di sintassi di questa funzione configurata per un periodo di tolleranza di 5 secondi:

```
/cache { 
    /gracePeriod "5"
```

## Invalidazione basata su TTL

Una nuova funzionalità del modulo Dispatcher era `Time To Live (TTL)` opzioni di invalidazione basate per gli elementi memorizzati nella cache. Quando un elemento viene memorizzato nella cache, cerca la presenza di intestazioni di controllo della cache e genera un file nella directory della cache con lo stesso nome e un `.ttl` estensione.

Ecco un esempio della funzione configurata nel file di configurazione della farm:

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Tieni presente che AEM deve ancora essere configurato per inviare intestazioni TTL per Dispatcher per rispettarle. L’attivazione di questa funzione consente solo a Dispatcher di sapere quando rimuovere i file per i quali AEM invia intestazioni di controllo cache. Se AEM non inizia a inviare intestazioni TTL, Dispatcher non farà nulla di speciale qui.
</div>

## Regole filtro cache

Ecco un esempio di configurazione della linea di base per cui gli elementi devono essere memorizzati nella cache su un editore:

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

Vogliamo rendere il nostro sito pubblicato più greedy possibile e memorizzare in cache tutto.

Se ci sono elementi che interrompono l&#39;esperienza quando è presente nella cache, puoi aggiungere regole per rimuovere l&#39;opzione per memorizzare in cache quell&#39;elemento. Come vedi nell’esempio precedente, i token csrf non dovrebbero mai essere memorizzati nella cache e sono stati esclusi. Ulteriori dettagli sulla stesura di queste regole sono disponibili [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Successivo -> Uso e nozioni di base sulle variabili](./variables.md)