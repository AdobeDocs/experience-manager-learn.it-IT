---
title: Tenere traccia del componente su cui è stato fatto clic con  Adobe Analytics
description: Utilizzate il livello Dati client del Adobe  basato su eventi per monitorare i clic di componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole del Experience Platform Launch per ascoltare questi eventi e inviare dati a un Adobe Analytics  con un beacon di collegamento di tracciamento.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 64c167ec1d625fdd8be1bc56f7f5e59460b8fed3
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 1%

---


# Tenere traccia del componente su cui è stato fatto clic con  Adobe Analytics

Utilizzate il [ Adobe Client Data Layer con componenti core AEM](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/data-layer/overview.html) basato sull&#39;evento per monitorare i clic di componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole del Experience Platform Launch per ascoltare gli eventi click, filtrare per componente e inviare i dati a un Adobe Analytics  con un beacon di collegamento track.

## Cosa verrà creato

Il team di marketing WKND vuole capire quali pulsanti Invito all’azione (CTA, Call To Action) eseguono al meglio sulla pagina principale. In questa esercitazione verrà aggiunta una nuova regola nel Experience Platform Launch che ascolta gli eventi `cmp:click` dai componenti **Teaser** e **Button** e che invia l&#39;ID componente e un nuovo evento ad  Adobe Analytics accanto al beacon di tracciamento del collegamento.

