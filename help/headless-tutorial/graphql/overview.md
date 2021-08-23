---
title: Guida introduttiva a AEM Headless - GraphQL
description: Panoramica delle API e funzionalità di AEM GraphQL.
feature: Frammenti di contenuto, API GraphQL, API
topic: Senza testa, gestione dei contenuti
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---


# Guida introduttiva a AEM Headless - GraphQL

AEM API GraphQL per frammenti di contenuto
supporta scenari CMS headless in cui le applicazioni client esterne eseguono il rendering delle esperienze utilizzando contenuti gestiti in AEM.

Un’API di distribuzione dei contenuti moderna è fondamentale per l’efficienza e le prestazioni delle applicazioni front-end basate su Javascript. L’utilizzo di un’API REST comporta delle sfide:

* Numerose richieste per il recupero di un oggetto alla volta
* Spesso il contenuto viene &quot;consegnato in eccesso&quot;, il che significa che l’applicazione riceve più di quanto necessario

Per superare queste sfide, GraphQL fornisce un’API basata su query che consente ai clienti di eseguire query AEM solo per il contenuto necessario e di ricevere utilizzando una singola chiamata API.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Questo video è una panoramica dell’API GraphQL implementata in AEM. L’API GraphQL in AEM è progettata principalmente per distribuire frammenti di contenuto AEM alle applicazioni downstream come parte di una distribuzione headless.

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