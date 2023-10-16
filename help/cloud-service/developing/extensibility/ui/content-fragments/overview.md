---
title: Estensioni dei frammenti di contenuto AEM
description: Scopri come generare e distribuire le estensioni di Frammenti di contenuto as a Cloud Service AEM
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 1%

---

# Estensibilità dei frammenti di contenuto AEM

L’interfaccia utente Frammenti di contenuto dell’AEM è una potente interfaccia utente estensibile per la gestione della creazione, della gestione e della modifica di Frammenti di contenuto. Sono disponibili diversi punti di estensione per personalizzare l’interfaccia utente in base alle tue esigenze. Sono disponibili diversi punti di estensione in base all’interfaccia utente che stai estendendo.

## Punti di estensione della console Frammenti di contenuto

La Console Frammenti di contenuto nell’AEM (Adobe Experience Manager) è un’interfaccia utente che fornisce una posizione centralizzata per la gestione e l’organizzazione dei frammenti di contenuto. Offre un set completo di strumenti e funzioni per creare, modificare, pubblicare e tenere traccia dei frammenti di contenuto, consentendo agli utenti di gestire in modo efficiente contenuti strutturati su vari canali e punti di contatto.

![Console Frammenti di contenuto](./assets/overview/cfc.png)

[Console Frammenti di contenuto AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=it) è l’interfaccia utente estensibile per l’elenco e la gestione dei frammenti di contenuto. [Vengono create le estensioni della console Frammenti di contenuto AEM](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) utilizzando `@adobe/aem-cf-admin-ui-ext-tpl` Modello App Builder.

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
              <p class="is-size-6">Personalizza le azioni per quando sono selezionati uno o più frammenti di contenuto.</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizzare la documentazione</span>
              </a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Colonne griglia" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="Colonne griglia">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Colonne griglia" target="_blank" rel="referrer">Colonne griglia</a></p>
          <p class="is-size-6">Personalizza i dati visualizzati nell’elenco Frammenti di contenuto.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizzare la documentazione</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu intestazione" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="Menu intestazione">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu intestazione" target="_blank" rel="referrer">Menu intestazione</a></p>
          <p class="is-size-6">Personalizza le azioni per quando non è selezionato alcun frammento di contenuto.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizzare la documentazione</span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Punti di estensione dell’Editor frammenti di contenuto

L’Editor frammenti di contenuto nell’AEM (Adobe Experience Manager) è un componente dell’interfaccia utente che consente agli utenti di creare, modificare e gestire frammenti di contenuto. Offre un ambiente visivamente intuitivo e facile da usare per lavorare con contenuti strutturati, consentendo agli utenti di definire e organizzare gli elementi di contenuto, applicare modelli, gestire le varianti e visualizzare in anteprima come il contenuto viene visualizzato tra canali diversi. L’Editor frammento di contenuto semplifica il processo di creazione di contenuti riutilizzabili e modulari che possono essere facilmente distribuiti e pubblicati in più esperienze digitali.

![Editor frammenti di contenuto](./assets/overview/cfe.png)

L’editor di frammenti di contenuto AEM è l’interfaccia utente estensibile per la modifica di frammenti di contenuto. [Vengono create le estensioni dell’Editor frammenti di contenuto dell’AEM](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) utilizzando `@adobe/aem-cf-editor-ui-ext-tpl` Modello App Builder.

Sono disponibili i seguenti punti di estensione dell’Editor frammenti di contenuto:

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" title="Menu intestazione" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="Menu intestazione">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="Menu intestazione" target="_blank" rel="referrer">Menu intestazione</a></p>
            <p class="is-size-6">Personalizza le azioni nel menu di intestazione dell’Editor frammento di contenuto.</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizzare la documentazione</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra degli strumenti dell’Editor Rich Text" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="Barra degli strumenti dell’Editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra degli strumenti dell’Editor Rich Text"  target="_blank" rel="referrer">Barra degli strumenti dell’Editor Rich Text</a></p>
          <p class="is-size-6">Pulsante per aggiungere un elemento personalizzato all’editor Rich Text dell’editor di frammenti di contenuto.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizzare la documentazione</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widget editor Rich Text" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="Widget editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widget editor Rich Text" target="_blank" rel="referrer">Widget editor Rich Text</a></p>
          <p class="is-size-6">Personalizzare le azioni nell'editor Rich Text associate alla sequenza di tasti.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizzare la documentazione</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Badge editor Rich Text" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Badge editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Badge editor Rich Text" target="_blank" rel="referrer">Badge editor Rich Text</a></p>
          <p class="is-size-6">Personalizza i blocchi con stili non modificabili all’interno dell’editor Rich Text.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizzare la documentazione</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## Esempi di estensione

Ti diamo il benvenuto in una raccolta di esempi di codice di estensibilità dell’interfaccia utente AEM. Questa risorsa è progettata per fornire dimostrazioni pratiche e informazioni approfondite sull’estensione dell’interfaccia utente di Adobe Experience Manager (AEM). Che tu sia uno sviluppatore interessato a migliorare la funzionalità dell’AEM, questi esempi di codice fungono da riferimento prezioso.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Aggiornamento proprietà in blocco" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Aggiornamento proprietà in blocco">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Aggiornamento proprietà in blocco">Aggiornamento proprietà frammento di contenuto in blocco</a></p>
          <p class="is-size-6">Estensione della barra delle azioni della console Frammenti di contenuto con azione modale e Adobe I/O Runtime.</p>
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
                    <p class="is-size-6">Esplora un esempio di estensione della barra delle azioni che genera un’immagine utilizzando OpenAI, la carica sull’AEM e aggiorna la proprietà dell’immagine nel frammento di contenuto selezionato.</p>
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
          <p class="is-size-6">Aggiungi una colonna personalizzata alla console Frammenti di contenuto.</p>
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
          <a href="./examples/editor-export-to-xml.md" title="Esporta in XML" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="Esporta in XML">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="Esporta in XML">Esporta in XML</a></p>
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
          <a href="./examples/editor-rte-toolbar.md" title="Pulsante della barra degli strumenti dell’Editor Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="Pulsante della barra degli strumenti dell’Editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Pulsante della barra degli strumenti dell’Editor Rich Text">Pulsante della barra degli strumenti dell’Editor Rich Text</a></p>
          <p class="is-size-6">Aggiungere pulsanti della barra degli strumenti personalizzati ai campi dell’Editor Rich Text nell’Editor frammento di contenuto.</p>
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
          <a href="./examples/editor-rte-widget.md" title="Widget editor Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="Widget editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Widget editor Rich Text">Widget editor Rich Text</a></p>
          <p class="is-size-6">Aggiungere widget all’Editor Rich Text nell’Editor frammento di contenuto.</p>
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
          <a href="./examples/editor-rte-badges.md" title="Badge editor Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="Badge editor Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="Badge editor Rich Text">Badge editor Rich Text</a></p>
          <p class="is-size-6">Aggiungi i badge all’Editor Rich Text nell’Editor frammento di contenuto.</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza l’esempio</span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
