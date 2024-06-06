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

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Abilitare Adobe Client Data Layer nel sito AEM? [Consulta le istruzioni qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Esplorare Data Layer

Puoi avere un’idea della funzionalità integrata di Adobe Client Data Layer semplicemente utilizzando gli strumenti per sviluppatori del browser e la versione live [Sito di riferimento WKND](https://wknd.site/us/en.html).

>[!NOTE]
>
> Schermate qui sotto prese dal browser Chrome.

1. Accedi a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Apri gli strumenti per sviluppatori e immetti il comando seguente nel **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Per visualizzare lo stato corrente del livello dati su un sito AEM, controlla la risposta. Dovresti visualizzare informazioni sulla pagina e sui singoli componenti.

   ![Adobe di risposta del livello dati](assets/data-layer-state-response.png)

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

1. Esegui il comando `adobeDataLayer.getState()` e trova la voce per `training-data`.
1. Quindi aggiungi un parametro percorso per restituire solo lo stato specifico di un componente:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Restituire solo un&#39;immissione di dati di un singolo componente](assets/return-just-single-component.png)

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

   Il codice di cui sopra esamina `event` e utilizza `adobeDataLayer.getState` per ottenere lo stato corrente dell&#39;oggetto che ha attivato l&#39;evento. Quindi il metodo helper controlla `filter` e solo se l&#39;attuale `dataObject` soddisfa i criteri di filtro restituiti.

   >[!CAUTION]
   >
   > È importante **non** per aggiornare il browser durante questo esercizio, altrimenti il JavaScript della console andrà perso.

1. Quindi, immetti un gestore eventi chiamato quando un **Teaser** componente è visualizzato all&#39;interno di un **Carosello**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/carousel/item"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Il `teaserShownHandler` la funzione chiama il `getDataObjectHelper` e trasmette un filtro di `wknd/components/carousel/item` come `@type` per filtrare gli eventi attivati da altri componenti.

1. Quindi, invia un listener di eventi sul livello dati per ascoltare `cmp:show` evento.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   Il `cmp:show` viene attivato da molti componenti diversi, ad esempio quando viene visualizzata una nuova diapositiva nel **Carosello** o quando viene selezionata una nuova scheda in **Linguetta** componente.

1. Sulla pagina, attiva le diapositive del carosello e osserva le istruzioni della console:

   ![Attiva/disattiva carosello e visualizza il listener di eventi](assets/teaser-console-slides.png)

1. Per interrompere l&#39;ascolto di `cmp:show` rimuovere il listener di eventi dal livello dati

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

   Tieni presente che il tipo di risorsa `wknd/components/page` viene utilizzato per filtrare l’evento.

1. Quindi, invia un listener di eventi sul livello dati per ascoltare `cmp:show` evento, chiamando `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Dovresti vedere immediatamente un’istruzione della console attivata con i dati della pagina:

   ![Mostra dati pagina](assets/page-show-console-data.png)

   Il `cmp:show` per la pagina viene attivato al caricamento di ogni pagina nella parte superiore della pagina. Potresti chiedere, perché è stato attivato il gestore eventi, quando la pagina è già stata chiaramente caricata?

   Una delle funzionalità esclusive di Adobe Client Data Layer è la possibilità di registrare i listener di eventi **prima di** o **dopo** Data Layer è stato inizializzato, aiuta ad evitare le race condition.

   Data Layer mantiene una matrice di coda di tutti gli eventi che si sono verificati in sequenza. Per impostazione predefinita, Data Layer attiva i callback di eventi per gli eventi che si sono verificati in **passato** ed eventi in **futuro**. È possibile filtrare gli eventi dal passato o dal futuro. [Per ulteriori informazioni consulta la documentazione](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Passaggi successivi

Ci sono due opzioni per continuare l’apprendimento, la prima, controlla il [raccogliere i dati della pagina e inviarli ad Adobe Analytics](../analytics/collect-data-analytics.md) tutorial che illustra l’utilizzo di Adobe Client Data Layer. La seconda opzione consiste nell&#39;apprendere come [Personalizzare Adobe Client Data Layer con i componenti AEM](./data-layer-customize.md)


## Risorse aggiuntive {#additional-resources}

* [Documentazione di Adobe Client Data Layer](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Utilizzo della documentazione di Adobe Client Data Layer e Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it)
