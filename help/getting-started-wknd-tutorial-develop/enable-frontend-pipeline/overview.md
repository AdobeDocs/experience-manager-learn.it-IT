---
title: Abilita pipeline front-end per il progetto standard Archetipo AEM
description: Scopri come abilitare una pipeline front-end per un progetto AEM standard per una distribuzione più rapida delle risorse statiche come CSS, JavaScript, Fonts, Icone. Separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 216
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---

# Abilita pipeline front-end per il progetto standard Archetipo AEM{#enable-front-end-pipeline-standard-aem-project}

Scopri come abilitare [Progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) (progetto AEM standard) creato con [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) distribuire risorse front-end come CSS, JavaScript, Fonts e Icone utilizzando una pipeline front-end per un ciclo più rapido dallo sviluppo alla distribuzione; La separazione dello sviluppo front-end dallo sviluppo back-end full-stack sull’AEM. Scopri anche come sono queste risorse front-end __non__ dall’archivio dell’AEM ma dalla rete CDN, un cambiamento nel paradigma di consegna.


In Adobe Cloud Manager viene creata una nuova pipeline front-end che si limita a generare e distribuire `ui.frontend` artefatti della rete CDN integrata e informa l’AEM sulla sua posizione. Sull&#39;AEM durante la generazione HTML della pagina Web, il `<link>` e `<script>` tag, fai riferimento a questa posizione dell’artefatto nel `href` valore attributo.

Tuttavia, dopo la conversione del progetto AEM WKND Sites, gli sviluppatori front-end possono lavorare separatamente e parallelamente a qualsiasi sviluppo back-end full-stack sull’AEM, che dispone di proprie pipeline di distribuzione.

>[!IMPORTANT]
>
>In generale, la pipeline front-end viene in genere utilizzata con [Creazione rapida di siti AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), è disponibile un tutorial correlato [Guida introduttiva ad AEM Sites - Creazione rapida di siti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) per saperne di più. Quindi in questo tutorial e nei video associati ci si imbatte in riferimenti ad esso, questo è per assicurarsi che siano evidenziate sottili differenze e che ci sia qualche confronto diretto o indiretto per spiegare i concetti fondamentali.


Un correlato [tutorial con più passaggi](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) illustra come implementare un sito AEM per un brand di lifestyle fittizio, WKND, utilizzando la funzione Creazione rapida di siti. Revisione della [Flusso di lavoro Theming](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) è utile anche comprendere il funzionamento della pipeline front-end.

## Panoramica, vantaggi e considerazioni per la pipeline front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Questo vale solo per le distribuzioni di AEM as a Cloud Service e non per le distribuzioni di Adobe Cloud Manager basate su AMS.

## Prerequisiti

Il passaggio di distribuzione di questa esercitazione si svolge in un Adobe di Cloud Manager, assicurati di disporre di __Responsabile dell’implementazione__ ruolo, consulta Cloud Manager [Definizioni dei ruoli](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Assicurati di utilizzare il [Programma sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) e [Ambiente di sviluppo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) al completamento di questa esercitazione.

## Passaggi successivi {#next-steps}

Un tutorial dettagliato illustra [Progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) per abilitarla per la pipeline front-end.

Cosa state aspettando? Avvia l’esercitazione passando al [Rivedi progetto full stack](review-uifrontend-module.md) e riepiloga il ciclo di vita dello sviluppo front-end nel contesto del progetto AEM Sites standard.
