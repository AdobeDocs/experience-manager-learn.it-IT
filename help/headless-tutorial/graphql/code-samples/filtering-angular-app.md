---
title: Applicazione Angular di filtro
description: Un semplice Angular di app che filtra le avventure WKND modellate utilizzando frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Applicazione Angular di filtro

Scopri AEM le API GraphQL headless per filtrare i dati utilizzando un [Angular](https://angular.io/) app. Questa app di Angular crea un elenco di avventure WKND filtrabili per tipo di attività.

Questo codice illustra l&#39;utilizzo di Adobe [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da Angular. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` query persistente e recupera i dettagli dell’avventura solo per le avventure del tipo di attività specificato.

Questo codice:

+ Si collega a un servizio AEM Publish e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
