---
title: Dispatcher informazioni sul caching
description: Scopri in che modo il modulo Dispatcher funziona la sua cache.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
duration: 491
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: tm+mt
source-wordcount: '1708'
ht-degree: 0%

---

# Informazioni sul caching

[Sommario](./overview.md)

[&lt;- Precedente: Spiegazione dei file di configurazione](./explanation-config-files.md)

Questo documento spiega come avviene il caching in Dispatcher e come può essere configurato

## Memorizzazione in cache delle directory

Nelle installazioni della linea di base vengono utilizzate le seguenti directory di cache predefinite

- Autore
   - `/mnt/var/www/author`
- Editore
   - `/mnt/var/www/html`

Quando ogni richiesta attraversa Dispatcher, le richieste seguono le regole configurate per mantenere una versione cache locale in grado di rispondere agli elementi idonei

>[!NOTE]
>
>Il carico di lavoro pubblicato viene intenzionalmente tenuto separato dal carico di lavoro dell’autore perché quando Apache cerca un file in DocumentRoot non sa da quale istanza AEM proviene. Pertanto, anche se la cache è disabilitata nella farm di authoring, se DocumentRoot dell’autore è uguale a publisher, i file presenti nella cache verranno distribuiti. Ciò significa che distribuirai i file di authoring per dalla cache pubblicata e che farai un’esperienza di mix davvero terribile per i visitatori.
>
>Anche mantenere directory DocumentRoot separate per contenuti pubblicati diversi è una pessima idea. Dovrai creare più elementi re-memorizzati nella cache che non differiscano tra i siti come clientlibs, nonché impostare un agente di svuotamento della replica per ogni DocumentRoot configurato. Aumentare la quantità di svuotamento sopra la testina con ogni attivazione della pagina. Utilizza lo spazio dei nomi dei file e i relativi percorsi nella cache completa ed evita più DocumentRoot per i siti pubblicati.

## File di configurazione

