---
title: Video sull’installazione e la configurazione di Tomcat
seo-title: Install and Configure Tomcat
description: Questa è parte 1 di tutorial multistep per la creazione del tuo primo documento di comunicazione interattivo.In questa parte, installeremo TOMCAT e distribuiremo il file sampleRest.war in TOMCAT. L'endpoint REST esposto da questo file WAR sarà la base per il nostro Data Source e Form Data Model.
seo-description: This is part 1 of multistep tutorial for creating your first interactive communications document.In this part, we will install TOMCAT and deploy the sampleRest.war file in TOMCAT. The REST endpoint exposed by this WAR file will be the basis for our Data Source and Form Data Model.
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
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Installa e configura Tomcat {#install-and-configure-tomcat}

In questa parte, installeremo TOMCAT e implementeremo il file sampleRest.war in TOMCAT. L&#39;endpoint REST esposto da questo file WAR sarà la base per il nostro Data Source e Form Data Model.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Per impostare tomcat, seguire le seguenti istruzioni:

1. Scarica e installa JDK1.8.
2. Imposta JAVA_HOME per puntare a JDK1.8.
3. Scarica [gatto](https://tomcat.apache.org/). Questo file di guerra è stato testato con Tomcat versione 8.5.x e 9.0.x.
4. Scarica la versione tomcat della tua preferenza. È possibile scaricare lo zip windows a 64 bit sotto la sezione core.
5. Decomprimi i contenuti nella cartella c:\tomcat.
6. Dovresti vedere qualcosa del genere nel tuo c drive **c:\tomcat\apache-tomcat-8.5.27** a seconda della versione del vostro gatto
7. Crea una variabile di ambiente denominata &quot;CATALINA_HOME&quot; e imposta il suo valore sulla cartella di installazione tomcat esempio c:\tomcat\apache- tomcat-8.5.27
8. Copia il file SampleRest.war nella cartella webapps della tua installazione Tomcat
9. Avvia la nuova finestra del prompt dei comandi.
10. Passa a &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin ed esegui startup.bat
11. Una volta avviato il tuo gatto, prova l&#39;endpoint esposto da file WAR per [clic qui](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. È necessario ottenere dati di esempio come risultato di questa chiamata.

Congratulazioni !!!. Hai configurato tomcat e distribuito il file SampleRest.war.

Il video seguente spiega la distribuzione di un’applicazione di esempio a Tomcat.
>[!VIDEO](https://video.tv.adobe.com/v/37815)
