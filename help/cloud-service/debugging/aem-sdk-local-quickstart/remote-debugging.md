---
title: Debug remoto dell’SDK AEM
description: L'avvio rapido locale dell'SDK AEM consente il debug Java remoto dall'IDE, consentendo di analizzare l'esecuzione del codice live in AEM per comprendere il flusso di esecuzione esatto.
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
source-wordcount: '275'
ht-degree: 0%

---


# Debug remoto dell’SDK AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

L&#39;avvio rapido locale dell&#39;SDK AEM consente il debug Java remoto dall&#39;IDE, consentendo di analizzare l&#39;esecuzione del codice live in AEM per comprendere il flusso di esecuzione esatto.

Per connettere un debugger remoto a AEM, è necessario avviare il quickstart locale dell&#39;SDK AEM con parametri specifici (`-agentlib:...`) che consentano all&#39;IDE di connettersi ad esso.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` specifica la porta AEM ascolto per le connessioni di debug remote e può essere modificata in qualsiasi porta disponibile nel computer di sviluppo locale.
+ L&#39;ultimo parametro (ad esempio `aem-author-p4502.jar`) è la AEM SKD QuickStart Jar. Può trattarsi del servizio AEM Author (`aem-author-p4502.jar`) o del servizio AEM Publish (`aem-publish-p4503.jar`).

## Istruzioni per l&#39;impostazione dell&#39;IDE

La maggior parte degli IDE Java fornisce il supporto per il debug remoto dei programmi Java, ma i passaggi esatti di configurazione di ogni IDE variano. Controlla le istruzioni di configurazione del debug remoto dell&#39;IDE per i passaggi esatti. In genere, le configurazioni IDE richiedono:

+ Il servizio di avvio rapido locale AEM SDK dell&#39;host è in ascolto, ovvero `localhost`.
+ La porta AEM&#39;avvio rapido locale dell&#39;SDK è in ascolto per la connessione di debug remota, che è la porta specificata dal parametro `address` quando si avvia AEM&#39;avvio rapido locale dell&#39;SDK.
+ Occasionalmente, devono essere specificati i progetti Maven che forniscono il codice sorgente al debug remoto; questo è il vostro progetto o progetti di aggregazione OSGi.

### Configurare le istruzioni

+ [Configurazione del debugger remoto Java del codice VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA Remote Debugger configurato](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Configurazione del debugger remoto Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
