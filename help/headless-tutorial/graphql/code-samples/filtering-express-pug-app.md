---
title: Applicazione di filtro Express
description: Una semplice app Express che filtra le avventure WKND modellate utilizzando frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# Applicazione di filtro Express

Esplorare AEM API GraphQL headless per filtrare i dati utilizzando un’ [Express](https://expressjs.com/) e [Pug](https://pugjs.org/) app. Questa app Express crea un elenco di avventure WKND filtrabili per tipo di attività.

Questo codice illustra l&#39;utilizzo di Adobe [Client AEM headless per NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) per richiamare query GraphQL persistenti utilizzando JavaScript basato su Node.js. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` query persistente e recupera i dettagli dell’avventura solo per le avventure del tipo di attività specificato. I dettagli dell’avventura vengono recuperati da AEM tramite `wknd-shared/adventures-by-slug` query persistente.

Questo codice:

+ Si collega a un servizio AEM Publish e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`e `wknd-shared/adventures-by-slug`
