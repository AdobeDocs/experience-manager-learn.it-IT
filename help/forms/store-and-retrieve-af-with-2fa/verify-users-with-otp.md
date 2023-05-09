---
title: Verificare gli utenti con OTP
description: Verifica il numero di cellulare associato al numero di applicazione utilizzando OTP.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# Verificare gli utenti con OTP

L’autenticazione a due fattori SMS (Dual Factor Authentication) è una procedura di verifica della sicurezza attivata tramite un accesso utente a un sito web, software o applicazione. Nel processo di accesso, l’utente viene automaticamente inviato un SMS al proprio numero di cellulare contenente un codice numerico univoco.

Esistono diverse organizzazioni che forniscono questo servizio e, finché dispongono di API REST ben documentate, è possibile integrare facilmente AEM Forms utilizzando le funzionalità di integrazione dei dati di AEM Forms. Ai fini di questa esercitazione, ho utilizzato [Nexmo](https://developer.nexmo.com/verify/overview) per dimostrare il caso d’uso SMS 2FA.

Sono stati seguiti i seguenti passaggi per implementare SMS 2FA con AEM Forms utilizzando il servizio Nexmo Verify .

## Creare un account sviluppatore

Crea un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/sign-in). Prendi nota della chiave API e della chiave segreto API. Queste chiavi sono necessarie per richiamare API REST del servizio di Nexmo.

## Crea file Swagger/OpenAPI

La specifica OpenAPI (precedentemente specifica Swagger) è un formato di descrizione API per le API REST. Un file OpenAPI ti consente di descrivere l’intera API, tra cui:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET/users, POST/users)
* Parametri operativi Input ed output per ogni operazione Metodi di autenticazione
* Informazioni di contatto, licenza, termini di utilizzo e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli umani che per le macchine.

Per creare il tuo primo file swagger/OpenAPI, segui [Documentazione OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms supporta la specifica OpenAPI versione 2.0 (fka Swagger).

Utilizza la [editor swagger](https://editor.swagger.io/) per creare il file swagger per descrivere le operazioni che inviano e verificano il codice OTP inviato utilizzando SMS. Il file swagger può essere creato in formato JSON o YAML. Il file swagger completato può essere scaricato da [qui](assets/two-factore-authentication-swagger.zip)

## Crea origine dati

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [Origine dati basata su REST che utilizza il file swagger](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. L&#39;origine dati completata viene fornita come parte di questo corso assets.

## Crea modello dati modulo

L’integrazione dei dati di AEM Forms offre un’interfaccia utente intuitiva con cui creare e lavorare [modelli di dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modello dati modulo si basa su origini dati per lo scambio di dati.
Il modello dati del modulo completato può essere [scaricato da qui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

[Creare il modulo principale](./create-the-main-adaptive-form.md)
