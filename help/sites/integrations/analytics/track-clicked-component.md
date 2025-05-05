---
title: Tracciare il componente su cui è stato fatto clic con Adobe Analytics
description: Utilizza Adobe Client Data Layer basato sugli eventi per tenere traccia dei clic su componenti specifici in un sito Adobe Experience Manager. Scopri come utilizzare le regole di tag per ascoltare questi eventi e inviare dati a una suite di rapporti di Adobe Analytics utilizzando un beacon di tracciamento dei collegamenti.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-6296
thumbnail: KT-6296.jpg
badgeIntegration: label="Integrazione" type="positive"
doc-type: Tutorial
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
duration: 394
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1750'
ht-degree: 1%

---

# Tracciare il componente su cui è stato fatto clic con Adobe Analytics

Utilizza [Adobe Client Data Layer basato sugli eventi con i componenti core di AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it) per tenere traccia dei clic su componenti specifici in un sito Adobe Experience Manager. Scopri come utilizzare le regole nella proprietà tag per rilevare gli eventi di clic, filtrare per componente e inviare i dati a un Adobe Analytics con un beacon di tracciamento dei collegamenti.

## Cosa intendi creare {#what-build}

Il team di marketing WKND è interessato a sapere quali pulsanti `Call to Action (CTA)` offrono le migliori prestazioni nella home page. In questa esercitazione, aggiungiamo una regola alla proprietà tag che ascolta gli eventi `cmp:click` dai componenti **Teaser** e **Button**. Quindi invia l’ID del componente e un nuovo evento ad Adobe Analytics insieme al beacon track link.

