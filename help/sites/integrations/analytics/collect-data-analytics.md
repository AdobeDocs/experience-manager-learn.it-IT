---
title: Raccogliere dati di pagina con Adobe Analytics
description: Utilizza Adobe Client Data Layer basato sugli eventi per raccogliere i dati sull’attività dell’utente su un sito web creato con Adobe Experience Manager. Scopri come utilizzare le regole dei tag per ascoltare questi eventi e inviare dati a una suite di rapporti di Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '2447'
ht-degree: 2%

---

# Raccogliere dati di pagina con Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch è stato classificato come una suite di tecnologie di raccolta dati in Adobe Experience Platform. Di conseguenza, sono state introdotte diverse modifiche terminologiche nella documentazione del prodotto. Consulta quanto segue [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) per un riferimento consolidato delle modifiche terminologiche.


Scopri come utilizzare le funzionalità integrate del [Adobe Client Data Layer con AEM componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it) per raccogliere dati su una pagina in Adobe Experience Manager Sites. [Experience Platform dei tag](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) e [Estensione Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) vengono utilizzati per creare regole per inviare i dati di pagina ad Adobe Analytics.

## Cosa verrà creato {#what-build}

![Tracciamento dei dati delle pagine](assets/collect-data-analytics/analytics-page-data-tracking.png)

In questa esercitazione, stai per attivare una regola di tag basata su un evento da Adobe Client Data Layer. Inoltre, aggiungi condizioni per quando la regola deve essere attivata e quindi invia il **Nome pagina** e **Modello di pagina** valori di una pagina AEM in Adobe Analytics.

### Obiettivi {#objective}

1. Creare una regola basata su eventi nella proprietà tag che acquisisca le modifiche dal livello dati
1. Mappatura delle proprietà del livello dati della pagina su elementi dati nella proprietà tag
1. Raccogliere e inviare dati di pagina in Adobe Analytics utilizzando il beacon di visualizzazione della pagina

## Prerequisiti

Sono necessari i seguenti requisiti:

