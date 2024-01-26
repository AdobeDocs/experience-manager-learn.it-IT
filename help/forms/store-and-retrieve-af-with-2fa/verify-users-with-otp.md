---
title: Verificare gli utenti con OTP
description: Verifica il numero di cellulare associato al numero dell’applicazione utilizzando OTP.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
duration: 104
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---

# Verificare gli utenti con OTP

L’autenticazione a due fattori SMS (Dual Factor Authentication) è una procedura di verifica della sicurezza attivata tramite l’accesso di un utente a un sito web, un software o un’applicazione. Nel processo di accesso, l’utente riceve automaticamente un SMS al proprio numero di cellulare contenente un codice numerico univoco.

Esistono diverse organizzazioni che forniscono questo servizio e, purché dispongano di API REST ben documentate, puoi integrare facilmente AEM Forms utilizzando le funzionalità di integrazione dei dati di AEM Forms. Ai fini di questa esercitazione, ho utilizzato [Nexmo](https://developer.nexmo.com/verify/overview) per illustrare il caso di utilizzo di SMS 2FA.

Sono stati seguiti i seguenti passaggi per implementare SMS 2FA con AEM Forms utilizzando il servizio Nexmo Verify.

## Crea account sviluppatore

Creare un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/sign-in). Prendi nota della chiave API e della chiave segreta API. Queste chiavi sono necessarie per richiamare le API REST del servizio di Nexmo.

## Crea file Swagger/OpenAPI

OpenAPI Specification (precedentemente Swagger Specification) è un formato di descrizione API per le API REST. Un file OpenAPI ti consente di descrivere l’intera API, tra cui:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET /users, POST /users)
* Parametri di operazione Input e output per ogni operazione Metodi di autenticazione
* Informazioni di contatto, licenza, condizioni d’uso e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli esseri umani che per le macchine.

Per creare il primo file swagger/OpenAPI, segui la [Documentazione di OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms supporta la versione 2.0 delle specifiche OpenAPI (fka Swagger).

Utilizza il [editor swagger](https://editor.swagger.io/) creare il file swagger per descrivere le operazioni di invio e verifica del codice OTP inviato tramite SMS. Il file swagger può essere creato in formato JSON o YAML. Il file Swagger completato può essere scaricato da [qui](assets/two-factore-authentication-swagger.zip)

## Crea origine dati

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [Origine dati basata su REST tramite il file Swagger](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. L’origine dati completata viene fornita come parte delle risorse di questo corso.

## Crea modello dati modulo

L’integrazione dei dati di AEM Forms offre un’interfaccia utente intuitiva per la creazione e l’utilizzo di [modelli dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modello per dati modulo si basa su origini dati per lo scambio di dati.
Il modello dati del modulo completato può essere [scaricato da qui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

[Creare il modulo principale](./create-the-main-adaptive-form.md)
