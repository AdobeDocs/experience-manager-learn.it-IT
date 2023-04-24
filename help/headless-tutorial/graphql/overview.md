---
title: Guida introduttiva a AEM Headless - GraphQL
description: Scopri le API GraphQL di Experience Manager e le relative funzionalità.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 11%

---

# Guida introduttiva ad AEM Headless: GraphQL {#getting-started-with-aem-headless}

AEM le API GraphQL per i frammenti di contenuto supporta scenari CMS headless in cui le applicazioni client esterne eseguono il rendering delle esperienze utilizzando contenuti gestiti in AEM.

Un’API di distribuzione dei contenuti moderna è fondamentale per l’efficienza e le prestazioni delle applicazioni front-end basate su Javascript. L’utilizzo di un’API REST comporta delle sfide:

* Numerose richieste per il recupero di un oggetto alla volta
* Spesso il contenuto viene &quot;consegnato in eccesso&quot;, il che significa che l’applicazione riceve più di quanto necessario

Per superare queste sfide, GraphQL fornisce un’API basata su query che consente ai clienti di eseguire query AEM solo per il contenuto necessario e di ricevere utilizzando una singola chiamata API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Questo video è una panoramica dell’API GraphQL implementata in AEM. L’API GraphQL in AEM è progettata principalmente per distribuire frammenti di contenuto AEM alle applicazioni downstream come parte di un’implementazione headless.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Guida introduttiva ad AEM Headless: GraphQL"
>abstract="Scopri come distribuire frammenti di contenuto utilizzando GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618/?captions=ita" text="Panoramica di GraphQL in AEM"

## AEM serie video GraphQL headless

Scopri AEM funzionalità di GraphQL tramite la dettagliata documentazione sui frammenti di contenuto e le API e gli strumenti di sviluppo di GraphQL AEM.

* [AEM serie video GraphQL headless](./video-series/modeling-basics.md)

## Tutorial AEM senza testa di GraphQL

Scopri AEM funzionalità di GraphQL creando un’app React che consuma frammenti di contenuto tramite AEM API GraphQL.

* [Tutorial AEM senza testa di GraphQL](./multi-step/overview.md)

## AEM GraphQL e servizi di contenuti AEM

|  | API di GraphQL AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definizione dello schema | Modelli per frammenti di contenuto strutturati | Componenti AEM |
| Contenuto | Frammenti di contenuto | Componenti AEM |
| Ricerca dei contenuti | Query per GraphQL | Per AEM pagina |
| Formato di consegna | JSON GraphQL | JSON AEM ComponentExporter |
