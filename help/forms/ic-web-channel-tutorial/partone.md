---
title: Installa e configura Tomcat
seo-title: Install and Configure Tomcat
description: Questa è parte 1 di tutorial multistep per la creazione del tuo primo documento di comunicazione interattivo.In questa parte, installeremo TOMCAT e distribuiremo il file sampleRest.war in TOMCAT.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# Installa e configura Tomcat {#install-and-configure-tomcat}

In questa parte, installiamo TOMCAT e distribuiamo il file sampleRest.war in TOMCAT. L’endpoint REST esposto da questo file WAR è la base del modello dati Origine dati e Modulo.

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
