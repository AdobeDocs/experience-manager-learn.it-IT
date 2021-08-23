---
title: Utilizzo di Adobe Client Data Layer con AEM componenti core
description: Adobe Client Data Layer introduce un metodo standard per raccogliere e memorizzare dati sull’esperienza di un visitatore su una pagina web e quindi semplificare l’accesso a tali dati. Adobe Client Data Layer è indipendente dalla piattaforma, ma è completamente integrato nei componenti core per l’utilizzo con AEM.
topic: Integrations (Integrazioni)
feature: Adobe Client Data Layer, Componenti core
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---


# Utilizzo di Adobe Client Data Layer con AEM componenti core {#overview}

Adobe Client Data Layer introduce un metodo standard per raccogliere e memorizzare dati sull’esperienza di un visitatore su una pagina web e quindi semplificare l’accesso a tali dati. Adobe Client Data Layer è indipendente dalla piattaforma, ma è completamente integrato nei componenti core per l’utilizzo con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Vuoi abilitare Adobe Client Data Layer sul tuo sito AEM? [Vedi le istruzioni qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Esplorare il livello dati

Puoi ottenere un&#39;idea delle funzionalità integrate di Adobe Client Data Layer solo utilizzando gli strumenti per sviluppatori del browser e il [sito di riferimento WKND](https://wknd.site/) live.

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

1. Esegui di nuovo il comando `adobeDataLayer.getState()` e trova la voce per `training-data`.
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

   Il codice riportato sopra controllerà l&#39;oggetto `event` e utilizzerà il metodo `adobeDataLayer.getState` per ottenere lo stato corrente dell&#39;oggetto che ha attivato l&#39;evento. Il metodo helper esaminerà quindi i criteri `filter` e verrà restituito solo se l&#39;elemento `dataObject` corrente soddisfa il filtro.

   >[!CAUTION]
   >
   > Sarà importante **non** aggiornare il browser durante questo esercizio, altrimenti il JavaScript della console verrà perso.

1. Quindi, immetti un gestore eventi che verrà chiamato quando un componente **Teaser** viene visualizzato all’interno di un **Carosello**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Il `teaserShownHandler` chiamerà il metodo `getDataObjectHelper` e trasmetterà un filtro di `wknd/components/teaser` come `@type` per filtrare gli eventi attivati da altri componenti.

1. Quindi, invia un listener di eventi sul livello dati per ascoltare l&#39;evento `cmp:show`.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   L’evento `cmp:show` viene attivato da molti componenti diversi, ad esempio quando una nuova diapositiva viene visualizzata nel componente **Carosello** o quando è selezionata una nuova scheda nel componente **Tab** .

1. Nella pagina attiva le diapositive del carosello e osserva le istruzioni della console:

   ![Attiva/disattiva il carosello e visualizza il listener di eventi](assets/teaser-console-slides.png)

1. Rimuovi il listener di eventi dal livello dati per interrompere l&#39;ascolto dell&#39;evento `cmp:show`:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Torna alla pagina e attiva le diapositive del carosello. Osserva che non vengono più registrate istruzioni e che l’evento non viene ascoltato.

1. Quindi, immetti un gestore di eventi che verrà chiamato quando l&#39;evento di visualizzazione della pagina viene attivato:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Il tipo di risorsa `wknd/components/page` viene utilizzato per filtrare l’evento.

1. Quindi, invia un listener di eventi sul livello dati per ascoltare l&#39;evento `cmp:show`, chiamando `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Dovresti visualizzare immediatamente un’istruzione console attivata con i dati della pagina:

   ![Visualizzazione dei dati della pagina](assets/page-show-console-data.png)

   L&#39;evento `cmp:show` per la pagina viene attivato su ogni caricamento di pagina nella parte superiore della pagina. Potresti chiedere, perché il gestore eventi è stato attivato, quando la pagina è chiaramente già stata caricata?

   Questa è una delle caratteristiche uniche di Adobe Client Data Layer, in quanto è possibile registrare listener di eventi **prima** o **dopo** che il livello dati è stato inizializzato. Questa è una caratteristica fondamentale per evitare le condizioni di razza.

   Il livello dati mantiene una matrice di coda di tutti gli eventi che si sono verificati in sequenza. Per impostazione predefinita, il Livello dati attiva i callback degli eventi per gli eventi che si sono verificati in **passato** e per gli eventi in **futuro**. È possibile filtrare gli eventi solo in passato o in futuro. [Ulteriori informazioni sono disponibili nella documentazione](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Passaggi successivi

Consulta la seguente esercitazione per scoprire come utilizzare il livello dati client Adobe basato su eventi per [raccogliere i dati di pagina e inviarli ad Adobe Analytics](../analytics/collect-data-analytics.md).

Oppure scopri come [personalizzare Adobe Client Data Layer con i componenti AEM](./data-layer-customize.md)


## Risorse aggiuntive {#additional-resources}

* [Documentazione di Adobe Client Data Layer](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Utilizzo della documentazione di Adobe Client Data Layer e Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
