---
title: Filtro dell’app Preact
description: Una semplice app Preact che filtra le attività WKND modellate utilizzando Frammenti di contenuto.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
exl-id: d2b7e8ab-8bbc-495f-94f1-362ea47b3853
duration: 26
source-git-commit: 30b98e82e78120bf9fb13c9d41780af4c07665d8
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 4%

---

# Filtro dell’app Preact

Scopri le API AEM Headless GraphQL per filtrare i dati tramite un&#39;app [Preact](https://preactjs.com/). Questa app Preact crea un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra l&#39;utilizzo di [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) di Adobe per richiamare query GraphQL persistenti da React. Questa app utilizza la query persistente `wknd-shared/adventures-all` per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato alla query persistente `wknd-shared/adventures-by-activity` e recupera i dettagli dell&#39;avventura solo per le avventure del tipo di attività specificato.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
