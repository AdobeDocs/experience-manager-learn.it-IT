---
title: Panoramica sulla personalizzazione
description: Scopri come personalizzare i siti web di AEM as a Cloud Service utilizzando le applicazioni Adobe Target e Adobe Experience Platform.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 3%

---

# Panoramica sulla personalizzazione

Scopri come AEM as a Cloud Service (AEMCS) si integra con Adobe Target e Adobe Experience Platform (AEP). Scopri come distribuire esperienze personalizzate utilizzando test A/B, indirizzando gli utenti in base al loro comportamento o personalizzando i contenuti utilizzando i profili dei clienti.

## Prerequisiti

Per illustrare vari scenari di personalizzazione, questo tutorial utilizza il progetto [AEM WKND](https://github.com/adobe/aem-guides-wknd/) di esempio. Per seguire, è necessario:

- Un’organizzazione Adobe con accesso a:
   - **Ambiente AEM as a Cloud Service**: per creare e gestire i contenuti
   - **Adobe Target** - per comporre e distribuire esperienze personalizzate
   - **Applicazioni Adobe Experience Platform**: per gestire profili cliente e pubblico
   - **Tag (precedentemente Launch) in AEP**: per distribuire il Web SDK e il JavaScript personalizzato per la raccolta e la personalizzazione dei dati

- Conoscenza di base dei componenti e dei frammenti esperienza di AEM

- Il progetto [AEM WKND](https://github.com/adobe/aem-guides-wknd/) distribuito nel tuo ambiente AEM as a Cloud Service.

## Introduzione

Prima di esplorare casi d’uso specifici, configura AEM as a Cloud Service per la personalizzazione. Inizia integrando Adobe Target e i tag per abilitare la personalizzazione lato client tramite AEP Web SDK. Questi passaggi fondamentali consentono alle pagine AEM di supportare la sperimentazione, il targeting del pubblico e la personalizzazione in tempo reale.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content as Adobe Target offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Integrare Adobe Target" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Integrare Adobe Target"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Integrare Adobe Target">Integrare Adobe Target</a>
                    </p>
                    <p class="is-size-6">Integra AEMCS con Adobe Target per attivare contenuti personalizzati come offerte da Adobe Target.</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrare Target</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="Integrare i tag" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="Integrare i tag"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="Integrare i tag">Integrare Tag</a>
                    </p>
                    <p class="is-size-6">Integra AEMCS con i tag per inserire il Web SDK e il JavaScript personalizzato per la raccolta e la personalizzazione dei dati.</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrare Tag</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## Casi d’uso

Esplora i seguenti casi d’uso comuni per la personalizzazione supportati da AEMCS, Adobe Target e Adobe Experience Platform.

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
  {title = Experimentation (A/B Testing)}
  {description = Learn how to test different content variations in AEMCS using Adobe Target for A/B testing.}
  {image = ./assets/use-cases/experiment/experimentation.png}
  {cta = Learn Experimentation}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="Sperimentazione (Test A/B)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="Sperimentazione (Test A/B)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="Sperimentazione (Test A/B)">Sperimentazione (Test A/B)</a>
                    </p>
                    <p class="is-size-6">Scopri come testare diverse varianti di contenuto in AEMCS utilizzando Adobe Target per il test A/B.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri la sperimentazione</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



















