---
title: Configurare Asset Insights con  AEM Assets e lancio  Adobe
description: In questa serie video di 5 parti, passiamo alla configurazione e configurazione di Asset Insights per  Experience Manager implementato tramite Launch by Adobe.
contentOwner: selvaraj
feature: asset-insights
topics: integrations, development, metadata
audience: developer, architect, administrator
doc-type: article
activity: implement
version: 6.3, 6.4, 6.5
redirect-form: https://docs.adobe.com/content/help/en/experience-manager-learn/assets/analytics/asset-insights-launch-tutorial-setup.html
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---


# Configurare Asset Insights con  AEM Assets e  Adobe Experience Platform Launch

In questa serie video di 5 parti, passiamo alla configurazione e alla configurazione di Asset Insights per  Experience Manager implementato tramite  Adobe Launch.

## Parte 1: Panoramica delle informazioni sulle risorse {#overview}

Panoramica di approfondimenti sulle risorse. Installate i componenti core, i componenti immagine di esempio e altri pacchetti di contenuti per preparare l&#39;ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### Diagramma di architettura {#architecture-diagram}

![Diagramma dell&#39;architettura](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Accertatevi di scaricare la [versione più recente di Core Components](https://github.com/adobe/aem-core-wcm-components) per la vostra implementazione.

Il video utilizza i componenti core v2.2.2 che non sono più la versione più recente; accertatevi di utilizzare la versione più recente prima di passare alla sezione successiva.

* Scarica [Informazioni sulle risorse Contenuto immagine campione](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Scarica [i componenti core di AEM più recenti di WCM](https://github.com/adobe/aem-core-wcm-components/releases)

## Parte 2: Abilitazione del tracciamento delle risorse per il componente immagine di esempio {#sample-image-component-asset-insights}

Miglioramenti ai componenti core e utilizzo del componente proxy (componente Immagine campione) per approfondimenti sulle risorse. Modifica dei criteri dei modelli delle pagine di contenuto per abilitare il componente Immagine di esempio per il sito di riferimento.

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>Il componente Image Core include la capacità di disabilitare il tracciamento UUID disattivando il tracciamento dell’UUID della risorsa (valore identificativo univoco per un nodo creato all’interno di JCR)

Il componente Immagine di base utilizza l&#39;attributo ***data-asset-id*** all&#39;interno dell&#39;elemento padre &lt;div> di un tag immagine per attivare/disattivare questa funzione. Il componente Proxy sostituisce il componente core con le seguenti modifiche.

* Rimuove la ***data-asset-id*** dal div padre di un elemento &lt;img> all&#39;interno dell&#39;immagine.html
* Aggiunge ***data-aem-asset-id*** direttamente all&#39;elemento &lt;img> all&#39;interno dell&#39;immagine.html
* Aggiunge il valore ***data-trackable=&#39;true&#39;*** all&#39;elemento &lt;img> all&#39;interno dell&#39;immagine.html
* ***data-aem-asset-*** idand  ***data-trackable=&#39;true&#39;*** sono mantenuti allo stesso livello di nodo

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* e  *data-trackable=&#39;true&#39;* sono gli attributi chiave che devono essere presenti per Impression risorse. Per Informazioni approfondite sui clic delle risorse, oltre agli attributi di dati sopra presenti nel tag &lt;img>, il tag &lt;a> principale deve avere un valore href valido.

## Parte 3:  Adobe Analytics — Creazione di suite di rapporti, abilitazione della raccolta dati in tempo reale e  AEM Assets Reporting {#adobe-analytics-asset-insights}

Per il tracciamento delle risorse viene creata una suite di rapporti con raccolta dati in tempo reale.  configurazione di AEM Assets Insights viene configurata utilizzando  credenziali Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
È necessario abilitare la raccolta di dati in tempo reale e AEM Report risorse per la suite di rapporti Adobe Analytics . Abilitazione AEM variabili di analisi delle riserve di Asset Reporting per tenere traccia delle informazioni sulle risorse.

Per  configurazione AEM Assets Insights sono necessarie le seguenti credenziali

* Datacenter
* Nome società Analytics
* Nome utente di Analytics
* Segreto condiviso (può essere ottenuto da *Adobe Analytics > Amministratore > Impostazioni società > Servizio Web*).
* Suite di rapporti (accertatevi di selezionare la suite di rapporti corretta utilizzata per i rapporti sulle risorse)

## Parte 4: Utilizzo di  Adobe Experience Platform Launch per aggiungere  estensione Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Aggiunta  Adobe Analytics Extension, creazione di regole di caricamento delle pagine e integrazione AEM con Launch con  account tecnico IMS Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
Assicuratevi di replicare tutte le modifiche dall’istanza di creazione all’istanza di pubblicazione.

### Regola 1: Page Tracker (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

Il tracciatore di pagina implementa due chiamate (registrate nel codice asset-embed)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;/code>** &lt;code>&lt;code>: chiamato quando viene inviato l&#39;evento &#39;load&#39; per l&#39;elemento asset-DOM.&lt;/code>&lt;/code>
* **\&lt;code>assetAnalytics.core.assetClic\&lt;/code>** &lt;code>&lt;code>: chiamato quando viene inviato l&#39;evento &#39;click&#39; per l&#39;elemento asset-DOM, questo è pertinente solo quando l&#39;elemento asset-DOM ha un tag di ancoraggio come elemento principale con un attributo &#39;href&#39; esterno valido&lt;/code>&lt;/code>

Infine, Pagetracker implementa una funzione di inizializzazione come.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>: chiamato per inizializzare il componente PageStracker.&lt;/code>&lt;/code> Questo comando DEVE essere invocato prima che uno degli eventi asset-insights-events (Impression e/o Clic) venga generato dalla pagina Web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>: accetta facoltativamente un oggetto AppMeasurement — se fornito, non tenta di creare una nuova istanza di oggetto AppMeasurement.&lt;/code>&lt;/code>

### Articolo 2: Tracker immagini — Azione 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### Articolo 2: Tracker immagini — Azione 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() : viene richiamato al termine del caricamento della pagina e attiva le impression delle risorse per tutte le immagini tracciabili
* Variabile di Analytics che include l&#39;elenco di risorse caricate: **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClick() : viene richiamato quando l&#39;elemento DOM della risorsa dispone di un tag di ancoraggio con un valore href valido. Quando un utente fa clic su una risorsa, viene creato un cookie con l’ID della risorsa su cui è stato fatto clic come valore.**(Nome cookie: a.assets.clickdid)**
* Variabile di Analytics che include l&#39;elenco di risorse caricate: **contextData[&#39;c.a.assets.clickedid&#39;]**
* Origine: **contextData[&#39;c.a.assets.source&#39;]**

### Console istruzioni di debug {#console-debug-statements}

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

Due estensioni del browser Google Chrome sono citati nel video come modi per eseguire il debug di Analytics. Estensioni simili sono disponibili anche per altri browser.

* [Avvia estensione Chrome Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

È anche possibile passare DTM in modalità debug con la seguente estensione Chrome: [Lancio e DTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). In questo modo è più semplice verificare se sono presenti errori relativi alla distribuzione di Gestione dinamica dei tag. Inoltre, puoi passare manualmente da DTM alla modalità di debug tramite qualsiasi browser *strumenti di sviluppo -> Console JS* aggiungendo il seguente snippet:

## Parte 5 : Verifica dei dati di monitoraggio e sincronizzazione analitici{#analytics-tracking-asset-insights}

Configurazione AEM rapporto sulle risorse Programma di sincronizzazione dei processi e Report sulle risorse

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
