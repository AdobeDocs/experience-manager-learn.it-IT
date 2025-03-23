---
title: Prima esercitazione su AEM Headless
description: Scopri come diventare una prima applicazione AEM Headless.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 3%

---

# Prima esercitazione su AEM Headless

{{aem-headless-trials-promo}}

Ti diamo il benvenuto al tutorial su come creare un’esperienza web utilizzando React, completamente basato sulle API AEM Headless e GraphQL. In questa esercitazione, ti guideremo attraverso il processo di creazione di un’applicazione web dinamica e interattiva, combinando la potenza delle API headless di React, Adobe Experience Manager (AEM) e GraphQL.

React è una popolare libreria JavaScript per la creazione di interfacce utente, nota per la sua semplicità, riutilizzabilità e architettura basata su componenti. AEM fornisce solide funzionalità di gestione dei contenuti ed espone API headless che consentono agli sviluppatori di accedere ai contenuti e ai dati memorizzati in AEM tramite una varietà di canali e applicazioni.

Sfruttando le API AEM Headless, puoi recuperare contenuti, risorse e dati dall’istanza AEM e utilizzarli per potenziare l’applicazione React. GraphQL, un linguaggio di query flessibile per le API, offre un modo efficiente e preciso per richiedere dati specifici all’istanza di AEM, consentendo un’integrazione perfetta tra React e AEM.

![Prima esercitazione AEM Headless](./assets/overview/overview.png)

In questo tutorial, ti guideremo attraverso il processo passo passo per creare un’esperienza web utilizzando le API headless React e AEM con GraphQL. Scopri come configurare l’ambiente di sviluppo, stabilire una connessione tra React e AEM, recuperare contenuti utilizzando query GraphQL ed eseguirne il rendering in modo dinamico nell’applicazione web.

Tratteremo argomenti quali la configurazione del progetto React, l’impostazione dell’autenticazione con AEM, l’esecuzione di query sul contenuto da AEM tramite GraphQL, la gestione dei dati nei componenti React e l’ottimizzazione delle prestazioni tramite caching e impaginazione.

Al termine di questa esercitazione, avrai una solida conoscenza di come sfruttare React, le API AEM Headless e GraphQL per creare un’esperienza web potente e coinvolgente. Quindi, immergiamoci e iniziamo a creare la tua prossima applicazione web!

## Prerequisiti

### Competenze

+ Conoscenza di React
+ Competenza in GraphQL
+ Conoscenza di base di AEM as a Cloud Service

### AEM as a Cloud Service

Questo tutorial richiede l’accesso come amministratore a un ambiente AEM as a Cloud Service.

### Software

+ [Node.js v16+](https://nodejs.org/it/)
   + Controllare la versione del nodo eseguendo `node -v` dalla riga di comando
+ [npm 6+](https://www.npmjs.com/)
   + Controllare la versione npm eseguendo `npm -v` dalla riga di comando
+ [Git](https://git-scm.com/)
   + Verifica la versione Git eseguendo `git -v` dalla riga di comando

Utilizzare [node version manager (nvm)](https://github.com/nvm-sh/nvm) per gestire più versioni di node.js nello stesso computer.

Assicurarsi di disporre dei privilegi per installare il software a livello globale nel computer.

## Passaggio successivo

Ora che l&#39;ambiente è configurato, passiamo al passaggio successivo: [Configura e crea contenuti in AEM as a Cloud Service](./1-content-modeling.md)
