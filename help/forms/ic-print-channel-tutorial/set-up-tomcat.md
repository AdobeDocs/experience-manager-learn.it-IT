---
title: Installare e configurare il video Tomcat
seo-title: Install and Configure Tomcat
description: Questa è la parte 1 del tutorial a più passaggi per creare il tuo primo documento di comunicazione interattiva.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 1%

---

# Installare e configurare Tomcat {#install-and-configure-tomcat}

In questa parte, viene installato TOMCAT e viene distribuito il file sampleRest.war in TOMCAT. L’endpoint REST esposto da questo file WAR è la base per il nostro modello dati Origine dati e Modulo.

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

Per impostare tomcat, seguire le seguenti istruzioni:

1. Scarica e installa JDK1.8.
2. Impostare JAVA_HOME su JDK1.8.
3. Scarica [tomcat](https://tomcat.apache.org/). Questo file di guerra è stato testato con Tomcat versione 8.5.x e 9.0.x.
4. Scarica la versione tomcat della tua preferenza. Puoi scaricare lo zip di Windows a 64 bit nella sezione core.
5. Decomprimere il contenuto nel file c:\tomcat.
6. Dovresti vedere qualcosa di simile nell&#39;unità C **c:\tomcat\apache-tomcat-8.5.27** a seconda della versione della tomcat
7. Creare una variabile di ambiente denominata &quot;CATALINA_HOME&quot; e impostarne il valore sulla cartella di installazione tomcat esempio c:\tomcat\apache- tomcat-8.5.27
8. Copia il file SampleRest.war nella cartella webapps dell’installazione Tomcat.
9. Avvia una nuova finestra del prompt dei comandi.
10. Accedi a &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin ed eseguire startup.bat
11. Una volta avviato il tomcat, verifica l’endpoint esposto da WAR File eseguendo il test di [clic qui](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Dovresti ottenere dati di esempio come risultato di questa chiamata.

Congratulazioni !!!. Hai configurato tomcat e distribuito il file SampleRest.war.

Il video seguente spiega la distribuzione di un’applicazione di esempio in Tomcat
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

## Passaggi successivi

[Crea origine dati RESTful](./create-data-source.md)