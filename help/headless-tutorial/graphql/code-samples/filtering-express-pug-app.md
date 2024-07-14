---
title: Filtraggio dell’app Express
description: Una semplice app Express che filtra le attività WKND modellate utilizzando Frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 29
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Filtraggio dell’app Express

Esplora le API GraphQL headless AEM per filtrare i dati utilizzando un&#39;app [Express](https://expressjs.com/) e [Pug](https://pugjs.org/). Questa app Express crea un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra come utilizzare il client headless AEM [di Adobe per NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) per richiamare query GraphQL persistenti utilizzando JavaScript basato su Node.js. Questa app utilizza la query persistente `wknd-shared/adventures-all` per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato alla query persistente `wknd-shared/adventures-by-activity` e recupera i dettagli dell&#39;avventura solo per le avventure del tipo di attività specificato. I dettagli dell&#39;avventura vengono recuperati dall&#39;AEM tramite la query persistente `wknd-shared/adventures-by-slug`.

Questo codice:

+ Si connette a un servizio Publish AEM e non richiede l&#39;autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity` e `wknd-shared/adventures-by-slug`
