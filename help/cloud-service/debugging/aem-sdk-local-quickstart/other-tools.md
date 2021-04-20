---
title: Altri strumenti per il debug dell’SDK di AEM
description: Diversi altri strumenti possono facilitare il debug dell’avvio rapido locale dell’SDK AEM.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 5%

---


# Altri strumenti per il debug dell’SDK di AEM

L’avvio rapido locale dell’SDK AEM consente di eseguire il debug dell’applicazione con altri strumenti.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite è un&#39;interfaccia basata sul Web per interagire con JCR, l&#39;archivio dati di AEM. CRXDE Lite offre una visibilità completa nel JCR, compresi nodi, proprietà, valori di proprietà e autorizzazioni.

CRXDE Lite si trova in:

+ Strumenti > Generale > CRXDE Lite
+ o direttamente da [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Spiega query

![Spiega query](./assets/other-tools/explain-query.png)

Spiega lo strumento web basato su query nell’avvio rapido locale dell’SDK di AEM, che fornisce informazioni chiave su come AEM interpreta ed esegue le query, e uno strumento prezioso per garantire che le query vengano eseguite in modo performante da AEM.

Spiega query si trova in:

+ Strumenti > Diagnosi > Prestazioni query > Scheda Spiega query
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)  > Scheda Spiega query

## Debugger di QueryBuilder

![Debugger di QueryBuilder](./assets/other-tools/query-debugger.png)

Il debugger di QueryBuilder è uno strumento basato su Web che consente di eseguire il debug e comprendere le query di ricerca utilizzando la sintassi [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) di AEM.

Il debugger di QueryBuilder si trova in:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer e plug-in AEM Chrome

![Sling Log Tracer e plug-in AEM Chrome](./assets/other-tools/log-tracer.png)

[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html), fornito con l’avvio rapido locale dell’SDK di AEM, consente un tracciamento approfondito delle richieste HTTP, esponendo informazioni di debug approfondite per richiesta. Per abilitare questa funzione, è necessario configurare la configurazione [Log Tracer OSGi](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1).

Il plug-in open source [AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US) per il [browser Web Google Chrome](https://www.google.com/chrome/) si integra con Log Tracer, esponendo le informazioni di debug direttamente in Chrome’s Dev Tools.

_Il plug-in AEM Chrome è uno strumento open source e Adobe non lo supporta._

