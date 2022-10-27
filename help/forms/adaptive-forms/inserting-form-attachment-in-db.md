---
title: Inserire un allegato al modulo nel database
description: Inserire un allegato al modulo nel database utilizzando AEM flusso di lavoro.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# Inserimento dell’allegato al modulo nel database

In questo articolo viene descritto il caso di utilizzo per la memorizzazione dell&#39;allegato del modulo nel database MySQL.

Una richiesta comune dei clienti è quella di memorizzare i dati del modulo acquisiti e l’allegato del modulo in una tabella di database.
Per eseguire questo caso d’uso, sono stati seguiti i seguenti passaggi

## Creazione di una tabella di database contenente i dati del modulo e l’allegato

Per contenere i dati del modulo è stata creata una tabella denominata Newhire. Osserva il nome della colonna immagine di tipo **LONGBLOB** per memorizzare l’allegato del modulo
![schema tabella](assets/insert-picture-table.png)

## Crea modello dati modulo

È stato creato un modello dati del modulo per comunicare con il database MySQL. Sarà necessario creare quanto segue

* [Origine dati JDBC in AEM](./data-integration-technical-video-setup.md)
* [Modello di dati modulo basato sull’origine dati JDBC](./jdbc-data-model-technical-video-use.md)

## Creare un flusso di lavoro

Se si configura il modulo adattivo per l’invio a un flusso di lavoro AEM, è possibile salvare gli allegati del modulo in una variabile di flusso di lavoro o salvarli in una cartella specifica sotto il payload. Per questo caso d’uso, è necessario salvare gli allegati in una variabile di flusso di lavoro di tipo ArrayList of Document. Da questo ArrayList è necessario estrarre il primo elemento e inizializzare una variabile di documento. Le variabili del flusso di lavoro denominate **listOfDocuments** e **dipendentePhoto** sono stati creati.
Quando il modulo adattivo viene inviato per attivare il flusso di lavoro, un passaggio nel flusso di lavoro inizializzerà la variabile dipendentePhoto utilizzando lo script ECMA. Di seguito è riportato il codice script ECMA

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

Il passaggio successivo nel flusso di lavoro consiste nell’inserire i dati e l’allegato del modulo nella tabella utilizzando il componente del servizio Invoke Form Data Model .
![insert-pic](assets/fdm-insert-pic.png)
[Il flusso di lavoro completo con lo script ecma di esempio può essere scaricato da qui](assets/add-new-employee.zip).

>[!NOTE]
> Sarà necessario creare un nuovo modello dati modulo basato su JDBC e utilizzare tale modello dati modulo nel flusso di lavoro

## Creare un modulo adattivo

Crea il modulo adattivo in base al modello di dati del modulo creato nel passaggio precedente. Trascinare gli elementi del modello dati del modulo sul modulo. Configura l’invio del modulo per attivare il flusso di lavoro e specifica le seguenti proprietà come mostrato nella schermata sottostante
![allegati](assets/form-attachments.png)
