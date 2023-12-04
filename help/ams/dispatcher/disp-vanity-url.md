---
title: Funzionalità degli URL personalizzati del dispatcher dell’AEM
description: Scopri in che modo l’AEM gestisce gli URL personalizzati e le tecniche aggiuntive che utilizzano regole di riscrittura per mappare il contenuto più vicino al limite della consegna.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 286
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---

# URL personalizzati del dispatcher

[Sommario](./overview.md)

[&lt;- Precedente: Svuotamento del Dispatcher](./disp-flushing.md)

## Panoramica

Questo documento ti aiuta a capire come l’AEM gestisce gli URL personalizzati e alcune tecniche aggiuntive che utilizzano regole di riscrittura per mappare il contenuto più vicino al limite della consegna

## Cosa sono gli URL personalizzati

Se il contenuto risiede in una struttura di cartelle, non sempre si trova in un URL di facile riferimento. Gli URL personalizzati sono come scelte rapide. URL più brevi o univoci che fanno riferimento a dove si trova il contenuto reale.

Ecco un esempio: `/aboutus` puntato a `/content/we-retail/us/en/about-us.html`

Gli autori dell’AEM possono impostare le proprietà dell’URL personalizzato su un contenuto nell’AEM e pubblicarlo.

Affinché questa funzione funzioni, è necessario regolare i filtri di Dispatcher per consentire il reindirizzamento. Questa situazione diventa poco ragionevole se si regolano i file di configurazione di Dispatcher alla velocità con cui gli autori dovrebbero impostare queste voci di pagina personalizzate.

Per questo motivo, il modulo Dispatcher dispone di una funzione che consente automaticamente tutto ciò che è elencato come reindirizzamento nella struttura del contenuto.


## Come funziona

### Creazione di URL personalizzati

L’autore visita una pagina in AEM, fa clic sulle proprietà della pagina e aggiunge voci nella _URL personalizzato_ sezione. Quando si salvano le modifiche e si attiva la pagina, il reindirizzamento viene assegnato alla pagina.

