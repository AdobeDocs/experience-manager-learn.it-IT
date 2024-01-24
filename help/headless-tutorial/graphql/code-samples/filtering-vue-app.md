---
title: Applicazione filtro Vue
description: Una semplice app Vue che filtra le attività WKND modellate utilizzando Frammenti di contenuto.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 8f96093a-4449-4249-9257-028e2ffd979b
duration: 28
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Applicazione filtro Vue

Esplora la capacità delle API GraphQL headless dell’AEM di filtrare i dati utilizzando una [Vue](https://vuejs.org/) app. Questa app React crea un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra l’utilizzo di Adobe [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti da Vue. Questa app utilizza `wknd-shared/adventures-all` query persistente per raccogliere tutte le avventure e derivare un elenco di tipi di attività disponibili. Quando un utente seleziona un Tipo di attività, il tipo selezionato viene passato al `wknd-shared/adventures-by-activity` persistente e recupera i dettagli dell’avventura solo per quelle avventure del tipo di attività specificato.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
