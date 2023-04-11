---
title: Guida introduttiva SPA Editor e SPA remoto - Panoramica
description: Ti diamo il benvenuto nell’esercitazione in più parti per gli sviluppatori che desiderano estendere un SPA remoto esistente con contenuto AEM modificabile tramite AEM Editor.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: 0fff8b53e3dffb835e070444b55a72f0b0cc3d14
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Panoramica

Ti diamo il benvenuto nell’esercitazione in più parti per gli sviluppatori che desiderano potenziare un SPA remoto basato su React (o Next.js) esistente con contenuto AEM modificabile utilizzando AEM SPA Editor.

Questa esercitazione si basa sul [App GraphQL WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=it), un’app React che consuma AEM contenuto Frammento di contenuto su AEM API GraphQL, tuttavia non fornisce alcuna creazione contestuale di contenuto SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Informazioni sull’esercitazione

L’esercitazione ha lo scopo di illustrare come è possibile aggiornare un SPA remoto o un SPA in esecuzione al di fuori del contesto di AEM per utilizzare e distribuire contenuti creati in AEM.

La maggior parte delle attività nell&#39;esercitazione si concentra sullo sviluppo JavaScript, tuttavia vengono trattati aspetti critici che ruotano intorno a AEM. Questi aspetti includono la definizione della posizione in cui il contenuto viene creato e memorizzato in AEM e la mappatura SPA percorsi alle pagine AEM.

L’esercitazione è progettata per lavorare con **AEM as a Cloud Service** ed è composto da due progetti:

1. La __Progetto AEM__ contiene la configurazione e il contenuto da distribuire a AEM.
1. __App WKND__ progetto è il SPA da integrare con AEM editor SPA

## Codice più recente

+ Il punto di partenza del codice di questa esercitazione è disponibile in [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) in `remote-spa-tutorial` cartella.

## Prerequisiti

Questa esercitazione richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/it/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Questa esercitazione presuppone:

+ [Codice Microsoft® Visual Studio](https://visualstudio.microsoft.com/) come IDE
+ Una directory di lavoro di `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Esecuzione dell’SDK AEM come servizio Author in `http://localhost:4502`
+ Esecuzione dell&#39;SDK AEM con il locale `admin` account con password `admin`
+ Esecuzione del SPA su `http://localhost:3000`

>[!NOTE]
>
> **Hai bisogno di aiuto per configurare l’ambiente di sviluppo locale?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).

## 1. Configurare AEM per l’editor SPA

Sono necessarie configurazioni AEM per integrare il SPA con AEM SPA Editor. Queste configurazioni vengono gestite e distribuite tramite un progetto AEM. In questo capitolo, scopri quali configurazioni sono necessarie e come definirle.

+ [Scopri come configurare AEM per l’editor di SPA](./aem-configure.md)

## 2. Bootstrap il SPA

Affinché AEM editor di SPA possa integrare un SPA nel suo contesto di authoring, è necessario apportare alcune aggiunte al SPA.

+ [Scopri come avviare il SPA per AEM Editor SPA](./spa-bootstrap.md)

## 3. Componenti fissi modificabili

Innanzitutto, esplora l’aggiunta di un &quot;componente fisso&quot; modificabile al SPA. Questo illustra come uno sviluppatore può inserire un componente modificabile specifico nella SPA. L’autore può modificare il contenuto del componente, ma non può rimuoverlo o modificarne il posizionamento, il posizionamento o le dimensioni.

+ [Informazioni sui componenti fissi modificabili](./spa-fixed-component.md)

## 4. Componenti del contenitore modificabili

Quindi, esplora l’aggiunta di un &quot;componente contenitore&quot; modificabile al SPA. Questo illustra come uno sviluppatore può inserire un componente contenitore nella SPA. I componenti contenitore consentono agli autori di inserire nel componente consentito e regolare il layout dei componenti.

+ [Informazioni sui componenti contenitore modificabili](./spa-container-component.md)

## 5. Instradamenti dinamici e componenti modificabili

Infine, utilizzare i concetti illustrati nei capitoli precedenti per tracciare percorsi dinamici; percorsi che visualizzano contenuti diversi in base al parametro della route. Questo illustra come AEM editor di SPA può essere utilizzato per creare contenuti su percorsi guidati e derivati a livello di programmazione.

+ [Informazioni sui percorsi dinamici e sui componenti modificabili](./spa-dynamic-routes.md)

## Risorse aggiuntive

+ [AEM SPA React Modificabili Components](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
