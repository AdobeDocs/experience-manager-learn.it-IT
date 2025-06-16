---
title: App Next.js di base
description: Un’app Next.js di base che mostra un elenco di avventure WKND e i relativi dettagli
version: Experience Manager as a Cloud Service
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
duration: 22
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 6%

---

# App Next.js di base

Questa app [Next.js](https://nextjs.org/) illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti. Questa applicazione esegue il rendering di un’avventura filtrabile in WKND Adventures e, dopo aver selezionato un’avventura, ne visualizza i dettagli completi.

Questo codice:

+ Si connette a un servizio di pubblicazione AEM e non richiede l’autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

>[!IMPORTANT]
>
> Codesandbox.io non supporta la modifica dell’applicazione Next.js nell’IDE incorporato. Per modificare questo esempio di codice, [apri l&#39;app Next.js direttamente su codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
