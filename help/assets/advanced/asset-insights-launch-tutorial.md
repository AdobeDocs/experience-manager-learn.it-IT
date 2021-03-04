---
title: Configurare Asset Insights con AEM Assets e Adobe Launch
description: In questa serie video in 5 parti, passiamo alla configurazione e alla configurazione di Asset Insights per Experience Manager implementata tramite Launch By Adobe.
feature: Informazioni sulla risorsa
version: 6.3, 6.4, 6.5
topic: Integrations (Integrazioni)
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---


# Configurare Asset Insights con AEM Assets e Adobe Experience Platform Launch

In questa serie video in 5 parti, passiamo alla configurazione e alla configurazione di Asset Insights per Experience Manager implementata tramite Adobe Launch.

## Parte 1: Panoramica di Asset Insights {#overview}

Panoramica di Asset Insights Installa i componenti core, i componenti immagine di esempio e altri pacchetti di contenuti per preparare il tuo ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### Diagramma dell&#39;architettura {#architecture-diagram}

![Diagramma dell&#39;architettura](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Scarica la [versione più recente dei componenti core](https://github.com/adobe/aem-core-wcm-components) per la tua implementazione.

Il video utilizza i componenti core v2.2.2 che non sono più aggiornati; assicurati di utilizzare la versione più recente prima di passare alla sezione successiva.

* Scarica [Contenuto immagine di esempio di Asset Insights](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Scarica [i più recenti componenti core di AEM WCM](https://github.com/adobe/aem-core-wcm-components/releases)

## Parte 2 : Abilitazione del tracciamento degli approfondimenti delle risorse per il componente immagine di esempio {#sample-image-component-asset-insights}

Miglioramenti ai componenti core e utilizzo del componente proxy (componente immagine di esempio) per Asset Insights. Modifica dei criteri dei modelli di pagina di contenuto per abilitare il componente immagine di esempio per il sito di riferimento.

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>Il componente di base immagine include la capacità di disabilitare il tracciamento UUID disattivando il tracciamento dell’UUID della risorsa (valore di identificatore univoco per un nodo creato in JCR)

Il componente Immagine di base utilizza l’attributo ***data-asset-id*** all’interno del tag immagine principale &lt;div> per abilitare/disabilitare questa funzione. Il componente Proxy sostituisce il componente principale con le seguenti modifiche.

* Rimuove il ***data-asset-id*** dal div padre di un elemento &lt;img> all’interno di image.html
* Aggiunge ***data-aem-asset-id*** direttamente all&#39;elemento &lt;img> all&#39;interno di image.html
* Aggiunge il valore ***data-trackable=&#39;true&#39;*** all&#39;elemento &lt;img> all&#39;interno di image.html
* ***data-aem-asset-*** idand  ***data-trackable=&#39;true&#39;*** sono mantenuti allo stesso livello di nodo

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* e  *data-trackable=&#39;true&#39;* sono gli attributi chiave che devono essere presenti per le impression di risorse. Per Informazioni approfondite sui clic delle risorse, oltre agli attributi di dati sopra presenti nel tag &lt;img> , il tag principale &lt;a> deve avere un valore href valido.

## Parte 3: Adobe Analytics: creazione di suite di rapporti, abilitazione della raccolta dati in tempo reale e reporting di AEM Assets {#adobe-analytics-asset-insights}

Per il tracciamento delle risorse viene creata una suite di rapporti con raccolta dati in tempo reale. La configurazione di AEM Assets Insights è configurata utilizzando le credenziali di Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
La raccolta dati in tempo reale e la generazione di rapporti sulle risorse AEM devono essere abilitate per la suite di rapporti di Adobe Analytics. L’abilitazione di AEM Asset Reporting riserva le variabili di analisi per il tracciamento delle informazioni sulle risorse.

Per la configurazione di AEM Assets Insights sono necessarie le seguenti credenziali

* Datacenter
* Nome società di Analytics
* Nome utente di Analytics
* Segreto condiviso (può essere ottenuto da *Adobe Analytics > Amministratore > Impostazioni società > Servizio Web*).
* Suite di rapporti (accertati di selezionare la suite di rapporti corretta utilizzata per la generazione di rapporti sulle risorse)

## Parte 4: Utilizzo di Adobe Experience Platform Launch per aggiungere l&#39;estensione Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Aggiunta dell’estensione Adobe Analytics, creazione di regole di caricamento pagina e Integrazione di AEM con Launch con l’account tecnico Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
Assicurati di replicare tutte le modifiche dall&#39;istanza di authoring all&#39;istanza di pubblicazione.

### Articolo 1: Tracciamento pagina (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

Il tracciamento della pagina implementa due chiamate back (registrate nel codice di incorporamento della risorsa)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;/code>** &lt;code>&lt;code>: chiamato quando viene inviato l’evento &quot;load&quot; per l’elemento asset-DOM-element.&lt;/code>&lt;/code>
* **\&lt;code>assetAnalytics.core.assetClic\&lt;/code>** &lt;code>&lt;code>: chiamato quando l’evento &quot;click&quot; viene inviato per l’elemento asset-DOM-element, è pertinente solo quando l’elemento asset-DOM-element ha un tag di ancoraggio come elemento principale con un attributo &quot;href&quot; esterno valido&lt;/code>&lt;/code>

Infine, Pagetracker implementa una funzione di inizializzazione come.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>: chiamato per inizializzare il componente Pagetracker.&lt;/code>&lt;/code> DEVE essere invocato prima che uno qualsiasi degli eventi asset-insights-events (Impression e/o Clic) venga generato dalla pagina web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>: facoltativamente accetta un oggetto AppMeasurement: se fornito, non tenta di creare una nuova istanza di oggetto AppMeasurement.&lt;/code>&lt;/code>

### Articolo 2: Tracciamento immagine — Azione 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### Articolo 2: Tracciamento immagine — Azione 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded() : viene richiamato al termine del caricamento della pagina e attiva le impressioni delle risorse per tutte le immagini tracciabili
* Variabile di Analytics con elenco di risorse caricate : **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClic() : viene richiamato quando l’elemento DOM della risorsa dispone di un tag di ancoraggio con un valore href valido. Quando fai clic su una risorsa, viene creato un cookie con l’ID della risorsa su cui hai fatto clic come valore.**(Nome cookie: a.assets.clickedid)**
* Variabile di Analytics con elenco di risorse caricate : **contextData[&#39;c.a.assets.clickedid&#39;]**
* Origine : **contextData[&#39;c.a.assets.source&#39;]**

### Istruzioni di debug della console {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

Nel video vengono indicate due estensioni del browser Google Chrome come modi per eseguire il debug di Analytics. Estensioni simili sono disponibili anche per altri browser.

* [Avvia l&#39;estensione Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

È anche possibile passare DTM in modalità di debug con la seguente estensione Chrome: [Avvia e switch DTM](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). Questo rende più facile vedere se ci sono errori relativi alla distribuzione DTM. Inoltre, puoi passare manualmente DTM alla modalità di debug tramite qualsiasi browser *strumenti per sviluppatori -> Console JS* aggiungendo il seguente frammento:

## Parte 5 : Verifica dei dati di Insight di tracciamento e sincronizzazione Analytics{#analytics-tracking-asset-insights}

Configurazione dell’utilità di pianificazione dei processi di sincronizzazione di AEM Asset Reporting e del rapporto Approfondimenti risorse

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
