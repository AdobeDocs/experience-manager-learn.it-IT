---
title: Utilizzo di Adobe Client Data Layer con AEM componenti core
description: Adobe Client Data Layer introduce un metodo standard per raccogliere e memorizzare dati sull’esperienza di un visitatore su una pagina web e quindi semplificare l’accesso a tali dati. Adobe Client Data Layer è indipendente dalla piattaforma, ma è completamente integrato nei Componenti core per l’utilizzo con AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 8%

---

# Utilizzo di Adobe Client Data Layer con AEM componenti core {#overview}

Adobe Client Data Layer introduce un metodo standard per raccogliere e memorizzare dati sull’esperienza di un visitatore su una pagina web e quindi semplificare l’accesso a tali dati. Adobe Client Data Layer è indipendente dalla piattaforma, ma è completamente integrato nei Componenti core per l’utilizzo con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Vuoi abilitare Adobe Client Data Layer sul tuo sito AEM? [Vedi le istruzioni qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Esplorare il livello dati

Puoi ottenere un&#39;idea delle funzionalità integrate di Adobe Client Data Layer solo utilizzando gli strumenti per sviluppatori del tuo browser e del live [Sito di riferimento WKND](https://wknd.site/).

>[!NOTE]
>
> Screenshot qui sotto tratto dal browser Chrome.

1. Passa a [https://wknd.site](https://wknd.site)
1. Apri gli strumenti per sviluppatori e immetti il seguente comando nella **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect la risposta per visualizzare lo stato corrente del livello di dati su un sito AEM. Dovresti visualizzare informazioni sulla pagina e sui singoli componenti.

   ![Adobe risposta livello dati](assets/data-layer-state-response.png)

1. Invia un oggetto dati a un livello dati immettendo quanto segue nella console:

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

1. Esegui il comando `adobeDataLayer.getState()` di nuovo e trova la voce per `training-data`.
1. Quindi aggiungi un parametro di percorso per restituire solo lo stato specifico di un componente:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Restituire una sola immissione di dati di un componente](assets/return-just-single-component.png)

## Utilizzo degli eventi

È consigliabile attivare qualsiasi codice personalizzato basato su un evento dal livello dati. Quindi, esplora la registrazione e l&#39;ascolto di eventi diversi.

1. Immetti il seguente metodo helper nella console:

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

   Il codice di cui sopra esamina il `event` e utilizza `adobeDataLayer.getState` per ottenere lo stato corrente dell&#39;oggetto che ha attivato l&#39;evento. Il metodo helper ispeziona quindi il `filter` e solo se `dataObject` soddisfa il filtro che verrà restituito.

   >[!CAUTION]
   >
   > È importante **not** per aggiornare il browser durante questo esercizio, altrimenti la console JavaScript viene persa.

1. Quindi, immetti un gestore eventi chiamato quando un **Teaser** viene visualizzato all’interno di un **Carosello**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   La `teaserShownHandler` chiamerà il `getDataObjectHelper` e passare un filtro di `wknd/components/teaser` come `@type` per filtrare gli eventi attivati da altri componenti.

1. Quindi, invia un listener di eventi sul livello dati per ascoltare il `cmp:show` evento.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   La `cmp:show` l’evento viene attivato da molti componenti diversi, ad esempio quando una nuova diapositiva viene visualizzata nella **Carosello** o quando viene selezionata una nuova scheda nel **Scheda** componente.

1. Nella pagina attiva le diapositive del carosello e osserva le istruzioni della console:

   ![Attiva/disattiva il carosello e visualizza il listener di eventi](assets/teaser-console-slides.png)

1. Rimuovi il listener di eventi dal livello dati per interrompere l&#39;ascolto del `cmp:show` evento:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Torna alla pagina e attiva le diapositive del carosello. Osserva che non vengono più registrate istruzioni e che l’evento non viene ascoltato.

1. Quindi, crea un gestore di eventi che viene chiamato quando viene attivato l&#39;evento di visualizzazione della pagina:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Tieni presente che il tipo di risorsa `wknd/components/page` viene utilizzato per filtrare l’evento.

1. Quindi, invia un listener di eventi sul livello dati per ascoltare il `cmp:show` evento, chiamando `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Dovresti visualizzare immediatamente un’istruzione console attivata con i dati della pagina:

   ![Visualizzazione dei dati della pagina](assets/page-show-console-data.png)

   La `cmp:show` l&#39;evento per la pagina viene attivato a ogni caricamento di pagina nella parte superiore della pagina. Potresti chiedere, perché il gestore eventi è stato attivato, quando la pagina è chiaramente già stata caricata?

   Questa è una delle caratteristiche uniche di Adobe Client Data Layer, in quanto puoi registrare i listener di eventi **prima** o **dopo** Livello dati inizializzato. Questa è una caratteristica fondamentale per evitare le condizioni di razza.

   Il livello dati mantiene una matrice di coda di tutti gli eventi che si sono verificati in sequenza. Il Livello dati per impostazione predefinita attiva i callback degli eventi per gli eventi che si sono verificati nel **passato** nonché gli eventi **futuro**. È possibile filtrare gli eventi solo in passato o in futuro. [Ulteriori informazioni sono disponibili nella documentazione](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Passaggi successivi

Consulta la seguente esercitazione per scoprire come utilizzare il livello dati client Adobe basato su eventi per [raccogliere i dati della pagina e inviarli ad Adobe Analytics](../analytics/collect-data-analytics.md).

Oppure impara come [Personalizzare Adobe Client Data Layer con i componenti AEM](./data-layer-customize.md)


## Risorse aggiuntive {#additional-resources}

* [Documentazione di Adobe Client Data Layer](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Utilizzo della documentazione di Adobe Client Data Layer e Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
