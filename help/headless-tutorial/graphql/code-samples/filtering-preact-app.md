---
title: Filtraggio dell’app esatta
description: Una semplice app Preact che filtra le attività WKND modellate utilizzando Frammenti di contenuto.
version: Cloud Service
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
duration: 42
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtraggio dell’app esatta

Esplora la capacità delle API GraphQL headless dell’AEM di filtrare i dati utilizzando una [Preatto](https://preactjs.com/) app. Questa app Preact crea un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra l’utilizzo di Adobe [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da React. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e derivare un elenco di tipi di attività disponibili. Quando un utente seleziona un Tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` persistente e recupera i dettagli dell’avventura solo per quelle avventure del tipo di attività specificato.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
