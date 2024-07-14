---
title: Guida introduttiva a AEM headless - GraphQL
description: Scopri le API GraphQL di Experience Manager e le loro funzionalità.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 10%

---

# Guida introduttiva ad AEM Headless: GraphQL {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

API GraphQL dell’AEM per i frammenti di contenuto
supporta scenari CMS headless in cui le applicazioni client esterne riproducono le esperienze utilizzando contenuti gestiti in AEM.

Una moderna API di distribuzione dei contenuti è fondamentale per l’efficienza e le prestazioni delle applicazioni front-end basate su JavaScript. L’utilizzo di un’API REST presenta alcune difficoltà:

* Numero elevato di richieste per il recupero di un oggetto alla volta
* Contenuti spesso &quot;in eccesso&quot;, ovvero l’applicazione riceve più del necessario

Per superare queste sfide, GraphQL fornisce un’API basata su query che consente ai clienti di eseguire query sull’AEM solo per il contenuto di cui hanno bisogno e di ricevere utilizzando una singola chiamata API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Questo video offre una panoramica dell’API GraphQL implementata nell’AEM. L’API GraphQL nell’AEM è progettata principalmente per distribuire i frammenti di contenuto AEM alle applicazioni a valle come parte di una distribuzione headless.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Guida introduttiva ad AEM Headless: GraphQL"
>abstract="Scopri come distribuire frammenti di contenuto utilizzando GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618/?captions=ita" text="Panoramica di GraphQL in AEM"

## Serie video GraphQL headless AEM

Scopri le funzionalità GraphQL dell’AEM tramite la procedura dettagliata dettagliata relativa a Frammenti di contenuto, API GraphQL dell’AEM e strumenti di sviluppo.

* [Serie video GraphQL headless AEM](./video-series/modeling-basics.md)

## Tutorial pratico su GraphQL headless AEM

Esplora le funzionalità GraphQL dell’AEM creando un’app React che utilizza frammenti di contenuto tramite le API GraphQL dell’AEM.

* [Tutorial pratico su GraphQL headless AEM](./multi-step/overview.md)

## GraphQL AEM e servizi di contenuti AEM

|                                | API GraphQL dell’AEM | Servizi di contenuti AEM |
|--------------------------------|:-----------------|:---------------------|
| Definizione schema | Modelli per frammenti di contenuto strutturati | Componenti AEM |
| Contenuto | Frammenti di contenuto | Componenti AEM |
| Individuazione dei contenuti | Query basata su GraphQL | Per pagina AEM |
| Formato di consegna | JSON GRAPHQL | JSON per l’esportazione di componenti AEM |
