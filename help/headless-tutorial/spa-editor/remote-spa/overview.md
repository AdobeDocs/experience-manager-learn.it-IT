---
title: Guida introduttiva all’editor di SPA e alle applicazioni a pagina singola remote - Panoramica
description: Ti diamo il benvenuto in questo tutorial in più parti progettato per gli sviluppatori che desiderano potenziare un’applicazione a pagina singola remota (SPA) esistente con contenuti AEM modificabili tramite l’editor di SPA di AEM.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: ht
source-wordcount: '571'
ht-degree: 100%

---

# Panoramica

{{edge-delivery-services}}

Ti diamo il benvenuto in questo tutorial in più parti progettato per gli sviluppatori che desiderano potenziare un’applicazione a pagina singola remota basata su React (o Next.js) con contenuti AEM modificabili tramite l’editor SPA di AEM.

Questo tutorial si basa sull’[app GraphQL WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=it), un’app React che utilizza contenuto di frammenti di contenuto AEM tramite le API GraphQL di AEM, ma non fornisce alcuna creazione di contenuti SPA nel contesto.

>[!VIDEO](https://video.tv.adobe.com/v/3444855?quality=12&learn=on&captions=ita)

## Informazioni sul tutorial

Il tutorial che illustra come aggiornare un’applicazione a pagina singola remota o in esecuzione al di fuori del contesto di AEM per utilizzare e distribuire contenuti creati in AEM.

La maggior parte delle attività del tutorial si concentrano sullo sviluppo di JavaScript, tuttavia vengono trattati gli aspetti critici legati ad AEM. Questi aspetti includono la definizione della posizione in cui il contenuto viene creato e memorizzato in AEM e la mappatura dei percorsi delle applicazioni a pagina singola nelle pagine AEM.

Il tutorial è progettato per utilizzare **AEM as a Cloud Service** ed è composto da due progetti:

1. Il __progetto AEM__ contiene la configurazione e il contenuto che devono essere distribuiti in AEM.
1. Il progetto __App WKND__ è l’applicazione a pagina singola (SPA) da integrare con l’editor di SPA di AEM

## Codice più recente

+ Il punto iniziale del codice di questo tutorial è disponibile in [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) nella cartella `remote-spa-tutorial`.

## Prerequisiti

Questo tutorial richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime)
+ [Node.js v18](https://nodejs.org/it/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Questo tutorial presuppone:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/it) come IDE
+ Una directory di lavoro di `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Esecuzione di AEM SDK come servizio di authoring su `http://localhost:4502`
+ Esecuzione di AEM SDK con l’account `admin` locale con password `admin`
+ Esecuzione dell’applicazione a pagina singola su `http://localhost:3000`

>[!NOTE]
>
> **Hai bisogno di assistenza per configurare l’ambiente di sviluppo locale?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando l’SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview).

## &#x200B;1. Configurare AEM per l’editor di SPA

Per integrare un’applicazione a pagina singola con l’editor di SPA di AEM sono necessarie configurazioni di AEM. Queste configurazioni vengono gestite e distribuite tramite un progetto AEM. In questo capitolo, scoprirai quali configurazioni sono necessarie e come definirle.

+ [Scopri come configurare AEM per l’editor di SPA](./aem-configure.md)

## &#x200B;2. Bootstrap dell’applicazione a pagina singola

Affinché l’editor SPA di AEM possa integrare un’applicazione a pagina singola nel contesto di authoring, è necessario apportare alcune aggiunte alla SPA.

+ [Scopri come eseguire il bootstrap dell’applicazione a pagina singola per l’editor di SPA di AEM](./spa-bootstrap.md)

## &#x200B;3. Componenti fissi modificabili

Innanzitutto, prova ad aggiungere un “componente fisso” modificabile all’applicazione a pagina singola. Questo illustra come uno sviluppatore può posizionare un componente modificabile specifico nell’applicazione a pagina singola. L’autore può modificare il contenuto del componente, ma non può rimuoverlo né modificarne la posizione o le dimensioni.

+ [Scopri di più sui componenti fissi modificabili](./spa-fixed-component.md)

## &#x200B;4. Componenti dei contenitori modificabili

Successivamente, prova ad aggiungere un “componente contenitore” modificabile all’applicazione a pagina singola. Questo illustra come uno sviluppatore può posizionare un componente contenitore nell’applicazione a pagina singola. I componenti contenitore consentono agli autori di posizionare in essi il componente consentito e di regolare il layout dei componenti.

+ [Scopri di più sui componenti contenitore modificabili](./spa-container-component.md)

## &#x200B;5. Percorsi dinamici e componenti modificabili

Infine, utilizza i concetti illustrati nei capitoli precedenti per i percorsi dinamici; percorsi che mostrano contenuti diversi in base al parametro del percorso. Questo illustra come l’editor di SPA di AEM può essere utilizzato per l’authoring di contenuti su percorsi guidati e derivati a livello di programmazione.

+ [Scopri di più sui percorsi dinamici e i componenti modificabili](./spa-dynamic-routes.md)

## Risorse aggiuntive

+ [Componenti modificabili di React SPA di AEM](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
