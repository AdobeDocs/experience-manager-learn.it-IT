---
title: Integrare AEM Sites con Adobe Analytics con l’estensione tag Adobe Analytics
description: Integra AEM Sites con Adobe Analytics, utilizzando Adobe Client Data Layer basato sugli eventi per raccogliere i dati sull’attività dell’utente su un sito web creato con Adobe Experience Manager. Scopri come utilizzare le regole di tag per ascoltare questi eventi e inviare dati a una suite di rapporti di Adobe Analytics.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="Integrazione" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
duration: 490
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 1%

---

# Integrare AEM Sites e Adobe Analytics

Scopri come integrare AEM Sites e Adobe Analytics con l’estensione tag Adobe Analytics, utilizzando le funzioni integrate di [Adobe Client Data Layer con i componenti core di AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it) per raccogliere i dati su una pagina in Adobe Experience Manager Sites. [I tag in Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=it) e l&#39;estensione [Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=it) vengono utilizzati per creare regole per inviare dati di pagina ad Adobe Analytics.

## Cosa intendi creare {#what-build}

![Tracciamento dati pagina](assets/collect-data-analytics/analytics-page-data-tracking.png)

In questa esercitazione, stai per attivare una regola di tag basata su un evento da Adobe Client Data Layer. Aggiungi inoltre le condizioni per l&#39;attivazione della regola, quindi invia i valori **Nome pagina** e **Modello pagina** di una pagina AEM ad Adobe Analytics.

### Obiettivi {#objective}

1. Crea una regola basata su eventi nella proprietà tag che acquisisce le modifiche dal livello dati
1. Mappare le proprietà del livello dati della pagina agli elementi dati nella proprietà tag
1. Raccogliere e inviare dati di pagina ad Adobe Analytics utilizzando il beacon di visualizzazione della pagina

## Prerequisiti

Sono necessari i seguenti elementi:

