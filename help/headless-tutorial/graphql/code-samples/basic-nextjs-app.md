---
title: App Basic Next.js
description: Un’app di base Next.js che visualizza un elenco di avventure WKND e relativi dettagli
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---


# App Basic Next.js

Questo [Next.js](https://nextjs.org/) l’app illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti. Questa applicazione rende un filtro di WKND Avventures, e dopo aver selezionato un&#39;avventura, visualizza i dettagli completi delle avventure.

Questo codice:

+ Si collega a un servizio AEM Publish e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Per una revisione più approfondita della modalità di creazione di questa app Next.js, controlla il [esempio di documentazione dell’app Next.js](../example-apps/next-js.md).
