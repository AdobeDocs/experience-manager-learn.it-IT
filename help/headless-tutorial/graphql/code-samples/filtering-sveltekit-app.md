---
title: App SvelteKit semplice
description: Una semplice app SvelteKit che mostra le avventure WKND modellate utilizzando Frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Filtraggio dell’app SvelteKit

Esplora la possibilità delle API GraphQL headless AEM di elencare i dati utilizzando un&#39;app [SvelteKit](https://kit.svelte.dev/). Questa app SvelteKit crea un elenco di avventure WKND, che possono essere selezionate per visualizzarne i dettagli.

Questo codice illustra l&#39;utilizzo del client headless AEM [ di Adobe per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da SvelteKit. Questa app utilizza la query persistente `wknd-shared/adventures-all` per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. I dettagli dell&#39;avventura sono richiesti tramite la query persistente `wknd-shared/adventures-by-slug`.

Questo codice:

+ Si connette a un servizio Publish AEM e non richiede l&#39;autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
