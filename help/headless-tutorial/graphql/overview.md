---
title: Guida introduttiva a AEM Headless - GraphQL
description: Scopri le API GraphQL di Experience Manager e le relative funzionalità.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
source-git-commit: 332ad831b6c49e8599aa2181caf978d5626c1aba
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 2%

---

# Guida introduttiva a AEM Headless - GraphQL {#getting-started-with-aem-headless}

AEM le API GraphQL per i frammenti di contenuto supportano scenari CMS headless in cui le applicazioni client esterne eseguono il rendering delle esperienze utilizzando contenuti gestiti in AEM.

Un’API di distribuzione dei contenuti moderna è fondamentale per l’efficienza e le prestazioni delle applicazioni front-end basate su Javascript. L’utilizzo di un’API REST comporta delle sfide:

* Numerose richieste per il recupero di un oggetto alla volta
* Spesso il contenuto viene &quot;consegnato in eccesso&quot;, il che significa che l’applicazione riceve più di quanto necessario

Per superare queste sfide, GraphQL fornisce un’API basata su query che consente ai clienti di eseguire query AEM solo per il contenuto necessario e di ricevere utilizzando una singola chiamata API.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Questo video è una panoramica dell’API GraphQL implementata in AEM. L’API GraphQL in AEM è progettata principalmente per distribuire frammenti di contenuto AEM alle applicazioni downstream come parte di una distribuzione headless.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Inizia con AEM senza testa"
>abstract="Scopri come distribuire frammenti di contenuto utilizzando GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618" text="Panoramica di GraphQL in AEM"

## AEM serie video GraphQL headless

Scopri AEM funzionalità di GraphQL tramite la dettagliata procedura dei frammenti di contenuto e le API e gli strumenti di sviluppo di GraphQL AEM .

* [AEM serie video GraphQL headless](./video-series/modeling-basics.md)

## Tutorial AEM graficoQL basato sulle mani

Scopri AEM funzionalità GraphQL creando un’app React che consuma frammenti di contenuto tramite API GraphQL AEM.

* [Tutorial AEM graficoQL basato sulle mani](./multi-step/overview.md)

## AEM GraphQL e i servizi per contenuti AEM

|  | AEM API GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definizione dello schema | Modelli per frammenti di contenuto strutturati | Componenti AEM |
| Contenuto | Frammenti di contenuto | Componenti AEM |
| Ricerca dei contenuti | Per query GraphQL | Per AEM pagina |
| Formato di consegna | JSON GraphQL | JSON AEM ComponentExporter |
