---
title: Abilitare la pipeline front-end per l’archetipo di un progetto AEM standard
description: Scopri come abilitare una pipeline front-end per un progetto AEM standard per una implementazione più rapida delle risorse statiche come CSS, JavaScript, font e icone. Oltre alla separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM.
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 206
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 100%

---

# Abilitare la pipeline front-end per l’archetipo di un progetto AEM standard{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

Scopri come abilitare il [progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) (o progetto AEM standard) creato utilizzando [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) per implementare le risorse front-end come CSS, JavaScript, font e icone utilizzando una pipeline front-end per un ciclo di sviluppo-implementazione più rapido. La separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM. Inoltre, scopri come queste risorse front-end __non__ sono fornite dall&#39;archivio AEM ma dalla rete CDN, un cambiamento nel paradigma di implementazione.


In Adobe Cloud Manager viene creata una nuova pipeline front-end che crea e implementa solo gli artefatti `ui.frontend` nella rete CDN incorporata e informa AEM sulla relativa posizione. In AEM, durante la generazione HTML della pagina web, i tag `<link>` e `<script>` fanno riferimento a questa posizione dell&#39;artefatto nel valore dell&#39;attributo `href`.

Tuttavia, dopo la conversione del progetto AEM WKND Sites, gli sviluppatori front-end possono lavorare separatamente e in parallelo a qualsiasi sviluppo back-end full-stack su AEM, che dispone di proprie pipeline di implementazione.

>[!IMPORTANT]
>
>In generale, la pipeline front-end viene in genere utilizzata con la [Creazione rapida sito di AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=it). È disponibile un&#39;esercitazione correlata [Guida introduttiva ad AEM Sites - Creazione rapida sito](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) per ulteriori informazioni. Quindi in questo tutorial e nei video associati ci si imbatte in riferimenti ad esso, in modo da assicurare che siano evidenziate sottili differenze e che ci sia qualche confronto diretto o indiretto per spiegare i concetti fondamentali.


Una [esercitazione con più passaggi](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) correlata illustra come implementare un sito AEM per un brand di lifestyle fittizio, WKND, utilizzando la funzione Creazione rapida di siti. È inoltre utile rivedere il [flusso di lavoro temi](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=it) per comprendere il funzionamento della pipeline front-end.

## Panoramica, vantaggi e considerazioni per la pipeline front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Vale solo per AEM as a Cloud Service e non per le distribuzioni Adobe Cloud Manager basate su AMS.

## Prerequisiti

Il passaggio di implementazione in questa esercitazione si svolge in Adobe Cloud Manager. Assicurati di avere il ruolo __Responsabile dell&#39;implementazione__, consulta la [Definizioni dei ruoli](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=it#role-definitions) di Cloud Manager.

Per completare questa esercitazione, assicurati di utilizzare il [programma Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=it) e l&#39;[ambiente di sviluppo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=it).

## Passaggi successivi {#next-steps}

Una esercitazione dettagliata illustra la conversione del [progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) per abilitarlo per la pipeline front-end.

Cosa aspetti? Avvia l&#39;esercitazione passando al capitolo [Rivedere il progetto full stack](review-uifrontend-module.md) che riepiloga il ciclo di vita dello sviluppo front-end nel contesto del progetto AEM Sites standard.
