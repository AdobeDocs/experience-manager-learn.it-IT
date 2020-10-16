---
title: Raccolta di dati di pagina con  Adobe Analytics
description: Utilizzate il livello Dati client del Adobe  basato su eventi per raccogliere dati sull'attività dell'utente in un sito Web creato con Adobe Experience Manager. Scopri come utilizzare le regole del Experience Platform Launch per ascoltare questi eventi e inviare dati a una suite di rapporti Adobe Analytics .
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
translation-type: tm+mt
source-git-commit: 096cdccdf1675480aa0a35d46ce7b62a3906dad1
workflow-type: tm+mt
source-wordcount: '2414'
ht-degree: 2%

---


# Raccolta di dati di pagina con  Adobe Analytics

Scoprite come utilizzare le funzionalità integrate del livello dati client del [Adobe con AEM componenti](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/data-layer/overview.html) core per raccogliere i dati su una pagina in  Adobe Experience Manager Sites. [L&#39;Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) e l&#39; [estensione](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) Adobe Analytics saranno utilizzati per creare regole per inviare i dati di pagina a  Adobe Analytics.

## Cosa verrà creato

![Tracciamento dati pagina](assets/collect-data-analytics/analytics-page-data-tracking.png)

In questa esercitazione verrà attivata una regola Launch basata su un evento dal livello dati client del Adobe , verranno aggiunte le condizioni per l&#39;attivazione della regola e verrà inviato ad  Adobe Analytics il nome **della** pagina e il modello **di** pagina di una AEM Page.

### Obiettivi {#objective}

1. Creare una regola basata su eventi in Launch in base alle modifiche apportate al livello dati
1. Mappatura delle proprietà del livello dati della pagina su Elementi dati in Launch
1. Raccogli i dati della pagina e invia a  Adobe Analytics con il beacon di visualizzazione della pagina

## Prerequisiti

Sono necessari i seguenti requisiti:

* **Experience Platform Launch** , proprietà
* **Adobe Analytics** test/dev report suite ID e server di tracciamento. Consulta la documentazione seguente per [creare una nuova suite](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)di rapporti.
* [estensione del browser del debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) di Experience Platform. Screenshot in questa esercitazione acquisita dal browser Chrome.
* (Facoltativo) AEM Sito con il [Livello dati client abilitato](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Questa esercitazione utilizzerà il sito pubblico [https://wknd.site/us/en.html](https://wknd.site/us/en.html) , ma è possibile utilizzare il proprio sito.

>[!NOTE]
>
> Hai bisogno di aiuto per integrare Launch e il tuo sito AEM? [Guardate questa serie](../experience-platform-launch/overview.md)video.

## Cambiare gli ambienti di lancio per il sito WKND

[https://wknd.site](https://wknd.site) è un sito rivolto al pubblico basato su [un progetto](https://github.com/adobe/aem-guides-wknd) open source progettato come riferimento e come [esercitazione](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) per AEM implementazioni.

Invece di configurare un ambiente AEM e installare la base di codice WKND, potete utilizzare il debugger del Experience Platform  per **passare** dal vivo [https://wknd.site/](https://wknd.site/) alla ** proprietà Launch. Naturalmente è possibile utilizzare il proprio sito AEM se il livello dati client del [Adobe è già abilitato](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Accedete al Experience Platform Launch e [create una proprietà](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html) Launch (se non è già stata creata).
1. Accertati che sia stata creata [una](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library) libreria Launch iniziale e che sia stata promossa a un [ambiente](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)Launch.
1. Copiate il codice da incorporare di Launch dall’ambiente in cui è stata pubblicata la libreria.

   ![Copia codice di incorporamento lancio](assets/collect-data-analytics/launch-environment-copy.png)

1. Nel browser aprite una nuova scheda e andate a [https://wknd.site/](https://wknd.site/)
1. Aprire l&#39;estensione del browser  Experience Platform Debugger

   ![Debugger Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Andate a **Launch** > **Configuration** (Configurazione **) e in Codici** di incorporamento *iniettati sostituite il codice di incorporamento di Launch esistente con* il codice da incorporare copiato dal passaggio 3.

   ![Sostituisci codice da incorporare](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Abilita **Console Logging** e **blocca** il debugger nella scheda WKND.

   ![Registrazione console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verifica  livello dati client Adobe sul sito WKND

Il progetto [](https://github.com/adobe/aem-guides-wknd) WKND Reference è realizzato con AEM componenti core e per impostazione predefinita dispone del livello dati client del [Adobe abilitato](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) . Quindi, verificare che il livello dati client del Adobe  sia attivato.

1. Andate a [https://wknd.site](https://wknd.site).
1. Apri gli strumenti di sviluppo del browser e passa alla **console**. Eseguite il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Questo restituisce lo stato corrente del livello dati client del Adobe .

   ![Stato livello dati Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Espandete la risposta ed esaminate la `page` voce. Dovrebbe essere visualizzato uno schema di dati simile al seguente:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: "WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world."
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Verranno utilizzate le proprietà standard derivate dallo schema [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)Pagina `dc:title`, `xdm:language` e `xdm:template` dal livello dati per inviare i dati della pagina a  Adobe Analytics.

   >[!NOTE]
   >
   > Non vedete l&#39;oggetto `adobeDataLayer` javascript? Assicurarsi che il livello dati client [Adobe sia stato attivato](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) sul sito.

## Creare una regola di caricamento pagina

Il livello dati client del Adobe  è un livello dati basato sull&#39; **evento** . Quando il livello dati **Pagina** AEM viene caricato, viene attivato un evento `cmp:show`. Creare una regola che verrà attivata in base all&#39; `cmp:show` evento.

1. Passare all&#39;Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Andate alla sezione **Regole** nell&#39;interfaccia utente di avvio, quindi fate clic su **Crea nuova regola**.

   ![Crea regola](assets/collect-data-analytics/analytics-create-rule.png)

1. Denominate la regola **Pagina caricata**.
1. Fate clic su **Eventi** **Aggiungi** per aprire la procedura guidata Configurazione **** evento.
1. In Tipo **** evento, selezionate Codice **** personalizzato.

   ![Denominate la regola e aggiungete l&#39;evento codice personalizzato](assets/collect-data-analytics/custom-code-event.png)

1. Fate clic su **Apri Editor** nel pannello principale e immettete il seguente frammento di codice:

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

   Lo snippet di codice riportato sopra aggiunge un listener di eventi [spingendo una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando l&#39; `cmp:show` evento viene attivato, viene chiamata la `pageShownEventHandler` funzione. In questa funzione vengono aggiunti alcuni controlli di integrità e `event` viene costruito un nuovo [stato con l&#39;ultimo livello](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) di dati per il componente che ha attivato l&#39;evento.

   Dopo che `trigger(event)` è stato chiamato. `trigger()` è un nome riservato in Launch e &quot;attiverà&quot; la regola Launch. Passiamo l&#39; `event` oggetto come parametro che a sua volta sarà esposto da un altro nome riservato in Launch denominato `event`. Gli elementi dati in Launch possono ora fare riferimento a varie proprietà come ad esempio: `event.component['someKey']`.

1. Salva le modifiche.
1. Nella sezione **Azioni** fare clic su **Aggiungi** per aprire la procedura guidata **Configurazione** azione.
1. In Tipo **** azione, scegliete Codice **** personalizzato.

   ![Tipo azione codice personalizzato](assets/collect-data-analytics/action-custom-code.png)

1. Fate clic su **Apri Editor** nel pannello principale e immettete il seguente frammento di codice:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   L&#39; `event` oggetto viene passato dal `trigger()` metodo chiamato nell&#39;evento personalizzato. `component` è la pagina corrente derivata dal livello dati `getState` nell&#39;evento personalizzato. Richiamate in precedenza lo schema [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) Pagina esposto dal livello dati per vedere le varie chiavi esposte fuori dalla casella.

1. Salvare le modifiche ed eseguire una [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) in Launch per promuovere il codice nell&#39; [ambiente](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) utilizzato sul sito AEM.

   >[!NOTE]
   >
   > Può essere molto utile utilizzare [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) per convertire il codice da incorporare in un ambiente di **sviluppo** .

1. Andate al sito AEM e aprite gli strumenti di sviluppo per visualizzare la console. Aggiorna la pagina e dovresti vedere che i messaggi della console sono stati registrati:

   ![Messaggi della console caricata pagina](assets/collect-data-analytics/page-show-event-console.png)

## Crea elementi dati

Create quindi diversi elementi di dati per acquisire valori diversi dal livello dati client del Adobe . Come mostrato nell&#39;esercizio precedente, è possibile accedere direttamente alle proprietà del livello dati tramite codice personalizzato. Il vantaggio di utilizzare gli elementi dati è che possono essere riutilizzati nelle regole di Launch.

Richiama da una versione precedente lo schema [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) Pagina esposto dal livello dati:

Gli elementi dati verranno mappati sulle `@type`, `dc:title`e `xdm:template` sulle proprietà.

### Tipo risorsa componente

1. Passare all&#39;Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Andate alla sezione **Elementi** dati e fate clic su **Crea nuovo elemento** dati.
1. Per **Nome** , immettere **Component Resource Type**.
1. Per Tipo **elemento** dati, selezionare Codice **** personalizzato.

   ![Tipo risorsa componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Fate clic su **Apri editor** e immettete quanto segue nell’editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Salva le modifiche.

   >[!NOTE]
   >
   > Ricordare che l&#39; `event` oggetto è reso disponibile e con ambito in base all&#39;evento che ha attivato la **regola** in Launch. Il valore di un elemento dati non viene impostato finché non viene fatto *riferimento* all&#39;elemento dati all&#39;interno di una regola. Pertanto è sicuro utilizzare questo elemento dati all&#39;interno di una regola come la regola **Pagina caricata** creata nel passaggio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.

### Nome pagina

1. Fare clic su **Aggiungi elemento** dati.
1. Per **Nome** , immettete Nome **** pagina.
1. Per Tipo **elemento** dati, selezionare Codice **** personalizzato.
1. Fate clic su **Apri editor** e immettete quanto segue nell’editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salva le modifiche.

### Modello pagina

1. Fare clic su **Aggiungi elemento** dati.
1. Per **Nome** , immettete Nome **** pagina.
1. Per Tipo **elemento** dati, selezionare Codice **** personalizzato.
1. Fate clic su **Apri editor** e immettete quanto segue nell’editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Salva le modifiche.

1. A questo punto dovrebbero essere presenti tre elementi di dati come parte della regola:

   ![Elementi dati nella regola](assets/collect-data-analytics/data-elements-page-rule.png)

## Aggiungere l’estensione Analytics

Quindi aggiungi l’estensione Analytics alla proprietà Launch. Dobbiamo inviare questi dati da qualche parte!

1. Passare all&#39;Experience Platform Launch e alla proprietà Web integrata con il sito AEM.
1. Vai a **Estensioni** > **Catalogo**
1. Individuate l’estensione **Adobe Analytics** e fate clic su **Installa**

   ![Adobe Analytics Extension](assets/collect-data-analytics/analytics-catalog-install.png)

1. In **Library Management** (Gestione libreria) > **Report Suites (Suite di rapporti**), immetti gli ID della suite di rapporti che desideri utilizzare con ogni ambiente Launch.

   ![Inserire gli ID suite di rapporti](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In questa esercitazione è possibile utilizzare una suite di rapporti per tutti gli ambienti, ma in realtà si desidera utilizzare suite di rapporti separate, come illustrato nell&#39;immagine seguente

   >[!TIP]
   >
   >È consigliabile utilizzare l&#39;opzione *Gestisci la libreria per* me come impostazione Gestione libreria, in quanto consente di mantenere la `AppMeasurement.js` libreria aggiornata molto più facilmente.

1. Selezionare la casella per abilitare **Usa  Activity Map**.

   ![Abilita  Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. In **Generale** > Server **di** tracciamento, immetti il server di tracciamento, ad esempio `tmd.sc.omtrdc.net`. Inserire il server di tracciamento SSL se il sito supporta `https://`

   ![Immissione dei server di tracciamento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Click **Save** to save the changes.

## Aggiungere una condizione alla regola Pagina caricata

Quindi, aggiornate la regola **Pagina caricata** per utilizzare l&#39;elemento dati Tipo **risorsa** componente per garantire che la regola venga attivata solo quando l&#39; `cmp:show` evento è relativo alla **pagina**. L’ `cmp:show` evento può essere attivato da altri componenti, ad esempio il componente Carosello verrà attivato quando le diapositive cambiano. Pertanto è importante aggiungere una condizione per questa regola.

1. Nell’interfaccia utente di Launch, passa alla regola **Pagina caricata** creata in precedenza.
1. In **Condizioni** , fare clic su **Aggiungi** per aprire la procedura guidata **Configurazione** condizione.
1. Per Tipo **di** condizione, selezionare Confronto tra **valori**.
1. Impostare il primo valore del campo modulo su `%Component Resource Type%`. È possibile utilizzare l&#39;icona ![dell&#39;elemento](assets/collect-data-analytics/cylinder-icon.png) dati Icona elemento dati per selezionare l&#39;elemento dati Tipo **risorsa** componente. Lasciare impostato il confronto su `Equals`.
1. Impostare il secondo valore su `wknd/components/page`.

   ![Configurazione delle condizioni per la regola di caricamento della pagina](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > È possibile aggiungere questa condizione all&#39;interno della funzione di codice personalizzata che esegue l&#39;ascolto dell&#39; `cmp:show` evento creato in precedenza nell&#39;esercitazione. Tuttavia, l&#39;aggiunta di questa opzione all&#39;interno dell&#39;interfaccia utente offre maggiore visibilità agli utenti che potrebbero dover apportare modifiche alla regola. Inoltre abbiamo utilizzato il nostro elemento dati!

1. Salva le modifiche.

## Imposta variabili Analytics e attiva beacon Visualizzazione pagina

Attualmente la regola **Pagina caricata** genera semplicemente un&#39;istruzione console. Quindi, utilizzate gli elementi dati e l&#39;estensione Analytics per impostare le variabili Analytics come **azione** nella regola **Pagina caricata** . Verrà inoltre impostata un&#39;azione aggiuntiva per attivare il beacon **di visualizzazione** pagina e inviare i dati raccolti a  Adobe Analytics.

1. Nella regola **Pagina caricata** **rimuovere** l’azione **Core - Codice** personalizzato (le istruzioni della console):

   ![Rimuovi azione codice personalizzata](assets/collect-data-analytics/remove-console-statements.png)

1. In Azioni, fare clic su **Aggiungi** per aggiungere una nuova azione.
1. Impostate il tipo di **estensione** su **Adobe Analytics** e impostate Tipo **** azione su **Imposta variabili**

   ![Imposta estensione azione su variabili di set di Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Nel pannello principale selezionare un **eVar** disponibile e impostare come valore del modello **di** pagina elemento dati. Utilizzate l&#39;icona ![Elementi dati](assets/collect-data-analytics/cylinder-icon.png) per selezionare l&#39;elemento Modello **di** pagina.

   ![Imposta come modello di pagina  eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Scorri verso il basso, in Impostazioni **** aggiuntive impostate Nome **** pagina sull’elemento dati **Nome** pagina:

   ![Set di variabili Ambiente nome pagina](assets/collect-data-analytics/page-name-env-variable-set.png)

   Salva le modifiche.

1. Quindi, aggiungi un’altra azione a destra della **Adobe Analytics - Imposta variabili** toccando l’icona **più** :

   ![Aggiunta di un&#39;altra azione di avvio](assets/collect-data-analytics/add-additional-launch-action.png)

1. Impostate il tipo di **estensione** su **Adobe Analytics** e impostate Tipo **** azione su **Invia beacon**. Poiché questa è considerata una visualizzazione di pagina, lasciate impostato il tracciamento predefinito su **`s.t()`**.

   ![Azione Invia beacon  Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salva le modifiche. La regola **Pagina caricata** deve ora avere la configurazione seguente:

   ![Configurazione del lancio finale](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Ascoltare l&#39; `cmp:show` evento.
   * **2.** Verificare che l&#39;evento sia stato attivato da una pagina.
   * **3.** Imposta le variabili di Analytics per Nome **** pagina e Modello **pagina**
   * **4.** Invia beacon di visualizzazione pagina di Analytics
1. Salvate tutte le modifiche e create la libreria Launch, promuovendo l&#39;ambiente appropriato.

## Convalida del beacon di visualizzazione pagina e della chiamata di Analytics

Ora che la regola **Pagina caricata** invia il beacon di Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando il debugger di Experience Platform .

1. Aprite il sito [WKND](https://wknd.site/us/en.html) nel browser.
1. Fate clic sull&#39;icona Debugger ![della piattaforma Experience](assets/collect-data-analytics/experience-cloud-debugger.png) per aprire il Debugger Experience Platform .
1. Assicurati che il debugger stia mappando la proprietà Launch al *tuo* ambiente di sviluppo, come descritto in precedenza, e che **Console Logging** sia selezionato.
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata sulla *tua* suite di rapporti. È inoltre necessario compilare il Nome pagina:

   ![Debugger scheda Analisi](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Scorri verso il basso ed espandi Richieste **** di rete. Dovrebbe essere possibile trovare il set **evar** per il modello **di** pagina:

   ![Set Evar and Page Name](assets/collect-data-analytics/evar-page-name-set.png)

1. Tornate al browser e aprite la console per sviluppatori. Fate clic sul **carosello** nella parte superiore della pagina.

   ![Fare clic sulla pagina del carosello](assets/collect-data-analytics/click-carousel-page.png)

1. Osservare nella console del browser l’istruzione della console:

   ![Condizione non soddisfatta](assets/collect-data-analytics/condition-not-met.png)

   Questo perché il carosello attiva un `cmp:show` evento *ma* , a causa del controllo del tipo **di risorsa del** componente, non viene attivato alcun evento.

   >[!NOTE]
   >
   > Se non visualizzi alcun registro di console, accertati che **Console Logging** sia selezionato in **Avvia** nel debugger di Experience Platform .

1. Passate a una pagina di articolo come [Western Australia](https://wknd.site/us/en/magazine/western-australia.html). Osservate la modifica di Nome pagina e Tipo modello.

## Congratulazioni!

È stato utilizzato il livello dati client  Adobe basato su eventi e il Experience Platform Launch per raccogliere i dati delle pagine da un sito AEM e inviarli a  Adobe Analytics.

### Passaggi successivi

Per informazioni su come utilizzare il livello Dati client del Adobe basato su eventi per [monitorare i clic di componenti specifici su un sito](track-clicked-component.md)Adobe Experience Manager, consultate la seguente esercitazione.
