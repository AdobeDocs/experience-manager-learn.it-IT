---
title: Tenere traccia del componente su cui è stato fatto clic con Adobe Analytics
description: Utilizza Adobe Client Data Layer basato su eventi per tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole nel Experience Platform Launch per ascoltare questi eventi e inviare dati a un Adobe Analytics con un beacon di collegamento di tracciamento.
version: cloud-service
topic: Integrations (Integrazioni)
feature: Livello dati client di Adobe
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1814'
ht-degree: 1%

---


# Tenere traccia del componente su cui è stato fatto clic con Adobe Analytics

Utilizza [Adobe Client Data Layer basato su eventi con AEM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) per tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole in Experience Platform Launch per rilevare gli eventi di clic, filtrare per componente e inviare i dati a un Adobe Analytics con un beacon di tracciamento dei collegamenti.

## Cosa verrà creato

Il team di marketing WKND desidera capire quali pulsanti Invito all’azione (CTA) hanno prestazioni migliori nella home page. In questa esercitazione aggiungeremo una nuova regola in Experience Platform Launch che ascolta gli eventi `cmp:click` dai componenti **Teaser** e **Button** e invieremo l’ID componente e un nuovo evento ad Adobe Analytics insieme al beacon di tracciamento del collegamento.

![Cosa verrà creato per tracciare i clic](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Obiettivi {#objective}

1. Crea una regola basata su eventi in Launch in base all&#39;evento `cmp:click` .
1. Filtrare i diversi eventi per tipo di risorsa componente.
1. Imposta l’id del componente su cui hai fatto clic e invia l’evento Adobe Analytics con il beacon di tracciamento del collegamento.

## Prerequisiti

Questa esercitazione è una continuazione di [Raccogliere dati di pagina con Adobe Analytics](./collect-data-analytics.md) e presuppone che tu abbia:

* A **Proprietà Launch** con l&#39;estensione [Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) abilitata
* **Adobe ID suite di rapporti** Analytics/dev e server di tracciamento. Consulta la seguente documentazione per [creare una nuova suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform estensione ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Debuggerbrowser configurata con la proprietà Launch caricata su  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlor un sito AEM con l’Adobe Data Layer abilitato.

## Inspect Pulsante e schema teaser

Prima di creare regole in Launch è utile rivedere lo schema [per i pulsanti e i teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) e ispezionarli nell&#39;implementazione del livello dati.

1. Passa a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Apri gli strumenti di sviluppo del browser e passa alla **Console**. Esegui il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Restituisce lo stato corrente del livello dati client di Adobe.

   ![Stato del livello dati tramite la console del browser](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Espandi la risposta e trova le voci con prefisso `button-` e `teaser-xyz-cta`. Dovresti visualizzare uno schema di dati simile al seguente:

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

   Si basano su [Schema componente/contenitore elemento](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). La regola che creeremo in Launch utilizzerà questo schema.

## Creare una regola Clic su CTA

Adobe Client Data Layer è un livello dati guidato da **evento** . Quando fai clic su un componente core qualsiasi, un evento `cmp:click` viene inviato tramite il livello dati. Quindi crea una regola per ascoltare l&#39;evento `cmp:click` .

1. Passa a Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Passa alla sezione **Regole** nell’interfaccia utente di Launch, quindi fai clic su **Aggiungi regola**.
1. Denomina la regola **CTA Clic**.
1. Fai clic su **Eventi** > **Aggiungi** per aprire la procedura guidata **Configurazione evento**.
1. In **Tipo evento** selezionare **Codice personalizzato**.

   ![Denomina la regola CTA Selezionato e aggiungi l&#39;evento codice personalizzato](assets/track-clicked-component/custom-code-event.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente frammento di codice:

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

   Lo snippet di codice riportato sopra aggiunge un listener di eventi premendo [una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando l&#39;evento `cmp:click` viene attivato, viene chiamata la funzione `componentClickedHandler` . In questa funzione vengono aggiunti alcuni controlli di integrità e viene creato un nuovo oggetto `event` con lo stato più recente [del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l&#39;evento.

   Dopo la chiamata di `trigger(event)` . `trigger()` è un nome riservato in Launch e &quot;attiverà&quot; la regola Launch. Trasmettiamo l’oggetto `event` come parametro che a sua volta sarà esposto da un altro nome riservato in Launch denominato `event`. Gli elementi dati in Launch possono ora fare riferimento a varie proprietà come questa: `event.component['someKey']`.

1. Salva le modifiche.
1. Successivamente, in **Azioni** fare clic su **Aggiungi** per aprire la procedura guidata **Configurazione azione**.
1. In **Tipo azione** scegli **Codice personalizzato**.

   ![Tipo di azione Codice personalizzato](assets/track-clicked-component/action-custom-code.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente frammento di codice:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   L&#39;oggetto `event` viene passato dal metodo `trigger()` chiamato nell&#39;evento personalizzato. `component` è lo stato corrente del componente derivato dal livello dati  `getState` che ha attivato il clic.

1. Salva le modifiche ed esegui una [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch per promuovere il codice nell’ [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments.html) utilizzato sul sito AEM.

   >[!NOTE]
   >
   > Può essere molto utile utilizzare il [debugger Adobe Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) per passare il codice da incorporare a un ambiente **Sviluppo**.

1. Passa al [sito WKND](https://wknd.site/us/en.html) e apri gli strumenti per sviluppatori per visualizzare la console. Selezionare **Conserva registro**.

1. Fai clic su uno dei pulsanti **Teaser** o **Button** CTA per passare a un’altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Osserva nella console per sviluppatori che la regola **CTA Click** è stata attivata:

   ![Pulsante CTA selezionato](assets/track-clicked-component/cta-button-clicked-log.png)

## Creare elementi dati

Crea quindi un elemento dati per acquisire l’ID componente e il titolo su cui hai fatto clic. Ricorda nell&#39;esercizio precedente l&#39;output di `event.path` era simile a `component.button-b6562c963d` e il valore di `event.component['dc:title']` era simile a &quot;Visualizza viaggi&quot;.

### ID componente

1. Passa a Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Passa alla sezione **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** immetti **ID componente**.
1. Per **Tipo di elemento dati** seleziona **Codice personalizzato**.

   ![Modulo Elemento dati ID componente](assets/track-clicked-component/component-id-data-element.png)

1. Fai clic su **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Salva le modifiche.

   >[!NOTE]
   >
   > Ricorda che l&#39;oggetto `event` è reso disponibile e rientrato in base all&#39;evento che ha attivato la **Regola** in Launch. Il valore di un elemento dati non viene impostato finché l&#39;elemento dati non viene *referenziato* all&#39;interno di una regola. Pertanto è sicuro utilizzare questo elemento dati all’interno di una regola come la regola **CTA Clic** creata nell’esercizio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.

### Titolo componente

1. Passa alla sezione **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** immetti **Titolo componente**.
1. Per **Tipo di elemento dati** seleziona **Codice personalizzato**.
1. Fai clic su **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salva le modifiche.

## Aggiungi una condizione alla regola CTA Clic

Quindi, aggiorna la regola **CTA Clic** per garantire che la regola si attivi solo quando l&#39;evento `cmp:click` viene attivato per un **Teaser** o un **Button**. Poiché il CTA del teaser è considerato un oggetto separato nel livello dati, è importante controllare il padre per verificarlo che provenga da un teaser.

1. Nell’interfaccia utente di Launch, passa alla regola **CTA Clic** creata in precedenza.
1. In **Condizioni** fai clic su **Aggiungi** per aprire la procedura guidata **Configurazione condizione**.
1. Per **Tipo condizione** selezionare **Codice personalizzato**.

   ![Codice personalizzato condizione clic su CTA](assets/track-clicked-component/custom-code-condition.png)

1. Fai clic su **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

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

   Il codice di cui sopra controlla prima se il tipo di risorsa proviene da un **Pulsante**, quindi controlla se il tipo di risorsa proviene da un CTA all&#39;interno di un **Teaser**.

1. Salva le modifiche.

## Impostare le variabili di Analytics e attivare il beacon Track Link

Attualmente la regola **CTA Clic** restituisce semplicemente un&#39;istruzione console. Quindi, utilizza gli elementi dati e l&#39;estensione Analytics per impostare le variabili Analytics come **azione**. Imposta anche un’azione aggiuntiva per attivare il **Track Link** e inviare i dati raccolti ad Adobe Analytics.

1. Nella regola **CTA Clic** **rimuovi** l&#39;azione **Core - Custom Code** (le istruzioni della console):

   ![Rimuovi azione codice personalizzato](assets/track-clicked-component/remove-console-statements.png)

1. In Azioni, fai clic su **Aggiungi** per aggiungere una nuova azione.
1. Imposta il tipo **Estensione** su **Adobe Analytics** e imposta **Tipo azione** su **Imposta variabili**.

1. Imposta i seguenti valori per **eVars**, **Props** e **Events**:

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![Impostare eventi e prop eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Viene utilizzato qui `%Component ID%` in quanto garantisce un identificatore univoco per l’Invito all’azione selezionato. Un potenziale lato negativo dell’utilizzo di `%Component ID%` è che il rapporto di Analytics conterrà valori come `button-2e6d32893a`. L’utilizzo di `%Component Title%` fornisce un nome più descrittivo, ma il valore potrebbe non essere univoco.

1. Quindi, aggiungi un&#39;azione aggiuntiva a destra di **Adobe Analytics - Imposta variabili** toccando l&#39;icona **più** :

   ![Aggiungi un’azione Launch aggiuntiva](assets/track-clicked-component/add-additional-launch-action.png)

1. Imposta il tipo **Estensione** su **Adobe Analytics** e imposta **Tipo azione** su **Invia beacon**.
1. In **Tracking** impostare il pulsante di scelta su **`s.tl()`**.
1. Per **Tipo di collegamento** scegli **Collegamento personalizzato** e per **Nome collegamento** imposta il valore su: **`%Component Title%: CTA Clicked`**:

   ![Configurazione per il beacon Invia collegamento](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   In questo modo si combinano la variabile dinamica dell&#39;elemento dati **Titolo componente** e la stringa statica **CTA selezionata**.

1. Salva le modifiche. La regola **CTA Clic** deve ora avere la seguente configurazione:

   ![Configurazione di Final Launch](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Ascolta l&#39; `cmp:click` evento.
   * **2.** Controlla che l’evento sia stato attivato da un  **** Teaser o  **Teaser**.
   * **3.** Imposta le variabili di Analytics per per tenere traccia dell’ **ID** componente come  **eVar**,  **prop** e un  **evento**.
   * **4.** Invia il beacon Track Link di Analytics (e  **** non crearlo come visualizzazione di pagina).

1. Salva tutte le modifiche e crea la libreria Launch, promuovendo l’ambiente appropriato.

## Convalida il beacon Track Link e la chiamata di Analytics

Ora che la regola **CTA Click** invia il beacon di Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando il debugger di Experience Platform.

1. Apri il [sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fai clic sull’icona Debugger ![icona Debugger di Experience Platform](assets/track-clicked-component/experience-cloud-debugger.png) per aprire Experience Platform Debugger.
1. Accertati che Debugger mappi la proprietà Launch nel *tuo ambiente di sviluppo*, come descritto in precedenza e che sia selezionato **Logging console** .
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata su *la suite di rapporti*.

   ![Debugger scheda Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. Nel browser, fai clic su uno dei pulsanti **Teaser** o **Button** CTA per passare a un’altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Torna a Experience Platform Debugger, scorri verso il basso ed espandi **Richieste di rete** > *Suite di rapporti*. È necessario essere in grado di trovare il set **eVar**, **prop** e **event** .

   ![Eventi di Analytics, evar e prop tracciati al momento del clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Torna al browser e apri la console per sviluppatori. Passa al piè di pagina del sito e fai clic su uno dei collegamenti di navigazione:

   ![Fare clic sul collegamento Navigazione nel piè di pagina](assets/track-clicked-component/click-navigation-link-footer.png)

1. Osserva nella console del browser il messaggio *&quot;Custom Code&quot; per la regola &quot;CTA Clic&quot; non è stato soddisfatto*.

   Questo perché il componente Navigazione attiva un evento `cmp:click` *ma* a causa del controllo di rispetto al tipo di risorsa, non viene eseguita alcuna azione.

   >[!NOTE]
   >
   > Se non trovi registri della console, assicurati che **Registrazione console** sia selezionato in **Launch** in Experience Platform Debugger.

## Congratulazioni!

Hai appena utilizzato Adobe Client Data Layer e il Experience Platform Launch basati sugli eventi per tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager.