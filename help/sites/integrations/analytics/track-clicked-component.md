---
title: Tenere traccia del componente su cui è stato fatto clic con Adobe Analytics
description: Utilizza Adobe Client Data Layer basato su eventi per tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole nel Experience Platform Launch per ascoltare questi eventi e inviare dati a un Adobe Analytics con un beacon di collegamento di tracciamento.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1806'
ht-degree: 3%

---

# Tenere traccia del componente su cui è stato fatto clic con Adobe Analytics

Utilizzare il motore basato su eventi [Adobe Client Data Layer con AEM componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it) per tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager. Scopri come utilizzare le regole in Experience Platform Launch per rilevare gli eventi clic, filtrarli per componente e inviare i dati ad Adobe Analytics con un beacon di tracciamento dei collegamenti.

## Cosa verrà creato

Il team di marketing WKND desidera capire quali pulsanti Invito all’azione (CTA) hanno prestazioni migliori nella home page. In questa esercitazione verrà aggiunta una nuova regola in Experience Platform Launch che ascolta `cmp:click` eventi da **Teaser** e **Pulsante** e invia l’ID del componente e un nuovo evento ad Adobe Analytics insieme al beacon di tracciamento del collegamento.

![Cosa verrà creato per tracciare i clic](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Obiettivi {#objective}

1. Crea una regola basata su eventi in Launch basata su `cmp:click` evento.
1. Filtrare i diversi eventi per tipo di risorsa componente.
1. Imposta l’id del componente su cui hai fatto clic e invia l’evento Adobe Analytics con il beacon di tracciamento del collegamento.

## Prerequisiti

Questa esercitazione è una continuazione di [Raccogliere dati di pagina con Adobe Analytics](./collect-data-analytics.md) e presuppone che:

* A **Launch, proprietà** con [Estensione Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) abilitato
* **Adobe Analytics** ID suite di rapporti test/dev e server di tracciamento. Consulta la seguente documentazione per [creazione di una nuova suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Debugger Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) estensione del browser configurata con la proprietà Launch caricata in [https://wknd.site/us/en.html](https://wknd.site/us/en.html) o un sito AEM con Adobe Data Layer abilitato.

## Inspect Pulsante e schema teaser

Prima di creare regole in Launch, è utile rivedere le [schema per il pulsante e il teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) e le ispeziona nell’implementazione del livello dati.

1. Passa a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Apri gli strumenti di sviluppo del browser e passa alla **Console**. Esegui il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Restituisce lo stato corrente del livello dati client di Adobe.

   ![Stato del livello dati tramite la console del browser](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Espandi la risposta e trova le voci con il prefisso `button-` e  `teaser-xyz-cta` voce. Dovresti visualizzare uno schema di dati simile al seguente:

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

   Si basano sui [Schema componente/elemento contenitore](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). La regola che creeremo in Launch utilizzerà questo schema.

## Creare una regola Clic su CTA

Il livello dati client di Adobe è un **event** livello dati guidato. Quando si fa clic su un componente core qualsiasi `cmp:click` viene inviato tramite il livello dati. Crea quindi una regola per ascoltare `cmp:click` evento.

1. Passa a Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Passa a **Regole** nell’interfaccia utente di Launch, quindi fai clic su **Aggiungi regola**.
1. Denomina la regola **CTA selezionato**.
1. Fai clic su **Eventi** > **Aggiungi** per aprire **Configurazione evento** procedura guidata.
1. Sotto **Tipo evento** select **Codice personalizzato**.

   ![Denomina la regola CTA Selezionato e aggiungi l&#39;evento codice personalizzato](assets/track-clicked-component/custom-code-event.png)

1. Fai clic su **Open Editor** nel pannello principale e immetti il seguente frammento di codice:

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

   Lo snippet di codice di cui sopra aggiunge un listener di eventi da [spingere una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando il `cmp:click` viene attivato l&#39;evento `componentClickedHandler` viene chiamata la funzione . In questa funzione vengono aggiunti alcuni controlli di integrità e un nuovo `event` viene creato con l&#39;ultima [stato del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l’evento.

   Dopo `trigger(event)` viene chiamato. `trigger()` è un nome riservato in Launch e &quot;attiva&quot; la regola Launch. Passiamo il `event` oggetto come parametro che a sua volta è esposto da un altro nome riservato in Launch denominato `event`. Gli elementi dati in Launch possono ora fare riferimento a varie proprietà come questa: `event.component['someKey']`.

1. Salva le modifiche.
1. Successivo sotto **Azioni** click **Aggiungi** per aprire **Configurazione azione** procedura guidata.
1. Sotto **Tipo di azione** scegli **Codice personalizzato**.

   ![Tipo di azione Codice personalizzato](assets/track-clicked-component/action-custom-code.png)

1. Fai clic su **Open Editor** nel pannello principale e immetti il seguente frammento di codice:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   La `event` viene passato dall&#39;oggetto `trigger()` chiamato nell&#39;evento personalizzato. `component` è lo stato corrente del componente derivato dal livello dati `getState` che ha attivato il clic.

1. Salvare le modifiche ed eseguire un [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch per promuovere il codice nel [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) utilizzato sul sito AEM.

   >[!NOTE]
   >
   > Può essere molto utile utilizzare il [Debugger Adobe Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) per convertire il codice di incorporamento in un **Sviluppo** ambiente.

1. Passa a [Sito WKND](https://wknd.site/us/en.html) e apri gli strumenti per sviluppatori per visualizzare la console. Seleziona **Conserva registro**.

1. Fai clic su uno dei **Teaser** o **Pulsante** Pulsanti CTA per passare a un’altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Osserva nella console per sviluppatori che la **CTA selezionato** La regola è stata attivata:

   ![Pulsante CTA selezionato](assets/track-clicked-component/cta-button-clicked-log.png)

## Creare elementi dati

Crea quindi un elemento dati per acquisire l’ID componente e il titolo su cui hai fatto clic. Ricorda nell&#39;esercizio precedente l&#39;output di `event.path` era qualcosa di simile a `component.button-b6562c963d` e il valore `event.component['dc:title']` Era qualcosa tipo &quot;Viaggi in vista&quot;.

### ID componente

1. Passa a Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Passa a **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** enter **ID componente**.
1. Per **Tipo di elemento dati** select **Codice personalizzato**.

   ![Modulo Elemento dati ID componente](assets/track-clicked-component/component-id-data-element.png)

1. Fai clic su **Open Editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Salva le modifiche.

   >[!NOTE]
   >
   > Ricorda che la `event` l&#39;oggetto è reso disponibile e delimitato in base all&#39;evento che ha attivato il **Regola** in Launch. Il valore di un elemento dati non viene impostato finché l’elemento dati non è *referenza* all&#39;interno di una regola. Pertanto è sicuro utilizzare questo elemento dati all’interno di una regola come la **CTA selezionato** regola creata nell&#39;esercizio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.

### Titolo componente

1. Passa a **Elementi dati** e fai clic su **Aggiungi nuovo elemento dati**.
1. Per **Nome** enter **Titolo componente**.
1. Per **Tipo di elemento dati** select **Codice personalizzato**.
1. Fai clic su **Open Editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salva le modifiche.

## Aggiungi una condizione alla regola CTA Clic

Quindi, aggiorna il **CTA selezionato** per garantire che la regola si attivi solo quando `cmp:click` viene attivato per un **Teaser** o **Pulsante**. Poiché il CTA del teaser è considerato un oggetto separato nel livello dati, è importante controllare il padre per verificarlo che provenga da un teaser.

1. Nell’interfaccia utente di Launch, passa a **CTA selezionato** creato in precedenza.
1. Sotto **Condizioni** click **Aggiungi** per aprire **Configurazione condizione** procedura guidata.
1. Per **Tipo di condizione** select **Codice personalizzato**.

   ![Codice personalizzato condizione clic su CTA](assets/track-clicked-component/custom-code-condition.png)

1. Fai clic su **Open Editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

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

   Il codice di cui sopra controlla prima se il tipo di risorsa proviene da un **Pulsante** quindi controlla se il tipo di risorsa proviene da un CTA all’interno di un **Teaser**.

1. Salva le modifiche.

## Impostare le variabili di Analytics e attivare il beacon Track Link

Attualmente **CTA selezionato** restituisce semplicemente un&#39;istruzione console. Quindi, utilizza gli elementi dati e l’estensione Analytics per impostare le variabili Analytics come **action**. Inoltre, verrà impostata un’azione aggiuntiva per attivare la **Traccia collegamento** e inviare i dati raccolti ad Adobe Analytics.

1. In **CTA selezionato** regola **remove** la **Core - Codice personalizzato** azione (le istruzioni della console):

   ![Rimuovi azione codice personalizzato](assets/track-clicked-component/remove-console-statements.png)

1. In Azioni, fai clic su **Aggiungi** per aggiungere una nuova azione.
1. Imposta la **Estensione** digitare **Adobe Analytics** e imposta **Tipo di azione** a  **Imposta variabili**.

1. Imposta i seguenti valori per **eVar**, **Proprietà** e **Eventi**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Impostare eventi e prop eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Qui `%Component ID%` viene utilizzato in quanto garantisce un identificatore univoco per il CTA su cui hai fatto clic. Un potenziale svantaggio nell&#39;utilizzo di `%Component ID%` è che il rapporto di Analytics conterrà valori come `button-2e6d32893a`. Utilizzo `%Component Title%` darebbe un nome più umano amichevole, ma il valore potrebbe non essere unico.

1. Quindi, aggiungi un’azione aggiuntiva a destra di **Adobe Analytics - Impostare le variabili** toccando **plus** icona:

   ![Aggiungi un’azione Launch aggiuntiva](assets/track-clicked-component/add-additional-launch-action.png)

1. Imposta la **Estensione** digitare **Adobe Analytics** e imposta **Tipo di azione** a  **Invia beacon**.
1. Sotto **Tracking** imposta il pulsante di scelta su **`s.tl()`**.
1. Per **Tipo di collegamento** scegli **Collegamento personalizzato** e **Nome collegamento** imposta il valore su: **`%Component Title%: CTA Clicked`**:

   ![Configurazione per il beacon Invia collegamento](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   In questo modo si combina la variabile dinamica dell’elemento dati **Titolo componente** e la stringa statica **CTA selezionato**.

1. Salva le modifiche. La **CTA selezionato** La regola deve ora avere la seguente configurazione:

   ![Configurazione di Final Launch](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Ascolta i `cmp:click` evento.
   * **2.** Controlla che l&#39;evento sia stato attivato da un **Pulsante** o **Teaser**.
   * **3.** Imposta le variabili di Analytics per per tenere traccia del **ID componente** come **eVar**, **prop** e un **event**.
   * **4.** Invia il beacon Track Link di Analytics (e fai **not** trattarla come visualizzazione di una pagina).

1. Salva tutte le modifiche e crea la libreria Launch, promuovendo l’ambiente appropriato.

## Convalida il beacon Track Link e la chiamata di Analytics

Ora che **CTA selezionato** La regola invia il beacon di Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando il debugger di Experience Platform.

1. Apri [Sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fai clic sull’icona Debugger ![Icona di Experience Platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) per aprire Experience Platform Debugger.
1. Accertati che Debugger mappi la proprietà Launch su *le* Ambiente di sviluppo, come descritto in precedenza e **Registrazione console** è controllata.
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata su *le* suite di rapporti.

   ![Debugger scheda Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. Nel browser, fai clic su una delle **Teaser** o **Pulsante** Pulsanti CTA per passare a un’altra pagina.

   ![Pulsante CTA su cui fare clic](assets/track-clicked-component/cta-button-to-click.png)

1. Torna a Experience Platform Debugger, scorri verso il basso ed espandi **Richieste di rete** > *Suite di rapporti*. Dovresti essere in grado di trovare il **eVar**, **prop** e **event** impostato.

   ![Eventi di Analytics, evar e prop tracciati al momento del clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Torna al browser e apri la console per sviluppatori. Passa al piè di pagina del sito e fai clic su uno dei collegamenti di navigazione:

   ![Fare clic sul collegamento Navigazione nel piè di pagina](assets/track-clicked-component/click-navigation-link-footer.png)

1. Osserva il messaggio nella console del browser *&quot;Codice personalizzato&quot; per la regola &quot;CTA cliccato&quot; non è stato soddisfatto*.

   Questo perché il componente Navigazione attiva un `cmp:click` event *ma* a causa del controllo di rispetto al tipo di risorsa non viene eseguita alcuna azione.

   >[!NOTE]
   >
   > Se non trovi registri della console, assicurati che **Registrazione console** è controllato in **Launch** in Experience Platform Debugger.

## Congratulazioni!

Hai appena utilizzato Adobe Client Data Layer e il Experience Platform Launch basati sugli eventi per tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager.
