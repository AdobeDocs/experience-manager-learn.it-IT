---
title: Estrazione dati OCR
description: Estrarre i dati dai documenti rilasciati dal governo per compilare i moduli.
feature: Barcoded Forms
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 2%

---



# Estrazione dati OCR

Estrarre automaticamente i dati da un&#39;ampia varietà di documenti rilasciati dal governo per compilare i moduli adattivi.

Esistono diverse organizzazioni che forniscono questo servizio e, finché dispongono di API REST ben documentate, è possibile integrarsi facilmente con AEM Forms utilizzando la funzionalità di integrazione dei dati. Ai fini di questa esercitazione, ho utilizzato [ID Analyzer](https://www.idanalyzer.com/) per dimostrare l&#39;estrazione dei dati OCR dei documenti caricati.

I passaggi seguenti sono stati seguiti per implementare l’estrazione dei dati OCR con AEM Forms utilizzando il servizio ID Analyzer .

## Creare un account sviluppatore

Crea un account sviluppatore con [Analizzatore ID](https://portal.idanalyzer.com/signin.html). Prendi nota della chiave API. Questa chiave sarà necessaria per richiamare le API REST del servizio di ID Analyzer.

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

Utilizza l’ [editor di swagger](https://editor.swagger.io/) per creare il file di swagger per descrivere le operazioni che inviano e verificano il codice OTP inviato utilizzando SMS. Il file swagger può essere creato in formato JSON o YAML. Il file swagger completato può essere scaricato da [qui](assets/drivers-license-swagger.zip)

## Crea origine dati

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [creare un’origine dati](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. Utilizza il [file swagger](assets/drivers-license-swagger.zip) per creare la tua origine dati.

## Crea modello dati modulo

L’integrazione dei dati di AEM Forms fornisce un’interfaccia utente intuitiva per creare e utilizzare [modelli di dati modulo](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basare il modello dati del modulo sull’origine dati creata nel passaggio precedente.

![fdm](assets/test-dl-fdm.PNG)

## Crea libreria client

Dovremo ottenere una stringa codificata base64 del documento caricato. Questa stringa codificata base64 viene quindi passata come uno dei parametri della nostra chiamata REST.
La libreria client può essere scaricata [da qui.](assets/drivers-license-client-lib.zip)

## Creare un modulo adattivo

Integra le chiamate POST del modello dati modulo con il modulo adattivo per estrarre i dati dal documento caricato dall’utente nel modulo. Puoi creare il tuo modulo adattivo e utilizzare la chiamata POST del modello di dati del modulo per inviare la stringa codificata base64 del documento caricato.

## Distribuisci sul server

Se desideri utilizzare le risorse di esempio con la tua chiave API, segui i seguenti passaggi:

* [Scarica l’origine dati ](assets/drivers-license-source.zip) e importala in AEM utilizzando  [il gestore dei pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Scarica il ](assets/drivers-license-fdm.zip) modello di dati del modulo e importalo in AEM utilizzando  [package manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Scarica la libreria client](assets/drivers-license-client-lib.zip)
* Scarica il modulo adattivo di esempio che può essere [scaricato da qui](assets/adaptive-form-dl.zip). Questo modulo di esempio utilizza le chiamate di servizio del modello dati del modulo fornito come parte di questo articolo.
* Importa il modulo in AEM da [Forms e Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Apri il modulo in modalità [modifica.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Specifica la chiave API come valore predefinito nel campo apikey e salva le modifiche
* Apri l&#39;editor di regole per il campo Base 64 String. Osserva la chiamata del servizio quando viene modificato il valore di questo campo.
* Salvare il modulo
* [Anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), caricamento dell&#39;immagine frontale della licenza del driver


