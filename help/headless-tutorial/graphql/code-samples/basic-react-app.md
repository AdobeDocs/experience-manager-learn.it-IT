---
title: App React di base
description: Un’app React di base che mostra un elenco di avventure WKND e i relativi dettagli
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 2%

---

# App React di base

Questo [React](https://reactjs.org/) app illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti. Questa applicazione esegue il rendering di un’avventura filtrabile in WKND Adventures e, dopo aver selezionato un’avventura, ne visualizza i dettagli completi.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Per una recensione più approfondita della modalità di creazione di questa app Next.js, consulta [esempio di documentazione dell’app React](../example-apps/react-app.md).
