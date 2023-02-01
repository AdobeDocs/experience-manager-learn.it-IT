---
title: Filtraggio dell’app ExpressJS e Pug
description: Una semplice app ExpressJS/Pug che filtra le avventure WKND modellate utilizzando frammenti di contenuto.
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
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---


# Filtraggio dell’app ExpressJS e Pug

Esplorare AEM API GraphQL headless per filtrare i dati utilizzando un’ [ExpressJS](https://expressjs.com/)/[Pug](https://pugjs.org/) app. Questa app ExpressJS/Pug crea un elenco di avventure WKND filtrabili per tipo di attività.

Questo codice illustra l&#39;utilizzo di Adobe [Client AEM headless per NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) per richiamare query GraphQL persistenti utilizzando JavaScript basato su Node.js. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` query persistente e recupera i dettagli dell’avventura solo per le avventure del tipo di attività specificato.

Questo codice:

+ Si collega a un servizio AEM Publish e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
