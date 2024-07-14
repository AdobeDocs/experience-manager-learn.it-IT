---
title: Filtraggio con jQuery e Handlebars
description: Un’implementazione di JavaScript che utilizza jQuery e Handlebars che filtra le avventure WKND da visualizzare. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Filtraggio con jQuery e Handlebars

Esplora le API GraphQL headless AEM per filtrare i dati utilizzando un&#39;app JavaScript che utilizza [jQuery](https://jquery.com/) e [Handlebars](https://handlebarsjs.com/). Questa app crea un elenco di attività WKND filtrabili per tipo di attività.

Questo codice illustra l&#39;utilizzo del client headless AEM [ di Adobe per JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) per richiamare query GraphQL persistenti. Questa app utilizza la query persistente `wknd-shared/adventures-all` per raccogliere tutte le avventure e ottenere un elenco dei tipi di attività disponibili. Quando un utente seleziona un tipo di attività, il tipo selezionato viene passato alla query persistente `wknd-shared/adventures-by-activity` e recupera i dettagli dell&#39;avventura solo per le avventure del tipo di attività specificato.

Questo codice:

+ Si connette a un servizio Publish AEM e non richiede l&#39;autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
