---
title: Autenticazione a due fattori SMS
description: Aggiungi un ulteriore livello di sicurezza per confermare l’identità di un utente quando desidera eseguire determinate attività
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# Verificare gli utenti utilizzando i numeri di telefono cellulare

L’autenticazione a due fattori SMS (Dual Factor Authentication) è una procedura di verifica della sicurezza attivata tramite l’accesso di un utente a un sito web, un software o un’applicazione. Nel processo di accesso, l’utente riceve automaticamente un SMS al proprio numero di cellulare contenente un codice numerico univoco.

Esistono diverse organizzazioni che forniscono questo servizio e, purché dispongano di API REST ben documentate, puoi integrare facilmente AEM Forms utilizzando le funzionalità di integrazione dei dati di AEM Forms. Ai fini di questa esercitazione, ho utilizzato [Nexmo](https://developer.nexmo.com/verify/overview) per dimostrare il caso d&#39;uso di SMS 2FA.

Sono stati seguiti i seguenti passaggi per implementare SMS 2FA con AEM Forms utilizzando il servizio Nexmo Verify.

## Crea account sviluppatore

Crea un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/sign-in). Prendi nota della chiave API e della chiave segreta API. Queste chiavi sono necessarie per richiamare le API REST del servizio di Nexmo.

## Crea file Swagger/OpenAPI

OpenAPI Specification (precedentemente Swagger Specification) è un formato di descrizione API per le API REST. Un file OpenAPI ti consente di descrivere l’intera API, tra cui:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET /users, POST /users)
* Parametri di funzionamento Input e output per ogni operazione
Metodi di autenticazione
* Informazioni di contatto, licenza, condizioni d’uso e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli esseri umani che per le macchine.

Per creare il primo file swagger/OpenAPI, segui la [documentazione OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms supporta la versione 2.0 delle specifiche OpenAPI (fka Swagger).

Utilizza l&#39;[editor Swagger](https://editor.swagger.io/) per creare il file Swagger e descrivere le operazioni di invio e verifica del codice OTP inviato tramite SMS. Il file swagger può essere creato in formato JSON o YAML. Il file Swagger completato può essere scaricato da [qui](assets/two-factore-authentication-swagger.zip)

## Creazione di Data Source

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [creare l&#39;origine dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud.

## Crea modello dati modulo

L&#39;integrazione dei dati di AEM Forms fornisce un&#39;interfaccia utente intuitiva per la creazione e l&#39;utilizzo di [modelli di dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modello per dati modulo si basa su origini dati per lo scambio di dati.
Il modello dati del modulo completato può essere [scaricato da qui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Creare un modulo adattivo

Integra le chiamate POST del modello dati modulo con il modulo adattivo per verificare il numero di telefono cellulare immesso dall’utente nel modulo. Puoi creare un modulo adattivo personalizzato e utilizzare la chiamata POST del modello di dati del modulo per inviare e verificare il codice OTP in base alle tue esigenze.

Se desideri utilizzare le risorse di esempio con le tue chiavi API, segui i passaggi seguenti:

* [Scarica il modello dati del modulo](assets/sms-2fa-fdm.zip) e importa in AEM utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Il modulo adattivo di esempio scaricabile può essere [scaricato da qui](assets/sms-2fa-verification-af.zip). In questo modulo di esempio vengono utilizzate le chiamate di servizio del modello dati del modulo fornito come parte di questo articolo.
* Importa il modulo in AEM dall&#39;[interfaccia utente Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Apri il modulo in modalità di modifica. Apri l’editor di regole per il campo seguente

![sms-send](assets/check-sms.PNG)

* Modifica la regola associata al campo. Fornisci le chiavi API appropriate
* Salvare il modulo
* [Anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) e verifica la funzionalità
