---
title: App React di base
description: Un’app React di base che mostra un elenco di avventure WKND e i relativi dettagli
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 17
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---

# App React di base

Questa app [React](https://reactjs.org/) illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti. Questa applicazione esegue il rendering di un’avventura filtrabile in WKND Adventures e, dopo aver selezionato un’avventura, ne visualizza i dettagli completi.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Per un&#39;analisi più approfondita della modalità di creazione dell&#39;app Next.js, consulta l&#39;[esempio di documentazione sull&#39;app React](../example-apps/react-app.md).
