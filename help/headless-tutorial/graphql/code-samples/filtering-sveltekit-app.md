---
title: App SvelteKit semplice
description: Una semplice app SvelteKit che mostra le avventure WKND modellate utilizzando Frammenti di contenuto.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Filtraggio dell’app SvelteKit

Scopri le API AEM Headless GraphQL per elencare i dati utilizzando un&#39;app [SvelteKit](https://kit.svelte.dev/). Questa app SvelteKit crea un elenco di avventure WKND, che possono essere selezionate per visualizzarne i dettagli.

Questo codice illustra l&#39;utilizzo di [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) di Adobe per richiamare query GraphQL persistenti da SvelteKit. Questa app utilizza la query persistente `wknd-shared/adventures-all` per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. I dettagli dell&#39;avventura sono richiesti tramite la query persistente `wknd-shared/adventures-by-slug`.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
