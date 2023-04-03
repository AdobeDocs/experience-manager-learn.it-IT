---
title: Eseguire il debug remoto dell’SDK AEM
description: L’avvio rapido locale dell’SDK AEM consente il debug remoto di Java dall’IDE, consentendoti di analizzare l’esecuzione del codice in tempo reale in AEM per comprendere il flusso di esecuzione esatto.
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---

# Eseguire il debug remoto dell’SDK AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

L’avvio rapido locale dell’SDK AEM consente il debug remoto di Java dall’IDE, consentendoti di analizzare l’esecuzione del codice in tempo reale in AEM per comprendere il flusso di esecuzione esatto.

Per collegare un debugger remoto a AEM, è necessario avviare l&#39;avvio rapido locale dell&#39;SDK AEM con parametri specifici (`-agentlib:...`) che consente all&#39;IDE di connettersi ad esso.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` specifica la porta AEM di ascolto per le connessioni di debug remote e può essere modificata in qualsiasi porta disponibile nel computer di sviluppo locale.
+ L’ultimo parametro (ad esempio `aem-author-p4502.jar`) è AEM SKD Quickstart Jar. Può trattarsi del servizio AEM Author (`aem-author-p4502.jar`) o il servizio AEM Publish (`aem-publish-p4503.jar`).

## Istruzioni per la configurazione dell&#39;IDE

La maggior parte degli IDE Java forniscono supporto per il debug remoto dei programmi Java, tuttavia i passaggi di configurazione esatti di ciascun IDE variano. Controlla le istruzioni di configurazione del debug remoto dell&#39;IDE per i passaggi esatti. In genere le configurazioni IDE richiedono:

+ L&#39;avvio rapido locale dell&#39;SDK dell&#39;host AEM è in ascolto, ovvero `localhost`.
+ La porta AEM&#39;avvio rapido locale dell&#39;SDK è in ascolto per la connessione di debug remoto, che è la porta specificata da `address` quando si avvia AEM&#39;avvio rapido locale dell&#39;SDK.
+ Occasionalmente, è necessario specificare i progetti Maven che forniscono il codice sorgente al debug remoto; questo è il tuo progetto/i di progetti Maven bundle OSGi.

### Istruzioni per la configurazione

+ [Configurazione del debugger remoto Java del codice VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configurazione del debugger remoto IntelliJ IDEA](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Configurazione del debugger remoto Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
