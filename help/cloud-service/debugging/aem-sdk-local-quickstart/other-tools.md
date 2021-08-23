---
title: Altri strumenti per il debug AEM SDK
description: Diversi altri strumenti possono facilitare il debug dell'avvio rapido locale dell'SDK AEM.
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Sviluppo
role: Developer
level: Beginner, Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 7%

---


# Altri strumenti per il debug AEM SDK

L’avvio rapido locale dell’SDK di AEM può essere utile con diversi altri strumenti per il debug dell’applicazione.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite è un’interfaccia basata su Web per interagire con JCR, AEM archivio dati. CRXDE Lite offre una visibilità completa del JCR, compresi nodi, proprietà, valori di proprietà e autorizzazioni.

CRXDE Lite si trova in:

+ Strumenti > Generale > CRXDE Lite
+ o direttamente da [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Spiega query

![Spiega query](./assets/other-tools/explain-query.png)

Spiega lo strumento basato su web Query nell’avvio rapido locale AEM’SDK, che fornisce informazioni chiave su come AEM interpreta ed esegue le query, e uno strumento prezioso per garantire che le query vengano eseguite in modo performante da AEM.

Spiega query si trova in:

+ Strumenti > Diagnosi > Prestazioni query > Scheda Spiega query
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)  > Scheda Spiega query

## Debugger di QueryBuilder

![Debugger di QueryBuilder](./assets/other-tools/query-debugger.png)

Il debugger di QueryBuilder è uno strumento basato su Web che consente di eseguire il debug e comprendere le query di ricerca utilizzando AEM sintassi [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

Il debugger di QueryBuilder si trova in:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

