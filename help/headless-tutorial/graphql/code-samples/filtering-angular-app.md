---
title: Applicazione di Angular filtro
description: Una semplice app di Angular che filtra le attività WKND modellate utilizzando Frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 32
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Applicazione di Angular filtro

Esplora la capacità delle API GraphQL headless dell’AEM di filtrare i dati utilizzando una [Angular](https://angular.io/) app. In questo Angular viene creato un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra l’utilizzo di Adobe [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da Angular. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e derivare un elenco di tipi di attività disponibili. Quando un utente seleziona un Tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` persistente e recupera i dettagli dell’avventura solo per quelle avventure del tipo di attività specificato.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
