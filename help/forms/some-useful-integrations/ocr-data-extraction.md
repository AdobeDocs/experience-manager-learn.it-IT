---
title: Estrazione dati OCR
description: Estrarre i dati dai documenti rilasciati dal governo per compilare i moduli.
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 1%

---

# Estrazione dati OCR

Estrarre automaticamente i dati da un&#39;ampia varietà di documenti rilasciati dal governo per compilare i moduli adattivi.

Esistono diverse organizzazioni che forniscono questo servizio e, finché dispongono di API REST ben documentate, puoi facilmente integrarsi con AEM Forms utilizzando la funzionalità di integrazione dei dati. Ai fini di questa esercitazione, ho utilizzato [Analizzatore di ID](https://www.idanalyzer.com/) dimostrare l’estrazione dei dati OCR dei documenti caricati.

Sono stati seguiti i passaggi seguenti per implementare l’estrazione dei dati OCR con AEM Forms utilizzando il servizio ID Analyzer .

## Creare un account sviluppatore

Crea un account sviluppatore con [Analizzatore di ID](https://portal.idanalyzer.com/signin.html). Prendi nota della chiave API. Questa chiave è necessaria per richiamare le API REST del servizio di ID Analyzer.

## Crea file Swagger/OpenAPI

La specifica OpenAPI (precedentemente specifica Swagger) è un formato di descrizione API per le API REST. Un file OpenAPI ti consente di descrivere l’intera API, tra cui:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET/users, POST/users)
* Parametri operativi Input ed output per ogni operazione Metodi di autenticazione
* Informazioni di contatto, licenza, termini di utilizzo e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli umani che per le macchine.

Per creare il tuo primo file swagger/OpenAPI, segui [Documentazione OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms supporta la specifica OpenAPI versione 2.0 (fka Swagger).

Utilizza la [editor swagger](https://editor.swagger.io/) per creare il file swagger per descrivere le operazioni che inviano e verificano il codice OTP inviato utilizzando SMS. Il file swagger può essere creato in formato JSON o YAML. Il file swagger completato può essere scaricato da [qui](assets/drivers-license-swagger.zip)

## Considerazioni durante la definizione del file di swagger

* Definizioni necessarie
* $ref deve essere utilizzato per le definizioni dei metodi
* Preferisci che le sezioni di consumo e di produzione siano definite
* Non definire parametri del corpo della richiesta in linea o parametri di risposta. Cerca di modularizzare il più possibile. Ad esempio, la seguente definizione non è supportata

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

È supportato quanto segue con un riferimento alla definizione requestBody

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Esempio di file Swagger per il riferimento](assets/sample-swagger.json)

## Crea origine dati

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [crea origine dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. Utilizza la [file swagger](assets/drivers-license-swagger.zip) per creare l&#39;origine dati.

## Crea modello dati modulo

L’integrazione dei dati di AEM Forms offre un’interfaccia utente intuitiva con cui creare e lavorare [modelli di dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basare il modello dati del modulo sull’origine dati creata nel passaggio precedente.

![fdm](assets/test-dl-fdm.PNG)

## Crea libreria client

Dovremo ottenere una stringa codificata base64 del documento caricato. Questa stringa codificata base64 viene quindi passata come uno dei parametri della nostra chiamata REST.
È possibile scaricare la libreria client [da qui.](assets/drivers-license-client-lib.zip)

## Creare un modulo adattivo

Integra le chiamate POST del modello dati modulo con il modulo adattivo per estrarre i dati dal documento caricato dall’utente nel modulo. Puoi creare un modulo adattivo e utilizzare la chiamata POST del modello dati modulo per inviare la stringa codificata base64 del documento caricato.

## Distribuisci sul server

Se desideri utilizzare le risorse di esempio con la tua chiave API, segui i seguenti passaggi:

* [Scaricare l’origine dati](assets/drivers-license-source.zip) e importano in AEM utilizzando [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Scaricare il modello dati del modulo](assets/drivers-license-fdm.zip) e importano in AEM utilizzando [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Scarica la libreria client](assets/drivers-license-client-lib.zip)
* Scarica il modulo adattivo di esempio [scaricato da qui](assets/adaptive-form-dl.zip). Questo modulo di esempio utilizza le chiamate di servizio del modello dati del modulo fornito come parte di questo articolo.
* Importa il modulo in AEM dal [Interfaccia utente Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Apri il modulo in [modalità di modifica.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Specifica la chiave API come valore predefinito nel campo apikey e salva le modifiche
* Apri l&#39;editor di regole per il campo Base 64 String. Osserva la chiamata del servizio quando viene modificato il valore di questo campo.
* Salvare il modulo
* [Anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), carica l&#39;immagine frontale della tua patente di guida