* **Proprietà tag** in Experience Platform
* **Adobe Analytics** ID suite di rapporti test/dev e server di tracciamento. Consulta la seguente documentazione per [creazione di una suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Debugger Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) estensione del browser. Schermate in questa esercitazione acquisite dal browser Chrome.
* (Facoltativo) AEM sito con il [Adobe Client Data Layer abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Questa esercitazione utilizza il pubblico [WKND](https://wknd.site/us/en.html) sito, ma puoi utilizzare il tuo sito.

>[!NOTE]
>
> Hai bisogno di aiuto con l’integrazione della proprietà tag e AEM sito? [Guarda questa serie video](../experience-platform/data-collection/tags/overview.md).

## Cambia ambiente tag per il sito WKND

La [WKND](http://wknd.site/us/en.html) è un sito rivolto al pubblico basato su [un progetto open-source](https://github.com/adobe/aem-guides-wknd) progettati come riferimento e [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it) per un’implementazione AEM.

Invece di configurare un ambiente AEM e installare la base di codice WKND, puoi utilizzare il debugger di Experience Platform per **interruttore** il live [Sito WKND](http://wknd.site/us/en.html) a *le* proprietà tag . Tuttavia, puoi utilizzare il tuo sito AEM se ha già [Adobe Client Data Layer abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. Accedi all’Experience Platform e [creare una proprietà Tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (se non lo hai già fatto).
1. Assicurati che un tag iniziale JavaScript [è stata creata una libreria](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) e promosso al tag [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=it).
1. Copia il codice di incorporamento JavaScript dall&#39;ambiente tag in cui è stata pubblicata la libreria.

   ![Copia codice di incorporamento proprietà tag](assets/collect-data-analytics/launch-environment-copy.png)

1. Nel browser, apri una nuova scheda e passa a [Sito WKND](http://wknd.site/us/en.html)
1. Apri l’estensione del browser Experience Platform Debugger.

   ![Debugger Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Passa a **Experience Platform di tag** > **Configurazione** e **Codici di incorporamento inseriti** sostituisci il codice di incorporamento esistente con *le* codice di incorporamento copiato dal passaggio 3.

   ![Sostituisci codice di incorporamento](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Abilita **Registrazione console** e **Blocca** debugger nella scheda WKND.

   ![Registrazione console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verifica livello dati client di Adobe sul sito WKND

La [Progetto di riferimento WKND](https://github.com/adobe/aem-guides-wknd) è generato con AEM componenti core e dispone dei [Adobe Client Data Layer abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) per impostazione predefinita. Quindi, verifica che Adobe Client Data Layer sia abilitato.

1. Passa a [Sito WKND](http://wknd.site/us/en.html).
1. Apri gli strumenti di sviluppo del browser e passa alla **Console**. Esegui il comando seguente:

   ```js
   adobeDataLayer.getState();
   ```

   Il codice riportato sopra restituisce lo stato corrente dell&#39;Adobe Client Data Layer.

   ![Adobe stato livello dati](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Espandi la risposta ed esamina la `page` voce. Dovresti visualizzare uno schema di dati simile al seguente:

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

   Per inviare dati di pagina ad Adobe Analytics, utilizziamo le proprietà standard come `dc:title`, `xdm:language`e `xdm:template` del livello dati.

   Per ulteriori informazioni, consulta la sezione [Schema pagina](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) dagli schemi di dati dei componenti core.

   >[!NOTE]
   >
   > Se non vedi il `adobeDataLayer` Oggetto JavaScript? Assicurati che [Adobe Client Data Layer è stato abilitato](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) sul tuo sito.

## Creare una regola di caricamento pagina

Il livello dati client di Adobe è un **event** livello dati guidato. Quando viene caricato il livello dati AEM pagina, viene attivato un `cmp:show` evento. Crea una regola che viene attivata quando `cmp:show` viene attivato dal livello dati della pagina.

1. Accedi ad Experience Platform e alla proprietà tag integrata con il sito AEM.
1. Passa a **Regole** nell’interfaccia utente della proprietà tag, quindi fai clic su **Crea nuova regola**.

   ![Crea regola](assets/collect-data-analytics/analytics-create-rule.png)

1. Denomina la regola **Pagina caricata**.
1. In **Eventi** sottosezione, fai clic su **Aggiungi** per aprire **Configurazione evento** procedura guidata.
1. Per **Tipo evento** campo , seleziona **Codice personalizzato**.

   ![Denomina la regola e aggiungi l&#39;evento codice personalizzato](assets/collect-data-analytics/custom-code-event.png)

1. Fai clic su **Open Editor** nel pannello principale e immetti il seguente frammento di codice:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
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

   Lo snippet di codice di cui sopra aggiunge un listener di eventi da [spingere una funzione](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) nel livello dati. Quando `cmp:show` viene attivato l&#39;evento `pageShownEventHandler` viene chiamata la funzione . In questa funzione vengono aggiunti alcuni controlli di integrità e un nuovo `event` è costruito con [stato del livello dati](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) per il componente che ha attivato l’evento.

   Infine, `trigger(event)` viene chiamata la funzione . La `trigger()` è un nome riservato nella proprietà tag ed è **trigger** la regola. La `event` l&#39;oggetto viene passato come parametro che a sua volta viene esposto da un altro nome riservato nella proprietà tag. Gli elementi dati nella proprietà tag possono ora fare riferimento a varie proprietà utilizzando uno snippet di codice come `event.component['someKey']`.

1. Salva le modifiche.
1. Successivo sotto **Azioni** click **Aggiungi** per aprire **Configurazione azione** procedura guidata.
1. Per **Tipo di azione** campo, scegli **Codice personalizzato**.

   ![Tipo di azione Codice personalizzato](assets/collect-data-analytics/action-custom-code.png)

1. Fai clic su **Open Editor** nel pannello principale e immetti il seguente frammento di codice:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   La `event` viene passato dall&#39;oggetto `trigger()` chiamato nell&#39;evento personalizzato. Qui `component` è la pagina corrente derivata dal livello dati `getState` nell&#39;evento personalizzato.

1. Salvare le modifiche ed eseguire un [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) nella proprietà tag per promuovere il codice nel [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=it) utilizzato sul sito AEM.

   >[!NOTE]
   >
   > Può essere utile utilizzare il [Debugger Adobe Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) per convertire il codice di incorporamento in un **Sviluppo** ambiente.

1. Accedi al sito AEM e apri gli strumenti per sviluppatori per visualizzare la console. Aggiorna la pagina per visualizzare che i messaggi della console sono stati registrati:

![Messaggi console caricati pagina](assets/collect-data-analytics/page-show-event-console.png)

## Creare elementi dati

Crea quindi diversi elementi dati per acquisire valori diversi da Adobe Client Data Layer. Come mostrato nell’esercizio precedente, è possibile accedere alle proprietà del livello dati direttamente tramite codice personalizzato. Il vantaggio dell’utilizzo degli elementi dati è che possono essere riutilizzati nelle regole dei tag.

Gli elementi dati sono mappati sul `@type`, `dc:title`e `xdm:template` proprietà.

### Tipo di risorsa componente

1. Accedi ad Experience Platform e alla proprietà tag integrata con il sito AEM.
1. Passa a **Elementi dati** e fai clic su **Crea nuovo elemento dati**.
1. Per **Nome** campo , inserisci **Tipo di risorsa componente**.
1. Per **Tipo di elemento dati** campo , seleziona **Codice personalizzato**.

   ![Tipo di risorsa componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Fai clic su **Open Editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Salva le modifiche.

   >[!NOTE]
   >
   > Ricorda che la `event` l&#39;oggetto è reso disponibile e delimitato in base all&#39;evento che ha attivato il **Regola** nella proprietà tag . Il valore di un elemento dati non viene impostato finché l’elemento dati non è *referenza* all&#39;interno di una regola. Pertanto è sicuro utilizzare questo elemento dati all’interno di una regola come la **Pagina caricata** regola creata nel passaggio precedente *ma* non sarebbe sicuro da utilizzare in altri contesti.

### Nome pagina

1. Fai clic su **Aggiungi elemento dati** pulsante
1. Per **Nome** campo, immettere **Nome pagina**.
1. Per **Tipo di elemento dati** campo , seleziona **Codice personalizzato**.
1. Fai clic su **Open Editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Salva le modifiche.

### Modello della pagina

1. Fai clic sul pulsante **Aggiungi elemento dati** pulsante
1. Per **Nome** campo, immettere **Modello di pagina**.
1. Per **Tipo di elemento dati** campo , seleziona **Codice personalizzato**.
1. Fai clic su **Open Editor** e immetti quanto segue nell&#39;editor di codice personalizzato:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Salva le modifiche.

1. Ora devi disporre di tre elementi dati come parte della regola:

   ![Elementi dati nella regola](assets/collect-data-analytics/data-elements-page-rule.png)

## Aggiungere l’estensione Analytics

Quindi aggiungi l’estensione Analytics alla proprietà tag per inviare dati in una suite di rapporti.

1. Accedi ad Experience Platform e alla proprietà tag integrata con il sito AEM.
1. Vai a **Estensioni** > **Catalogo**
1. Individua il **Adobe Analytics** estensione e fai clic su **Installa**

   ![Estensione Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Sotto **Gestione libreria** > **Suite di rapporti**, immetti gli ID suite di rapporti che desideri utilizzare con ogni ambiente di tag.

   ![Immetti gli ID suite di rapporti](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In questa esercitazione puoi utilizzare una suite di rapporti per tutti gli ambienti, ma in tempo reale puoi utilizzare suite di rapporti separate, come illustrato nell’immagine seguente

   >[!TIP]
   >
   >Si consiglia di utilizzare *Opzione Gestisci la libreria per me* l&#39;impostazione Library Management rende molto più semplice mantenere `AppMeasurement.js` libreria aggiornata.

1. Seleziona la casella per abilitare **Utilizzare Activity Map**.

   ![Abilita Usa Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. Sotto **Generale** > **Server di tracciamento**, immetti il server di tracciamento, ad esempio `tmd.sc.omtrdc.net`. Immetti SSL Tracking Server se il sito supporta `https://`

   ![Immettere i server di tracciamento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Fai clic su **Salva** per salvare le modifiche.

## Aggiungi una condizione alla regola Page Loaded

Quindi, aggiorna il **Pagina caricata** regola per utilizzare **Tipo di risorsa componente** elemento dati per garantire che la regola si attivi solo quando `cmp:show` l&#39;evento è per **Pagina**. Altri componenti possono attivare `cmp:show` Ad esempio, il componente Carosello viene attivato quando le diapositive cambiano. Pertanto è importante aggiungere una condizione per questa regola.

1. Nell’interfaccia utente della proprietà di tag, passa a **Pagina caricata** creato in precedenza.
1. Sotto **Condizioni** click **Aggiungi** per aprire **Configurazione condizione** procedura guidata.
1. Per **Tipo di condizione** campo , seleziona **Value Comparison** opzione .
1. Impostare il primo valore nel campo modulo su `%Component Resource Type%`. Puoi utilizzare l’icona Elemento dati ![icona dell’elemento dati](assets/collect-data-analytics/cylinder-icon.png) per selezionare **Tipo di risorsa componente** elemento dati. Lascia impostato il comparatore su `Equals`.
1. Imposta il secondo valore su `wknd/components/page`.

   ![Configurazione delle condizioni per la regola di caricamento pagina](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > È possibile aggiungere questa condizione all&#39;interno della funzione di codice personalizzato che ascolta il `cmp:show` creato in precedenza nell&#39;esercitazione. Tuttavia, l’aggiunta di questa funzionalità all’interno dell’interfaccia utente offre maggiore visibilità agli utenti aggiuntivi che potrebbero dover apportare modifiche alla regola. Inoltre, possiamo utilizzare il nostro elemento dati!

1. Salva le modifiche.

## Impostare le variabili di Analytics e attivare il beacon Vista pagina

Attualmente **Pagina caricata** restituisce semplicemente un&#39;istruzione console. Quindi, utilizza gli elementi dati e l’estensione Analytics per impostare le variabili Analytics come **action** in **Pagina caricata** regola. Inoltre, abbiamo impostato un&#39;azione aggiuntiva per attivare la **Beacon di visualizzazione pagina** e inviare i dati raccolti ad Adobe Analytics.

1. Nella regola Page Loaded, **remove** la **Core - Codice personalizzato** azione (le istruzioni della console):

   ![Rimuovi azione codice personalizzato](assets/collect-data-analytics/remove-console-statements.png)

1. Nella sottosezione Azioni, fai clic su **Aggiungi** per aggiungere una nuova azione.

1. Imposta la **Estensione** digitare **Adobe Analytics** e imposta **Tipo di azione** a  **Imposta variabili**

   ![Impostare l’estensione dell’azione su Imposta variabili di Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Nel pannello principale, seleziona una **eVar** e impostato come valore dell&#39;elemento dati **Modello di pagina**. Usa l’icona Elementi dati ![Icona degli elementi dati](assets/collect-data-analytics/cylinder-icon.png) per selezionare **Modello di pagina** elemento.

   ![Imposta come modello di pagina eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Scorri verso il basso, sotto **Impostazioni aggiuntive** set **Nome pagina** all’elemento dati **Nome pagina**:

   ![Set di variabili per l’ambiente Nome pagina](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Salva le modifiche.

1. Quindi, aggiungi un’azione in più a destra di **Adobe Analytics - Impostare le variabili** toccando **plus** icona:

   ![Aggiungi un’azione aggiuntiva Regola di tag](assets/collect-data-analytics/add-additional-launch-action.png)

1. Imposta la **Estensione** digitare **Adobe Analytics** e imposta **Tipo di azione** a  **Invia beacon**. Poiché questa azione è considerata una visualizzazione di pagina, lascia impostato il tracciamento predefinito su **`s.t()`**.

   ![Azione Invia beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salva le modifiche. La **Pagina caricata** La regola deve ora avere la seguente configurazione:

   ![Configurazione della regola di tag finale](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Ascolta i `cmp:show` evento.
   * **2.** Controlla che l&#39;evento sia stato attivato da una pagina.
   * **3.** Imposta le variabili di Analytics per **Nome pagina** e **Modello di pagina**
   * **4.** Invia il beacon Vista pagina di Analytics

1. Salva tutte le modifiche e crea la libreria di tag , promuovendo l’ambiente appropriato.

## Convalida il beacon Vista pagina e la chiamata Analytics

Ora che **Pagina caricata** La regola invia il beacon di Analytics, dovresti essere in grado di visualizzare le variabili di tracciamento di Analytics utilizzando il debugger di Experience Platform.

1. Apri [Sito WKND](https://wknd.site/us/en.html) nel browser.
1. Fai clic sull’icona Debugger ![Icona di Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) per aprire Experience Platform Debugger.
1. Assicurati che Debugger mappi la proprietà tag in *le* Ambiente di sviluppo, come descritto in precedenza e **Registrazione console** è controllata.
1. Apri il menu Analytics e verifica che la suite di rapporti sia impostata su *le* suite di rapporti. È inoltre necessario compilare il campo Nome pagina :

   ![Debugger scheda Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Scorri verso il basso ed espandi **Richieste di rete**. Dovresti essere in grado di trovare il **evar** impostato per **Modello di pagina**:

   ![Impostazione Evar e Nome pagina](assets/collect-data-analytics/evar-page-name-set.png)

1. Torna al browser e apri la console per sviluppatori. Fai clic su **Carosello** nella parte superiore della pagina.

   ![Fai clic sulla pagina del carosello](assets/collect-data-analytics/click-carousel-page.png)

1. Osserva l’istruzione console nella console del browser:

   ![Condizione non soddisfatta](assets/collect-data-analytics/condition-not-met.png)

   Questo perché il carosello attiva un `cmp:show` event *ma* a causa del controllo del **Tipo di risorsa componente**, non viene attivato alcun evento.

   >[!NOTE]
   >
   > Se non trovi registri della console, assicurati che **Registrazione console** è controllato in **Experience Platform di tag** in Experience Platform Debugger.

1. Passa a una pagina dell’articolo come [Australia Occidentale](https://wknd.site/us/en/magazine/western-australia.html). Osserva che il nome della pagina e il tipo di modello cambiano.

## Congratulazioni. 

Hai appena utilizzato Adobe Client Data Layer e Tag basati sull’evento in Experience Platform per raccogliere i dati della pagina da un sito AEM e inviarli ad Adobe Analytics.

### Passaggi successivi

Consulta la seguente esercitazione per scoprire come utilizzare il livello dati client Adobe basato su eventi per [tenere traccia dei clic su componenti specifici su un sito Adobe Experience Manager](track-clicked-component.md).
