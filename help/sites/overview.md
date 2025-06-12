---
title: Video e tutorial su AEM Sites
description: Sfoglia video e tutorial sulle funzioni e funzionalità di Adobe Experience Manager Sites. AEM Sites è una piattaforma leader per la gestione delle esperienze.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 36917be459162e5399620c976bfe953cc5553c82
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 62%

---

# Video e tutorial su AEM Sites {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites è una piattaforma leader per la gestione delle esperienze. Questa guida utente contiene video e tutorial sulle numerose funzioni e funzionalità di Aem Sites.

## Tre modi per creare con AEM Sites

AEM Sites offre tre modi per creare, eseguire l’authoring e distribuire esperienze. Sia per la creazione di pagine, l’ottimizzazione delle performance Edge o il potenziamento di app headless, AEM Sites offre opzioni flessibili per rispondere alle esigenze del tuo progetto:

1. I siti Web di **Edge Delivery Services** utilizzano l&#39;authoring basato su documenti o Adobe Universal Editor per l&#39;authoring dei contenuti, che viene quindi attivato e quindi consegnato agli utenti finali da Edge Delivery Services come pagine Web di HTML. Questa opzione è principalmente per _progetti nuovi ed esistenti_ che richiedono prestazioni, scalabilità e velocità elevate.
1. **Headless/API-first** le esperienze web utilizzano l&#39;Editor frammento di contenuto o l&#39;Editor universale per creare contenuti, che viene quindi attivato e distribuito da AEM Publish come JSON. Questa opzione è principalmente per _progetti nuovi ed esistenti_ che richiedono la distribuzione headless di contenuti ad app mobili, applicazioni a pagina singola o altre applicazioni headless.
1. **Il tradizionale AEM** non è l&#39;approccio più attuale per la generazione di esperienze web con AEM Sites. La versione tradizionale di AEM utilizza l’Editor pagina di AEM Author per l’authoring dei contenuti, che vengono quindi attivati e consegnati agli utenti finali dalle pagine web di AEM Publish as HTML. AEM tradizionale è consigliato per _progetti esistenti_.

Queste opzioni sono progettate per soddisfare le diverse esigenze delle organizzazioni di marketing, per offrire esperienze personalizzate e coinvolgenti ad alta velocità e su larga scala su qualsiasi canale o dispositivo.

>[!IMPORTANT]
>
> **Edge Delivery Services** è il metodo più recente per la generazione con AEM Sites. È progettato per offrire siti web ad alte prestazioni su larga scala, sfruttando la potenza di Edge Network di Adobe.

Il diagramma seguente illustra i diversi percorsi:

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### Confrontare i metodi di creazione con AEM Sites

La tabella seguente fornisce un confronto di alto livello dei tre percorsi. Si incentra sui dettagli di authoring dei contenuti e distribuzione delle esperienze di ciascun percorso.

|            | Edge Delivery Services | Headless/API-First | AEM tradizionale |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **Ottimo Per** | Siti Web con traffico elevato, prestazioni e scalabilità richieste | App mobili, SPA e altre applicazioni headless | Progetti esistenti (approccio non più attuale) |
| **Strumenti di authoring** | Authoring basato su documenti, editor universale | Frammenti di contenuto, editor universale | Editor pagina |
| **Archivio contenuti creato** | Documenti o Authoring AEM (JCR) | Authoring AEM (JCR) | Authoring AEM (JCR) |
| **Consegna** | Edge Delivery Services | Pubblicazione AEM (con Adobe CDN e Dispatcher) | Pubblicazione AEM (con Adobe CDN e Dispatcher) |
| **Archivio contenuti della consegna** | Edge Delivery Services | Pubblicazione AEM (JCR) | Pubblicazione AEM (JCR) |
| **Formato di consegna** | HTML | JSON | HTML |
| **Tecnologia di sviluppo** | JavaScript, CSS | Qualsiasi (ad es. Swift, React, ecc.) | Java™, JavaScript, CSS |
| **Fase di implementazione** | Progetti nuovi ed esistenti | Progetti nuovi ed esistenti | Solo progetti esistenti |

## Tutorial

Scopri ognuno dei tre percorsi con cui creare tramite AEM Sites utilizzando i seguenti tutorial:

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - Guide" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - Guide"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - Guide">Edge Delivery Services - Guide</a>
                    </p>
                    <p class="is-size-6">Esplora Edge Delivery Services con guide complete. Le guide Build, Pubblicazione e Lancio includono tutto il necessario per iniziare a utilizzare EDS.</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API-First - Tutorial" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API-First - Tutorial"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API-First - Tutorial">Headless/API-First - Tutorial</a>
                    </p>
                    <p class="is-size-6">Scopri come creare applicazioni headless basate su contenuti AEM. I tutorial riguardano framework come iOS, Android e React e consentono di scegliere ciò che si adatta al tuo stack.</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="Esercitazione WKND su AEM tradizionale" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="Esercitazione WKND su AEM tradizionale"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="Esercitazione WKND su AEM tradizionale">Esercitazione AEM tradizionale - WKND</a>
                    </p>
                    <p class="is-size-6">Scopri come creare un progetto AEM Sites di esempio utilizzando il tutorial WKND. Questa guida descrive la configurazione del progetto, i componenti core, i modelli modificabili, le librerie lato client e lo sviluppo di componenti.</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Risorse aggiuntive

* [Documentazione sull’authoring di AEM Sites](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [Documentazione sullo sviluppo di AEM Sites](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [Documentazione sull’amministrazione di AEM Sites](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/sites/administering/home)
* [Documentazione sull’implementazione di AEM Sites](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [Tutorial su AEM as a Cloud Service](/help/cloud-service/overview.md)
* [Tutorial su AEM Assets](/help/assets/overview.md)
* [Tutorial su AEM Forms](/help/forms/overview.md)
* [Tutorial su AEM Foundation](/help/foundation/overview.md)
