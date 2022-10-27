---
title: App React di base
description: App React di base che presenta un elenco delle avventure WKND e i relativi dettagli
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
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 2%

---


# App React di base

Questo [Reagire](https://reactjs.org/) l’app illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti. Questa applicazione rende un filtro di WKND Avventures, e dopo aver selezionato un&#39;avventura, visualizza i dettagli completi delle avventure.

Questo codice:

+ Si collega a un servizio AEM Publish e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Per una revisione più approfondita della modalità di creazione di questa app Next.js, controlla il [esempio Documentazione dell’app React](../example-apps/react-app.md).
