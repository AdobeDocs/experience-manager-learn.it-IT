---
title: Abilita pipeline front-end per Archetype di progetto AEM standard
description: Converti un progetto AEM standard per una distribuzione più rapida di risorse statiche come CSS, JavaScript, Font e icone utilizzando la pipeline front-end. E la separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Abilita pipeline front-end per Archetype di progetto AEM standard{#enable-front-end-pipeline-standard-aem-project}

Scopri come abilitare il progetto AEM standard creato utilizzando [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype) per distribuire risorse statiche come CSS, JavaScript, font e icone utilizzando la pipeline front-end per un ciclo di sviluppo-distribuzione più veloce. E la separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM. Inoltre, viene illustrato come sono __not__ è servito da AEM archivio ma dalla rete CDN, un cambiamento nel paradigma di consegna.

In Adobe Cloud Manager viene creata una nuova pipeline front-end che genera e distribuisce solo `ui.frontend` artefatti a CDN e informa AEM sulla sua posizione. Durante la generazione di HTML della pagina web, il `<link>` e `<script>` i tag fanno riferimento a questa posizione nel `href` valore dell&#39;attributo.

Dopo la conversione standard del progetto AEM, gli sviluppatori front-end possono lavorare separatamente e in parallelo a qualsiasi sviluppo back-end completo dello stack su AEM, che dispone di proprie pipeline di distribuzione.

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>Questo è applicabile solo alle distribuzioni di Adobe Manager as a Cloud Service e non basate su AMS.

