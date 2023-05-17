---
title: Prima esercitazione AEM headless
description: Scopri come diventare una prima applicazione AEM Headless.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---


# Prima esercitazione AEM headless

![Prima esercitazione su AEM Headless](./assets/overview/overview.png)

Ti diamo il benvenuto nell’esercitazione sulla creazione di un’esperienza web utilizzando React, completamente fornita dalle API headless AEM e GraphQL. Questa esercitazione ti guiderà attraverso il processo di creazione di un’applicazione web dinamica e interattiva combinando la potenza delle API React, Adobe Experience Manager (AEM) Headless e GraphQL.

React è una libreria JavaScript popolare per la creazione di interfacce utente, nota per la sua semplicità, riutilizzabilità e architettura basata su componenti. AEM fornisce solide funzionalità di gestione dei contenuti ed espone le API headless che consentono agli sviluppatori di accedere ai contenuti e ai dati memorizzati in AEM tramite diversi canali e applicazioni.

Sfruttando AEM API headless, puoi recuperare contenuti, risorse e dati dall’istanza AEM e utilizzarli per potenziare l’applicazione React. GraphQL, un linguaggio di query flessibile per le API, fornisce un modo efficiente e preciso per richiedere dati specifici dall’istanza AEM, consentendo un’integrazione diretta tra React e AEM.

Nel corso di questa esercitazione, ti guideremo attraverso il processo dettagliato di creazione di un’esperienza web utilizzando le API React e AEM Headless con GraphQL. Scoprirai come configurare l’ambiente di sviluppo, stabilire una connessione tra React e AEM, recuperare il contenuto utilizzando le query GraphQL ed eseguirne il rendering dinamico nell’applicazione Web.

Verranno trattati argomenti quali la configurazione del progetto React, l’autenticazione con AEM, l’esecuzione di query sui contenuti da AEM utilizzando GraphQL, la gestione dei dati nei componenti React e l’ottimizzazione delle prestazioni utilizzando la memorizzazione in cache e l’impaginazione.

Al termine di questa esercitazione, comprenderai appieno come sfruttare le API React, AEM Headless e GraphQL per creare un’esperienza web potente e coinvolgente. Quindi, tuffiamoci e cominciamo a costruire la vostra prossima applicazione web!

## Prerequisiti

### Competenze

+ Efficienza nella reazione
+ Efficienza in GraphQL
+ Conoscenza di base di AEM as a Cloud Service

### AEM as a Cloud Service

Questa esercitazione richiede l’accesso dell’amministratore a un ambiente AEM as a Cloud Service.

### Software

+ [Node.js v16+](https://nodejs.org/it/)
   + Controlla la versione del nodo eseguendo `node -v` dalla riga di comando
+ [npm 6+](https://www.npmjs.com/)
   + Controlla la versione npm eseguendo `npm -v` dalla riga di comando
+ [Git](https://git-scm.com/)
   + Controlla la versione git eseguendo `git -v` dalla riga di comando

Utilizzo [node version manager (nvm)](https://github.com/nvm-sh/nvm) per risolvere il problema di avere più versioni di node.js sullo stesso computer.

Verificare di disporre dei privilegi necessari per installare il software a livello globale nel computer.

## Passaggio successivo

Ora che l’ambiente è configurato, passiamo al passaggio successivo: [Configurazione e creazione di contenuti in AEM as a Cloud Service](./1-content-modeling.md)
