---
title: Utilizzo del livello dati client del Adobe  con AEM componenti core
description: Il  Adobe Client Data Layer introduce un metodo standard per raccogliere e archiviare dati su un'esperienza visitatore in una pagina Web e semplificare l'accesso a tali dati. Il livello dati client del Adobe  è agnostico della piattaforma, ma è completamente integrato nei componenti core per l'utilizzo con AEM.
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: e13a5171fbeb9e1eb5f78d1c691bc8b4b896a998
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 1%

---


# Utilizzo del livello dati client del Adobe  con AEM componenti core {#overview}

Il  Adobe Client Data Layer introduce un metodo standard per raccogliere e archiviare dati su un&#39;esperienza visitatore in una pagina Web e semplificare l&#39;accesso a tali dati. Il livello dati client del Adobe  è agnostico della piattaforma, ma è completamente integrato nei componenti core per l&#39;utilizzo con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Vuoi attivare il  Adobe Client Data Layer sul tuo sito AEM? [Consulta le istruzioni qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Esplora il livello dati

È possibile ottenere un&#39;idea delle funzionalità integrate del Livello dati client del Adobe  utilizzando gli strumenti di sviluppo del browser e il sito [di riferimento](https://wknd.site/)WKND attivo.

>[!NOTE]
>
> Screenshot di seguito scattato dal browser Chrome.

1. Andate a [https://wknd.site](https://wknd.site)
1. Apri gli strumenti per sviluppatori e immetti il comando seguente nella **console**:

   ```js
   window.adobeDataLayer.getState();
   ```

    Inspect la risposta per visualizzare lo stato corrente del livello dati su un sito AEM. Vengono visualizzate informazioni sulla pagina e sui singoli componenti.

   ![risposta livello dati Adobe](assets/data-layer-state-response.png)

1. Inviate un oggetto dati sul livello dati immettendo quanto segue nella console:

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. Eseguire di `adobeDataLayer.getState()` nuovo il comando e trovare la voce per `training-data`.
1. Quindi aggiungete un parametro di percorso per restituire solo lo stato specifico di un componente:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Restituire una sola voce di dati componente](assets/return-just-single-component.png)

## Utilizzo degli eventi

È consigliabile attivare qualsiasi codice personalizzato basato su un evento presente nel livello dati. Quindi, provate a registrare e ascoltare diversi eventi.

1. Immettete il seguente metodo helper nella console:

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   Il codice riportato sopra controllerà l&#39; `event` oggetto e utilizzerà il `adobeDataLayer.getState` metodo per ottenere lo stato corrente dell&#39;oggetto che ha attivato l&#39;evento. Il metodo helper quindi ispezionerà i `filter` criteri e verrà restituito solo se l&#39;elemento corrente `dataObject` soddisfa il filtro.

   >[!CAUTION]
   >
   > Sarà importante **non** aggiornare il browser durante questo esercizio, altrimenti la console JavaScript andrà persa.

1. Quindi, immettete un gestore eventi che verrà chiamato quando un componente **Teaser** viene visualizzato all’interno di un **carosello**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Il metodo `teaserShownHandler` verrà chiamato e passato un filtro di `getDataObjectHelper` come `wknd/components/teaser` `@type` filtro degli eventi attivati da altri componenti.

1. Quindi, inviate un listener di eventi al livello dati per ascoltare l&#39; `cmp:show` evento.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   L&#39; `cmp:show` evento viene attivato da molti componenti diversi, ad esempio quando una nuova diapositiva viene visualizzata nel **carosello** o quando viene selezionata una nuova scheda nel componente **Tab** .

1. Passate alla pagina e osservate le istruzioni della console nelle diapositive del carosello:

   ![Attiva/disattiva il carosello e visualizza il listener di eventi](assets/teaser-console-slides.png)

1. Rimuovete il listener di eventi dal livello dati per interrompere l&#39;ascolto dell&#39; `cmp:show` evento:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Tornate alla pagina e attivate le diapositive del carosello. Osservate che non vengono registrate altre istruzioni e che l&#39;evento non viene ascoltato.

1. Quindi, immettere un gestore eventi che verrà chiamato quando viene attivato l&#39;evento pageShow:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Il tipo di risorsa `wknd/components/page` viene utilizzato per filtrare l&#39;evento.

1. Quindi, inviate un listener di eventi sul livello dati per ascoltare l&#39; `cmp:show` evento, chiamando il `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. È consigliabile visualizzare immediatamente un&#39;istruzione console attivata con i dati della pagina:

   ![Dati della visualizzazione della pagina](assets/page-show-console-data.png)

   L’ `cmp:show` evento per la pagina viene attivato su ogni caricamento di pagina nella parte superiore della pagina. È possibile chiedere, perché il gestore eventi è stato attivato quando la pagina è già stata caricata?

   Si tratta di una delle caratteristiche uniche del Livello dati client del Adobe , in quanto è possibile registrare i listener di eventi **prima** o **dopo** l&#39;inizializzazione del Livello dati. Questa è una caratteristica fondamentale per evitare le condizioni della razza.

   Il livello dati mantiene un array di coda di tutti gli eventi che si sono verificati in sequenza. Per impostazione predefinita, il livello dati attiva il callback di eventi per gli eventi che si sono verificati in **passato** e per gli eventi in **futuro**. È possibile filtrare gli eventi solo in passato o in futuro. [Ulteriori informazioni sono disponibili nella documentazione](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Passaggi successivi

Per informazioni su come utilizzare il livello Dati client del Adobe basato sull&#39;evento per [raccogliere i dati della pagina e inviarli a  Adobe Analytics](../analytics/collect-data-analytics.md), consultate la seguente esercitazione.


## Risorse aggiuntive {#additional-resources}

* [Documentazione sul livello dei dati del cliente  Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Utilizzo della documentazione sul livello dati client e sui componenti core del Adobe](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/data-layer/overview.html)
