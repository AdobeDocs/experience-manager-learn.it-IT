---
title: 'Guida introduttiva all’SPA Editor e al SPA remoto: panoramica'
description: Benvenuti nel tutorial in più parti per gli sviluppatori che desiderano integrare un SPA remoto esistente con contenuti AEM modificabili utilizzando l’editor AEM SPA.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 9%

---

# Panoramica

{{edge-delivery-services}}

Benvenuti al tutorial in più parti per gli sviluppatori che desiderano integrare un SPA remoto basato su React (o Next.js) esistente con contenuti AEM modificabili tramite l’editor AEM SPA.

Questo tutorial si basa su [App GraphQL WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=it), app React che utilizza contenuti di frammenti di contenuto AEM tramite API GraphQL AEM, ma non fornisce alcuna funzione di authoring nel contesto dei contenuti SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Informazioni sull’esercitazione

Il tutorial che illustra come un SPA remoto, o un SPA in esecuzione al di fuori del contesto dell’AEM, può essere aggiornato per utilizzare e distribuire contenuti creati in AEM.

La maggior parte delle attività del tutorial si concentrano sullo sviluppo JavaScript, tuttavia vengono trattati gli aspetti critici che ruotano intorno all’AEM. Questi aspetti includono la definizione di dove il contenuto viene creato e memorizzato nell&#39;AEM e la mappatura delle vie dell&#39;SPA verso le Pagine AEM.

L’esercitazione è progettata per funzionare con **AEM as a Cloud Service** ed è composto da due progetti:

1. Il __Progetto AEM__ contiene la configurazione e il contenuto che devono essere distribuiti all’AEM.
1. __App WKND__ Il progetto è l&#39;SPA da integrare con AEM SPA Editor

## Codice più recente

+ Il punto iniziale del codice di questo tutorial è disponibile su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) nel `remote-spa-tutorial` cartella.

## Prerequisiti

Questo tutorial richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/it/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Questo tutorial presuppone:

+ [Codice Microsoft® Visual Studio](https://visualstudio.microsoft.com/) come IDE
+ Una directory di lavoro di `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Esecuzione dell’SDK dell’AEM come servizio di authoring il `http://localhost:4502`
+ Esecuzione dell’SDK dell’AEM con il `admin` account con password `admin`
+ Esecuzione dell’SPA il `http://localhost:3000`

>[!NOTE]
>
> **Hai bisogno di assistenza per configurare l’ambiente di sviluppo locale?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).

## 1. Configurare AEM per l’editor SPA

Per integrare l’SPA con l’Editor SPA dell’AEM sono necessarie configurazioni dell’AEM. Queste configurazioni vengono gestite e implementate tramite un progetto AEM. In questo capitolo, scopri quali configurazioni sono necessarie e come definirle.

+ [Scopri come configurare l’AEM per l’editor SPA](./aem-configure.md)

## 2. Bootstrap dell’SPA

Affinché l&#39;AEM SPA Editor possa integrare un SPA nel proprio contesto di authoring, è necessario apportare alcune aggiunte all&#39;SPA.

+ [Scopri come avviare l’SPA per l’Editor SPA dell’AEM](./spa-bootstrap.md)

## 3. Componenti fissi modificabili

Innanzitutto, esplora la possibilità di aggiungere un &quot;componente fisso&quot; modificabile all’SPA. Questo illustra come uno sviluppatore può inserire un componente modificabile specifico nell’SPA. L’autore può modificare il contenuto del componente, ma non può rimuoverlo né modificarne la posizione o le dimensioni.

+ [Informazioni sui componenti fissi modificabili](./spa-fixed-component.md)

## 4. Componenti dei contenitori modificabili

Quindi, prova ad aggiungere un &quot;componente contenitore&quot; modificabile all’SPA. Questo illustra come uno sviluppatore può inserire un componente contenitore nell’SPA. I componenti Contenitore consentono agli autori di inserire in essi il componente consentito e di regolare il layout dei componenti.

+ [Scopri i componenti contenitore modificabili](./spa-container-component.md)

## 5. Percorsi dinamici e componenti modificabili

Infine, utilizzare i concetti illustrati nei capitoli precedenti per le route dinamiche, ovvero route che visualizzano contenuti diversi in base al parametro della route. Questo illustra come utilizzare l’Editor SPA dell’AEM per creare contenuti su route guidate e derivate da programma.

+ [Scopri le route dinamiche e i componenti modificabili](./spa-dynamic-routes.md)

## Risorse aggiuntive

+ [Componenti modificabili di React dell’AEM SPA](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
