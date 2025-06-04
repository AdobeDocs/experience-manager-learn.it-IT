---
title: Introduzione ad AEM Headless - GraphQL
description: Informazioni sulle API GraphQL di Experience Manager e sulle relative funzionalità.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '266'
ht-degree: 100%

---

# Guida introduttiva ad AEM Headless - GraphQL {#getting-started-with-aem-headless}

Le API GraphQL di AEM per i frammenti di contenuto
supportano scenari CMS headless in cui le applicazioni client esterne eseguono il rendering delle esperienze utilizzando contenuti gestiti in AEM.

Una moderna API per la distribuzione dei contenuti è fondamentale per l’efficienza e le prestazioni delle applicazioni front-end basate su JavaScript. L’utilizzo di un’API REST presenta alcune difficoltà:

* Numero elevato di richieste per il recupero di un oggetto alla volta
* Contenuto distribuito spesso “in eccesso”, ovvero l’applicazione riceve più del necessario

Per superare queste difficoltà, GraphQL fornisce un’API basata su query che consente ai client di eseguire query su AEM solo per il contenuto di cui necessitano, e di ricevere utilizzando una singola chiamata API.

>[!VIDEO](https://video.tv.adobe.com/v/3452887?quality=12&learn=on&captions=ita)

Questo video offre una panoramica dell’API GraphQL implementata in AEM. L’API GraphQL in AEM è progettata principalmente per distribuire i frammenti di contenuto AEM alle applicazioni a valle come parte di una distribuzione headless.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Guida introduttiva ad AEM Headless - GraphQL"
>abstract="Scopri come distribuire frammenti di contenuto utilizzando GraphQL."
>additional-url="https://video.tv.adobe.com/v/3452887?captions=ita" text="Panoramica di GraphQL in AEM"

## Serie video su GraphQL di AEM Headless

Informazioni sulle funzionalità GraphQL di AEM tramite la procedura dettagliata di frammenti di contenuto, API GraphQL e strumenti di sviluppo di AEM.

* [Serie video su GraphQL di AEM Headless](./video-series/modeling-basics.md)

## Tutorial pratico su GraphQL di AEM Headless

Esplora le funzionalità GraphQL di AEM creando un’app React che utilizza frammenti di contenuto tramite le API GraphQL di AEM.

* [Tutorial pratico su GraphQL di AEM Headless](./multi-step/overview.md)

## Confronto tra AEM GraphQL e AEM Content Services

|                                | API GraphQL di AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definizione dello schema | Modelli per frammenti di contenuto strutturati | Componenti di AEM |
| Contenuto | Frammenti di contenuto | Componenti di AEM |
| Individuazione del contenuto | Per query basata su GraphQL | Per pagina AEM |
| Formato di consegna | JSON di GraphQL | JSON ComponentExporter di AEM |
