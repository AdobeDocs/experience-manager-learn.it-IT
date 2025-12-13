---
title: Memorizzazione in cache di AEM as a Cloud Service
description: Panoramica generale sulla memorizzazione in cache di AEM as a Cloud Service
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 100%

---

# Memorizzazione in cache di AEM as a Cloud Service

In AEM as a Cloud Service, comprendere la memorizzazione in cache è fondamentale. La memorizzazione in cache comporta l’archiviazione e il riutilizzo dei dati recuperati in precedenza per migliorare l’efficienza del sistema e ridurre i tempi di caricamento. Questo meccanismo accelera in modo significativo la distribuzione dei contenuti, migliora le prestazioni del sito web e ottimizza l’esperienza utente.

AEM as a Cloud Service dispone di più livelli di memorizzazione in cache e strategie che differiscono tra i servizi Author e Publish.

![Panoramica sulla memorizzazione in cache AEM as a Cloud Service](./assets/overview/all.png){align="center"}

## Memorizzazione in cache di AEM

AEM as a Cloud Service dispone di una solida strategia di memorizzazione in cache configurabile a più livelli, che include una rete CDN, AEM Dispatcher e, facoltativamente, una rete CDN gestita dal cliente. La memorizzazione in cache tra i livelli può essere ottimizzata per migliorare le prestazioni, garantendo in tal modo che AEM fornisca solo le esperienze migliori. AEM presenta diversi problemi di memorizzazione in cache per i servizi Author e Publish. Esplora le strategie di memorizzazione in cache per ciascun servizio qui di seguito.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="Servizio di pubblicazione AEM" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Memorizzazione in cache del servizio Publish AEM">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Memorizzazione in cache del servizio Publish AEM">Memorizzazione in cache del servizio Publish AEM</a></p>
            <p class="is-size-6">Il servizio Publish AEM utilizza una rete CDN gestita e AEM Dispatcher per ottimizzare le esperienze web degli utenti finali.</p>
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
                <a href="./author.md" title="Memorizzazione in cache del servizio Author AEM" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Memorizzazione in cache del servizio Author AEM">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Memorizzazione in cache del servizio Author AEM">Memorizzazione in cache del servizio Author AEM</a></p>
                <p class="is-size-6">Il servizio Author di AEM utilizza una rete CDN gestita per fornire esperienze di authoring ottimizzate.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
