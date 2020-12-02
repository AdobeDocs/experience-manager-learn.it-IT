---
title: Altri strumenti per il debug AEM SDK
description: Diversi altri strumenti possono facilitare il debug del QuickStart locale dell’SDK AEM.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 3%

---


# Altri strumenti per il debug AEM SDK

Diversi altri strumenti possono facilitare il debug dell’applicazione sul quickstart locale dell’SDK AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite è un&#39;interfaccia basata sul Web per interagire con il repository AEM dati JCR. Il CRXDE Lite fornisce una visibilità completa nel JCR, compresi nodi, proprietà, valori delle proprietà e autorizzazioni.

CRXDE Lite si trova in:

+ Strumenti > Generale > CRXDE Lite
+ o direttamente da [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Spiega query

![Spiega query](./assets/other-tools/explain-query.png)

Spiega lo strumento basato su Web Query in AEM QuickStart locale dell’SDK, che fornisce informazioni chiave su come AEM interpreta ed esegue le query, e uno strumento prezioso per garantire che le query vengano eseguite in modo efficace da AEM.

Spiega query si trova in:

+ Strumenti > Diagnosi > Prestazioni query > Scheda Spiega query
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Scheda Spiega query

## Debugger QueryBuilder

![Debugger QueryBuilder](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger è uno strumento basato sul Web che consente di eseguire il debug e comprendere le query di ricerca utilizzando AEM sintassi [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

Il debugger di QueryBuilder si trova in:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer e AEM plug-in Chrome

![Sling Log Tracer e AEM plug-in Chrome](./assets/other-tools/log-tracer.png)

[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html), fornito con il servizio QuickStart locale AEM SDK, consente di tracciare in modo approfondito le richieste HTTP, esponendo informazioni di debug dettagliate per ogni richiesta. Per abilitare questa funzione, è necessario configurare la configurazione [Log Tracer OSGi](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1).

Il [AEM plug-in Chrome ](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US) open source per il [browser Web Google Chrome](https://www.google.com/chrome/), si integra con Log Tracer, esponendo le informazioni di debug direttamente in Chrome&#39;s Dev Tools.

_Il plug-in AEM Chrome è uno strumento open source e  Adobe non fornisce supporto per esso._

