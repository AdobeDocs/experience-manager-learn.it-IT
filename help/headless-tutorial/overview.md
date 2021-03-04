---
title: Tutorial su AEM Headless
description: Raccolta di esercitazioni sull’utilizzo di Adobe Experience Manager as a Headless CMS.
feature: Frammenti di contenuto, API
topic: Senza testa, gestione dei contenuti
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 4%

---


# Tutorial su AEM Headless

Adobe Experience Manager offre diverse opzioni per definire endpoint headless e distribuire il relativo contenuto come JSON. Utilizza i tutorial pratici per scoprire come utilizzare le varie opzioni e scegliere ciò che fa per te.

## Esercitazione sulle API di AEM GraphQL

>[!CAUTION]
>
> L’API GraphQL di AEM per la distribuzione di frammenti di contenuto è disponibile su richiesta.
> Contatta il supporto Adobe per abilitare l’API per il tuo programma AEM as a Cloud Service.

API GraphQL di AEM per frammenti di contenuto
supporta scenari CMS headless in cui le applicazioni client esterne eseguono il rendering delle esperienze utilizzando contenuti gestiti in AEM.

Un’API di distribuzione dei contenuti moderna è fondamentale per l’efficienza e le prestazioni delle applicazioni front-end basate su Javascript. L’utilizzo di un’API REST comporta delle sfide:

* Numerose richieste per il recupero di un oggetto alla volta
* Spesso il contenuto viene &quot;consegnato in eccesso&quot;, il che significa che l’applicazione riceve più di quanto necessario

Per superare queste sfide, GraphQL fornisce un’API basata su query che consente ai clienti di eseguire query su AEM solo per il contenuto necessario e di ricevere utilizzando una singola chiamata API.

* Scopri come utilizzare le API GraphQL di AEM nell’ esercitazione [Guida introduttiva alle API GraphQL di AEM](./graphql/overview.md)

## Esercitazione sull’autenticazione basata su token

AEM espone una varietà di endpoint HTTP con cui è possibile interagire in modo headless, da GraphQL, AEM Content Services all’API HTTP Assets. Spesso, questi consumatori headless possono dover eseguire l’autenticazione in AEM per accedere a contenuti o azioni protetti. Per facilitare questa fase, AEM supporta l’autenticazione basata su token delle richieste HTTP provenienti da applicazioni, servizi o sistemi esterni.

* Scopri come eseguire l’autenticazione in AEM tramite HTTP utilizzando i token di accesso in [Autenticazione in AEM as a Cloud Service tramite un’esercitazione su un’applicazione esterna](./authentication/overview.md)

## Esercitazione su AEM Content Services

I Content Services di AEM sfruttano le pagine AEM tradizionali per comporre endpoint API REST headless e i componenti AEM definiscono, o fanno riferimento, i contenuti da esporre su questi endpoint.

AEM Content Services consente di definire il contenuto e gli schemi di queste API HTTP mediante le stesse astrazioni di contenuto utilizzate per la creazione di pagine web in AEM Sites. L’utilizzo di AEM Pages e dei componenti AEM consente agli addetti al marketing di comporre e aggiornare rapidamente API JSON flessibili che possono alimentare qualsiasi applicazione.

* Scopri come utilizzare i Content Services di AEM nell’ esercitazione [Guida introduttiva ad AEM Content Services](./content-services/overview.md)

## Confronto tra AEM GraphQL e AEM Content Services

|  | API GraphQL di AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definizione dello schema | Modelli per frammenti di contenuto strutturati | Componenti AEM |
| Contenuto | Frammenti di contenuto | Componenti AEM |
| Ricerca dei contenuti | Per query GraphQL | Per pagina AEM |
| Formato di consegna | JSON GraphQL | JSON AEM ComponentExporter |

## Altre esercitazioni utili

Altre esercitazioni di AEM relative a concetti headless includono:

* [Guida introduttiva ad AEM SPA Editor e Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Guida introduttiva ad AEM SPA Editor e React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)