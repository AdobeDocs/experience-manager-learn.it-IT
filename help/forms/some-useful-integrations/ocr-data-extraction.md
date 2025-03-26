---
title: Estrazione dati OCR
description: Estrai dati dai documenti governativi emessi per compilare i moduli.
feature: Barcoded Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 145
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# Estrazione dati OCR

Estrai automaticamente i dati da un’ampia gamma di documenti governativi per compilare i moduli adattivi.

Esistono diverse organizzazioni che forniscono questo servizio e, purché dispongano di API REST ben documentate, puoi facilmente integrarle con AEM Forms utilizzando la funzionalità di integrazione dei dati. Ai fini di questa esercitazione, ho utilizzato [ID Analyzer](https://www.idanalyzer.com/) per dimostrare l&#39;estrazione dei dati OCR dei documenti caricati.

Sono stati seguiti i seguenti passaggi per implementare l’estrazione dei dati OCR con AEM Forms utilizzando il servizio ID Analyzer.

## Crea account sviluppatore

Crea un account sviluppatore con [ID Analyzer](https://portal.idanalyzer.com/signin.html). Prendi nota della chiave API. Questa chiave è necessaria per richiamare le API REST del servizio di ID Analyzer.

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

Utilizza l&#39;[editor Swagger](https://editor.swagger.io/) per creare il file Swagger e descrivere le operazioni di invio e verifica del codice OTP inviato tramite SMS. Il file swagger può essere creato in formato JSON o YAML. Il file Swagger completato può essere scaricato da [qui](assets/drivers-license-swagger.zip)

## Considerazioni durante la definizione del file Swagger

* Sono necessarie delle definizioni
* $ref deve essere utilizzato per le definizioni dei metodi
* Preferisci che vengano definite sezioni di consumo e produzione
* Non definire i parametri del corpo della richiesta in linea o i parametri di risposta. Prova a modulare il più possibile. Ad esempio, la seguente definizione non è supportata

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

Di seguito è riportato un riferimento alla definizione requestBody.

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Esempio di file Swagger per riferimento](assets/sample-swagger.json)

## Creazione di Data Source

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [creare l&#39;origine dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. Utilizza il [file Swagger](assets/drivers-license-swagger.zip) per creare l&#39;origine dati.

## Crea modello dati modulo

L&#39;integrazione dei dati di AEM Forms fornisce un&#39;interfaccia utente intuitiva per la creazione e l&#39;utilizzo di [modelli di dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basare il modello dati del modulo sull&#39;origine dati creata nel passaggio precedente.

![fdm](assets/test-dl-fdm.PNG)

## Crea libreria client

Dovremmo ottenere la stringa con codifica base64 del documento caricato. Questa stringa con codifica base64 viene quindi passata come uno dei parametri della chiamata REST.
La libreria client può essere scaricata [da qui.](assets/drivers-license-client-lib.zip)

## Creare un modulo adattivo

Integra le chiamate POST del modello dati del modulo con il modulo adattivo per estrarre i dati dal documento caricato dall’utente nel modulo. Puoi creare un modulo adattivo personalizzato e utilizzare la chiamata POST del modello di dati del modulo per inviare la stringa con codifica base64 del documento caricato.

## Distribuisci sul server

Se desideri utilizzare le risorse di esempio con la tua chiave API, segui i seguenti passaggi:

* [Scarica l&#39;origine dati](assets/drivers-license-source.zip) e importa in AEM utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Scarica il modello dati del modulo](assets/drivers-license-fdm.zip) e importa in AEM utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Scarica la libreria client](assets/drivers-license-client-lib.zip)
* Il modulo adattivo di esempio scaricabile può essere [scaricato da qui](assets/adaptive-form-dl.zip). In questo modulo di esempio vengono utilizzate le chiamate di servizio del modello dati del modulo fornito come parte di questo articolo.
* Importa il modulo in AEM da [Forms e interfaccia utente documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Apri il modulo in modalità [modifica.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Specifica la chiave API come valore predefinito nel campo apikey e salva le modifiche
* Apri l’editor di regole per il campo Stringa Base 64. Osserva la chiamata del servizio quando il valore di questo campo viene modificato.
* Salvare il modulo
* [Visualizza l&#39;anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), carica la foto di copertina della tua patente
