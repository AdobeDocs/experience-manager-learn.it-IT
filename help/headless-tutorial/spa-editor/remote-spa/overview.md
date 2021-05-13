---
title: Guida introduttiva SPA Editor e SPA remoto - Panoramica
description: Ti diamo il benvenuto nell’esercitazione in più parti per gli sviluppatori che desiderano estendere un SPA remoto esistente con contenuto AEM modificabile tramite AEM Editor.
topic: Senza testa, SPA, Sviluppo
feature: Editor SPA, componenti core, API, sviluppo
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
source-git-commit: cede0c97e0f322fe5d20d5c4f685ed10b90af1d4
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 6%

---


# Panoramica

Ti diamo il benvenuto nell’esercitazione in più parti per gli sviluppatori che desiderano potenziare un SPA remoto basato su React (o Next.js) esistente con contenuto AEM modificabile utilizzando AEM SPA Editor.

Questa esercitazione si basa sull’ [app GraphQL WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html), un’app React che consuma AEM contenuto di frammenti di contenuto rispetto AEM API GraphQL, ma non fornisce alcuna creazione contestuale di contenuto SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## Informazioni sull’esercitazione

L’esercitazione ha lo scopo di illustrare come è possibile aggiornare un SPA remoto o un SPA in esecuzione al di fuori del contesto di AEM per utilizzare e distribuire contenuti creati in AEM.

La maggior parte delle attività nell&#39;esercitazione si concentra sullo sviluppo JavaScript, tuttavia vengono trattati aspetti critici che ruotano intorno a AEM. Questi aspetti includono la definizione della posizione in cui il contenuto viene creato e memorizzato in AEM e la mappatura SPA percorsi alle pagine AEM.

L’esercitazione è progettata per funzionare con **AEM come Cloud Service** ed è composta da due progetti:

1. Il __AEM Project__ contiene la configurazione e il contenuto da distribuire in AEM.
1. __WKND__ Appproject è il SPA da integrare con AEM Editor SPA

## Codice più recente

+ Il codice di questa esercitazione si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql) nel ramo `feature/spa-editor`.

## Prerequisiti

Questa esercitazione richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/it/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql)

Questa esercitazione presuppone:

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Code come IDE
+ Una directory di lavoro di `~/Code/wknd-app`
+ Esecuzione dell&#39;SDK AEM come servizio Author su `http://localhost:4502`
+ Esecuzione dell&#39;SDK AEM con l&#39;account locale `admin` con password `admin`
+ Esecuzione del SPA su `http://localhost:3000`

>[!NOTE]
>
> **Hai bisogno di aiuto per configurare l’ambiente di sviluppo locale?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).


## Configurazione rapida

Configurazione rapida ti permette di iniziare a usare l’SPA dell’app WKND e l’editor SPA di AEM in 15 minuti. Questa configurazione accelerata consente di passare direttamente allo stato finale dell’esercitazione, consentendoti di esplorare la creazione del SPA in AEM Editor SPA.

+ [Informazioni sulla configurazione rapida](./quick-setup.md)

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

## Altro materiale di riferimento

+ [Modifica di un SPA esterno in documenti AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM componenti WCM - Implementazione di React Core](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM componenti WCM - Editor Spa - Implementazione react Core](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
