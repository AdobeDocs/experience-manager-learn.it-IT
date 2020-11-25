---
title: Caricamento e attivazione di una chiamata Target
description: Scopri come caricare, trasmettere parametri alla richiesta di pagina e attivare una chiamata Target dalla pagina del sito utilizzando una regola di lancio. Le informazioni di pagina vengono recuperate e trasmesse come parametri utilizzando il Livello dati client del Adobe  che consente di raccogliere e archiviare dati sull'esperienza dei visitatori in una pagina Web e quindi di semplificare l'accesso a tali dati.
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 3%

---


# Caricamento e attivazione di una chiamata Target {#load-fire-target}

Scopri come caricare, trasmettere parametri alla richiesta di pagina e attivare una chiamata Target dalla pagina del sito utilizzando una regola di lancio. Le informazioni della pagina Web vengono recuperate e trasmesse come parametri utilizzando il Livello dati client del Adobe  che consente di raccogliere e archiviare i dati sull&#39;esperienza dei visitatori in una pagina Web e quindi semplificare l&#39;accesso a tali dati.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regola di caricamento della pagina

Il livello dati client del Adobe  è un livello dati basato sull&#39;evento. Quando viene caricato il livello dati AEM pagina, viene attivato un evento `cmp:show` . Nel video, la `Launch Library Loaded` regola viene chiamata utilizzando un evento personalizzato. Di seguito sono riportati gli snippet di codice utilizzati nel video per l&#39;evento personalizzato e per gli elementi di dati.

### Evento pagina personalizzata visualizzata{#page-event}

![Configurazione evento mostrata dalla pagina e codice personalizzato](assets/load-and-fire-target-call.png)

Nella proprietà Launch, aggiungere un nuovo **evento** alla **regola**

+ __Estensione:__ Core
+ __Tipo evento:__ Codice personalizzato
+ __Nome:__ Gestore evento Page Show (o qualcosa di descrittivo)

Toccate il pulsante __Open Editor__ e incollate il testo nel frammento di codice seguente. Questo codice __deve__ essere aggiunto alla Configurazione ____ evento e a una successiva __azione__.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Una funzione personalizzata definisce `pageShownEventHandler`e ascolta gli eventi emessi dai componenti core AEM, ottiene le informazioni pertinenti sul componente core, le crea un pacchetto in un oggetto evento e attiva l&#39;evento Launch con le informazioni sull&#39;evento derivato al suo payload.

La regola di avvio viene attivata utilizzando la `trigger(...)` funzione di Launch, disponibile __solo__ all&#39;interno della definizione dello snippet di codice personalizzato dell&#39;evento di una regola.

La `trigger(...)` funzione prende un oggetto evento come parametro che a sua volta viene esposto in Launch Data Elements, con un altro nome riservato in Launch denominato `event`. Data Elements in Launch ora può fare riferimento ai dati di questo oggetto evento dall&#39; `event` oggetto utilizzando sintassi come `event.component['someKey']`.

Se `trigger(...)` viene utilizzato al di fuori del contesto del tipo di evento Codice personalizzato dell&#39;evento (ad esempio, in un&#39;azione), l&#39;errore JavaScript `trigger is undefined` viene generato sul sito Web integrato con la proprietà Launch.


### Elementi dati

![Elementi dati](assets/data-elements.png)

 Adobe Launch Data Elements mappa i dati dall&#39;oggetto evento [attivato nell&#39;evento](#page-event) Page Shown personalizzato alle variabili disponibili in  Adobe Target, tramite il tipo di elemento di codice personalizzato dell&#39;estensione Core.

#### Elemento dati ID pagina

```
if (event && event.id) {
    return event.id;
}
```

Questo codice restituisce l’ID univoco generato dal componente core.

![ID pagina](assets/pageid.png)

### Elemento dati percorso pagina

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Questo codice restituisce il percorso della pagina AEM.

![Percorso pagina](assets/pagepath.png)

### Elemento dati titolo pagina

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Questo codice restituisce il titolo della pagina AEM.

![Titolo pagina](assets/pagetitle.png)

## Risoluzione dei problemi

### Perché le mbox non vengono attivate sulle pagine Web?

#### Messaggio di errore quando il cookie mboxDisable non è impostato**

![Errore del dominio del cookie di Target](assets/target-cookie-error.png)

#### Soluzione

A volte i clienti di Target utilizzano istanze basate su cloud con Target per eseguire il test o per scopi semplici come prova del concetto. Questi domini, e molti altri, fanno parte del Public Suffix List.
I browser moderni non salvano i cookie se utilizzi questi domini a meno che non personalizzate le `cookieDomain` impostazioni utilizzando `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Passaggi successivi

+ [Esporta frammento esperienza in  Adobe Target](./export-experience-fragment-target.md)

## Collegamenti di supporto

+ [Documentazione sul livello dei dati del cliente  Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Utilizzo della documentazione sul livello dati client e sui componenti core del Adobe](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Introduzione ad Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)