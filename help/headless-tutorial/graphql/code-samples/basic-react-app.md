---
title: App React di base
description: Un’app React di base che mostra un elenco di avventure WKND e i relativi dettagli
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---

# App React di base

Questa app [React](https://reactjs.org/) illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell&#39;AEM utilizzando query persistenti. Questa applicazione esegue il rendering di un’avventura filtrabile in WKND Adventures e, dopo aver selezionato un’avventura, ne visualizza i dettagli completi.

Questo codice:

+ Si connette a un servizio Publish AEM e non richiede l&#39;autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Per un&#39;analisi più approfondita della modalità di creazione dell&#39;app Next.js, consulta l&#39;[esempio di documentazione sull&#39;app React](../example-apps/react-app.md).
