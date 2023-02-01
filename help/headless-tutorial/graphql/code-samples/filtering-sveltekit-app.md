---
title: Applicazione SvelteKit filtro
description: Una semplice app SvelteKit che filtra le avventure WKND modellate utilizzando frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Applicazione SvelteKit filtro

Esplorare AEM API GraphQL headless per filtrare i dati utilizzando un’ [SvelteKit](https://kit.svelte.dev/) app. Questa app SvelteKit crea un elenco di avventure WKND filtrabili per tipo di attività.

Questo codice illustra l&#39;utilizzo di Adobe [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da SvelteKit. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` query persistente e recupera i dettagli dell’avventura solo per le avventure del tipo di attività specificato.

Questo codice:

+ Si collega a un servizio AEM Publish e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
