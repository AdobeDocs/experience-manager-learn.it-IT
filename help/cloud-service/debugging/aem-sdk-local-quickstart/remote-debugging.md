---
title: Eseguire il debug remoto dell’SDK dell’AEM
description: L’avvio rapido locale dell’SDK dell’AEM consente il debug Java remoto dall’IDE, che consente di analizzare l’esecuzione del codice in tempo reale nell’AEM per comprendere l’esatto flusso di esecuzione.
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Eseguire il debug remoto dell’SDK dell’AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

L’avvio rapido locale dell’SDK dell’AEM consente il debug Java remoto dall’IDE, che consente di analizzare l’esecuzione del codice in tempo reale nell’AEM per comprendere l’esatto flusso di esecuzione.

Per connettere un debugger remoto all&#39;AEM, è necessario avviare l&#39;avvio rapido locale dell&#39;SDK dell&#39;AEM con parametri specifici (`-agentlib:...`) che consentano all&#39;IDE di connettersi ad esso.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ L’SDK per AEM supporta solo Java 11
+ `address` specifica la porta su cui l&#39;AEM è in ascolto per le connessioni di debug remoto e può essere modificata in qualsiasi porta disponibile nel computer di sviluppo locale.
+ L&#39;ultimo parametro (esempio: `aem-author-p4502.jar`) è il file JAR Quickstart SKD dell&#39;AEM. Può essere il servizio di creazione AEM (`aem-author-p4502.jar`) o il servizio di Publish AEM (`aem-publish-p4503.jar`).


## Istruzioni per la configurazione di IDE

La maggior parte degli IDE Java fornisce il supporto per il debug remoto dei programmi Java, tuttavia i passaggi di configurazione esatti di ogni IDE variano. Verificare le istruzioni di impostazione del debug remoto dell&#39;IDE per i passaggi esatti. In genere, le configurazioni IDE richiedono:

+ L&#39;avvio rapido locale dell&#39;SDK dell&#39;AEM host è in ascolto su, ovvero `localhost`.
+ L&#39;avvio rapido locale dell&#39;SDK AEM della porta è in ascolto della connessione di debug remoto, che è la porta specificata dal parametro `address` all&#39;avvio dell&#39;avvio rapido locale dell&#39;SDK AEM.
+ A volte, è necessario specificare i progetti Maven che forniscono il codice sorgente al debug remoto; si tratta dei tuoi progetti Maven bundle OSGi.

### Configurare le istruzioni

+ [Configurazione debugger remoto Java per codice VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configurazione di IntelliJ IDEA Remote Debugger](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Configurazione di Eclipse Remote Debugger](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
