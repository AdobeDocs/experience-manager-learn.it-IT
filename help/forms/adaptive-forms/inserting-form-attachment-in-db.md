---
title: Inserisci allegato modulo nel database
description: Inserire l’allegato del modulo nel database utilizzando il flusso di lavoro AEM.
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
duration: 82
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# Inserimento di un allegato modulo nel database

In questo articolo verrà illustrato il caso d&#39;uso relativo all&#39;archiviazione di allegati di moduli nel database MySQL.

I clienti chiedono spesso di memorizzare i dati dei moduli acquisiti e l’allegato del modulo in una tabella di database.
Per eseguire questo caso d’uso sono stati seguiti i seguenti passaggi

## Crea una tabella di database per contenere i dati del modulo e l&#39;allegato

È stata creata una tabella denominata newhire che contiene i dati del modulo. Osserva l&#39;immagine del nome della colonna di tipo **LONGBLOB** per memorizzare l&#39;allegato del modulo
![schema-tabella](assets/insert-picture-table.png)

## Crea modello dati modulo

È stato creato un modello dati modulo per comunicare con il database MySQL. Dovrai creare i seguenti elementi

* [Origine dati JDBC in AEM](./data-integration-technical-video-setup.md)
* [Modello dati modulo basato sull&#39;origine dati JDBC](./jdbc-data-model-technical-video-use.md)

## Crea flusso di lavoro

Se si configura un modulo adattivo per l’invio a un flusso di lavoro AEM, è possibile salvare gli allegati del modulo in una variabile del flusso di lavoro o salvarli in una cartella specifica sotto il payload. Per questo caso d’uso, è necessario salvare gli allegati in una variabile di flusso di lavoro di tipo ArrayList of Document. Da questo ArrayList è necessario estrarre il primo elemento e inizializzare una variabile di documento. Sono state create le variabili del flusso di lavoro **listOfDocuments** e **employeePhoto**.
Quando il modulo adattivo viene inviato per attivare il flusso di lavoro, un passaggio nel flusso di lavoro inizializza la variabile employeePhoto utilizzando lo script ECMA. Di seguito è riportato il codice di script ECMA

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

Il passaggio successivo nel flusso di lavoro consiste nell’inserire i dati e l’allegato del modulo nella tabella utilizzando il componente del servizio Richiama modello dati modulo.
![insert-pic](assets/fdm-insert-pic.png)
[Il flusso di lavoro completo con lo script ecma di esempio può essere scaricato da qui](assets/add-new-employee.zip).

>[!NOTE]
> Dovrai creare un nuovo modello dati modulo basato su JDBC e utilizzare tale modello dati modulo nel flusso di lavoro

## Creare un modulo adattivo

Crea il modulo adattivo in base al modello dati del modulo creato nel passaggio precedente. Trascina e rilascia gli elementi del modello di dati del modulo nel modulo. Configura l’invio del modulo per attivare il flusso di lavoro e specifica le seguenti proprietà come mostrato nella schermata seguente
![allegati modulo](assets/form-attachments.png)