![Elementi per la generazione di clic di tracciamento](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Obiettivi {#objective}

1. Create una regola basata sull&#39;evento in Launch in base all&#39;evento `cmp:click`.
1. Filtrare i diversi eventi in base al tipo di risorsa del componente.
1. Impostate l’ID del componente su cui avete fatto clic e inviate l’evento  Adobe Analytics con il beacon di tracciamento del collegamento.

## Prerequisiti

Questa esercitazione è una continuazione di [Raccogliere i dati di pagina con  Adobe Analytics](./collect-data-analytics.md) e presuppone che possiate:

* A **Proprietà lancio** con l&#39;estensione [ Adobe Analytics](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) abilitata
* **ID suite di rapporti** Analytics/dev di Adobe e server di tracciamento. Consulta la seguente documentazione per [creare una nuova suite di rapporti](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [&#39;estensione ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) del browser del debugger di Experience Platform configurata con la proprietà Launch caricata in  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlor un sito AEM con  Adobe livello dati di.

##  Inspect lo schema Pulsante e Teaser

Prima di creare le regole in Launch è utile rivedere lo schema [per Button e Teaser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) e controllarle nell&#39;implementazione del livello dati.

1. Passa a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Apri gli strumenti di sviluppo del browser e passa alla **Console**. Eseguite il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Questo restituisce lo stato corrente del livello dati client del Adobe .

   ![Stato del livello dati tramite la console del browser](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Espandete la risposta e individuate le voci con il prefisso `button-` e `teaser-xyz-cta`. Dovrebbe essere visualizzato uno schema di dati simile al seguente:

   Schema pulsante:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Schema teaser:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Questi sono basati sullo [Schema componente/elemento contenitore](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item). La regola che creeremo in Launch utilizzerà questo schema.

## Creare una regola con clic CTA

Il livello dati client del Adobe  è un livello dati basato su **event**. Quando si fa clic su un componente core, un evento `cmp:click` viene inviato tramite il livello dati. Quindi create una regola per ascoltare l&#39;evento `cmp:click`.

1. Passare all&#39;Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Passare alla sezione **Regole** nell&#39;interfaccia utente di avvio, quindi fare clic su **Aggiungi regola**.
1. Denominate la regola **CTA Selezionata**.
1. Fare clic su **Eventi** > **Aggiungi** per aprire la procedura guidata **Configurazione evento**.
1. In **Tipo evento** selezionare **Codice personalizzato**.

   ![Denominate la regola CTA Selezionato e aggiungete l&#39;evento codice personalizzato](assets/track-clicked-component/custom-code-event.png)

1. Fare clic su **Apri editor** nel pannello principale e immettere il frammento di codice seguente:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   Lo snippet di codice riportato sopra aggiunge un listener di eventi [spingendo una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando l&#39;evento `cmp:click` viene attivato, viene chiamata la funzione `componentClickedHandler`. In questa funzione vengono aggiunti alcuni controlli di integrità e viene creato un nuovo oggetto `event` con lo stato [più recente del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l&#39;evento.

   Dopo che `trigger(event)` è stato chiamato. `trigger()` è un nome riservato in Launch e &quot;attiverà&quot; la regola Launch. Passiamo l&#39;oggetto `event` come parametro che a sua volta sarà esposto da un altro nome riservato in Launch denominato `event`. Gli elementi dati in Launch possono ora fare riferimento a varie proprietà come ad esempio: `event.component['someKey']`.

1. Salva le modifiche.
1. Successivamente, in **Azioni** fare clic su **Aggiungi** per aprire la procedura guidata **Configurazione azione**.
1. In **Tipo azione** scegliere **Codice personalizzato**.

   ![Tipo azione codice personalizzato](assets/track-clicked-component/action-custom-code.png)

1. Fare clic su **Apri editor** nel pannello principale e immettere il frammento di codice seguente:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   L&#39;oggetto `event` viene passato dal metodo `trigger()` chiamato nell&#39;evento personalizzato. `component` è lo stato corrente del componente derivato dal livello dati  `getState` che ha attivato il clic.

1. Salvare le modifiche ed eseguire una [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) in Launch per promuovere il codice nell&#39; [ambiente](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) utilizzato sul sito AEM.

   >[!NOTE]
   >
   > Può essere molto utile utilizzare il [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) per passare al codice da incorporare in un ambiente **Development**.

1. Andate al [sito WKND](https://wknd.site/us/en.html) e aprite gli strumenti di sviluppo per visualizzare la console. Selezionare **Mantieni registro**.

1. Fare clic su uno dei pulsanti **Teaser** o **Button** CTA per passare a un&#39;altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Osservate nella console dello sviluppatore che la regola **CTA Click** è stata attivata:

   ![Pulsante CTA selezionato](assets/track-clicked-component/cta-button-clicked-log.png)

## Crea elementi dati

Create quindi un elemento dati per acquisire l’ID componente e il titolo su cui è stato fatto clic. Ricordare nell&#39;esercizio precedente l&#39;output di `event.path` era simile a `component.button-b6562c963d` e il valore di `event.component['dc:title']` era simile a &quot;View Trips&quot;.

### ID componente

1. Passare all&#39;Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Andate alla sezione **Elementi dati** e fate clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** immettere **ID componente**.
1. Per **Data Element Type** selezionare **Custom Code**.

   ![Modulo elemento dati ID componente](assets/track-clicked-component/component-id-data-element.png)

1. Fare clic su **Apri editor** e immettere quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Salva le modifiche.

   >[!NOTE]
   >
   > Ricordare che l&#39;oggetto `event` è reso disponibile e l&#39;ambito in base all&#39;evento che ha attivato la **regola** in Launch. Il valore di un elemento dati è impostato solo se l&#39;elemento dati è *a cui si fa riferimento* all&#39;interno di una regola. Pertanto è sicuro utilizzare questo elemento dati all&#39;interno di una regola come la regola **CTA Clic** creata nell&#39;esercizio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.

### Titolo componente

1. Andate alla sezione **Elementi dati** e fate clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** immettere **Titolo componente**.
1. Per **Data Element Type** selezionare **Custom Code**.
1. Fare clic su **Apri editor** e immettere quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salva le modifiche.

## Aggiunta di una condizione alla regola CTA Clic

Quindi, aggiornate la regola **CTA Clic** per garantire che la regola venga attivata solo quando l&#39;evento `cmp:click` viene attivato per un **teaser** o un **Button**. Poiché il CTA del teaser è considerato un oggetto separato nel livello dati, è importante controllare il padre per verificare che provenga da un teaser.

1. Nell&#39;interfaccia utente di Launch, passare alla regola **CTA Click** creata in precedenza.
1. In **Condizioni** fare clic su **Aggiungi** per aprire la **Configurazione condizione**.
1. Per **Tipo condizione** selezionare **Codice personalizzato**.

   ![Codice personalizzato condizione clic su CTA](assets/track-clicked-component/custom-code-condition.png)

1. Fare clic su **Apri editor** e immettere quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   Il codice riportato sopra controlla prima se il tipo di risorsa proviene da un **Pulsante**, quindi controlla se il tipo di risorsa proviene da un CTA all&#39;interno di un **Teaser**.

1. Salva le modifiche.

## Imposta variabili Analytics e attiva il beacon Track Link

Attualmente, la regola **CTA Click** genera semplicemente un&#39;istruzione console. Quindi, utilizzate gli elementi dati e l&#39;estensione Analytics per impostare le variabili Analytics come **action**. Verrà inoltre impostata un&#39;azione aggiuntiva per attivare il **Collegamento traccia** e inviare i dati raccolti a  Adobe Analytics.

1. Nella regola **CTA Clic** **remove** l&#39;azione **Core - Custom Code** (le istruzioni della console):

   ![Rimuovi azione codice personalizzata](assets/track-clicked-component/remove-console-statements.png)

1. In Azioni, fare clic su **Aggiungi** per aggiungere una nuova azione.
1. Impostate il tipo **Extension** su **Adobe Analytics** e impostate il tipo di azione **Tipo di azione** su **Imposta variabili**.

1. Impostate i seguenti valori per **eVar**, **Props** e **Events**:

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![Impostare  Prop e gli eventi](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Qui si utilizza `%Component ID%` perché garantisce un identificatore univoco per il CTA su cui è stato fatto clic. Un potenziale lato negativo dell&#39;utilizzo di `%Component ID%` è rappresentato dal fatto che il report di Analytics conterrà valori come `button-2e6d32893a`. L&#39;utilizzo di `%Component Title%` darebbe un nome più descrittivo, ma il valore potrebbe non essere univoco.

1. Quindi, aggiungere un&#39;azione aggiuntiva a destra dell&#39;Adobe Analytics **- Imposta variabili** toccando l&#39;icona **più**:

   ![Aggiunta di un&#39;altra azione di avvio](assets/track-clicked-component/add-additional-launch-action.png)

1. Impostate il tipo **Extension** su **Adobe Analytics** e impostate il tipo di azione **Tipo di azione** su **Invia beacon**.
1. In **Tracking** impostare il pulsante di scelta su **`s.tl()`**.
1. Per **Tipo di collegamento** scegliere **Collegamento personalizzato** e per **Nome collegamento** impostare il valore su: **`%Component Title%: CTA Clicked`**:

   ![Configurazione per Send Link beacon](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   In questo modo si combinano la variabile dinamica dell&#39;elemento dati **Titolo componente** e la stringa statica **CTA Click**.

1. Salva le modifiche. La regola **CTA Click** deve ora avere la seguente configurazione:

   ![Configurazione del lancio finale](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Ascoltare l&#39; `cmp:click` evento.
   * **2.** Verificate che l&#39;evento sia stato attivato da un  **** Teaser  **Buttonor**.
   * **3.** Impostate le variabili di Analytics per tenere traccia dell’ **ID** componente come un  **eVar**,  **prop** e un  **evento**.
   * **4.** Invia il beacon per il collegamento traccia di Analytics (e non  **** lo realizza come visualizzazione di pagina).

1. Salvate tutte le modifiche e create la libreria Launch, promuovendo l&#39;ambiente appropriato.

## Convalida del beacon e della chiamata Track Link

Ora che la regola **CTA Click** invia il beacon di Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando il debugger di Experience Platform .

1. Aprite il [sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fate clic sull&#39;icona Debugger ![icona Debugger piattaforma esperienza](assets/track-clicked-component/experience-cloud-debugger.png) per aprire il Debugger Experience Platform .
1. Assicurarsi che il debugger esegua la mappatura della proprietà Launch in *l&#39;ambiente di sviluppo*, come descritto in precedenza, e che **Console Logging** sia selezionato.
1. Aprite il menu Analytics e verificate che la suite di rapporti sia impostata su *la vostra* suite di rapporti.

   ![Debugger scheda Analisi](assets/track-clicked-component/analytics-tab-debugger.png)

1. Nel browser, fare clic su uno dei pulsanti **Teaser** o **Button** CTA per passare a un&#39;altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Tornare al debugger del Experience Platform , scorrere verso il basso ed espandere **Richieste di rete** > *Suite di rapporti*. È necessario essere in grado di trovare il set **eVar**, **prop** e **event**.

   ![Eventi di Analytics, evar e prop tracciati al momento del clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Tornate al browser e aprite la console per sviluppatori. Andate al piè di pagina del sito e fate clic su uno dei collegamenti di navigazione:

   ![Fare clic sul collegamento Navigazione nel piè di pagina](assets/track-clicked-component/click-navigation-link-footer.png)

1. Osservate nella console del browser il messaggio *&quot;Codice personalizzato&quot; per la regola &quot;CTA Click&quot; non è stato soddisfatto*.

   Questo perché il componente di navigazione attiva un evento `cmp:click` *ma* a causa del controllo dell&#39;evento rispetto al tipo di risorsa, non viene eseguita alcuna azione.

   >[!NOTE]
   >
   > Se non sono presenti registri di console, assicurarsi che **Console Logging** sia selezionato in **Launch** nel debugger di Experience Platform .

## Congratulazioni!

Avete appena utilizzato il Livello dati client  Adobe basato su eventi e il Experience Platform Launch per monitorare i clic di componenti specifici su un sito Adobe Experience Manager.