* **Proprietà tag** in Experience Platform
* **Adobe Analytics** ID suite di rapporti test/dev e server di tracciamento. Consulta la seguente documentazione per [creare una suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=it).
* Estensione del browser [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=it). Schermate di questo tutorial acquisite dal browser Chrome.
* (Facoltativo) Sito AEM con [Adobe Client Data Layer abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it#installation-activation). Questa esercitazione utilizza il sito pubblico [WKND](https://wknd.site/us/en.html), ma puoi utilizzare un sito personale.

>[!NOTE]
>
> Hai bisogno di assistenza per integrare la proprietà tag e il sito AEM? [Guarda questa serie di video](../experience-platform/data-collection/tags/overview.md).

## Cambia ambiente tag per sito WKND

[WKND](https://wknd.site/us/en.html) è un sito pubblico creato in base a [un progetto open source](https://github.com/adobe/aem-guides-wknd) progettato come riferimento e [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it) per un&#39;implementazione AEM.

Invece di configurare un ambiente AEM e installare la base di codice WKND, puoi utilizzare il debugger di Experience Platform per **cambiare** il [sito WKND](https://wknd.site/us/en.html) live in *proprietà tag*. Tuttavia, puoi utilizzare il tuo sito AEM se dispone già di [Adobe Client Data Layer abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it#installation-activation).

1. Accedi ad Experience Platform e [crea una proprietà tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=it) (se non lo hai già fatto).
1. Assicurati che sia stata creata una libreria [ di JavaScript con tag iniziale](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html?lang=it#create-a-library) e promossa al tag [environment](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=it).
1. Copia il codice di incorporamento di JavaScript dall’ambiente di tag in cui è stata pubblicata la libreria.

   ![Copia codice di incorporamento proprietà tag](assets/collect-data-analytics/launch-environment-copy.png)

1. Nel browser, apri una nuova scheda e passa a [Sito WKND](https://wknd.site/us/en.html)
1. Apri l’estensione del browser Experience Platform Debugger

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Passa a **Tag di Experience Platform** > **Configurazione** e in **Codici di incorporamento inseriti** sostituisci il codice di incorporamento esistente con *il codice di incorporamento* copiato dal passaggio 3.

   ![Sostituisci codice di incorporamento](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Abilita **Registrazione console** e **Blocca** il debugger nella scheda WKND.

   ![Registrazione console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verificare Adobe Client Data Layer nel sito WKND

Il [progetto di riferimento WKND](https://github.com/adobe/aem-guides-wknd) è stato creato con i componenti core di AEM e per impostazione predefinita ha abilitato [Adobe Client Data Layer](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it#installation-activation). Successivamente, verifica che Adobe Client Data Layer sia abilitato.

1. Passa a [Sito WKND](https://wknd.site/us/en.html).
1. Apri gli strumenti per sviluppatori del browser e passa alla **console**. Esegui il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Il codice riportato sopra restituisce lo stato corrente di Adobe Client Data Layer.

   ![Stato di Adobe Data Layer](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Espandere la risposta ed esaminare la voce `page`. Dovresti visualizzare uno schema di dati come il seguente:

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

   Per inviare i dati di pagina ad Adobe Analytics, utilizziamo le proprietà standard come `dc:title`, `xdm:language` e `xdm:template` del livello dati.

   Per ulteriori informazioni, controlla lo [Schema pagina](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it#page) dagli schemi dati dei Componenti core.

   >[!NOTE]
   >
   > Se l&#39;oggetto JavaScript `adobeDataLayer` non è visualizzato? Verifica che [Adobe Client Data Layer sia stato abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it#installation-activation) sul tuo sito.

## Creare una regola Page Loaded

Adobe Client Data Layer è un livello dati **basato su eventi**. Quando il livello dati della pagina AEM viene caricato, attiva un evento `cmp:show`. Creare una regola che viene attivata quando l&#39;evento `cmp:show` viene attivato dal livello dati della pagina.

1. Passa ad Experience Platform e accedi alla proprietà tag integrata con il sito AEM.
1. Passare alla sezione **Rules** nell&#39;interfaccia utente di Tag Property, quindi fare clic su **Create New Rule**.

   ![Crea regola](assets/collect-data-analytics/analytics-create-rule.png)

1. Denomina la regola **Pagina caricata**.
1. Nella sottosezione **Events**, fai clic su **Aggiungi** per aprire la procedura guidata **Configurazione evento**.
1. Per il campo **Tipo evento**, selezionare **Codice personalizzato**.

   ![Denomina la regola e aggiungi l&#39;evento del codice personalizzato](assets/collect-data-analytics/custom-code-event.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente snippet di codice:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
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

   Il frammento di codice sopra riportato aggiunge un listener di eventi [inviando una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando viene attivato l&#39;evento `cmp:show`, viene chiamata la funzione `pageShownEventHandler`. In questa funzione vengono aggiunti alcuni controlli di integrità e viene costruito un nuovo `event` con l&#39;ultimo [stato del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l&#39;evento.

   Infine, viene chiamata la funzione `trigger(event)`. La funzione `trigger()` è un nome riservato nella proprietà tag e **attiva** la regola. L&#39;oggetto `event` viene passato come parametro che a sua volta è esposto da un altro nome riservato nella proprietà tag. Gli elementi dati nella proprietà tag ora possono fare riferimento a varie proprietà utilizzando lo snippet di codice come `event.component['someKey']`.

1. Salva le modifiche.
1. Avanti in **Azioni** fare clic su **Aggiungi** per aprire la **Configurazione azione** guidata.
1. Per il campo **Tipo azione**, scegli **Codice personalizzato**.

   ![Tipo azione Codice Personalizzato](assets/collect-data-analytics/action-custom-code.png)

1. Fai clic su **Apri editor** nel pannello principale e immetti il seguente snippet di codice:

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   L&#39;oggetto `event` è passato dal metodo `trigger()` chiamato nell&#39;evento personalizzato. `component` è la pagina corrente derivata dal livello dati `getState` nell&#39;evento personalizzato.

1. Salva le modifiche ed esegui una [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=it) nella proprietà tag per promuovere il codice nell&#39;[ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=it) utilizzato nel tuo sito AEM.

   >[!NOTE]
   >
   > Può essere utile utilizzare [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=it) per passare il codice da incorporare a un ambiente **Development**.

1. Vai al tuo sito AEM e apri gli strumenti per sviluppatori per visualizzare la console. Aggiorna la pagina per verificare che i messaggi della console siano stati registrati:

![Messaggi console caricati pagina](assets/collect-data-analytics/page-show-event-console.png)

## Creare elementi dati

Quindi crea diversi elementi dati per acquisire valori diversi da Adobe Client Data Layer. Come mostrato nell’esercizio precedente, è possibile accedere alle proprietà del livello dati direttamente tramite il codice personalizzato. Il vantaggio di utilizzare gli elementi dati è che possono essere riutilizzati in più regole di tag.

Gli elementi dati sono mappati alle proprietà `@type`, `dc:title` e `xdm:template`.

### Tipo risorsa componente

1. Passa ad Experience Platform e accedi alla proprietà tag integrata con il sito AEM.
1. Passare alla sezione **Elementi dati** e fare clic su **Crea nuovo elemento dati**.
1. Per il campo **Nome**, immettere il **Tipo risorsa componente**.
1. Per il campo **Tipo elemento dati**, selezionare **Codice personalizzato**.

   ![Tipo risorsa componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Fai clic sul pulsante **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Salva le modifiche.

   >[!NOTE]
   >
   > Ricorda che l&#39;oggetto `event` è reso disponibile e con ambito in base all&#39;evento che ha attivato la **regola** nella proprietà tag. Il valore di un elemento dati non viene impostato finché all&#39;elemento dati non viene fatto riferimento **&#x200B; in una regola. Pertanto, è sicuro utilizzare questo elemento dati all&#39;interno di una regola come la regola &#x200B;** Pagina caricata** creata nel passaggio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.

### Nome pagina

1. Fai clic sul pulsante **Aggiungi elemento dati**
1. Per il campo **Nome**, immetti **Nome pagina**.
1. Per il campo **Tipo elemento dati**, selezionare **Codice personalizzato**.
1. Fai clic sul pulsante **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Salva le modifiche.

### Modello della pagina

1. Fai clic sul pulsante **Aggiungi elemento dati**
1. Per il campo **Nome**, immetti **Modello pagina**.
1. Per il campo **Tipo elemento dati**, selezionare **Codice personalizzato**.
1. Fai clic sul pulsante **Apri editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Salva le modifiche.

1. Ora dovrebbero essere presenti tre elementi dati come parte della regola:

   ![Elementi dati nella regola](assets/collect-data-analytics/data-elements-page-rule.png)

## Aggiungere l’estensione Analytics

Quindi aggiungi l’estensione Analytics alla proprietà tag per inviare dati a una suite di rapporti.

1. Passa ad Experience Platform e accedi alla proprietà tag integrata con il sito AEM.
1. Vai a **Estensioni** > **Catalogo**
1. Individua l&#39;estensione **Adobe Analytics** e fai clic su **Installa**

   ![Estensione Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. In **Gestione libreria** > **Suite per report**, immetti gli ID suite per report che desideri utilizzare con ogni ambiente di tag.

   ![Immetti gli ID delle suite di rapporti](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In questa esercitazione puoi utilizzare una suite di rapporti per tutti gli ambienti, ma in uno scenario reale vorresti utilizzare suite di rapporti separate, come illustrato nell’immagine seguente

   >[!TIP]
   >
   >È consigliabile utilizzare l&#39;opzione *Gestisci la libreria per me* come impostazione di Gestione libreria, in quanto consente di mantenere la libreria `AppMeasurement.js` aggiornata in modo molto più semplice.

1. Seleziona la casella per abilitare **Utilizza Activity Map**.

   ![Abilita Usa Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. In **Generale** > **Server di monitoraggio**, immetti il server di monitoraggio, ad esempio `tmd.sc.omtrdc.net`. Immetti SSL Tracking Server se il tuo sito supporta `https://`

   ![Immettere i server di monitoraggio](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Fai clic su **Salva** per salvare le modifiche.

## Aggiungere una condizione alla regola Page Loaded

Quindi, aggiorna la regola **Pagina caricata** per utilizzare l&#39;elemento dati **Tipo risorsa componente** per garantire che la regola venga attivata solo quando l&#39;evento `cmp:show` è per la **Pagina**. Altri componenti possono attivare l&#39;evento `cmp:show`, ad esempio il componente Carosello lo viene attivato quando cambiano le diapositive. Pertanto, è importante aggiungere una condizione per questa regola.

1. Nell&#39;interfaccia utente della proprietà Tag, passa alla regola **Pagina caricata** creata in precedenza.
1. In **Condizioni** fare clic su **Aggiungi** per aprire la procedura guidata **Configurazione condizione**.
1. Per il campo **Tipo condizione**, selezionare l&#39;opzione **Confronto valori**.
1. Impostare il primo valore nel campo modulo su `%Component Resource Type%`. È possibile utilizzare l&#39;icona dell&#39;elemento dati ![icona dell&#39;elemento dati](assets/collect-data-analytics/cylinder-icon.png) per selezionare l&#39;elemento dati **Tipo risorsa componente**. Lascia il comparatore impostato su `Equals`.
1. Impostare il secondo valore su `wknd/components/page`.

   ![Configurazione condizione per regola caricata pagina](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > È possibile aggiungere questa condizione all&#39;interno della funzione di codice personalizzato che ascolta l&#39;evento `cmp:show` creato in precedenza nell&#39;esercitazione. Tuttavia, aggiungerla all’interno dell’interfaccia utente offre maggiore visibilità ad altri utenti che potrebbero dover apportare modifiche alla regola. In più, possiamo utilizzare il nostro elemento dati!

1. Salva le modifiche.

## Impostare le variabili di Analytics e attivare il beacon Visualizzazione pagina

Attualmente la regola **Pagina caricata** restituisce semplicemente un&#39;istruzione della console. Quindi, utilizza gli elementi dati e l&#39;estensione Analytics per impostare le variabili Analytics come **azione** nella regola **Pagina caricata**. È stata inoltre impostata un&#39;azione aggiuntiva per attivare il **beacon Visualizzazione pagina** e inviare i dati raccolti ad Adobe Analytics.

1. Nella regola Page Loaded, **rimuovi** l&#39;azione **Core - Custom Code** (istruzioni della console):

   ![Rimuovi azione codice personalizzato](assets/collect-data-analytics/remove-console-statements.png)

1. In sottosezione Azioni, fai clic su **Aggiungi** per aggiungere una nuova azione.

1. Imposta il tipo **Extension** su **Adobe Analytics** e imposta il tipo **Action** su **Set Variables**

   ![Imposta l&#39;estensione dell&#39;azione sulle variabili impostate da Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Nel pannello principale, seleziona un **eVar** disponibile e imposta come valore dell&#39;elemento dati **Modello pagina**. Utilizza l&#39;icona Elementi dati ![Icona Elementi dati](assets/collect-data-analytics/cylinder-icon.png) per selezionare l&#39;elemento **Modello pagina**.

   ![Imposta come modello pagina eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Scorri verso il basso, in **Impostazioni aggiuntive** imposta **Nome pagina** sull&#39;elemento dati **Nome pagina**:

   ![Set di variabili di ambiente nome pagina](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Salva le modifiche.

1. Quindi, aggiungi un&#39;azione aggiuntiva a destra di **Adobe Analytics - Imposta variabili** toccando l&#39;icona **più**:

   ![Aggiungi un&#39;azione aggiuntiva per la regola di tag](assets/collect-data-analytics/add-additional-launch-action.png)

1. Imposta il tipo **Extension** su **Adobe Analytics** e imposta il tipo **Action** su **Send Beacon**. Poiché questa azione è considerata una visualizzazione di pagina, lasciare impostato il tracciamento predefinito su **`s.t()`**.

   ![Azione Invia beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salva le modifiche. La regola **Pagina caricata** deve ora avere la seguente configurazione:

   ![Configurazione regola tag finale](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Ascolta l&#39;evento `cmp:show`.
   * **2.** Verificare che l&#39;evento sia stato attivato da una pagina.
   * **3.** Imposta le variabili di Analytics per **Nome pagina** e **Modello pagina**
   * **4.** Invia il beacon di visualizzazione della pagina di Analytics

1. Salva tutte le modifiche e crea la libreria tag, passando all’ambiente appropriato.

## Convalidare la chiamata del beacon Visualizzazione pagina e di Analytics

Ora che la regola **Page Loaded** invia il beacon Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando Experience Platform Debugger.

1. Apri il [sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fai clic sull&#39;icona Debugger ![icona Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) per aprire Experience Platform Debugger.
1. Accertati che Debugger mappi la proprietà tag nell&#39;ambiente di sviluppo *your*, come descritto in precedenza, e che sia selezionato **Registrazione console**.
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata su *la tua* suite di rapporti. Compilare anche il Nome pagina:

   ![Debugger scheda Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Scorri verso il basso ed espandi **Richieste di rete**. Dovresti essere in grado di trovare il set **evar** per il **modello pagina**:

   ![Evar e nome pagina impostati](assets/collect-data-analytics/evar-page-name-set.png)

1. Torna al browser e apri la console per sviluppatori. Fai clic sul **carosello** nella parte superiore della pagina.

   ![Fai clic sulla pagina del carosello](assets/collect-data-analytics/click-carousel-page.png)

1. Osserva nella console del browser l’istruzione della console:

   ![Condizione non soddisfatta](assets/collect-data-analytics/condition-not-met.png)

   Il carosello non attiva un evento `cmp:show` *ma* a causa del controllo del tipo di risorsa **Component**, non viene attivato alcun evento.

   >[!NOTE]
   >
   > Se non trovi alcun registro della console, assicurati che **Registrazione console** sia selezionato in **Tag Experience Platform** nel debugger di Experience Platform.

1. Passa a una pagina di articolo come [Australia occidentale](https://wknd.site/us/en/magazine/western-australia.html). Osserva che il Nome pagina e il Tipo di modello cambiano.

## Congratulazioni.

Hai appena utilizzato Adobe Client Data Layer e i tag basati sugli eventi in Experience Platform per raccogliere i dati della pagina da un sito AEM e inviarli ad Adobe Analytics.

### Passaggi successivi

Consulta il seguente tutorial per scoprire come utilizzare Adobe Client Data Layer basato sugli eventi per [tenere traccia dei clic di componenti specifici su un sito Adobe Experience Manager](track-clicked-component.md).
