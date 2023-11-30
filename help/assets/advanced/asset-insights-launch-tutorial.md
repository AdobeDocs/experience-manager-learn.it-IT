---
title: Configurare Asset Insights con AEM Assets e Adobe Launch
description: In questa serie di video in cinque parti, esaminiamo l’impostazione e la configurazione di Asset Insights, ad Experience Manager implementato tramite Launch by Adobe.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Assets as a Cloud Service, AEM Assets 6.5" before-title="false"
doc-type: Tutorial
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---

# Configurare Asset Insights con AEM Assets e Adobe Experience Platform Launch

In questa serie video in cinque parti, esaminiamo l’impostazione e la configurazione di Asset Insights per un Experience Manager implementato tramite Adobe Launch.

## Parte 1: Panoramica di Asset Insights {#overview}

Panoramica di Asset Insights. Installa i componenti core, il componente Immagine di esempio e altri pacchetti di contenuti per preparare il tuo ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### Diagramma architettura {#architecture-diagram}

![Diagramma architettura](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Assicurati di scaricare [versione più recente dei Componenti core](https://github.com/adobe/aem-core-wcm-components) per la tua implementazione.

Il video utilizza i Componenti core v2.2.2, che non è la versione più recente; assicurati di utilizzare la versione più recente prima di procedere alla sezione successiva.

* Scarica [Contenuto immagine di esempio per Informazioni su risorse](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Scarica [componenti core WCM dell’AEM più recenti](https://github.com/adobe/aem-core-wcm-components/releases)

## Parte 2: Abilitazione del tracciamento di Informazioni su risorse per il componente Immagine di esempio {#sample-image-component-asset-insights}

Sono stati apportati miglioramenti ai componenti core e all’utilizzo del componente proxy (componente immagine di esempio) per Informazioni su risorse. Modifica dei criteri del modello della pagina di contenuto per abilitare il componente immagine di esempio per il sito di riferimento.

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>Il componente core Immagine include la funzionalità di disabilitazione del tracciamento UUID disabilitando il tracciamento dell’UUID della risorsa (valore di identificatore univoco per un nodo creato in JCR)

Il componente core Immagine utilizza ***data-asset-id*** attributo all&#39;interno dell&#39;elemento padre &lt;div> di un tag immagine per abilitare/disabilitare questa funzione. Il componente proxy sostituisce il componente core con le seguenti modifiche.

* Elimina il ***data-asset-id*** dal div padre di un elemento &lt;img> all’interno del file image.html
* Aggiunte ***data-aem-asset-id*** direttamente all’elemento &lt;img> all’interno di image.html
* Aggiunte ***data-trackable=&#39;true&#39;*** all&#39;elemento &lt;img> all&#39;interno di image.html
* ***data-aem-asset-id*** e ***data-trackable=&#39;true&#39;*** sono mantenuti allo stesso livello di nodo

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* e *data-trackable=&#39;true&#39;* sono gli attributi chiave che devono essere presenti per Asset Impression. Per Informazioni sul clic delle risorse, oltre agli attributi di dati indicati sopra presenti nel tag &lt;img> , il tag principale deve avere un valore href valido.

## Parte 3: Adobe Analytics — Creazione di suite di rapporti, abilitazione della raccolta dati in tempo reale e reporting di AEM Assets {#adobe-analytics-asset-insights}

Viene creata una suite di rapporti con raccolta dati in tempo reale per il tracciamento delle risorse. La configurazione di AEM Assets Insights viene impostata utilizzando le credenziali di Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
>
La raccolta dei dati in tempo reale e il reporting delle risorse AEM devono essere abilitati per la suite di rapporti Adobe Analytics. L’abilitazione di AEM Asset Reporting riserva le variabili di analisi per il tracciamento degli insight sulle risorse.

Per la configurazione di AEM Assets Insights sono necessarie le seguenti credenziali

* Datacenter
* Nome società Analytics
* Nome utente di Analytics
* Segreto condiviso (può essere ottenuto da *Adobe Analytics > Amministratore > Impostazioni società > Servizio Web*).
* Suite di rapporti (assicurati di selezionare la suite di rapporti corretta utilizzata per il reporting delle risorse)

## Parte 4: Utilizzo di Adobe Experience Platform Launch per aggiungere l’estensione Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Aggiunta dell’estensione Adobe Analytics, creazione di regole di caricamento pagina e Integrazione di AEM con Launch con l’account tecnico Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
>
Assicurati di replicare tutte le modifiche dall’istanza di authoring a quella di pubblicazione.

### Regola 1: tracciamento pagina (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

Il tracciamento pagina implementa due callback (registrati in asset-embed-code)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** : chiamato quando viene inviato l’evento &quot;load&quot; per l’elemento asset-DOM.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** : chiamato quando viene inviato l’evento &quot;click&quot; per l’elemento asset-DOM-element questo è rilevante solo se l’elemento asset-DOM-element ha un tag di ancoraggio come elemento principale con un attributo &quot;href&quot; esterno valido

Infine, il tracciamento delle pagine implementa una funzione di inizializzazione come.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : chiamato per inizializzare il componente di tracciamento pagina. Questo DEVE essere richiamato prima che uno qualsiasi degli eventi di approfondimento delle risorse (impressioni e/o clic) venga generato dalla pagina web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : accetta facoltativamente un oggetto AppMeasurement; se fornito, non tenta di creare un’istanza dell’oggetto AppMeasurement.

### Regola 2: Tracciamento immagini — Azione 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### Regola 2: Tracciamento immagini — Azione 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() : viene richiamato al completamento del caricamento della pagina e attiva Impression risorse per tutte le immagini tracciabili
* Variabile di Analytics contenente l’elenco di risorse caricato: **contextData[&quot;c.a.assets.idList&quot;]**
* assetAnalytics.core.assetClicked() : viene richiamato quando l’elemento DOM della risorsa ha un tag di ancoraggio con valore href valido. Quando si fa clic su una risorsa, viene creato un cookie il cui valore è l’ID della risorsa selezionata.**(Nome cookie: a.assets.clickedid)**
* Variabile di Analytics contenente l’elenco di risorse caricato: **contextData[&#39;c.a.assets.clickedid&#39;]**
* Origine : **contextData[&quot;c.a.assets.source&quot;]**

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

Nel video, due estensioni del browser Google Chrome vengono utilizzate come riferimento per il debug di Analytics. Estensioni simili sono disponibili anche per altri browser.

* [Estensione Chrome dello switch di avvio](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

È inoltre possibile passare da DTM alla modalità di debug con la seguente estensione di Chrome: [Launch e switch DTM](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). In questo modo è più facile verificare la presenza di errori relativi all’implementazione di DTM. Inoltre, puoi passare manualmente da DTM alla modalità di debug tramite qualsiasi browser *strumento per sviluppatori -> Console JS* aggiungendo il seguente frammento:

## Parte 5: Verifica del tracciamento analitico e sincronizzazione dei dati di approfondimento{#analytics-tracking-asset-insights}

Configurazione del report AEM Asset Reporting Sync Job Scheduler e del report Assets Insights

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
