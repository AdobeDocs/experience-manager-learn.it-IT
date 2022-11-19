---
title: AEM funzione URL personalizzati di Dispatcher
description: Scopri come AEM gli URL personalizzati e le tecniche aggiuntive utilizzando le regole di riscrittura per mappare i contenuti più vicino al bordo della consegna.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---


# URL personalizzati del dispatcher

[Sommario](./overview.md)

[&lt;- Precedente: Scaricamento del Dispatcher](./disp-flushing.md)

## Panoramica

Questo documento ti aiuterà a capire come AEM gli URL personalizzati e alcune tecniche aggiuntive che utilizzano regole di riscrittura per mappare il contenuto più vicino al bordo della consegna

## Cosa sono gli URL personalizzati

Se hai contenuti che risiedono in una struttura di cartelle che ha senso, non sempre si trovano in un URL a cui è facile fare riferimento.  Gli URL personalizzati sono come collegamenti.  URL più brevi o univoci che fanno riferimento a dove si trova il contenuto reale.

Esempio: `/aboutus` indicato `/content/we-retail/us/en/about-us.html`

Gli autori AEM possono impostare le proprietà dell’url personalizzato su un contenuto in AEM e pubblicarlo.

Per il corretto funzionamento di questa funzione è necessario regolare i filtri di Dispatcher per consentire il reindirizzamento.  Ciò diventa irragionevole quando si regolano i file di configurazione di Dispatcher alla velocità con cui gli autori dovrebbero impostare queste voci di pagina personalizzate.

Per questo motivo il modulo Dispatcher dispone di una funzione per consentire automaticamente tutto ciò che è elencato come reindirizzamento nella struttura del contenuto.


## Come funziona

### Authoring di URL personalizzati

L’autore visita una pagina in AEM e visita le proprietà della pagina e aggiunge voci nella sezione URL personalizzato.

Una volta salvate le modifiche e attivate la pagina, il reindirizzamento viene ora assegnato a questa pagina.

#### Interfaccia utente touch:

![Menu a discesa per l’interfaccia di authoring AEM nella schermata dell’editor del sito](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-menu a discesa")

![pagina di dialogo delle proprietà della pagina aem](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Ricerca contenuti classica:

![AEM proprietà della pagina della barra laterale dell’interfaccia utente classica siteadmin](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Finestra di dialogo delle proprietà della pagina dell’interfaccia classica](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Vi prego di comprendere che questo è molto propenso a dare un nome ai problemi di spazio.

Le voci di reindirizzamento sono globali per tutte le pagine, questo è solo uno dei difetti da pianificare per soluzioni alternative che illustreremo in seguito.
</div>

## Risoluzione/mappatura delle risorse

Ogni voce di reindirizzamento personalizzato è una voce di mappa sling per un reindirizzamento interno.

Queste mappe sono visibili visitando la console Felix AEM istanze ( `/system/console/jcrresolver` )

Ecco una schermata della voce mappa creata da una voce di reindirizzamento:
![schermata della console di una voce di reindirizzamento nelle regole di risoluzione delle risorse](assets/disp-vanity-url/vanity-resource-resolver-entry.png "voce di vanity-resource-resolver-entry")

Nell&#39;esempio precedente quando chiediamo all&#39;istanza AEM di visitare `/aboutus` deciderà di `/content/we-retail/us/en/about-us.html`

## Filtri di autorizzazione automatica del Dispatcher

Il Dispatcher in uno stato sicuro esclude le richieste nel percorso `/` tramite il Dispatcher perché è la radice della struttura JCR.

È importante assicurarsi che gli editori autorizzino solo i contenuti dal `/content` e altri percorsi sicuri, ecc.  e non percorsi come `/system` ecc.

Ecco il problema, gli url personalizzati vivono nella cartella base di `/` come possiamo permettere agli editori di raggiungere gli editori in condizioni di sicurezza?

Il Dispatcher semplice dispone di un meccanismo di filtro automatico per consentire l’installazione di un pacchetto AEM e quindi configura il Dispatcher in modo che punti alla pagina dei pacchetti.

[https://experience.adobe.com/#/downloads/content/software-distribution/it/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/it/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher ha una sezione di configurazione nel suo file farm:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

Questa configurazione comunica al Dispatcher di recuperare questo URL dall’istanza AEM che esegue ogni 300 secondi per recuperare l’elenco degli elementi che desideri consentire.

Memorizza la cache della risposta nel `/file` argomento in questo esempio `/tmp/vanity_urls`

Quindi, se visitate l&#39;istanza AEM all&#39;URI, vedrete cosa recupera:
![schermata del contenuto renderizzato da /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

È letteralmente una lista, super semplice

## Riscrittura delle regole come regole personalizzate

Perché menzionare l’utilizzo di regole di riscrittura invece del meccanismo predefinito integrato in AEM come descritto sopra?

Semplicemente, problemi di spazio dei nomi, prestazioni e logica di livello superiore che possono essere gestiti meglio.

Passiamo ora a un esempio della voce di reindirizzamento `/aboutus` al suo contenuto `/content/we-retail/us/en/about-us.html` utilizzo di Apache `mod_rewrite` modulo per eseguire questa operazione.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Questa regola cercherà la vanità `/aboutus` e recupera il percorso completo dal renderer con il flag PT (Pass Through).

Inoltre smetterà di elaborare tutte le altre regole L flag (Last) che significa che non dovrà attraversare un enorme elenco di regole come JCR Resolving deve fare.

Oltre a non dover delegare la richiesta e attendere che l&#39;editore AEM risponda questi due elementi di questo metodo, lo rendono molto più performante.

Poi la ciliegina sulla torta qui è il flag NC (No Case-Sensitive) che significa se un cliente sbaglia l&#39;URI con `/AboutUs` anziché `/aboutus` funzionerà comunque e consentirà il recupero della pagina corretta.

Per creare una regola di riscrittura a questo scopo, crei un file di configurazione sul Dispatcher (ad esempio: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) e includerla nel `.vhost` file che gestisce il dominio che richiede l&#39;applicazione di questi url personalizzati.

Di seguito è riportato un frammento di codice di esempio dell’elemento include inside `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## Quale metodo e dove

L&#39;utilizzo di AEM per controllare le voci personalizzate presenta i seguenti vantaggi
- Gli autori possono crearli immediatamente
- Vivono con il contenuto e possono essere confezionati con il contenuto

Utilizzo `mod_rewrite` per controllare le voci personalizzate ha i seguenti vantaggi
- Risoluzione più rapida dei contenuti
- Più vicino al bordo delle richieste di contenuto degli utenti finali
- Maggiore estensibilità e opzioni per controllare la mappatura del contenuto su altre condizioni
- Può non fare distinzione tra maiuscole e minuscole

Utilizza entrambi i metodi, ma di seguito sono riportati i consigli e i criteri in base ai quali utilizzare quando:
- Se il reindirizzamento è temporaneo e prevede bassi livelli di traffico, utilizza la funzione integrata AEM
- Se il reindirizzamento è un endpoint di base che non cambia spesso e che viene utilizzato frequentemente, utilizza un `mod_rewrite` regola.
- Se lo spazio dei nomi personalizzato (ad esempio: `/aboutus`) deve essere riutilizzato per molti marchi della stessa istanza AEM e quindi utilizzare le regole di riscrittura.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Se desideri utilizzare la funzione di reindirizzamento AEM ed evitare lo spazio dei nomi, puoi creare una convenzione di denominazione.  Utilizzo di URL personalizzati nidificati come `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Avanti -> Registrazione comune](./common-logs.md)