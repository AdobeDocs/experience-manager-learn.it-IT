---
title: Installazione e configurazione di Tomcat
seo-title: Installazione e configurazione di Tomcat
description: Questa è parte 1 di esercitazione multistep per la creazione del primo documento di comunicazione interattiva.In questa parte, installeremo TOMCAT e distribuiremo il file sampleRest.war in TOMCAT. L'endpoint REST esposto da questo file WAR sarà la base per il nostro Data Source e Form Data Model.
seo-description: Questa è parte 1 di esercitazione multistep per la creazione del primo documento di comunicazione interattiva.In questa parte, installeremo TOMCAT e distribuiremo il file sampleRest.war in TOMCAT. L'endpoint REST esposto da questo file WAR sarà la base per il nostro Data Source e Form Data Model.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---


# Installazione e configurazione di Tomcat {#install-and-configure-tomcat}

In questa parte, installeremo TOMCAT e implementeremo il file sampleRest.war in TOMCAT. L&#39;endpoint REST esposto da questo file WAR sarà la base per il nostro Data Source e Form Data Model.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Per configurare tomcat, seguire le istruzioni seguenti:

1. Scaricate e installate JDK1.8.
2. Impostate JAVA_HOME affinché punti a JDK1.8.
3. Scarica [tomcat](https://tomcat.apache.org/). Questo file di guerra è stato testato con Tomcat versione 8.5.x e 9.0.x.
4. Scarica la versione tomcat della tua preferenza. Potete scaricare il file ZIP di Windows a 64 bit nella sezione principale.
5. Decomprimete i contenuti nella cartella c:\tomcat.
6. Dovresti vedere qualcosa di simile nel tuo c drive **c:\tomcat\apache-tomcat-8.5.27** a seconda della versione del tuo gatto
7. Create una variabile di ambiente denominata &quot;CATALINA_HOME&quot; e impostatene il valore nell’esempio di cartella di installazione tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copiate il file SampleRest.war nella cartella delle app Web dell&#39;installazione Tomcat
9. Avvia nuova finestra del prompt dei comandi.
10. Andate alla cartella di installazione &lt;tomcat>\bin ed eseguite startup.bat
11. Una volta che il vostro gatto è iniziato, testare l&#39;endpoint esposto da WAR File [cliccando qui](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Dovreste ottenere dati di esempio come risultato di questa chiamata.

Congratulazioni !!!. Hai configurato tomcat e distribuito il file SampleRest.war.

Il seguente video spiega la distribuzione di un’applicazione di esempio a Tomcat
>[!VIDEO](https://video.tv.adobe.com/v/37815)