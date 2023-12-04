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
duration: 46
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Filtraggio dell’app Express

Esplora la capacità delle API GraphQL headless dell’AEM di filtrare i dati utilizzando una [Espresso](https://expressjs.com/) e [Pug](https://pugjs.org/) app. Questa app Express crea un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra l’utilizzo di Adobe [Client AEM headless per NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) per richiamare query GraphQL persistenti utilizzando JavaScript basato su Node.js. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e derivare un elenco di tipi di attività disponibili. Quando un utente seleziona un Tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` persistente e recupera i dettagli dell’avventura solo per quelle avventure del tipo di attività specificato. I dettagli delle avventure vengono recuperati dall’AEM tramite `wknd-shared/adventures-by-slug` query persistente.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, e `wknd-shared/adventures-by-slug`
