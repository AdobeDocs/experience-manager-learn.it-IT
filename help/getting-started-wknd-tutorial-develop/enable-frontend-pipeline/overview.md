---
title: Abilita pipeline front-end per il progetto standard Archetipo di AEM
description: Scopri come abilitare una pipeline front-end per un progetto AEM standard per una distribuzione più rapida delle risorse statiche come CSS, JavaScript, Font e icone. Separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM.
version: Experience Manager as a Cloud Service
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
duration: 206
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 1%

---

# Abilita pipeline front-end per il progetto standard Archetipo di AEM{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

Scopri come abilitare il [progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) (o progetto AEM standard) creato utilizzando [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) per distribuire risorse front-end come CSS, JavaScript, Fonts e Icone utilizzando una pipeline front-end per un ciclo di sviluppo-distribuzione più rapido. La separazione dello sviluppo front-end dallo sviluppo back-end full-stack su AEM. Inoltre, scopri come queste risorse front-end sono __non__ fornite dall&#39;archivio AEM ma dalla rete CDN, un cambiamento nel paradigma di consegna.


In Adobe Cloud Manager viene creata una nuova pipeline front-end che crea e distribuisce solo gli artefatti `ui.frontend` nella rete CDN integrata e informa AEM sulla relativa posizione. In AEM, durante la generazione HTML della pagina Web, i tag `<link>` e `<script>` fanno riferimento a questa posizione dell&#39;artefatto nel valore dell&#39;attributo `href`.

Tuttavia, dopo la conversione del progetto AEM WKND Sites, gli sviluppatori front-end possono lavorare separatamente e in parallelo a qualsiasi sviluppo back-end full-stack su AEM, che dispone di proprie pipeline di distribuzione.

>[!IMPORTANT]
>
>In generale, la pipeline front-end viene in genere utilizzata con la [Creazione rapida sito di AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en). È disponibile un&#39;esercitazione correlata [Guida introduttiva ad AEM Sites - Creazione rapida sito](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) per ulteriori informazioni. Quindi in questo tutorial e nei video associati ci si imbatte in riferimenti ad esso, questo è per assicurarsi che siano evidenziate sottili differenze e che ci sia qualche confronto diretto o indiretto per spiegare i concetti fondamentali.


Un [tutorial con più passaggi](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) correlato illustra come implementare un sito AEM per un brand di lifestyle fittizio, WKND, utilizzando la funzione Creazione rapida di siti. È inoltre utile rivedere il [flusso di lavoro Theming](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) per comprendere il funzionamento della pipeline front-end.

## Panoramica, vantaggi e considerazioni per la pipeline front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Questo vale solo per AEM as a Cloud Service e non per le distribuzioni Adobe Cloud Manager basate su AMS.

## Prerequisiti

Il passaggio di distribuzione in questa esercitazione si svolge in un Cloud Manager di Adobe. Assicurati di avere il ruolo __Responsabile dell&#39;implementazione__, consulta Cloud Manager [Definizioni dei ruoli](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Per completare questa esercitazione, assicurati di utilizzare il [programma sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) e l&#39;[ambiente di sviluppo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=it).

## Passaggi successivi {#next-steps}

Un tutorial dettagliato illustra la conversione di [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd) per abilitarla per la pipeline front-end.

Cosa state aspettando? Avvia l&#39;esercitazione passando al capitolo [Rivedi progetto full stack](review-uifrontend-module.md) e riepiloga il ciclo di vita dello sviluppo front-end nel contesto del progetto AEM Sites standard.
