---
title: AEM Esercitazioni senza titolo
description: Una raccolta di esercitazioni su come utilizzare Adobe Experience Manager come CMS headless.
translation-type: tm+mt
source-git-commit: eabd8650886fa78d9d177f3c588374a443ac1ad6
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 4%

---


# AEM Esercitazioni senza titolo

Adobe Experience Manager offre diverse opzioni per definire endpoint headless e distribuire il contenuto come JSON. Utilizzate le esercitazioni pratiche per scoprire come usare le varie opzioni e scegliere quello che fa al caso vostro.

## Esercitazione sulle API AEM GraphQL

>[!CAUTION]
>
> L&#39;API AEM GraphQL per la distribuzione dei frammenti di contenuto è disponibile su richiesta.
> Per abilitare l&#39;API per il AEM come programma di Cloud Service, contattate  supporto di Adobe.

AEM GraphQL API per frammenti di contenuto
supporta scenari CMS headless in cui le applicazioni client esterne eseguono il rendering delle esperienze utilizzando contenuto gestito in AEM.

Una moderna API per la distribuzione dei contenuti è fondamentale per l&#39;efficienza e le prestazioni delle applicazioni frontend basate su JavaScript. L&#39;utilizzo di un&#39;API REST presenta delle problematiche:

* Numero elevato di richieste per il recupero di un oggetto alla volta
* Spesso il contenuto viene &quot;consegnato in eccesso&quot;, ovvero l&#39;applicazione riceve più di quanto necessario

Per superare queste sfide, GraphQL fornisce un&#39;API basata su query che consente ai client di eseguire query AEM solo per il contenuto necessario e di ricevere utilizzando una singola chiamata API.

* Scoprite come utilizzare AEM GraphQL API nella [Esercitazione iniziale con AEM GraphQL APIs tutorial](./graphql/overview.md)

## Esercitazione di autenticazione basata su token

AEM espone una varietà di endpoint HTTP con cui è possibile interagire in modo headless, da GraphQL, AEM Content Services all&#39;API HTTP Assets. Spesso, questi consumatori headless potrebbero dover autenticarsi per AEM per accedere ai contenuti o alle azioni protetti. Per facilitare questa fase, AEM supporta l&#39;autenticazione basata su token delle richieste HTTP da applicazioni, servizi o sistemi esterni.

* Scoprite come autenticarsi per AEM tramite HTTP utilizzando i token di accesso in [Autenticazione per AEM come Cloud Service da un&#39;esercitazione di applicazione esterna](./authentication/overview.md)

## Esercitazione su AEM Content Services

AEM Content Services sfrutta AEM pagine tradizionali per comporre endpoint API REST headless, e AEM componenti definiscono, o fanno riferimento, il contenuto da esporre su questi endpoint.

AEM Content Services consente le stesse astrazioni di contenuto utilizzate per la creazione di pagine Web in  AEM Sites, per definire il contenuto e gli schemi di queste API HTTP. L&#39;utilizzo di AEM pagine e componenti AEM consente agli esperti di marketing di comporre e aggiornare rapidamente API JSON flessibili in grado di alimentare qualsiasi applicazione.

* Scoprite come utilizzare AEM Content Services nella [Guida introduttiva a AEM Content Services tutorial](./content-services/overview.md)

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