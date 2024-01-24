---
title: Memorizzazione in cache as a Cloud Service AEM
description: Panoramica generale del caching as a Cloud Service per AEM.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 71
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# Memorizzazione in cache as a Cloud Service AEM

In AEM as a Cloud Service, comprendere il caching è fondamentale. La memorizzazione nella cache comporta l’archiviazione e il riutilizzo dei dati recuperati in precedenza per migliorare l’efficienza del sistema e ridurre i tempi di caricamento. Questo meccanismo accelera in modo significativo la distribuzione dei contenuti, migliora le prestazioni del sito web e ottimizza l’esperienza utente.

AEM as a Cloud Service ha più livelli di caching e strategie che differiscono tra i servizi di authoring e pubblicazione.

![Panoramica sul caching as a Cloud Service da AEM](./assets/overview/all.png){align="center"}

## Memorizzazione in cache AEM

AEM as a Cloud Service dispone di una solida strategia di caching a più livelli configurabile, che include una rete CDN, un dispatcher AEM e, facoltativamente, una rete CDN gestita dal cliente. La memorizzazione nella cache tra i livelli può essere ottimizzata per ottimizzare le prestazioni, garantendo che l’AEM offra solo le migliori esperienze. AEM ha diversi problemi di caching per i servizi Author e Publish. Esplora le strategie di caching per ciascun servizio di seguito.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="Servizio di pubblicazione AEM" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Memorizzazione in cache del servizio di pubblicazione AEM">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Memorizzazione in cache del servizio di pubblicazione AEM">Memorizzazione in cache del servizio di pubblicazione AEM</a></p>
            <p class="is-size-6">Il servizio di pubblicazione AEM utilizza una rete CDN gestita e un Dispatcher AEM per ottimizzare le esperienze web degli utenti finali.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="Memorizzazione in cache del servizio di authoring AEM" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Memorizzazione in cache del servizio di authoring AEM">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Memorizzazione in cache del servizio di authoring AEM">Memorizzazione in cache del servizio di authoring AEM</a></p>
                <p class="is-size-6">Il servizio di authoring AEM utilizza una rete CDN gestita per fornire esperienze di authoring ottimizzate.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
