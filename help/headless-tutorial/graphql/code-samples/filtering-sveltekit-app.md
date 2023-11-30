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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---

# Filtraggio dell’app SvelteKit

Esplora la capacità delle API GraphQL headless dell’AEM di elencare i dati utilizzando una [SvelteKit](https://kit.svelte.dev/) app. Questa app SvelteKit crea un elenco di avventure WKND, che possono essere selezionate per visualizzarne i dettagli.

Questo codice illustra l’utilizzo di Adobe [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da SvelteKit. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e derivare un elenco di tipi di attività disponibili. I dettagli dell’avventura sono richiesti tramite `wknd-shared/adventures-by-slug` query persistente.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
