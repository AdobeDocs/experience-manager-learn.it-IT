---
title: Primo tutorial su AEM Headless
description: Scopri come creare una prima applicazione di AEM Headless.
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
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '421'
ht-degree: 100%

---

# Primo tutorial su AEM Headless

Ti diamo il benvenuto al tutorial su come creare un’esperienza web utilizzando React, completamente basato sulle API di AEM Headless e GraphQL. Questo tutorial illustra il processo di creazione di un’applicazione web dinamica e interattiva, combinando la potenza delle API headless di React, Adobe Experience Manager (AEM) e GraphQL.

React è una libreria JavaScript popolare utilizzata per la creazione di interfacce utente, nota per la semplicità, riutilizzabilità e architettura basata su componenti. AEM fornisce solide funzionalità di gestione dei contenuti ed espone API headless che consentono agli sviluppatori di accedere ai contenuti e ai dati archiviati in AEM tramite una varietà di canali e applicazioni.

Sfruttando le API di AEM Headless, puoi recuperare contenuti, risorse e dati dall’istanza AEM e utilizzarli per potenziare l’applicazione React. GraphQL, un linguaggio di query flessibile per le API, offre un modo efficiente e preciso per richiedere dati specifici all’istanza di AEM, consentendo un’integrazione perfetta tra React e AEM.

![Primo tutorial su AEM Headless](./assets/overview/overview.png)

Questo tutorial illustra il processo dettagliato per creare un’esperienza web utilizzando le API React e AEM Headless con GraphQL. Scopri come configurare l’ambiente di sviluppo, stabilire una connessione tra React e AEM, recuperare contenuti utilizzando query GraphQL ed eseguirne il rendering in modo dinamico nell’applicazione web.

Saranno trattati argomenti quali la configurazione del progetto React, la determinazione dell’autenticazione con AEM, l’esecuzione di query sul contenuto da AEM tramite GraphQL, la gestione dei dati nei componenti React e l’ottimizzazione delle prestazioni tramite memorizzazione in cache e paginazione.

Al termine di questo tutorial, avrai una solida conoscenza di come sfruttare le API React, AEM Headless e GraphQL per creare un’esperienza web potente e coinvolgente. Quindi iniziamo a creare la tua prossima applicazione web.

## Prerequisiti

### Competenze

+ Conoscenza di React
+ Conoscenza di GraphQL
+ Conoscenza di base di AEM as a Cloud Service

### AEM as a Cloud Service

Questo tutorial richiede l’accesso come amministratore a un ambiente di AEM as a Cloud Service.

### Software

+ [Node.js v16+](https://nodejs.org/it/)
   + Controlla la versione del nodo eseguendo `node -v` dalla riga di comando
+ [npm 6+](https://www.npmjs.com/)
   + Controlla la versione npm eseguendo `npm -v` dalla riga di comando
+ [Git](https://git-scm.com/)
   + Controlla la versione Git eseguendo `git -v` dalla riga di comando

Utilizza [node version manager (nvm)](https://github.com/nvm-sh/nvm) per gestire più versioni di node.js nello stesso computer.

Assicurarti di disporre dei privilegi per installare il software a livello globale nel computer.

## Passaggio successivo

Ora che l’ambiente è configurato, prosegui al passaggio successivo: [Configurare e creare contenuti in AEM as a Cloud Service](./1-content-modeling.md)
