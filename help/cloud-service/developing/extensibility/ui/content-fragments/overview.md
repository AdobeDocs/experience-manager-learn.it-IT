---
title: Estensioni per frammenti di contenuto di AEM
description: Scopri come generare e distribuire le estensioni per frammenti di contenuto di AEM as a Cloud Service
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '773'
ht-degree: 100%

---

# Estensibilità dei frammenti di contenuto di AEM

L’interfaccia utente della funzione Frammenti di contenuto di AEM è una potente interfaccia utente estensibile per gestire la creazione, la gestione e la modifica di frammenti di contenuto. Diversi punti di estensione consentono di personalizzare l’interfaccia utente in base alle esigenze. I punti di estensione disponibili dipendono dall’interfaccia utente che desideri estendere.

## Punti di estensione della console Frammenti di contenuto

La console Frammenti di contenuto in AEM (Adobe Experience Manager) è un’interfaccia utente che fornisce una posizione centralizzata per la gestione e l’organizzazione dei frammenti di contenuto. Offre un set completo di strumenti e funzioni per creare, modificare, pubblicare e tracciare i frammenti di contenuto, consentendo agli utenti di gestire in modo efficiente contenuti strutturati su vari canali e punti di contatto.

![Console Frammenti di contenuto](./assets/overview/cfc.png)

La [console Frammenti di contenuto di AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=it) è l’interfaccia utente estensibile dal punto di vista di elenchi e gestione dei frammenti di contenuto. Le [estensioni per la console Frammenti di contenuto di AEM sono create](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) utilizzando il modello di App Builder `@adobe/aem-cf-admin-ui-ext-tpl`.

Sono disponibili i seguenti punti di estensione della console Frammenti di contenuto:

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra delle azioni" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="Barra delle azioni">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra delle azioni" target="_blank" rel="referrer">Barra delle azioni</a></p>
              <p class="is-size-6">Personalizza le azioni disponibili quando sono selezionati uno o più frammenti di contenuto.</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Consulta la documentazione</span>
              </a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Colonne della griglia" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="Colonne della griglia">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Colonne della griglia" target="_blank" rel="referrer">Colonne della griglia</a></p>
          <p class="is-size-6">Personalizza i dati che vengono visualizzati nell’elenco Frammenti di contenuto.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Consulta la documentazione</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu di intestazione" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="Menu di intestazione">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu di intestazione" target="_blank" rel="referrer">Menu di intestazione</a></p>
          <p class="is-size-6">Personalizza le azioni disponibili quando non è selezionato alcun frammento di contenuto.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Consulta la documentazione</span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Punti di estensione dell’editor di frammenti di contenuto

L’editor di frammenti di contenuto in AEM (Adobe Experience Manager) è un componente dell’interfaccia utente che consente agli utenti di creare, modificare e gestire i frammenti di contenuto. Offre un ambiente visivo, intuitivo e facile da usare per utilizzare contenuti strutturati, consentendo agli utenti di definire e organizzare gli elementi di contenuto, applicare modelli, gestire le varianti e visualizzare in anteprima come si presenta il contenuto sui diversi canali. L’editor di frammenti di contenuto semplifica il processo di creazione di contenuti riutilizzabili e modulari che possono essere facilmente distribuiti e pubblicati in più esperienze digitali.

![Editor di frammenti di contenuto](./assets/overview/cfe.png)

L’editor di frammenti di contenuto di AEM è l’interfaccia utente estensibile per la modifica di frammenti di contenuto. Le [estensioni dell’editor di frammenti di contenuto di AEM sono create](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) utilizzando il modello di App Builder `@adobe/aem-cf-editor-ui-ext-tpl`.

