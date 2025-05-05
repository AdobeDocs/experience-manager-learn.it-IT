---
title: Utilizzo di Adobe Client Data Layer con i componenti core AEM
description: Adobe Client Data Layer introduce un metodo standard per raccogliere e memorizzare i dati sull’esperienza di un visitatore in una pagina web e semplifica l’accesso a tali dati. Adobe Client Data Layer è indipendente dalla piattaforma, ma è completamente integrato nei Componenti core per l’utilizzo con AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
jira: KT-6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
doc-type: Tutorial
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
duration: 777
source-git-commit: dc40b8e022477d2b1d8f0ffe3b5e8bcf13be30b3
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 6%

---

# Utilizzo di Adobe Client Data Layer con i componenti core AEM {#overview}

Adobe Client Data Layer introduce un metodo standard per raccogliere e memorizzare i dati sull’esperienza di un visitatore in una pagina web e semplifica l’accesso a tali dati. Adobe Client Data Layer è indipendente dalla piattaforma, ma è completamente integrato nei Componenti core per l’utilizzo con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330115?quality=12&learn=on&captions=ita)

>[!NOTE]
>
> Abilitare Adobe Client Data Layer nel sito AEM? [Consulta le istruzioni](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it#installation-activation).

## Esplorare Data Layer

Puoi avere un&#39;idea della funzionalità integrata di Adobe Client Data Layer utilizzando gli strumenti per sviluppatori del browser e il [sito di riferimento WKND](https://wknd.site/us/en.html) attivo.

>[!NOTE]
>
> Screenshot prese dal browser Chrome.

1. Passa a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Apri gli strumenti per sviluppatori e immetti il comando seguente nella **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Per visualizzare lo stato corrente del livello dati su un sito AEM, controlla la risposta. Dovresti visualizzare informazioni sulla pagina e sui singoli componenti.

   ![Risposta Adobe Data Layer](assets/data-layer-state-response.png)

1. Invia un oggetto dati al livello dati immettendo quanto segue nella console:

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

1. Eseguire nuovamente il comando `adobeDataLayer.getState()` e trovare la voce per `training-data`.
1. Quindi aggiungi un parametro percorso per restituire solo lo stato specifico di un componente:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Restituire solo una voce di dati di un singolo componente](assets/return-just-single-component.png)

## Utilizzo degli eventi

È consigliabile attivare qualsiasi codice personalizzato basato su un evento dal livello dati. Quindi, esplora la registrazione e l’ascolto di diversi eventi.

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

   Il codice riportato sopra esamina l&#39;oggetto `event` e utilizza il metodo `adobeDataLayer.getState` per ottenere lo stato corrente dell&#39;oggetto che ha attivato l&#39;evento. Il metodo helper esamina `filter` e solo se l&#39;attuale `dataObject` soddisfa i criteri di filtro viene restituito.

   >[!CAUTION]
   >
   > È importante **not** aggiornare il browser durante questo esercizio, altrimenti la console JavaScript andrà persa.

1. Quindi, immetti un gestore eventi chiamato quando viene visualizzato un componente **Teaser** in un **Carosello**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/carousel/item"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   La funzione `teaserShownHandler` chiama la funzione `getDataObjectHelper` e passa un filtro di `wknd/components/carousel/item` come `@type` per filtrare gli eventi attivati da altri componenti.

1. Quindi, invia un listener di eventi sul livello dati per l&#39;ascolto dell&#39;evento `cmp:show`.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   L&#39;evento `cmp:show` viene attivato da molti componenti diversi, ad esempio quando viene visualizzata una nuova diapositiva nel **carosello** o quando viene selezionata una nuova scheda nel componente **Scheda**.

1. Sulla pagina, attiva le diapositive del carosello e osserva le istruzioni della console:

   ![Attiva/disattiva carosello e visualizza il listener di eventi](assets/teaser-console-slides.png)

1. Per interrompere l&#39;ascolto per l&#39;evento `cmp:show`, rimuovere il listener di eventi dal livello dati

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Torna alla pagina e commuta le diapositive del carosello. Tieni presente che non vengono registrate altre istruzioni e che l’evento non viene ascoltato.

1. Quindi, crea un gestore eventi chiamato quando viene attivato l&#39;evento di visualizzazione della pagina:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Tieni presente che per filtrare l&#39;evento viene utilizzato il tipo di risorsa `wknd/components/page`.

1. Quindi, invia un listener di eventi sul livello dati per l&#39;ascolto dell&#39;evento `cmp:show`, chiamando `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Dovresti vedere immediatamente un’istruzione della console attivata con i dati della pagina:

   ![Dati visualizzazione pagina](assets/page-show-console-data.png)

   L&#39;evento `cmp:show` per la pagina viene attivato a ogni caricamento di pagina nella parte superiore della pagina. Potresti chiedere, perché è stato attivato il gestore eventi, quando la pagina è già stata chiaramente caricata?

   Una delle caratteristiche esclusive di Adobe Client Data Layer è la possibilità di registrare i listener di eventi **prima** o **dopo** l&#39;inizializzazione di Data Layer, in modo da evitare le race condition.

   Data Layer mantiene una matrice di coda di tutti gli eventi che si sono verificati in sequenza. Per impostazione predefinita, Data Layer attiva i callback di eventi per gli eventi che si sono verificati nel **passato** e per gli eventi nel **futuro**. È possibile filtrare gli eventi dal passato o dal futuro. [Ulteriori informazioni sono disponibili nella documentazione](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Passaggi successivi

Sono disponibili due opzioni per continuare l&#39;apprendimento: la prima consiste nell&#39;estrarre [raccogliere i dati della pagina e inviarli all&#39;esercitazione di Adobe Analytics](../analytics/collect-data-analytics.md) che illustra l&#39;utilizzo di Adobe Client Data Layer. La seconda opzione consiste nell&#39;apprendere come [Personalizzare Adobe Client Data Layer con i componenti AEM](./data-layer-customize.md)


## Risorse aggiuntive {#additional-resources}

* [Documentazione di Adobe Client Data Layer](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Utilizzo della documentazione Adobe Client Data Layer e Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it)
