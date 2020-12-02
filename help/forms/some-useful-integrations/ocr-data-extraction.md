---
title: Estrazione dati OCR
description: Estrarre i dati dai documenti governativi per la compilazione dei moduli.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
translation-type: tm+mt
source-git-commit: c0db84f25334106c798d555c754d550113e91eac
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---



# Estrazione dati OCR

Estrarre automaticamente i dati da un&#39;ampia gamma di documenti governativi per compilare i moduli adattivi.

Ci sono diverse organizzazioni che forniscono questo servizio e finché hanno ben documentato REST API è possibile integrarsi facilmente con  AEM Forms utilizzando la funzionalità di integrazione dei dati. Per questa esercitazione, ho utilizzato [ID Analyzer](https://www.idanalyzer.com/) per dimostrare l&#39;estrazione dei dati OCR dei documenti caricati.

I passaggi seguenti sono stati seguiti per implementare l’estrazione dei dati OCR con  AEM Forms tramite il servizio ID Analyzer.

## Creare un account sviluppatore

Create un account sviluppatore con [ID Analyzer](https://portal.idanalyzer.com/signin.html). Prendete nota della chiave API. Questa chiave sarà necessaria per richiamare le API REST del servizio ID Analyzer.

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

Utilizzate [swagger editor](https://editor.swagger.io/) per creare il file swagger per descrivere le operazioni che inviano e verificano il codice OTP inviato tramite SMS. Il file swagger può essere creato in formato JSON o YAML. Il file swagger completato può essere scaricato da [qui](assets/drivers-license-swagger.zip)

## Crea origine dati

Per integrare AEM Forms AEM/ con applicazioni di terze parti, è necessario [creare un&#39;origine dati](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. Utilizzare il [file swagger](assets/drivers-license-swagger.zip) per creare l&#39;origine dati.

## Crea modello dati modulo

&#39;integrazione dei dati AEM Forms fornisce un&#39;interfaccia utente intuitiva per creare e utilizzare i modelli di dati dei moduli [](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basare il modello dati del modulo sull&#39;origine dati creata nel passaggio precedente.

![fdm](assets/test-dl-fdm.PNG)

## Crea libreria client

Dovremo ottenere una stringa codificata base64 del documento caricato. Questa stringa codificata base64 viene quindi passata come uno dei parametri della nostra chiamata REST.
La libreria client può essere scaricata [da qui.](assets/drivers-license-client-lib.zip)

## Crea modulo adattivo

Integrare le chiamate POST del modello dati modulo con il modulo adattivo per estrarre dati dal documento caricato dall&#39;utente nel modulo. È possibile creare un modulo adattivo personalizzato e utilizzare la chiamata POST del modello dati modulo per inviare la stringa codificata base64 del documento caricato.

## Implementazione sul server

Per utilizzare le risorse di esempio con la chiave API, effettuate le seguenti operazioni:

* [Scarica l&#39;](assets/drivers-license-source.zip) origine dati e importa in AEM utilizzando  [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Scaricare il ](assets/drivers-license-fdm.zip) modello di dati del modulo e importarlo in AEM utilizzando  [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Download della libreria client](assets/drivers-license-client-lib.zip)
* Scaricate il modulo adattivo di esempio da [qui](assets/adaptive-form-dl.zip). In questo modulo di esempio vengono utilizzate le chiamate di servizio del modello dati del modulo fornite come parte di questo articolo.
* Importare il modulo in AEM dall&#39; [Forms e dall&#39;interfaccia utente del documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Aprire il modulo in [modalità di modifica.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Specificate la chiave API come valore predefinito nel campo apikey e salvate le modifiche
* Aprire l&#39;editor di regole per il campo Stringa base 64. Osservate la chiamata del servizio quando il valore di questo campo viene modificato.
* Salvare il modulo
* [Anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), caricamento dell&#39;immagine anteriore della licenza del driver


