---
title: Filtraggio dell’app React
description: Una semplice app React che filtra le attività WKND modellate utilizzando Frammenti di contenuto.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtraggio dell’app React

Scopri le API AEM Headless GraphQL per filtrare i dati tramite un&#39;app [React](https://reactjs.org/). Questa app React crea un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra l&#39;utilizzo di [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) di Adobe per richiamare query GraphQL persistenti da React. Questa app utilizza la query persistente `wknd-shared/adventures-all` per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato alla query persistente `wknd-shared/adventures-by-activity` e recupera i dettagli dell&#39;avventura solo per le avventure del tipo di attività specificato.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
