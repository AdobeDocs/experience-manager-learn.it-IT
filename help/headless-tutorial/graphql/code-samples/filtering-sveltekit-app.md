---
title: App SvelteKit semplice
description: Una semplice app SvelteKit che mostra le avventure WKND modellate utilizzando frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# Applicazione SvelteKit filtro

Esplorare AEM API GraphQL headless la possibilità di elencare i dati utilizzando un [SvelteKit](https://kit.svelte.dev/) app. Questa app SvelteKit crea un elenco di avventure WKND, che può essere selezionato per visualizzare i dettagli dell&#39;avventura.

Questo codice illustra l&#39;utilizzo di Adobe [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da SvelteKit. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. I dettagli dell&#39;avventura sono richiesti tramite il `wknd-shared/adventures-by-slug` query persistente.

Questo codice:

+ Si collega a un servizio AEM Publish e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
