---
title: Video e tutorial su AEM Sites
description: Sfoglia video e tutorial sulle funzioni e funzionalità di Adobe Experience Manager Sites. AEM Sites è una piattaforma leader per la gestione delle esperienze.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 14ca2ba3d5b6c116e3fa8b437aa9ed90375ae468
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 38%

---

# Video e tutorial su AEM Sites {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites è la piattaforma di gestione delle esperienze di Adobe che consente di creare, gestire e distribuire esperienze digitali tramite siti web, app mobili o qualsiasi altro canale digitale.

## Tre modi per fornire esperienze con AEM Sites

AEM Sites offre tre modi per creare, eseguire l’authoring e distribuire esperienze. Sia che si tratti della creazione di siti web, dell’ottimizzazione delle prestazioni edge o dell’attivazione di app headless, AEM Sites offre opzioni flessibili per soddisfare le esigenze del progetto:

1. Le **esperienze Edge Delivery Services** utilizzano l&#39;Edge Network di Adobe per distribuire contenuti ad alta velocità e a bassa latenza. Il servizio ottimizza automaticamente i contenuti per il dispositivo che consuma, i motori di ricerca e gli agenti GenAI. Gli autori creano contenuti utilizzando Adobe Universal Editor o l’authoring basato su documenti.
1. **Headless/API-first** le esperienze utilizzano AEM Publish per distribuire contenuti come JSON su API HTTP per app mobili, applicazioni a pagina singola (SPA) o altri client headless. Gli autori creano contenuti utilizzando l’Editor frammento di contenuto o l’Editor universale.
1. **Le esperienze AEM** tradizionali utilizzano AEM Publish per distribuire contenuti come pagine Web HTML. Gli autori creano contenuti utilizzando Editor pagina di AEM Author. Questa opzione è consigliata per i progetti esistenti o per i progetti già migrati.

Tutte e tre le opzioni sono approcci efficaci e la scelta migliore dipende dal caso d’uso e dalle esigenze organizzative. Ogni approccio consente ai team di fornire esperienze personalizzate e coinvolgenti in modo rapido e scalabile su qualsiasi canale o dispositivo.

>[!IMPORTANT]
>
> **Edge Delivery Services** è il metodo più recente e avanzato per distribuire siti Web con AEM. Combina la velocità e la scalabilità dell’Edge Network di Adobe con le opzioni di authoring moderne. Sebbene Edge Delivery Services sia consigliato per i nuovi progetti, AEM Sites continua a supportare gli approcci headless e tradizionali, in modo da poter scegliere il percorso più adatto alle tue esigenze.

Il diagramma seguente illustra le diverse opzioni per la creazione di esperienze con AEM Sites:

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### Confrontare i metodi di creazione con AEM Sites

La tabella seguente fornisce un confronto di alto livello dei tre percorsi. Si incentra sui dettagli di authoring dei contenuti e distribuzione delle esperienze di ciascun percorso.

|            | Edge Delivery Services | Headless/API-First | AEM tradizionale |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **Ottimo per** | Siti web con elevate esigenze di traffico, prestazioni e scalabilità | App mobili, SPA e altre applicazioni headless | Progetti esistenti o progetti migrati |
| **Strumenti di creazione** | Authoring basato su documenti, Editor universale, Editor pagina | Frammenti di contenuto, editor universale | Editor pagina, Editor universale |
| **Archivio contenuti creato** | Documenti o Authoring AEM (JCR) | Authoring AEM (JCR) | Authoring AEM (JCR) |
| **Consegna** | Edge Delivery Services | Pubblicazione AEM (con Adobe CDN e Dispatcher) | Pubblicazione AEM (con Adobe CDN e Dispatcher) |
| **Archivio contenuti di consegna** | Edge Delivery Services | Pubblicazione AEM (JCR) | Pubblicazione AEM (JCR) |
| **Formato di consegna** | HTML | JSON | HTML |
| **Tecnologia di sviluppo** | JavaScript, CSS | Qualsiasi (ad es. Swift, React, ecc.) | Java™, HTL, JavaScript, CSS |
| **Supporto di bot di ricerca e agenti GenAI** | Ottimizzato per bot, motori di ricerca e agenti GenAI | Funziona per bot e agenti, ma potrebbe richiedere SSR o configurazione aggiuntiva | Adatto per i bot, ma le prestazioni potrebbero essere più lente rispetto a Edge Delivery Services |

## Migrazione da AMS o On-Premise

Se esegui la migrazione da AMS o on-premise (OTP) ad AEM as a Cloud Service, Adobe ti incoraggia a valutare la possibilità di passare direttamente a Edge Delivery Services. In genere, l’impegno non è maggiore della migrazione ad AEM as a Cloud Service Publish, ma allo stesso tempo fornisce prestazioni più veloci e maggiore scalabilità. Se decidi che Edge Delivery Services non è la scelta giusta per te in questo momento, o se gli altri approcci soddisfano meglio le tue esigenze, rimangono opzioni completamente supportate e valide per il tuo progetto.

## Tutorial

Esplora i tre approcci per la creazione di con AEM Sites in modo più dettagliato. I tutorial seguenti spiegano come funziona ogni opzione, gli strumenti coinvolti e quando utilizzarli.

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with Edge Delivery Services.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
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
                    <p class="is-size-6">Esplora Edge Delivery Services con guide complete. Le guide di Build, Publish e Launch descrivono tutto il necessario per iniziare a utilizzare Edge Delivery Services.</p>
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
                    <p class="is-size-6">Scopri come creare applicazioni headless basate su contenuti AEM. I tutorial riguardano framework come iOS, Android e React: scegli ciò che si adatta al tuo stack.</p>
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
                    <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="AEM tradizionale - Tutorial WKND" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="AEM tradizionale - Tutorial WKND"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="AEM tradizionale - Tutorial WKND">AEM tradizionale - Tutorial WKND</a>
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
