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
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 4%

---


# Caricamento e attivazione di una chiamata Target {#load-fire-target}

Scopri come caricare, trasmettere parametri alla richiesta di pagina e attivare una chiamata Target dalla pagina del sito utilizzando una regola di lancio. Le informazioni di pagina vengono recuperate e trasmesse come parametri utilizzando il Livello dati client del Adobe  che consente di raccogliere e archiviare dati sull&#39;esperienza dei visitatori in una pagina Web e quindi di semplificare l&#39;accesso a tali dati.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regola di caricamento della pagina

Il livello dati client del Adobe  è un livello dati basato sull&#39;evento. Quando viene caricato il livello dati AEM pagina, viene attivato un evento `cmp:show` . Nel video, la `Launch Library Loaded` regola viene chiamata utilizzando un evento personalizzato. Di seguito sono riportati gli snippet di codice utilizzati nel video per l&#39;evento personalizzato e per gli elementi di dati.

### Evento personalizzato

Lo snippet di codice seguente aggiunge un listener di eventi spingendo una funzione nel livello dati. Quando l&#39; `cmp:show` evento viene attivato, viene chiamata la `pageShownEventHandler` funzione. In questa funzione, vengono aggiunti alcuni controlli di integrità e `dataObject` viene creato un nuovo stato con l’ultimo stato del livello dati per il componente che ha attivato l’evento.

Dopo che `trigger(dataObject)` è stato chiamato. `trigger()` è un nome riservato in Launch e &quot;attiverà&quot; la regola Launch. Trasmettiamo l&#39;oggetto evento come parametro che a sua volta sarà esposto da un altro nome riservato in Launch event. Gli elementi dati in Launch possono ora fare riferimento a varie proprietà come ad esempio: `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
      //get the state of the component that triggered the event
      component: window.adobeDataLayer.getState(evt.eventInfo.path)
   };

      //Trigger the Launch Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
      // i.e `event.component['someKey']`
      trigger(event);
   }
}

//set the namespace to avoid a potential race condition
window.adobeDataLayer = window.adobeDataLayer || [];
//push the event listener for cmp:show into the data layer
window.adobeDataLayer.push(function (dl) {
   //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
   dl.addEventListener("cmp:show", pageShownEventHandler);
});
```

### ID pagina livello dati

```
if(event && event.id) {
    return event.id;
}
```

![ID pagina](assets/pageid.png)

### Percorso pagina

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![Percorso pagina](assets/pagepath.png)

### Titolo pagina

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![Titolo pagina](assets/pagetitle.png)

### Problemi comuni

#### Perché le mbox non vengono attivate sulle pagine Web?

**Messaggio di errore quando il cookie mboxDisable non è impostato**

![Errore del dominio del cookie di Target](assets/target-cookie-error.png)

**Soluzione**

A volte i clienti di Target utilizzano istanze basate su cloud con Target per eseguire il test o per scopi semplici come prova del concetto. Questi domini, e molti altri, fanno parte del Public Suffix List.
I browser moderni non salvano i cookie se utilizzi questi domini a meno che non personalizzate le `cookieDomain` impostazioni utilizzando `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## Passaggi successivi

1. [Esporta frammento esperienza in  Adobe Target](./export-experience-fragment-target.md)

## Collegamenti di supporto

* [Documentazione sul livello dei dati del cliente  Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [Utilizzo della documentazione sul livello dati client e sui componenti core del Adobe](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Introduzione ad Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)