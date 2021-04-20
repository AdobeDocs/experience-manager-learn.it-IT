---
title: Eseguire il debug remoto dell’SDK di AEM
description: L’avvio rapido locale dell’SDK AEM consente il debug remoto di Java dall’IDE, consentendoti di analizzare l’esecuzione di codice in tempo reale in AEM per comprendere il flusso di esecuzione esatto.
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
source-wordcount: '281'
ht-degree: 1%

---


# Eseguire il debug remoto dell’SDK di AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

L’avvio rapido locale dell’SDK AEM consente il debug remoto di Java dall’IDE, consentendoti di analizzare l’esecuzione di codice in tempo reale in AEM per comprendere il flusso di esecuzione esatto.

Per collegare un debugger remoto ad AEM, è necessario avviare l’avvio rapido locale dell’SDK AEM con parametri specifici (`-agentlib:...`) che consentano all’IDE di connettersi ad esso.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` specifica la porta su cui AEM ascolta le connessioni di debug remote e può essere modificata in qualsiasi porta disponibile nel computer di sviluppo locale.
+ L’ultimo parametro (ad esempio `aem-author-p4502.jar`) è il Jar AEM SKD Quickstart. Può essere il servizio di authoring AEM (`aem-author-p4502.jar`) o il servizio di pubblicazione AEM (`aem-publish-p4503.jar`).

## Istruzioni per la configurazione dell&#39;IDE

La maggior parte degli IDE Java forniscono supporto per il debug remoto dei programmi Java, tuttavia i passaggi di configurazione esatti di ciascun IDE variano. Controlla le istruzioni di configurazione del debug remoto dell&#39;IDE per i passaggi esatti. In genere le configurazioni IDE richiedono:

+ L’avvio rapido locale dell’SDK AEM dell’host è in ascolto, ovvero `localhost`.
+ L&#39;avvio rapido locale dell&#39;SDK AEM porta è in ascolto per la connessione di debug remoto, che è la porta specificata dal parametro `address` all&#39;avvio dell&#39;avvio rapido locale dell&#39;SDK AEM.
+ Occasionalmente, è necessario specificare i progetti Maven che forniscono il codice sorgente al debug remoto; questo è il tuo progetto/i di progetti Maven bundle OSGi.

### Istruzioni per la configurazione

+ [Configurazione del debugger remoto Java del codice VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configurazione del debugger remoto IntelliJ IDEA](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Configurazione del debugger remoto Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