Sono disponibili i seguenti punti di estensione dell’Editor frammenti di contenuto:

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="Menu di intestazione" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="Menu di intestazione">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="Menu di intestazione" target="_blank" rel="referrer">Menu di intestazione</a></p>
            <p class="is-size-6">Personalizzare le azioni nel menu di intestazione dell’Editor frammento di contenuto.</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza i documenti</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra degli strumenti dell’editor Rich Text" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="Barra degli strumenti dell’editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra degli strumenti dell’editor Rich Text"  target="_blank" rel="referrer">Barra degli strumenti dell’editor Rich Text</a></p>
          <p class="is-size-6">Aggiungere un pulsante personalizzato all’editor Rich Text (RTE) dell’editor di frammento di contenuto.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza i documenti</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widget dell’editor Rich Text" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="Widget dell’editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widget dell’editor Rich Text" target="_blank" rel="referrer">Widget dell’editor Rich Text</a></p>
          <p class="is-size-6">Personalizzare le azioni nell’editor Rich Text associate ai tasti.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza i documenti</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Badge dell’editor Rich Text (RTE)" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Badge dell’editor Rich Text (RTE)">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Badge dell’editor Rich Text (RTE)" target="_blank" rel="referrer">Badge dell’editor Rich Text (RTE)</a></p>
          <p class="is-size-6">Personalizzare i blocchi con stili non modificabili all’interno dell’editor Rich Text.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza i documenti</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## Esempi di estensione

Ti diamo il benvenuto in una raccolta di esempi di codice di estensibilità dell’interfaccia utente di AEM. Questa risorsa è progettata per fornire dimostrazioni pratiche e approfondimenti sull’estensione dell’interfaccia utente di Adobe Experience Manager (AEM). In qualità di sviluppatore, se ti interessa migliorare le funzionalità di AEM, questi esempi di codice fungono da riferimento prezioso.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Aggiornamento delle proprietà in blocco" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Aggiornamento delle proprietà in blocco">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Aggiornamento delle proprietà in blocco">Aggiornamento della proprietà frammento di contenuto in blocco</a></p>
          <p class="is-size-6">Estensione della barra delle azioni della console Frammento di contenuto con azione modale e Adobe I/O Runtime.</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="Generazione di immagini basate su OpenAI e caricamento nell’estensione AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="Generazione di immagini basate su OpenAI e caricamento nell’estensione AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="Generazione di immagini basate su OpenAI e caricamento nell’estensione AEM">Generazione di immagini OpenAPI</a></p>
                    <p class="is-size-6">Esplora un’estensione della barra delle azioni di esempio che genera un’immagine utilizzando OpenAI, la carica su AEM e aggiorna la proprietà dell’immagine sul frammento di contenuto selezionato.</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
                    </a>
                </div>
            </div>
        </div>
    </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/custom-grid-columns.md" title="Colonne personalizzate" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/custom-grid-columns/card.png" alt="Colonne personalizzate">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/custom-grid-columns.md" title="Colonne personalizzate">Colonne personalizzate</a></p>
          <p class="is-size-6">Aggiungere una colonna personalizzata alla console Frammento di contenuto.</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Export to XML">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-export-to-xml.md" title="Esportare in XML" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="Esportare in XML">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="Esportare in XML">Esportare in XML</a></p>
          <p class="is-size-6">Esportare un frammento di contenuto come XML dall’Editor frammento di contenuto.</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="Pulsante della barra degli strumenti dell’editor Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="Pulsante della barra degli strumenti dell’editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Pulsante della barra degli strumenti dell’editor Rich Text">Pulsante della barra degli strumenti dell’editor Rich Text</a></p>
          <p class="is-size-6">Aggiungere pulsanti della barra degli strumenti personalizzati ai campi dell’editor Rich Text nell’Editor frammento di contenuto.</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Widget">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-widget.md" title="Widget dell’editor Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="Widget dell’editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Widget dell’editor Rich Text">Widget dell’editor Rich Text</a></p>
          <p class="is-size-6">Aggiungere i widget all’Editor Rich Text nell’Editor frammento di contenuto.</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="Badge dell’editor Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="Badge dell’editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="Badge dell’editor Rich Text">Badge dell’editor Rich Text</a></p>
          <p class="is-size-6">Aggiungere i badge all’Editor Rich Text nell’Editor frammento di contenuto.</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom fields">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-custom-field.md" title="Campi personalizzati" tabindex="-1">
            <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3427585?format=jpeg" alt="Campi personalizzati">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-custom-field.md" title="Campi personalizzati">Campi personalizzati</a></p>
          <p class="is-size-6">Creare campi di frammento di contenuto personalizzati.</p>
          <a href="./examples/editor-custom-field.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
