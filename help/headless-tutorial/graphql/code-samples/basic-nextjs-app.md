---
title: App Next.js di base
description: Un’app Next.js di base che mostra un elenco di avventure WKND e i relativi dettagli
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 29
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# App Next.js di base

Questo [Next.js](https://nextjs.org/) app illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti. Questa applicazione esegue il rendering di un’avventura filtrabile in WKND Adventures e, dopo aver selezionato un’avventura, ne visualizza i dettagli completi.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti di WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Per una recensione più approfondita della modalità di creazione di questa app Next.js, consulta [esempio di documentazione dell’app Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io non supporta la modifica dell’applicazione Next.js nell’IDE incorporato. Per modificare questo esempio di codice: [apri l’app Next.js direttamente su codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