Dispatcher controlla ciò che è qualificato come &quot;in cache&quot; in `/cache {` sezione di qualsiasi file farm. 
Nelle farm di configurazione della linea di base di AMS, troverai le nostre inclusioni come mostrato di seguito:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Quando crei le regole per ciò che deve essere memorizzato in cache o meno, consulta la documentazione [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Caching di Author

Abbiamo visto molte implementazioni in cui le persone non memorizzano in cache i contenuti di authoring. 
Gli autori non riescono a ottenere un enorme miglioramento delle prestazioni e della reattività.

Parliamo della strategia adottata per configurare la farm di authoring in modo che venga memorizzata correttamente la cache.

Ecco un autore di base `/cache {` sezione del file farm dell’autore:


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

Le cose importanti da notare sono che `/docroot` è impostato sulla directory della cache per l’authoring.

>[!NOTE]
>
>Assicurati che le `DocumentRoot` nel file dell&#39;autore `.vhost` file corrisponde alle farm `/docroot` parametro

L’istruzione cache rules include il file `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` che contiene le seguenti regole:

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

In uno scenario di authoring, il contenuto cambia continuamente e di proposito. Desideri memorizzare nella cache solo gli elementi che non verranno modificati di frequente.
Abbiamo regole da memorizzare in cache `/libs` perché fanno parte dell’installazione AEM linea di base e cambierebbero finché non hai installato un Service Pack, un Cumulative Fix Pack, un aggiornamento o un Hotfix. La memorizzazione in cache di questi elementi ha molto senso e offre agli utenti finali che utilizzano il sito enormi vantaggi in termini di esperienza di authoring.

>[!NOTE]
>
>Tieni presente che anche queste regole memorizzano in cache <b>`/apps`</b> qui risiede il codice personalizzato dell’applicazione. Se stai sviluppando il codice in questa istanza, allora si dimostrerà molto confuso quando salvi il file e non vedi se si riflette nell&#39;interfaccia utente a causa di esso che serve una copia memorizzata nella cache. L’intenzione qui è che se esegui una distribuzione del codice nell’AEM, anche questa sarebbe infrequente e parte dei passaggi di distribuzione dovrebbe consistere nel cancellare la cache di authoring. Anche in questo caso, il vantaggio è enorme e rende il codice memorizzabile in cache più veloce per gli utenti finali.

## ServeOnStale (o Serve su Stale/SOS)

Questa è una delle gemme di una funzione di Dispatcher. Se l’editore è sotto carico o non risponde, in genere genera un codice di risposta http 502 o 503. Se questo accade e questa funzione è abilitata, Dispatcher verrà istruito a distribuire comunque tutto il contenuto ancora nella cache come best practice, anche se non si tratta di una nuova copia. È meglio distribuire qualcosa se lo si ha, piuttosto che semplicemente mostrare un messaggio di errore che non offre alcuna funzionalità.

>[!NOTE]
>
>Tieni presente che se il renderer del server di pubblicazione ha un timeout del socket o un messaggio di errore 500, questa funzione non verrà attivata. Se l&#39;AEM è semplicemente irraggiungibile questa funzione non fa nulla

Questa impostazione può essere impostata in qualsiasi farm, ma ha senso applicarla solo ai file farm di pubblicazione. Di seguito è riportato un esempio di sintassi della funzione abilitata in un file farm:

```
/cache { 
    /serveStaleOnError "1"
```

## Memorizzazione in cache di pagine con parametri di query/argomenti

>[!NOTE]
>
>Uno dei comportamenti normali del modulo Dispatcher è che se una richiesta ha un parametro di query nell’URI (in genere mostrato come `/content/page.html?myquery=value`) salta la memorizzazione in cache del file e passa direttamente all’istanza AEM. Sta considerando questa richiesta come una pagina dinamica e non deve essere memorizzata in cache. Questo può causare effetti negativi sull’efficienza della cache.

Vedi questo [articolo](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) mostrare come importanti parametri di query possono influenzare le prestazioni del sito.

Per impostazione predefinita, si desidera impostare `ignoreUrlParams` regole da consentire `*`.  Ciò significa che tutti i parametri di query vengono ignorati e consentono di memorizzare in cache tutte le pagine, indipendentemente dai parametri utilizzati.

Ecco un esempio in cui qualcuno ha creato un meccanismo di riferimento per collegamenti profondi (deep link) nei social media che utilizza il riferimento dell’argomento nell’URI per sapere da dove proviene la persona.

*Esempio ignorabile:*

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

La pagina è memorizzabile nella cache al 100% ma non la memorizza in cache perché gli argomenti sono presenti. 
Configurazione del `ignoreUrlParams` come elenco consentiti aiuterà a risolvere questo problema:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Ora, quando Dispatcher visualizza la richiesta, ignora il fatto che la richiesta ha il `query` parametro di `?` fare riferimento alla pagina e memorizzarla nella cache

<b>Esempio dinamico:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Tieni presente che se disponi di parametri di query che apportano modifiche a una pagina, viene eseguito il rendering dell’output, dovrai escluderli dall’elenco ignorato e rendere di nuovo la pagina non memorizzabile in cache.  Ad esempio, una pagina di ricerca che utilizza un parametro di query modifica l’HTML non elaborato sottoposto a rendering.

Ecco quindi la sorgente html di ogni ricerca:

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

Se hai visitato `/search.html?q=fruit` prima memorizzava nella cache l’html e i risultati mostravano i loro frutti.

Poi visiti `/search.html?q=vegetables` secondo, ma mostrerebbe risultati di frutta.
Questo perché il parametro query di `q` viene ignorato per quanto riguarda il caching.  Per evitare questo problema è necessario prendere nota delle pagine che eseguono il rendering di diversi HTML in base ai parametri di query e negare il caching per tali pagine.

Esempio:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Le pagine che utilizzano parametri di query tramite JavaScript continueranno a funzionare completamente ignorando i parametri di questa impostazione.  Perché non cambiano il file html a riposo.  Utilizzano JavaScript per aggiornare i browser dom in tempo reale sul browser locale.  Ciò significa che se utilizzi i parametri di query con JavaScript, è molto probabile che tu possa ignorare questo parametro per il caching delle pagine.  Consenti alla pagina di memorizzare in cache e ottenere prestazioni migliori.

>[!NOTE]
>
>Tenere traccia di queste pagine richiede una certa manutenzione, ma vale la pena migliorare le prestazioni.  Puoi chiedere al tuo CSE di eseguire un rapporto sul traffico dei tuoi siti web per fornirti un elenco di tutte le pagine che utilizzano i parametri di query negli ultimi 90 giorni per analizzare e assicurarti di sapere quali pagine esaminare e quali parametri di query non ignorare

## Memorizzazione nella cache delle intestazioni di risposta

È abbastanza ovvio che Dispatcher memorizza in cache `.html` pagine e clientlibs (ad es. `.js`, `.css`), ma sapevi che può anche memorizzare in cache determinate intestazioni di risposta lungo il contenuto di un file con lo stesso nome ma con un `.h` estensione file. Questo consente di rispondere successivamente non solo al contenuto, ma anche alle intestazioni di risposta che devono accompagnarlo dalla cache.

L&#39;AEM può gestire più della semplice codifica UTF-8

A volte gli elementi hanno intestazioni speciali che aiutano a controllare i dettagli di codifica della cache TTL e le ultime marche temporali modificate.

Quando vengono memorizzati nella cache, questi valori vengono rimossi per impostazione predefinita e il server web Apache httpd esegue il proprio lavoro di elaborazione della risorsa con i normali metodi di gestione dei file, normalmente limitati alla indovinazione del tipo mime in base alle estensioni dei file.

Se disponi della cache di Dispatcher per la risorsa e le intestazioni desiderate, puoi esporre l’esperienza corretta e verificare che tutti i dettagli vengano visualizzati nel browser dei client.

Ecco un esempio di farm con le intestazioni da memorizzare in cache specificate:

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


Nell’esempio hanno configurato l’AEM per distribuire le intestazioni che la rete CDN cerca per sapere quando annullare la validità della cache. Ciò significa che ora l’AEM può determinare correttamente quali file vengono invalidati in base alle intestazioni.

>[!NOTE]
>
>Tieni presente che non puoi utilizzare espressioni regolari o la corrispondenza glob. È un elenco letterale delle intestazioni da memorizzare in cache. Inserisci solo un elenco delle intestazioni letterali da memorizzare in cache.

## Invalida automaticamente periodo di tolleranza

Sui sistemi AEM con un’elevata attività da parte di autori che eseguono molte attivazioni di pagina, si può verificare una race condition che causa ripetuti invalidamenti. Le richieste di scaricamento ripetute in modo massiccio non sono necessarie e puoi introdurre una certa tolleranza per non ripetere uno scaricamento finché il periodo di tolleranza non è trascorso.

### Esempio di come funziona:

Se hai 5 richieste di annullamento della validità `/content/exampleco/en/` il tutto avviene in un periodo di 3 secondi.

Con questa funzione, la directory della cache verrà invalidata `/content/exampleco/en/` 5 volte

Attivando questa funzione e impostandola su 5 secondi, la directory della cache verrebbe invalidata `/content/exampleco/en/` <b>una volta</b>

Di seguito è riportato un esempio di sintassi di questa funzione configurata per un periodo di tolleranza di 5 secondi:

```
/cache { 
    /gracePeriod "5"
```

## Annullamento della validità basato su TTL

Una nuova funzione del modulo Dispatcher era `Time To Live (TTL)` opzioni di annullamento della validità basate sugli elementi memorizzati in cache. Quando un elemento viene memorizzato nella cache, cerca la presenza di intestazioni di controllo cache e genera un file nella directory cache con lo stesso nome e un `.ttl` estensione.

Ecco un esempio della funzione configurata nel file di configurazione della farm:

```
/cache { 
    /enableTTL "1"
```

>[!NOTE]
>
>Tieni presente che l’AEM deve ancora essere configurato per inviare le intestazioni TTL in modo che Dispatcher le onori. L’attivazione di questa funzione consente solo a Dispatcher di sapere quando rimuovere i file per i quali l’AEM ha inviato le intestazioni di controllo cache. Se l’AEM non inizia a inviare intestazioni TTL, Dispatcher non farà nulla di speciale qui.

## Regole filtro cache

Di seguito è riportato un esempio di configurazione di base per la quale gli elementi da memorizzare in cache su un editore:

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

Vogliamo rendere il nostro sito pubblicato il più greedy possibile e memorizzare tutto in cache.

Se vi sono elementi che interrompono l’esperienza quando vengono memorizzati nella cache, puoi aggiungere regole per rimuovere l’opzione di memorizzare in cache tale elemento. Come vedi nell’esempio precedente, i token csrf non devono mai essere memorizzati in cache e sono stati esclusi. Ulteriori dettagli sulla stesura di queste regole sono disponibili sul sito [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Successivo -> Utilizzo e nozioni di base sulle variabili](./variables.md)
