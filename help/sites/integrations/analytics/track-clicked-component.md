---
title: Tracciare il componente su cui è stato fatto clic con Adobe Analytics
description: Utilizza Adobe Client Data Layer basato su eventi per monitorare i clic di componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole di tag per ascoltare questi eventi e inviare dati a una suite di rapporti di Adobe Analytics utilizzando un beacon di tracciamento dei collegamenti.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1750'
ht-degree: 1%

---

# Tracciare il componente su cui è stato fatto clic con Adobe Analytics

Utilizzare la funzionalità basata su eventi [Adobe Client Data Layer con componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it) per tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole nella proprietà tag per rilevare gli eventi di clic, filtrare per componente e inviare i dati a un Adobe Analytics con un beacon di tracciamento dei collegamenti.

## Cosa intendi creare {#what-build}

Il team di marketing WKND è interessato a sapere quale `Call to Action (CTA)` I pulsanti offrono prestazioni ottimali nella home page. In questa esercitazione, aggiungiamo una regola alla proprietà tag che ascolta `cmp:click` eventi da **Teaser** e **Pulsante** componenti. Quindi invia l’ID del componente e un nuovo evento ad Adobe Analytics insieme al beacon track link.

![Cosa creerai tracciare i clic](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Obiettivi {#objective}

1. Crea una regola basata su eventi nella proprietà tag che acquisisce `cmp:click` evento.
1. Filtra i diversi eventi in base al tipo di risorsa del componente.
1. Imposta l’ID del componente e invia un evento ad Adobe Analytics con il beacon track link.

## Prerequisiti

Questo tutorial è una continuazione di [Raccogliere dati di pagina con Adobe Analytics](./collect-data-analytics.md) e presuppone che tu abbia:

* A **Tag, proprietà** con [Estensione Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) abilitato
* **Adobe Analytics** ID suite di rapporti test/dev e server di tracciamento. Consulta la seguente documentazione per [creazione di una suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Debugger Experienci Platform](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) estensione del browser configurata con la proprietà tag caricata su [Sito WKND](https://wknd.site/us/en.html) o un sito AEM con Adobe Data Layer abilitato.

## Schema Pulsante e teaser di Inspect

Prima di creare le regole nella proprietà tag, è utile rivedere [schema per il pulsante e il teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) ed esaminali nell’implementazione del livello dati.

1. Accedi a [Home page WKND](https://wknd.site/us/en.html)
1. Apri gli strumenti di sviluppo del browser e passa a **Console**. Esegui il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Il codice riportato sopra restituisce lo stato corrente di Adobe Client Data Layer.

   ![Stato del livello dati tramite la console del browser](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Espandi la risposta e trova le voci con il prefisso `button-` e  `teaser-xyz-cta` voce. Dovresti visualizzare uno schema di dati come il seguente:

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

   I dettagli dei dati di cui sopra si basano sulla [Schema Componente/Contenitore](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). La nuova regola di tag utilizza questo schema.

## Creare una regola con clic su CTA

Adobe Client Data Layer è un **evento** livello dati guidato. Ogni volta che si fa clic su uno dei Componenti core, si `cmp:click` viene inviato tramite il livello dati. Per ascoltare `cmp:click` evento, creiamo una regola.

1. Passa a Experienci Platform e accedi alla proprietà tag integrata con il sito AEM.
1. Accedi a **Regole** nell’interfaccia utente della proprietà Tag, quindi fai clic su **Aggiungi regola**.
1. Denomina la regola **CTA selezionato**.
1. Clic **Eventi** > **Aggiungi** per aprire **Configurazione evento** procedura guidata.
1. Per **Tipo di evento** campo, seleziona **Codice personalizzato**.

   ![Denomina la regola CTA su cui hai fatto clic e aggiungi l’evento di codice personalizzato](assets/track-clicked-component/custom-code-event.png)

1. Clic **Apri editor** nel pannello principale e immetti il seguente frammento di codice:

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

   Lo snippet di codice sopra riportato aggiunge un listener di eventi di [push di una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando `cmp:click` viene attivato l&#39;evento `componentClickedHandler` viene chiamata la funzione. In questa funzione, vengono aggiunti alcuni controlli di integrità e una nuova `event` l&#39;oggetto è costruito con il più recente [stato del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l’evento.

   Infine, il `trigger(event)` viene chiamata la funzione. Il `trigger()` è un nome riservato nella proprietà tag e **trigger** la regola. Il `event` L&#39;oggetto viene passato come parametro che a sua volta viene esposto da un altro nome riservato nella proprietà tag. Gli elementi dati nella proprietà tag ora possono fare riferimento a varie proprietà utilizzando uno snippet di codice come `event.component['someKey']`.

1. Salva le modifiche.
1. Successivo sotto **Azioni** click **Aggiungi** per aprire **Configurazione azione** procedura guidata.
1. Per **Tipo di azione** campo, scegli **Codice personalizzato**.

   ![Tipo azione codice personalizzato](assets/track-clicked-component/action-custom-code.png)

1. Clic **Apri editor** nel pannello principale e immetti il seguente frammento di codice:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   Il `event` l&#39;oggetto viene passato dal `trigger()` metodo chiamato nell&#39;evento personalizzato. Il `component` object è lo stato corrente del componente derivato dal livello dati `getState()` ed è l&#39;elemento che ha attivato il clic.

1. Salva le modifiche ed esegui una [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) nella proprietà tag per promuovere il codice in [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=it) utilizzati nel sito AEM.

   >[!NOTE]
   >
   > Può essere utile utilizzare il [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) per convertire il codice di incorporamento in una **Sviluppo** ambiente.

1. Accedi a [Sito WKND](https://wknd.site/us/en.html) e apri gli strumenti per sviluppatori per visualizzare la console. Inoltre, seleziona la **Mantieni registro** casella di controllo.

1. Fai clic su una delle opzioni **Teaser** o **Pulsante** pulsanti CTA per passare a un’altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Osserva nella console per sviluppatori che il **CTA selezionato** la regola è stata attivata:

   ![Pulsante CTA selezionato](assets/track-clicked-component/cta-button-clicked-log.png)

## Creare elementi dati

Quindi crea un elemento dati per acquisire l’ID componente e il titolo su cui hai fatto clic. Ricorda nell’esercizio precedente l’output di `event.path` era simile a `component.button-b6562c963d` e il valore di `event.component['dc:title']` Era qualcosa come &quot;Viaggi di Vista&quot;.

### ID componente

1. Passa a Experienci Platform e accedi alla proprietà tag integrata con il sito AEM.
1. Accedi a **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** campo, immetti **ID componente**.
1. Per **Tipo di elemento dati** campo, seleziona **Codice personalizzato**.

   ![Modulo elemento dati ID componente](assets/track-clicked-component/component-id-data-element.png)

1. Clic **Apri editor** e immetti quanto segue nell’editor di codice personalizzato:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. Salva le modifiche.

   >[!NOTE]
   >
   > Ricorda che `event` l&#39;oggetto viene reso disponibile e con ambito in base all&#39;evento che ha attivato **Regola** nella proprietà tag. Il valore di un elemento dati non viene impostato finché l’elemento dati non è *con riferimento* all&#39;interno di una regola. Pertanto, è sicuro utilizzare questo elemento dati all’interno di una regola come **Pagina caricata** regola creata nel passaggio precedente *ma* non è sicuro utilizzarlo in altri contesti.


### Titolo componente

1. Accedi a **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** campo, immetti **Titolo componente**.
1. Per **Tipo di elemento dati** campo, seleziona **Codice personalizzato**.
1. Clic **Apri editor** e immetti quanto segue nell’editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Salva le modifiche.

## Aggiungi una condizione alla regola CTA Clicked

Quindi, aggiorna **CTA selezionato** per garantire che la regola venga attivata solo quando `cmp:click` evento viene attivato per un **Teaser** o un **Pulsante**. Poiché il CTA del teaser è considerato un oggetto separato nel livello dati, è importante controllare l’elemento principale per verificare che provenga da un teaser.

1. Nell’interfaccia utente della proprietà Tag, passa a **CTA selezionato** regola creata in precedenza.
1. Sotto **Condizioni** click **Aggiungi** per aprire **Configurazione condizione** procedura guidata.
1. Per **Tipo di condizione** campo, seleziona **Codice personalizzato**.

   ![Codice personalizzato condizione clic CTA](assets/track-clicked-component/custom-code-condition.png)

1. Clic **Apri editor** e immetti quanto segue nell’editor di codice personalizzato:

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

   Il codice di cui sopra verifica innanzitutto se il tipo di risorsa proviene da un **Pulsante** o se il tipo di risorsa proviene da un CTA all’interno di un **Teaser**.

1. Salva le modifiche.

## Imposta variabili di Analytics e attiva il beacon Track Link

Attualmente il **CTA selezionato** la regola restituisce semplicemente un’istruzione della console. Quindi, utilizza gli elementi dati e l’estensione Analytics per impostare le variabili Analytics come **azione**. Impostiamo anche un’azione aggiuntiva per attivare **Traccia collegamento** e invia i dati raccolti ad Adobe Analytics.

1. In **CTA selezionato** regola, **rimuovere** il **Core - Custom Code** azione (istruzioni della console):

   ![Rimuovi azione codice personalizzato](assets/track-clicked-component/remove-console-statements.png)

1. In Azioni, fai clic su **Aggiungi** per creare un&#39;azione.
1. Imposta il **Estensione** digita in **Adobe Analytics** e imposta **Tipo di azione** a  **Imposta variabili**.

1. Imposta i seguenti valori per **eVar**, **Proprietà**, e **Eventi**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Imposta proprietà ed eventi eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Qui `%Component ID%` viene utilizzato in quanto garantisce un identificatore univoco per il CTA su cui è stato fatto clic. Un potenziale lato negativo dell’utilizzo di `%Component ID%` è che il rapporto di Analytics contiene valori come `button-2e6d32893a`. Utilizzo di `%Component Title%` darebbe un nome più descrittivo, ma il valore potrebbe non essere univoco.

1. Quindi, aggiungi un’azione aggiuntiva a destra del **Adobe Analytics - Imposta variabili** toccando il **più** icona:

   ![Aggiungi un&#39;azione aggiuntiva alla regola di tag](assets/track-clicked-component/add-additional-launch-action.png)

1. Imposta il **Estensione** digita in **Adobe Analytics** e imposta **Tipo di azione** a  **Invia beacon**.
1. Sotto **Tracciamento** impostare il pulsante di opzione su **`s.tl()`**.
1. Per **Tipo di collegamento** campo, scegli **Collegamento personalizzato** e per **Nome collegamento** imposta il valore su: **`%Component Title%: CTA Clicked`**:

   ![Configurazione per il beacon Invia collegamento](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   La configurazione precedente combina la variabile dinamica dell’elemento dati **Titolo componente** e la stringa statica **CTA selezionato**.

1. Salva le modifiche. Il **CTA selezionato** la regola ora deve avere la seguente configurazione:

   ![Configurazione finale regola tag](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Ascolta la `cmp:click` evento.
   * **2.** Verifica che l’evento sia stato attivato da un **Pulsante** o **Teaser**.
   * **3.** Imposta le variabili di Analytics per tenere traccia di **ID componente** come **eVar**, **prop**, e un **evento**.
   * **4.** Inviare il beacon Track Link di Analytics (ed eseguire **non** la considera come una visualizzazione pagina).

1. Salva tutte le modifiche e crea la libreria tag, passando all’ambiente appropriato.

## Convalidare la chiamata Track Link Beacon and Analytics

Ora che il **CTA selezionato** La regola invia il beacon Analytics. Dovresti essere in grado di visualizzare le variabili di tracciamento Analytics utilizzando Experienci Platform Debugger.

1. Apri [Sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fai clic sull’icona Debugger ![Icona di Experience Platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) per aprire Experienci Platform Debugger.
1. Assicurati che Debugger mappi la proprietà tag a *tuo* Ambiente di sviluppo, come descritto in precedenza e **Registrazione console** è selezionato.
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata su *tuo* suite di rapporti.

   ![Debugger scheda Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. Nel browser, fai clic su una delle seguenti opzioni **Teaser** o **Pulsante** pulsanti CTA per passare a un’altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Torna a Debugger Experienci Platform, scorri verso il basso ed espandi **Richieste di rete** > *Suite di rapporti*. Dovresti essere in grado di trovare **eVar**, **prop**, e **evento** impostata.

   ![Eventi, evar e prop di Analytics tracciati al clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Torna al browser e apri la console per sviluppatori. Passare al piè di pagina del sito e fare clic su uno dei collegamenti di spostamento:

   ![Fai clic sul collegamento Navigazione nel piè di pagina](assets/track-clicked-component/click-navigation-link-footer.png)

1. Osserva il messaggio nella console del browser *&quot;Codice personalizzato&quot; per la regola &quot;CTA selezionato&quot; non soddisfatto*.

   Il messaggio di cui sopra è perché il componente Navigazione non attiva una `cmp:click` evento *ma* a causa di [Condizione della regola](#add-a-condition-to-the-cta-clicked-rule) che controlla il tipo di risorsa non viene eseguita alcuna azione.

   >[!NOTE]
   >
   > Se non trovi alcun registro della console, assicurati che **Registrazione console** è controllato in **Tag Experience Platform** in Experienci Platform Debugger.

## Congratulazioni.

Hai appena utilizzato Adobe Client Data Layer e Tag basati sugli eventi in Experienci Platform per monitorare i clic di componenti specifici su un sito AEM.
