---
title: Tutorial per sviluppatori di Edge Delivery Services e editor universale
description: Scopri le nozioni di base per lo sviluppo di un nuovo sito web creato con l’editor universale di AEM e distribuito tramite Edge Delivery Services.
short-description: Un tutorial per sviluppatori per la creazione di siti web AEM che combinano funzionalità avanzate di authoring con Universal Editor e una distribuzione estremamente rapida tramite Edge Delivery Services. Scopri come creare un nuovo progetto, configurare un ambiente di sviluppo locale e creare blocchi personalizzati.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Catalog
jira: KT-15832
duration: 88
exl-id: aeac08a2-75a0-4adb-b32e-0e7f85e7eb1d
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '658'
ht-degree: 94%

---

# Tutorial per sviluppatori di Edge Delivery Services e editor universale

![Tutorial per sviluppatori di Edge Delivery Services e editor universale](./assets/0-overview/hero.png)

In questo tutorial imparerai le nozioni di base per la creazione di un sito web AEM che combina funzionalità di authoring avanzate con l’editor universale e una distribuzione estremamente rapida tramite Edge Delivery Services. Al termine, avrai acquisito le nozioni di base per creare un nuovo progetto, configurare un ambiente di sviluppo locale e creare un nuovo blocco.

## Configurazione del progetto

Scopri come creare un progetto di codice e configurare un nuovo sito in AEM as a Cloud Service. Questa configurazione consente uno sviluppo diretto con l’editor universale per la creazione di contenuti e la distribuzione rapida dei contenuti tramite Edge Delivery Services.

<!-- CARDS 

* ./1-new-code-project.md
* ./2-new-aem-site.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a code project">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./1-new-code-project.md" title="Creare un progetto di codice" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/1-new-project/new-project.png" alt="Creare un progetto di codice"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./1-new-code-project.md" target="_blank" rel="referrer" title="Creare un progetto di codice">Creare un progetto di codice</a>
                    </p>
                    <p class="is-size-6">Crea un progetto di codice per Edge Delivery Services, modificabile tramite l’editor universale.</p>
                </div>
                <a href="./1-new-code-project.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create an AEM site">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./2-new-aem-site.md" title="Creare un sito AEM" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/2-new-aem-site/new-site.png" alt="Creare un sito AEM"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./2-new-aem-site.md" target="_blank" rel="referrer" title="Creare un sito AEM">Creare un sito AEM</a>
                    </p>
                    <p class="is-size-6">Crea un sito in AEM Sites per Edge Delivery Services, modificabile tramite l’editor universale.</p>
                </div>
                <a href="./2-new-aem-site.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Configurazione di sviluppo

Scopri come configurare l’ambiente di sviluppo locale per consentire lo sviluppo rapido di siti web. Questa configurazione consente una creazione diretta dei siti con l’editor universale e una distribuzione efficiente dei contenuti tramite Edge Delivery Services, garantendo un flusso di lavoro di sviluppo fluido e ottimizzato.
<!-- CARDS 

* ./3-local-development-environment.md
* ./4-website-branding.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up a local development environment">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./3-local-development-environment.md" title="Configurare un ambiente di sviluppo locale" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3443978/?format=jpeg&nocache=1753811862249" alt="Configurare un ambiente di sviluppo locale"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./3-local-development-environment.md" target="_blank" rel="referrer" title="Configurare un ambiente di sviluppo locale">Configurare un ambiente di sviluppo locale</a>
                    </p>
                    <p class="is-size-6">Configurare un ambiente di sviluppo locale per i siti da distribuire tramite Edge Delivery Services e modificabili con l’editor universale.</p>
                </div>
                <a href="./3-local-development-environment.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Add website branding">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./4-website-branding.md" title="Aggiungere il branding al sito web" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/4-website-branding/github-issues.png" alt="Aggiungere il branding al sito web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./4-website-branding.md" target="_blank" rel="referrer" title="Aggiungere il branding al sito web">Aggiungere il branding al sito web</a>
                    </p>
                    <p class="is-size-6">Definisci i CSS globali, le variabili CSS e i font web per un sito Edge Delivery Services.</p>
                </div>
                <a href="./4-website-branding.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Sviluppo di un blocco

Scopri come creare un nuovo blocco definendone il modello di contenuto e configurando contenuti di esempio per il test e lo sviluppo. Esplora due metodi per il rendering del blocco e scopri come strutturarlo per ottenere prestazioni e flessibilità ottimali in AEM e Edge Delivery Services.

