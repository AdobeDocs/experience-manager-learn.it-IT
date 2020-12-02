---
title: AEM Esercitazioni senza titolo
description: Una raccolta di esercitazioni su come utilizzare Adobe Experience Manager come CMS headless.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 5%

---


# AEM Esercitazioni senza titolo

Adobe Experience Manager offre diverse opzioni per definire endpoint headless e distribuire il contenuto come JSON. Utilizzate le esercitazioni pratiche per scoprire come usare le varie opzioni e scegliere quello che fa al caso vostro.

## Esercitazione sulle API AEM GraphQL

>[!CAUTION]
>
> L&#39;API AEM GraphQL per la distribuzione dei frammenti di contenuto verrà rilasciata all&#39;inizio del 2021.
> La documentazione correlata è disponibile a scopo di anteprima.

AEM GraphQL API per frammenti di contenuto
supporta scenari CMS headless in cui le applicazioni client esterne eseguono il rendering delle esperienze utilizzando contenuto gestito in AEM.

Una moderna API per la distribuzione dei contenuti è fondamentale per l&#39;efficienza e le prestazioni delle applicazioni frontend basate su JavaScript. L&#39;utilizzo di un&#39;API REST presenta delle problematiche:

* Numero elevato di richieste per il recupero di un oggetto alla volta
* Spesso il contenuto viene &quot;consegnato in eccesso&quot;, ovvero l&#39;applicazione riceve più di quanto necessario

Per superare queste sfide, GraphQL fornisce un&#39;API basata su query che consente ai client di eseguire query AEM solo per il contenuto necessario e di ricevere utilizzando una singola chiamata API.

* Scoprite come utilizzare AEM API GraphQL seguendo l&#39;esercitazione [Guida introduttiva alle API AEM GraphQL](./graphql/overview.md)

## Esercitazione su AEM Content Services

AEM Content Services sfrutta AEM pagine tradizionali per comporre endpoint API REST headless, e AEM componenti definiscono, o fanno riferimento, il contenuto da esporre su questi endpoint.

AEM Content Services consente le stesse astrazioni di contenuto utilizzate per la creazione di pagine Web in  AEM Sites, per definire il contenuto e gli schemi di queste API HTTP. L&#39;utilizzo di AEM pagine e componenti AEM consente agli esperti di marketing di comporre e aggiornare rapidamente API JSON flessibili in grado di alimentare qualsiasi applicazione.

* Scoprite come utilizzare AEM Content Services per seguire l&#39;esercitazione [Guida introduttiva AEM Content Services](./content-services/overview.md)

## AEM GraphQL e AEM Content Services

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definizione dello schema | Modelli di frammenti di contenuto strutturati | Componenti AEM |
| Contenuto | Frammenti di contenuto | Componenti AEM |
| Rilevamento dei contenuti | Per query GraphQL | Per AEM pagina |
| Formato di consegna | GraphQL JSON | AEM ComponentExporter JSON |

## Altre utili esercitazioni

Altre AEM esercitazioni relative ai concetti headless includono:

* [Guida introduttiva ad AEM SPA Editor e Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Guida introduttiva ad AEM SPA Editor e React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)