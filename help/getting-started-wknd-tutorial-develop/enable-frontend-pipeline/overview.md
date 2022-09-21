---
title: Abilita la pipeline front-end per il progetto standard Archetype AEM
description: Scopri come abilitare una pipeline front-end per un progetto AEM standard per una distribuzione più rapida di risorse statiche come CSS, JavaScript, Font e icone. Anche la separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# Abilita la pipeline front-end per il progetto standard Archetype AEM{#enable-front-end-pipeline-standard-aem-project}

Scopri come abilitare il [AEM progetto Siti WKND](https://github.com/adobe/aem-guides-wknd) (aka Standard AEM Project) creato utilizzando [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype) per implementare risorse front-end come CSS, JavaScript, Fonts e Icons utilizzando una pipeline front-end per un ciclo di sviluppo-distribuzione più veloce. Separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM. Scopri anche come sono queste risorse front-end __not__ servito dall’archivio AEM ma dalla rete CDN, un cambiamento nel paradigma di consegna.


In Adobe Cloud Manager viene creata una nuova pipeline front-end che genera e distribuisce solo `ui.frontend` artefatti al CDN integrato e informa AEM sulla sua posizione. In AEM durante la generazione di HTML della pagina web, il `<link>` e `<script>` , fai riferimento a questa posizione dell&#39;artefatto nel `href` valore dell&#39;attributo.

Tuttavia, dopo la conversione del progetto in siti WKND AEM , gli sviluppatori front-end possono lavorare separatamente e in parallelo a qualsiasi sviluppo back-end completo dello stack su AEM, che dispone di proprie pipeline di distribuzione.

>[!IMPORTANT]
>
>In generale, la pipeline front-end viene generalmente utilizzata con [Creazione rapida AEM sito](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), esiste un tutorial correlato [Guida introduttiva ad AEM Sites - Creazione rapida di siti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) per saperne di più. Quindi in questo tutorial e nei video associati che incontrate riferimenti ad esso, è per assicurarsi che le differenze sottili vengano chiamate fuori e ci sono alcuni confronti diretti o indiretti per spiegare concetti cruciali.


A correlato [esercitazione in più passaggi](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) illustra come implementare un sito AEM per un brand di stile di vita fittizio il WKND utilizzando la funzione Creazione rapida siti. Revisione dei [Flusso di lavoro dei temi](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) è inoltre utile comprendere le funzioni della pipeline front-end.

## Panoramica, vantaggi e considerazioni sulla pipeline front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>Questo vale solo per AEM as a Cloud Service e non per le distribuzioni Adobe Cloud Manager basate su AMS.

## Prerequisiti

Il passaggio di distribuzione in questa esercitazione si svolge in un Adobe Cloud Manager e assicurati di disporre di un __Gestione distribuzione__ ruolo, consulta Cloud Manage [Definizioni dei ruoli](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Assicurati di utilizzare il [Programma sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) e [Ambiente di sviluppo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) al completamento di questa esercitazione.

## Passaggi successivi {#next-steps}

Un tutorial dettagliato illustra la procedura [AEM progetto Siti WKND](https://github.com/adobe/aem-guides-wknd) conversione per abilitarla per la pipeline front-end.

Cosa sta aspettando? Avvia l’esercitazione passando alla pagina [Rivedi progetto completo](review-uifrontend-module.md) capitolo e ricollegare il ciclo di vita dello sviluppo front-end nel contesto del progetto AEM Sites standard.

