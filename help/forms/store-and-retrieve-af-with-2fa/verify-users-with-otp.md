---
title: Verificare gli utenti con OTP
description: Verificare il numero di cellulare associato al numero dell'applicazione utilizzando OTP.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---



# Verificare gli utenti con OTP

SMS Two Factor Authentication (autenticazione a doppio fattore) è una procedura di verifica della sicurezza attivata dall&#39;utente che accede a un sito Web, software o applicazione. Nel processo di accesso, l&#39;utente riceve automaticamente un SMS al proprio numero di cellulare contenente un codice numerico univoco.

Ci sono diverse organizzazioni che forniscono questo servizio e finché hanno ben documentato REST API è possibile integrare facilmente  AEM Forms utilizzando le funzionalità di integrazione dei dati di  AEM Forms. Ai fini di questa esercitazione, ho utilizzato [Nexmo](https://developer.nexmo.com/verify/overview) per dimostrare l&#39;utilizzo di SMS 2FA.

Sono stati seguiti i passaggi seguenti per implementare SMS 2FA con  AEM Forms utilizzando il servizio Nexmo Verify.

## Creare un account sviluppatore

Create un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/sign-in). Prendete nota della chiave API e della chiave segreta API. Queste chiavi saranno necessarie per richiamare REST API del servizio di Nexmo.

## Creare un file Swagger/OpenAPI

La specifica OpenAPI (precedentemente specifica Swagger) è un formato di descrizione API per le API REST. Un file OpenAPI consente di descrivere l&#39;intera API, compresi:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET /users, POST /users)
* Parametri operativi Input ed output per ogni operazione
Metodi di autenticazione
* Informazioni di contatto, licenza, termini di utilizzo e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli umani che per le macchine.

Per creare il primo file swagger/OpenAPI, segui la [documentazione OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
>  AEM Forms supporta OpenAPI Specification versione 2.0 (swagger fka).

Utilizzate [swagger editor](https://editor.swagger.io/) per creare il file swagger per descrivere le operazioni che inviano e verificano il codice OTP inviato tramite SMS. Il file swagger può essere creato in formato JSON o YAML. Il file swagger completato può essere scaricato da [qui](assets/two-factore-authentication-swagger.zip)

## Crea origine dati

Per integrare AEM Forms AEM/ con applicazioni di terze parti, è necessario [origine dati basata su REST utilizzando il file swagger](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. L’origine dati completata viene fornita come parte delle risorse del corso.

## Crea modello dati modulo

&#39;integrazione dei dati AEM Forms fornisce un&#39;interfaccia utente intuitiva per creare e utilizzare i modelli di dati dei moduli [](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modello dati del modulo si basa sulle origini dati per lo scambio di dati.
Il modello dati modulo compilato può essere [scaricato da qui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
