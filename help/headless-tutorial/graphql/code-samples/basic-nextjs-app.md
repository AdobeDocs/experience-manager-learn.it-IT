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
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# App Next.js di base

Questa app [Next.js](https://nextjs.org/) illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell&#39;AEM tramite query persistenti. Questa applicazione esegue il rendering di un’avventura filtrabile in WKND Adventures e, dopo aver selezionato un’avventura, ne visualizza i dettagli completi.

Questo codice:

+ Si connette a un servizio Publish AEM e non richiede l&#39;autenticazione
+ Utilizza le query persistenti WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Per un&#39;analisi più approfondita della modalità di creazione dell&#39;app Next.js, consulta la [documentazione dell&#39;app Next.js esempio](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io non supporta la modifica dell’applicazione Next.js nell’IDE incorporato. Per modificare questo esempio di codice, [apri l&#39;app Next.js direttamente su codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