<!-- CARDS 

* ./5-new-block.md {image = ./assets/5-new-block/card.png}
* ./6-author-block.md {image = ./assets/6-author-block/card.png}
* ./7a-block-css.md {image = ./assets/7a-block-css/card.png}
* ./7b-block-js-css.md {image = ./assets/7b-block-js-css/card.png}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./5-new-block.md" title="Creare un blocco" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/5-new-block/card.png" alt="Creare un blocco"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./5-new-block.md" target="_blank" rel="referrer" title="Creare un blocco">Creare un blocco</a>
                    </p>
                    <p class="is-size-6">Crea un blocco per un sito web Edge Delivery Services modificabile con l’editor universale.</p>
                </div>
                <a href="./5-new-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Author a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./6-author-block.md" title="Authoring di un blocco" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/6-author-block/card.png" alt="Authoring di un blocco"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./6-author-block.md" target="_blank" rel="referrer" title="Authoring di un blocco">Authoring di un blocco</a>
                    </p>
                    <p class="is-size-6">Authoring di un blocco Edge Delivery Services con l’editor universale.</p>
                </div>
                <a href="./6-author-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7a-block-css.md" title="Sviluppare un blocco con CSS" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7a-block-css/card.png" alt="Sviluppare un blocco con CSS"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7a-block-css.md" target="_blank" rel="referrer" title="Sviluppare un blocco con CSS">Sviluppare un blocco con CSS</a>
                    </p>
                    <p class="is-size-6">Sviluppa un blocco con CSS per Edge Delivery Services, modificabile tramite l’Editor universale.</p>
                </div>
                <a href="./7a-block-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS and JS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7b-block-js-css.md" title="Sviluppare un blocco con CSS e JS" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7b-block-js-css/card.png" alt="Sviluppare un blocco con CSS e JS"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7b-block-js-css.md" target="_blank" rel="referrer" title="Sviluppare un blocco con CSS e JS">Sviluppare un blocco con CSS e JS</a>
                    </p>
                    <p class="is-size-6">Sviluppa un blocco con CSS e JavaScript per Edge Delivery Services, modificabile tramite l’editor universale.</p>
                </div>
                <a href="./7b-block-js-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Passaggi successivi

Dopo aver completato questo tutorial, metti in pratica ciò che hai imparato con queste procedure mirate. Queste guide approfondiscono il codice e i concetti trattati, esplorando casi d’uso specifici per singoli ruoli, tecniche avanzate e suggerimenti aggiuntivi per migliorare le competenze di sviluppo di Edge Delivery Services e dell’editor universale.

<!-- CARDS 

* ./how-to/block-options.md
* ./how-to/header-and-footer.md
* ./how-to/local-extension-preview.md {image = ./how-to/assets/local-extension-preview/extension-loaded.png}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Block options">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/block-options.md" title="Opzioni di blocco" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/block-options/main.png" alt="Opzioni di blocco"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/block-options.md" target="_blank" rel="referrer" title="Opzioni di blocco">Opzioni del blocco</a>
                    </p>
                    <p class="is-size-6">Scopri come creare un blocco con più opzioni di visualizzazione.</p>
                </div>
                <a href="./how-to/block-options.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header and Footer">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/header-and-footer.md" title="Intestazione e piè di pagina" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/header-and-footer/hero.png" alt="Intestazione e piè di pagina"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" title="Intestazione e piè di pagina">Intestazione e piè di pagina</a>
                    </p>
                    <p class="is-size-6">Scopri come intestazione e piè di pagina vengono sviluppati in Edge Delivery Services e nell’editor universale.</p>
                </div>
                <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Preview a Universal Editor extension">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/local-extension-preview.md" title="Visualizzare in anteprima un’estensione per l’editor universale" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./how-to/assets/local-extension-preview/extension-loaded.png" alt="Visualizzare in anteprima un’estensione per l’editor universale"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/local-extension-preview.md" target="_blank" rel="referrer" title="Visualizzare in anteprima un’estensione per l’editor universale">Visualizzare in anteprima un’estensione per l’editor universale</a>
                    </p>
                    <p class="is-size-6">Scopri come visualizzare in anteprima un’estensione per l’editor universale in esecuzione locale durante lo sviluppo.</p>
                </div>
                <a href="./how-to/local-extension-preview.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
