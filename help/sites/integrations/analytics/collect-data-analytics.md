---
title: Raccogliere dati di pagina con Adobe Analytics
description: Utilizza Adobe Client Data Layer basato sugli eventi per raccogliere i dati sull’attività dell’utente su un sito web creato con Adobe Experience Manager. Scopri come utilizzare le regole nel Experience Platform Launch per ascoltare questi eventi e inviare dati a una suite di rapporti Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2375'
ht-degree: 1%

---

# Raccogliere dati di pagina con Adobe Analytics

Scopri come utilizzare le funzioni integrate di [Adobe Client Data Layer con AEM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) per raccogliere i dati su una pagina in Adobe Experience Manager Sites. [Ad Experience Platform, ](https://www.adobe.com/experience-platform/launch.html) Launch e l’estensione  [Adobe Analytics ](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) verranno utilizzati per creare regole per inviare dati di pagina ad Adobe Analytics.

## Cosa verrà creato

![Tracciamento dei dati delle pagine](assets/collect-data-analytics/analytics-page-data-tracking.png)

In questa esercitazione verrà attivata una regola Launch basata su un evento dal Livello dati client Adobe, verranno aggiunte le condizioni per l’attivazione della regola e verranno inviati ad Adobe Analytics i valori **Nome pagina** e **Modello pagina** di una pagina AEM.

### Obiettivi {#objective}

1. Creare una regola basata su eventi in Launch in base alle modifiche al livello dati
1. Mappatura delle proprietà del livello dati della pagina su elementi dati in Launch
1. Raccogliere dati di pagina e inviarli ad Adobe Analytics con il beacon di visualizzazione della pagina

## Prerequisiti

Sono necessari i seguenti requisiti:

* **Experience Platform** LaunchProperty
* **Adobe ID suite di rapporti** Analytics/dev e server di tracciamento. Consulta la seguente documentazione per [creare una nuova suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Estensione Experience Platform ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Debuggerbrowser. Schermate in questa esercitazione acquisite dal browser Chrome.
* (Facoltativo) AEM sito con [Livello dati client Adobe abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Questa esercitazione utilizzerà il sito pubblico [https://wknd.site/us/en.html](https://wknd.site/us/en.html) ma puoi utilizzare il tuo sito.

>[!NOTE]
>
> Hai bisogno di aiuto per integrare Launch e il tuo sito AEM? [Guarda questa serie](../experience-platform-launch/overview.md) video.

## Cambiare ambienti Launch per il sito WKND

[https://wknd.](https://wknd.site) siteè un sito rivolto al pubblico basato su  [un progetto open source ](https://github.com/adobe/aem-guides-wknd) progettato come riferimento e  [](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) tutorial per implementazioni AEM.

Invece di configurare un ambiente AEM e installare la base di codice WKND, puoi utilizzare il debugger di Experience Platform per **passare** dalla Live [https://wknd.site/](https://wknd.site/) a *la proprietà* Launch. Naturalmente puoi utilizzare il tuo sito AEM se il [Livello dati client Adobe è già abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Accedi al Experience Platform Launch e [crea una proprietà Launch](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (se non lo hai già fatto).
1. Assicurati che una libreria Launch iniziale [sia stata creata](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) e promossa a un ambiente Launch [](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. Copia il codice di incorporamento Launch dall’ambiente in cui è stata pubblicata la libreria.

   ![Copia codice di incorporamento Launch](assets/collect-data-analytics/launch-environment-copy.png)

1. Nel browser apri una nuova scheda e passa a [https://wknd.site/](https://wknd.site/)
1. Apri l’estensione del browser Experience Platform Debugger.

   ![Debugger Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Passa a **Launch** > **Configurazione** e in **Codici di incorporamento inseriti** sostituisci il codice di incorporamento Launch esistente con *il codice di incorporamento* copiato dal passaggio 3.

   ![Sostituisci codice di incorporamento](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Abilita **Console Logging** e **Blocca** il debugger nella scheda WKND.

   ![Registrazione console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verifica livello dati client di Adobe sul sito WKND

Il [progetto di riferimento WKND](https://github.com/adobe/aem-guides-wknd) è generato con AEM componenti core e per impostazione predefinita è abilitato [Adobe Client Data Layer](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Successivamente, verifica che Adobe Client Data Layer sia abilitato.

1. Passa a [https://wknd.site](https://wknd.site).
1. Apri gli strumenti di sviluppo del browser e passa alla **Console**. Esegui il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Restituisce lo stato corrente del livello dati client di Adobe.

   ![Adobe stato livello dati](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Espandi la risposta ed esamina la voce `page`. Dovresti visualizzare uno schema di dati simile al seguente:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Useremo le proprietà standard derivate dallo [schema pagina](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title`, `xdm:language` e `xdm:template` del livello dati per inviare i dati di pagina ad Adobe Analytics.

   >[!NOTE]
   >
   > L&#39;oggetto javascript `adobeDataLayer` non viene visualizzato? Assicurati che il [Livello dati client di Adobe sia stato abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) sul tuo sito.

## Creare una regola di caricamento pagina

Adobe Client Data Layer è un livello dati guidato da **evento** . Quando viene caricato il livello dati AEM **Pagina**, viene attivato un evento `cmp:show`. Crea una regola che verrà attivata in base all&#39;evento `cmp:show` .

1. Passa a Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Passa alla sezione **Regole** nell’interfaccia utente di Launch, quindi fai clic su **Crea nuova regola**.

   ![Crea regola](assets/collect-data-analytics/analytics-create-rule.png)

1. Denomina la regola **Pagina caricata**.
1. Fai clic su **Eventi** **Aggiungi** per aprire la procedura guidata **Configurazione evento**.
1. In **Tipo evento** selezionare **Codice personalizzato**.

   ![Denomina la regola e aggiungi l&#39;evento codice personalizzato](assets/collect-data-analytics/custom-code-event.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente frammento di codice:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   Lo snippet di codice riportato sopra aggiunge un listener di eventi premendo [una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando l&#39;evento `cmp:show` viene attivato, viene chiamata la funzione `pageShownEventHandler` . In questa funzione vengono aggiunti alcuni controlli di integrità e viene costruito un nuovo `event` con lo stato più recente [del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l&#39;evento.

   Dopo la chiamata di `trigger(event)` . `trigger()` è un nome riservato in Launch e &quot;attiverà&quot; la regola Launch. Trasmettiamo l’oggetto `event` come parametro che a sua volta sarà esposto da un altro nome riservato in Launch denominato `event`. Gli elementi dati in Launch possono ora fare riferimento a varie proprietà come questa: `event.component['someKey']`.

1. Salva le modifiche.
1. Successivamente, in **Azioni** fare clic su **Aggiungi** per aprire la procedura guidata **Configurazione azione**.
1. In **Tipo azione** scegli **Codice personalizzato**.

   ![Tipo di azione Codice personalizzato](assets/collect-data-analytics/action-custom-code.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente frammento di codice:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   L&#39;oggetto `event` viene passato dal metodo `trigger()` chiamato nell&#39;evento personalizzato. `component` è la pagina corrente derivata dal livello dati  `getState` nell’evento personalizzato. Richiama da prima lo [schema di pagina](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) esposto dal livello di dati per vedere le varie chiavi esposte fuori dalla scatola.

1. Salva le modifiche ed esegui una [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch per promuovere il codice nell’ [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) utilizzato sul sito AEM.

   >[!NOTE]
   >
   > Può essere molto utile utilizzare il [debugger Adobe Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) per passare il codice da incorporare a un ambiente **Sviluppo**.

1. Accedi al sito AEM e apri gli strumenti per sviluppatori per visualizzare la console. Aggiorna la pagina per visualizzare che i messaggi della console sono stati registrati:

   ![Messaggi console caricati pagina](assets/collect-data-analytics/page-show-event-console.png)

## Creare elementi dati

Crea quindi diversi elementi dati per acquisire valori diversi da Adobe Client Data Layer. Come visto nell’esercizio precedente, è possibile accedere direttamente alle proprietà del livello dati tramite codice personalizzato. Il vantaggio dell’utilizzo degli elementi dati è che possono essere riutilizzati nelle regole di Launch.

Richiama da prima lo [schema pagina](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) esposto dal livello dati:

Gli elementi dati saranno mappati sulle proprietà `@type`, `dc:title` e `xdm:template` .

### Tipo di risorsa componente

1. Passa a Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Passa alla sezione **Elementi dati** e fai clic su **Crea nuovo elemento dati**.
1. Per **Nome** immetti **Tipo di risorsa componente**.
1. Per **Tipo di elemento dati** seleziona **Codice personalizzato**.

   ![Tipo di risorsa componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Fai clic su **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Salva le modifiche.

   >[!NOTE]
   >
   > Ricorda che l&#39;oggetto `event` è reso disponibile e rientrato in base all&#39;evento che ha attivato la **Regola** in Launch. Il valore di un elemento dati non viene impostato finché l&#39;elemento dati non viene *referenziato* all&#39;interno di una regola. Pertanto è sicuro utilizzare questo elemento dati all&#39;interno di una regola come la regola **Page Loaded** creata nel passaggio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.

### Nome pagina

1. Fai clic su **Aggiungi elemento dati**.
1. Per **Nome** immetti **Nome pagina**.
1. Per **Tipo di elemento dati** seleziona **Codice personalizzato**.
1. Fai clic su **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salva le modifiche.

### Modello pagina

1. Fai clic su **Aggiungi elemento dati**.
1. Per **Nome** immetti **Modello pagina**.
1. Per **Tipo di elemento dati** seleziona **Codice personalizzato**.
1. Fai clic su **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Salva le modifiche.

1. Ora devi disporre di tre elementi dati come parte della regola:

   ![Elementi dati nella regola](assets/collect-data-analytics/data-elements-page-rule.png)

## Aggiungere l’estensione Analytics

Quindi aggiungi l’estensione Analytics alla proprietà Launch. Dobbiamo inviare questi dati da qualche parte!

1. Passa a Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Vai a **Estensioni** > **Catalogo**
1. Individua l&#39;estensione **Adobe Analytics** e fai clic su **Installa**

   ![Estensione Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. In **Gestione libreria** > **Suite di rapporti**, immetti gli ID suite di rapporti che desideri utilizzare con ogni ambiente Launch.

   ![Immetti gli ID suite di rapporti](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In questa esercitazione puoi utilizzare una suite di rapporti per tutti gli ambienti, ma in tempo reale puoi utilizzare suite di rapporti separate, come illustrato nell’immagine seguente

   >[!TIP]
   >
   >È consigliabile utilizzare l&#39;opzione *Gestisci la libreria per me* come impostazione di Gestione libreria in quanto consente di mantenere la libreria `AppMeasurement.js` aggiornata molto più facilmente.

1. Seleziona la casella per abilitare **Usa Activity Map**.

   ![Abilita Usa Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. Alla voce **Generale** > **Server di tracciamento**, immetti il server di tracciamento, ad esempio `tmd.sc.omtrdc.net`. Immetti SSL Tracking Server se il sito supporta `https://`

   ![Immettere i server di tracciamento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Fai clic su **Salva** per salvare le modifiche.

## Aggiungi una condizione alla regola Page Loaded

Quindi, aggiorna la regola **Page Loaded** per utilizzare l&#39;elemento dati **Component Resource Type** per garantire che la regola si attivi solo quando l&#39;evento `cmp:show` è per la pagina **Page**. Altri componenti possono attivare l’evento `cmp:show` , ad esempio il componente Carosello si attiva quando le diapositive cambiano. Pertanto è importante aggiungere una condizione per questa regola.

1. Nell’interfaccia utente di Launch, passa alla regola **Page Loaded** creata in precedenza.
1. In **Condizioni** fai clic su **Aggiungi** per aprire la procedura guidata **Configurazione condizione**.
1. Per **Tipo condizione** selezionare **Confronto valori**.
1. Imposta il primo valore nel campo modulo su `%Component Resource Type%`. Puoi utilizzare l&#39;icona Elemento dati ![icona elemento dati](assets/collect-data-analytics/cylinder-icon.png) per selezionare l&#39;elemento dati **Tipo risorsa componente**. Lasciare il comparatore impostato su `Equals`.
1. Imposta il secondo valore su `wknd/components/page`.

   ![Configurazione delle condizioni per la regola di caricamento pagina](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > È possibile aggiungere questa condizione all&#39;interno della funzione del codice personalizzato che ascolta l&#39;evento `cmp:show` creato in precedenza nell&#39;esercitazione. Tuttavia, l’aggiunta di questa funzionalità all’interno dell’interfaccia utente offre maggiore visibilità agli utenti aggiuntivi che potrebbero dover apportare modifiche alla regola. Inoltre, possiamo utilizzare il nostro elemento dati!

1. Salva le modifiche.

## Impostare le variabili di Analytics e attivare il beacon Vista pagina

Attualmente la regola **Page Loaded** restituisce semplicemente un&#39;istruzione console. Quindi, utilizza gli elementi dati e l’estensione Analytics per impostare le variabili Analytics come **azione** nella regola **Pagina caricata**. Imposta anche un’azione aggiuntiva per attivare il **beacon Vista pagina** e inviare i dati raccolti ad Adobe Analytics.

1. Nella regola **Page Loaded** **remove** l&#39;azione **Core - Custom Code** (le istruzioni della console):

   ![Rimuovi azione codice personalizzato](assets/collect-data-analytics/remove-console-statements.png)

1. In Azioni, fai clic su **Aggiungi** per aggiungere una nuova azione.
1. Imposta il tipo **Estensione** su **Adobe Analytics** e imposta il **Tipo azione** su **Imposta variabili**

   ![Impostare l’estensione dell’azione su Imposta variabili di Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Nel pannello principale seleziona un elemento disponibile **eVar** e imposta come valore dell&#39;elemento dati **Modello pagina**. Utilizza l&#39;icona Elementi dati ![Icona Elementi dati](assets/collect-data-analytics/cylinder-icon.png) per selezionare l&#39;elemento **Modello pagina**.

   ![Imposta come modello di pagina eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Scorri verso il basso, in **Impostazioni aggiuntive** imposta **Nome pagina** sull&#39;elemento dati **Nome pagina**:

   ![Set di variabili per l’ambiente Nome pagina](assets/collect-data-analytics/page-name-env-variable-set.png)

   Salva le modifiche.

1. Quindi, aggiungi un&#39;azione aggiuntiva a destra di **Adobe Analytics - Imposta variabili** toccando l&#39;icona **più** :

   ![Aggiungi un’azione Launch aggiuntiva](assets/collect-data-analytics/add-additional-launch-action.png)

1. Imposta il tipo **Estensione** su **Adobe Analytics** e imposta **Tipo azione** su **Invia beacon**. Poiché questo viene considerato una visualizzazione di pagina, lascia il tracciamento predefinito impostato su **`s.t()`**.

   ![Azione Invia beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salva le modifiche. La regola **Page Loaded** deve ora avere la seguente configurazione:

   ![Configurazione di Final Launch](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Ascolta l&#39; `cmp:show` evento.
   * **2.** Controlla che l&#39;evento sia stato attivato da una pagina.
   * **3.** Imposta le variabili di Analytics per il nome  **pagina** e il modello  **pagina**
   * **4.** Invia il beacon Vista pagina di Analytics
1. Salva tutte le modifiche e crea la libreria Launch, promuovendo l’ambiente appropriato.

## Convalida il beacon Vista pagina e la chiamata Analytics

Ora che la regola **Page Loaded** invia il beacon di Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando il debugger di Experience Platform.

1. Apri il [sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fai clic sull’icona Debugger ![icona Debugger di Experience Platform](assets/collect-data-analytics/experience-cloud-debugger.png) per aprire Experience Platform Debugger.
1. Accertati che Debugger mappi la proprietà Launch nel *tuo ambiente di sviluppo*, come descritto in precedenza e che sia selezionato **Logging console** .
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata su *la suite di rapporti*. È inoltre necessario compilare il campo Nome pagina :

   ![Debugger scheda Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Scorri verso il basso ed espandi **Richieste di rete**. Dovresti essere in grado di trovare il set **evar** per il **Modello di pagina**:

   ![Impostazione Evar e Nome pagina](assets/collect-data-analytics/evar-page-name-set.png)

1. Torna al browser e apri la console per sviluppatori. Fai clic su **Carosello** nella parte superiore della pagina.

   ![Fai clic sulla pagina del carosello](assets/collect-data-analytics/click-carousel-page.png)

1. Osserva l’istruzione console nella console del browser:

   ![Condizione non soddisfatta](assets/collect-data-analytics/condition-not-met.png)

   Questo perché il carosello attiva un evento `cmp:show` *ma* a causa del controllo del **tipo di risorsa componente**, non viene attivato alcun evento.

   >[!NOTE]
   >
   > Se non trovi registri della console, assicurati che **Registrazione console** sia selezionato in **Launch** in Experience Platform Debugger.

1. Passa a una pagina di articolo come [Australia occidentale](https://wknd.site/us/en/magazine/western-australia.html). Osserva che il nome della pagina e il tipo di modello cambiano.

## Congratulazioni!

Hai appena utilizzato Adobe Client Data Layer e il Experience Platform Launch basati sugli eventi per raccogliere i dati della pagina da un sito AEM e inviarli ad Adobe Analytics.

### Passaggi successivi

Per scoprire come utilizzare il livello dati client Adobe basato su eventi per [tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager](track-clicked-component.md), consulta la seguente esercitazione.