![Cosa verrà creato per tenere traccia dei clic](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Obiettivi {#objective}

1. Creare una regola basata su eventi nella proprietà tag che acquisisce l&#39;evento `cmp:click`.
1. Filtra i diversi eventi in base al tipo di risorsa del componente.
1. Imposta l’ID del componente e invia un evento ad Adobe Analytics con il beacon track link.

## Prerequisiti

Questo tutorial è una continuazione di [Raccogli dati di pagina con Adobe Analytics](./collect-data-analytics.md) e presuppone che tu abbia:

* Una proprietà **Tag** con l&#39;estensione [Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) abilitata
* **Adobe Analytics** ID suite di rapporti test/dev e server di tracciamento. Consulta la seguente documentazione per [creare una suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* Estensione del browser [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) configurata con la proprietà tag caricata nel [sito WKND](https://wknd.site/us/en.html) o in un sito AEM con Adobe Data Layer abilitato.

## Controllare lo schema Button e Teaser

Prima di creare regole nella proprietà tag, è utile rivedere lo schema [per Button e Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) e analizzarli nell&#39;implementazione del livello dati.

1. Passa a [Home page WKND](https://wknd.site/us/en.html)
1. Apri gli strumenti per sviluppatori del browser e passa alla **console**. Esegui il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Il codice riportato sopra restituisce lo stato corrente di Adobe Client Data Layer.

   ![Stato del livello dati tramite la console del browser](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Espandere la risposta e trovare le voci con il prefisso `button-` e `teaser-xyz-cta`. Dovresti visualizzare uno schema di dati come il seguente:

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

   I dettagli dei dati di cui sopra si basano sullo schema [Componente/Contenitore](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). La nuova regola di tag utilizza questo schema.

## Creare una regola su cui è stato fatto clic in CTA

Adobe Client Data Layer è un livello dati basato su **evento**. Ogni volta che si fa clic su un Componente core, viene inviato un evento `cmp:click` tramite Data Layer. Per ascoltare l&#39;evento `cmp:click`, creiamo una regola.

1. Passa ad Experience Platform e accedi alla proprietà tag integrata con il sito AEM.
1. Passa alla sezione **Regole** nell&#39;interfaccia utente della proprietà Tag, quindi fai clic su **Aggiungi regola**.
1. Denomina la regola **CTA selezionata**.
1. Fai clic su **Eventi** > **Aggiungi** per aprire la procedura guidata **Configurazione evento**.
1. Per il campo **Tipo evento**, selezionare **Codice personalizzato**.

   ![Denomina la regola su cui CTA ha fatto clic e aggiungi l&#39;evento del codice personalizzato](assets/track-clicked-component/custom-code-event.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente snippet di codice:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
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

   Il frammento di codice sopra riportato aggiunge un listener di eventi [inviando una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Ogni volta che viene attivato l&#39;evento `cmp:click`, viene chiamata la funzione `componentClickedHandler`. In questa funzione vengono aggiunti alcuni controlli di integrità e viene costruito un nuovo oggetto `event` con il [stato più recente del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l&#39;evento.

   Infine, viene chiamata la funzione `trigger(event)`. La funzione `trigger()` è un nome riservato nella proprietà tag e **attiva** la regola. L&#39;oggetto `event` viene passato come parametro che a sua volta è esposto da un altro nome riservato nella proprietà tag. Gli elementi dati nella proprietà tag ora possono fare riferimento a varie proprietà utilizzando lo snippet di codice come `event.component['someKey']`.

1. Salva le modifiche.
1. Avanti in **Azioni** fare clic su **Aggiungi** per aprire la **Configurazione azione** guidata.
1. Per il campo **Tipo azione**, scegli **Codice personalizzato**.

   ![Tipo azione Codice Personalizzato](assets/track-clicked-component/action-custom-code.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente snippet di codice:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   L&#39;oggetto `event` è passato dal metodo `trigger()` chiamato nell&#39;evento personalizzato. L&#39;oggetto `component` è lo stato corrente del componente derivato dal metodo `getState()` del livello dati ed è l&#39;elemento che ha attivato il clic.

1. Salva le modifiche ed esegui una [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) nella proprietà tag per promuovere il codice nell&#39;[ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=it) utilizzato nel tuo sito AEM.

   >[!NOTE]
   >
   > Può essere utile utilizzare [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) per passare il codice da incorporare a un ambiente **Development**.

1. Passa al [sito WKND](https://wknd.site/us/en.html) e apri gli strumenti per sviluppatori per visualizzare la console. Selezionare inoltre la casella di controllo **Mantieni registro**.

1. Fare clic su uno dei pulsanti di CTA **Teaser** o **Pulsante** per passare a un&#39;altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Osserva nella console per sviluppatori che la regola **CTA Clicked** è stata attivata:

   ![Pulsante CTA Selezionato](assets/track-clicked-component/cta-button-clicked-log.png)

## Creare elementi dati

Quindi crea un elemento dati per acquisire l’ID componente e il titolo su cui hai fatto clic. Ricorda nell&#39;esercizio precedente che l&#39;output di `event.path` era simile a `component.button-b6562c963d` e il valore di `event.component['dc:title']` era simile a &quot;Viaggi di visualizzazione&quot;.

### ID componente

1. Passa ad Experience Platform e accedi alla proprietà tag integrata con il sito AEM.
1. Passa alla sezione **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per il campo **Nome**, immettere **ID componente**.
1. Per il campo **Tipo elemento dati**, selezionare **Codice personalizzato**.

   ![Modulo elemento dati ID componente](assets/track-clicked-component/component-id-data-element.png)

1. Fai clic sul pulsante **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. Salva le modifiche.

   >[!NOTE]
   >
   > Ricorda che l&#39;oggetto `event` è reso disponibile e con ambito in base all&#39;evento che ha attivato la **regola** nella proprietà tag. Il valore di un elemento dati non viene impostato finché all&#39;elemento dati non viene fatto riferimento **&#x200B; in una regola. Pertanto, è sicuro utilizzare questo elemento dati all&#39;interno di una regola come la regola &#x200B;** Pagina caricata** creata nel passaggio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.


### Titolo componente

1. Passa alla sezione **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per il campo **Nome**, immettere **Titolo componente**.
1. Per il campo **Tipo elemento dati**, selezionare **Codice personalizzato**.
1. Fai clic sul pulsante **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Salva le modifiche.

## Aggiungi una condizione alla regola CTA Clicked

Quindi, aggiorna la regola **CTA Clicked** per assicurarti che la regola venga attivata solo quando l&#39;evento `cmp:click` viene attivato per un **Teaser** o un **Button**. Poiché il CTA del teaser è considerato un oggetto separato nel livello dati, è importante controllare l’elemento principale per verificare che provenga da un teaser.

1. Nell&#39;interfaccia utente della proprietà Tag, passa alla regola **CTA Clicked** creata in precedenza.
1. In **Condizioni** fare clic su **Aggiungi** per aprire la procedura guidata **Configurazione condizione**.
1. Per il campo **Tipo condizione**, selezionare **Codice personalizzato**.

   ![Codice personalizzato condizione per clic su CTA](assets/track-clicked-component/custom-code-condition.png)

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

   Il codice sopra riportato verifica innanzitutto se il tipo di risorsa proviene da un **Button** o se il tipo di risorsa proviene da un CTA all&#39;interno di un **Teaser**.

1. Salva le modifiche.

## Imposta variabili di Analytics e attiva il beacon Track Link

Attualmente la regola **CTA Clicked** restituisce semplicemente un&#39;istruzione della console. Quindi, utilizza gli elementi dati e l&#39;estensione Analytics per impostare le variabili Analytics come **azione**. Impostiamo inoltre un&#39;azione aggiuntiva per attivare il **collegamento di tracciamento** e inviare i dati raccolti ad Adobe Analytics.

1. Nella regola **CTA Clicked**, **rimuovi** l&#39;azione **Core - Custom Code** (istruzioni della console):

   ![Rimuovi azione codice personalizzato](assets/track-clicked-component/remove-console-statements.png)

1. In Azioni, fai clic su **Aggiungi** per creare un&#39;azione.
1. Imposta il tipo **Extension** su **Adobe Analytics** e imposta il tipo **Action** su **Set Variables**.

1. Imposta i seguenti valori per **eVar**, **Prop** e **Eventi**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Imposta proprietà ed eventi di eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > `%Component ID%` viene utilizzato in quanto garantisce un identificatore univoco per il CTA su cui è stato fatto clic. Un potenziale svantaggio dell&#39;utilizzo di `%Component ID%` è che il report di Analytics contiene valori come `button-2e6d32893a`. Se si utilizza `%Component Title%`, verrà fornito un nome più descrittivo, ma il valore potrebbe non essere univoco.

1. Quindi, aggiungi un&#39;azione aggiuntiva a destra di **Adobe Analytics - Imposta variabili** toccando l&#39;icona **più**:

   ![Aggiungi un&#39;azione aggiuntiva alla regola di tag](assets/track-clicked-component/add-additional-launch-action.png)

1. Imposta il tipo **Extension** su **Adobe Analytics** e imposta il tipo **Action** su **Send Beacon**.
1. In **Tracciamento** impostare il pulsante di scelta su **`s.tl()`**.
1. Per il campo **Tipo di collegamento**, scegli **Collegamento personalizzato** e per **Nome collegamento** imposta il valore su: **`%Component Title%: CTA Clicked`**:

   ![Configurazione per il beacon Invia collegamento](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   La configurazione precedente combina la variabile dinamica dell&#39;elemento dati **Component Title** e la stringa statica **CTA Clicked**.

1. Salva le modifiche. La regola **CTA Clicked** deve ora avere la seguente configurazione:

   ![Configurazione regola tag finale](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Ascolta l&#39;evento `cmp:click`.
   * **2.** Verificare che l&#39;evento sia stato attivato da un **pulsante** o **teaser**.
   * **3.** Imposta le variabili di Analytics per tenere traccia dell&#39;**ID componente** come **eVar**, **prop** e un **evento**.
   * **4.** Invia il beacon Track Link di Analytics (e **non** lo considera come una visualizzazione di pagina).

1. Salva tutte le modifiche e crea la libreria tag, passando all’ambiente appropriato.

## Convalidare la chiamata Track Link Beacon and Analytics

Ora che la regola **CTA Clicked** invia il beacon Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando Experience Platform Debugger.

1. Apri il [sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fai clic sull&#39;icona Debugger ![icona Experience Platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) per aprire Experience Platform Debugger.
1. Accertati che Debugger mappi la proprietà tag nell&#39;ambiente di sviluppo *your*, come descritto in precedenza, e che sia selezionato **Registrazione console**.
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata su *la tua* suite di rapporti.

   ![Debugger scheda Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. Nel browser, fare clic su uno dei pulsanti di CTA **Teaser** o **Button** per passare a un&#39;altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Torna a Experience Platform Debugger, scorri verso il basso ed espandi **Richieste di rete** > *La tua suite di rapporti*. Dovresti trovare il set **eVar**, **prop** e **event**.

   ![Eventi, evar e prop di Analytics tracciati al clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Torna al browser e apri la console per sviluppatori. Passare al piè di pagina del sito e fare clic su uno dei collegamenti di spostamento:

   ![Fare clic sul collegamento di spostamento nel piè di pagina](assets/track-clicked-component/click-navigation-link-footer.png)

1. Osserva che nella console del browser il messaggio *&quot;Custom Code&quot; per la regola &quot;CTA Clicked&quot; non è stato soddisfatto*.

   Il messaggio sopra riportato è dovuto al fatto che il componente Navigazione attiva un evento `cmp:click` *ma* a causa della [condizione alla regola](#add-a-condition-to-the-cta-clicked-rule) che controlla il tipo di risorsa senza intraprendere alcuna azione.

   >[!NOTE]
   >
   > Se non trovi alcun registro della console, assicurati che **Registrazione console** sia selezionato in **Tag Experience Platform** nel debugger di Experience Platform.

## Congratulazioni.

Hai appena utilizzato Adobe Client Data Layer e Tag basati sugli eventi in Experience Platform per monitorare i clic di componenti specifici su un sito AEM.