Gli autori possono anche selezionare _Reindirizza Vanity URL_ casella di controllo durante l’aggiunta _URL personalizzato_ , questo determina il comportamento degli url personalizzati come reindirizzamenti 302. Significa che al browser viene richiesto di passare al nuovo URL (tramite `Location` e il browser effettua una nuova richiesta al nuovo URL.

#### Interfaccia utente touch:

![Menu di dialogo a discesa per l’interfaccia utente di creazione dell’AEM nella schermata dell’editor del sito](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-menu a discesa")

![pagina di dialogo proprietà pagina aem](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Finder contenuti classico:

![Proprietà della pagina nella barra laterale dell’interfaccia utente classica siteadmin di AEM](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Finestra di dialogo delle proprietà della pagina nell’interfaccia classica](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>Comprendere che questo è soggetto a problemi di spazio dei nomi. Le voci di reindirizzamento sono globali per tutte le pagine. Questo è solo uno dei difetti per cui devi pianificare soluzioni alternative. Ne illustreremo alcune in seguito.


## Risoluzione/mappatura risorse

Ogni voce di reindirizzamento è una voce di mappatura sling per un reindirizzamento interno.

Le mappe sono visibili visitando la console Felix delle istanze dell&#39;AEM ( `/system/console/jcrresolver` )

Ecco una schermata di una voce mappa creata da una voce di reindirizzamento:
![schermata della console di una voce di reindirizzamento nelle regole di risoluzione risorse](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

Nell&#39;esempio precedente quando chiediamo all&#39;istanza AEM di visitare `/aboutus` si risolve in `/content/we-retail/us/en/about-us.html`

## Filtri di Dispatcher per l’autorizzazione automatica

Il Dispatcher in uno stato protetto esclude le richieste nel percorso `/` tramite Dispatcher perché si tratta della radice della struttura JCR.

È importante assicurarsi che gli editori consentano solo il contenuto dal `/content` e altri percorsi sicuri e così via, e non percorsi come `/system`.

Ecco il problema: gli URL personalizzati si trovano nella cartella base di `/` come possiamo permettere che raggiungano gli editori in modo sicuro?

Semplice: Dispatcher dispone di un meccanismo di filtro automatico per l’autorizzazione. Occorre installare un pacchetto AEM e quindi configurare Dispatcher in modo che punti alla pagina del pacchetto.

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/it/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Il file farm di Dispatcher contiene una sezione di configurazione:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

Questa configurazione comunica a Dispatcher di recuperare questo URL dall’istanza AEM che esegue ogni 300 secondi per recuperare l’elenco di elementi che si desidera autorizzare.

Memorizza la cache della risposta in `/file` nell&#39;esempio seguente `/tmp/vanity_urls`

Quindi, se visiti l’istanza dell’AEM all’URI, puoi vedere cosa recupera:
![schermata del contenuto sottoposto a rendering da /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

È letteralmente una lista, molto semplice

## Riscrivi regole come regole personalizzate

Perché citare l’utilizzo di regole di riscrittura invece del meccanismo predefinito integrato nell’AEM come descritto sopra?

Semplicemente perché i problemi di spazio dei nomi, le prestazioni e la logica di livello superiore possono essere gestiti meglio.

Passiamo ora a un esempio della voce di reindirizzamento `/aboutus` al suo contenuto `/content/we-retail/us/en/about-us.html` utilizzo di Apache `mod_rewrite` per eseguire questa operazione.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Questa regola cerca il reindirizzamento `/aboutus` e recupera il percorso completo dal renderer con il flag PT (Pass Through).

Inoltre, interrompe l’elaborazione di tutte le altre regole con flag L (Last), il che significa che non deve esaminare un lungo elenco di regole come nel caso del Resolving JCR.

Oltre a non dover delegare la richiesta, e aspettare che l&#39;editore AEM risponda questi due elementi rendono questo metodo molto più performante.

La ciliegina sulla torta qui è il flag NC (No Case-Sensitive) che significa che se un cliente digita l’URI con `/AboutUs` invece di `/aboutus` funziona ancora.

Per creare una regola di riscrittura a questo scopo, crea un file di configurazione in Dispatcher (ad esempio: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) e includerlo nella `.vhost` file che gestisce il dominio che richiede l’applicazione di questi url personalizzati.

Di seguito è riportato un frammento di codice di esempio da includere all’interno `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

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

L’utilizzo dell’AEM per controllare le voci vanity presenta i seguenti vantaggi

- Gli autori possono crearli al volo
- Si trovano insieme al contenuto e possono essere confezionati con il contenuto

Utilizzo di `mod_rewrite` per controllare le voci di reindirizzamento sono disponibili i seguenti vantaggi

- Risoluzione più rapida dei contenuti
- È più vicino al limite delle richieste di contenuto degli utenti finali
- Maggiore estensibilità e opzioni per controllare il modo in cui il contenuto viene mappato su altre condizioni
- Può non fare distinzione tra maiuscole e minuscole

Utilizza entrambi i metodi, ma ecco i consigli e i criteri su quale utilizzare quando:

- Se il reindirizzamento è temporaneo e prevede bassi livelli di traffico, utilizza la funzione integrata AEM
- Se il reindirizzamento è un endpoint di base che non cambia spesso e che è usato di frequente, utilizza un `mod_rewrite` regola.
- Se lo spazio dei nomi personalizzato (ad esempio: `/aboutus`) deve essere riutilizzato per molti marchi nella stessa istanza AEM e quindi utilizzare le regole di riscrittura.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Se desideri utilizzare la funzione di reindirizzamento AEM ed evitare lo spazio dei nomi, puoi creare una convenzione di denominazione. Utilizzo di URL personalizzati nidificati come `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Successivo -> Registrazione comune](./common-logs.md)
