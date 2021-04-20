---
title: Autenticazione a due fattori SMS
description: Aggiungi un ulteriore livello di sicurezza per confermare l’identità di un utente quando desidera eseguire determinate attività
feature: Adaptive Forms
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 2%

---



# Verificare gli utenti utilizzando il numero di cellulare

L’autenticazione a due fattori SMS (Dual Factor Authentication) è una procedura di verifica della sicurezza attivata tramite un accesso utente a un sito web, software o applicazione. Nel processo di accesso, l’utente viene automaticamente inviato un SMS al proprio numero di cellulare contenente un codice numerico univoco.

Esistono diverse organizzazioni che forniscono questo servizio e, finché dispongono di API REST ben documentate, è possibile integrare facilmente AEM Forms utilizzando le funzionalità di integrazione dei dati di AEM Forms. Ai fini di questa esercitazione, ho utilizzato [Nexmo](https://developer.nexmo.com/verify/overview) per dimostrare il caso d’uso SMS 2FA.

Sono stati seguiti i passaggi seguenti per implementare SMS 2FA con AEM Forms utilizzando il servizio Verifica di Nexmo.

## Creare un account sviluppatore

Crea un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/sign-in). Prendi nota della chiave API e della chiave segreto API. Queste chiavi saranno necessarie per richiamare API REST del servizio di Nexmo.

## Crea file Swagger/OpenAPI

La specifica OpenAPI (precedentemente specifica Swagger) è un formato di descrizione API per le API REST. Un file OpenAPI ti consente di descrivere l’intera API, tra cui:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET /users, POST /users)
* Parametri operativi Input ed output per ciascuna operazione
Metodi di autenticazione
* Informazioni di contatto, licenza, termini di utilizzo e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli umani che per le macchine.

Per creare il tuo primo file swagger/OpenAPI, segui la [documentazione OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms supporta la specifica OpenAPI versione 2.0 (fka Swagger).

Utilizza l’ [editor di swagger](https://editor.swagger.io/) per creare il file di swagger per descrivere le operazioni che inviano e verificano il codice OTP inviato utilizzando SMS. Il file swagger può essere creato in formato JSON o YAML. Il file swagger completato può essere scaricato da [qui](assets/two-factore-authentication-swagger.zip)

## Crea origine dati

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [creare un’origine dati](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud.

## Crea modello dati modulo

L’integrazione dei dati di AEM Forms fornisce un’interfaccia utente intuitiva per creare e utilizzare [modelli di dati modulo](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modello dati modulo si basa su origini dati per lo scambio di dati.
Il modello dati del modulo completato può essere [scaricato da qui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Creare un modulo adattivo

Integra le chiamate POST del modello dati modulo con il modulo adattivo per verificare il numero di telefono cellulare immesso dall’utente nel modulo. Puoi creare il tuo modulo adattivo e utilizzare la chiamata POST del modello di dati del modulo per inviare e verificare il codice OTP in base alle tue esigenze.

Se desideri utilizzare le risorse di esempio con le tue chiavi API, segui i seguenti passaggi:

* [Scarica il ](assets/sms-2fa-fdm.zip) modello di dati del modulo e importalo in AEM utilizzando  [package manager](http://localhost:4502/crx/packmgr/index.jsp)
* Scarica il modulo adattivo di esempio che può essere [scaricato da qui](assets/sms-2fa-verification-af.zip). Questo modulo di esempio utilizza le chiamate di servizio del modello dati del modulo fornito come parte di questo articolo.
* Importa il modulo in AEM da [Forms e Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Apri il modulo in modalità di modifica. Apri l’editor di regole per il campo seguente

![sms-send](assets/check-sms.PNG)

* Modifica la regola associata al campo. Fornire le chiavi API appropriate
* Salvare il modulo
* [Visualizzare l’anteprima del ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) modulo e verificare la funzionalità


