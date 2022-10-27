---
title: Utilizzo dell’API Batch per generare documenti di comunicazione interattiva
description: Risorse di esempio per la generazione di documenti del canale di stampa utilizzando l’API batch
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 2%

---

# API Batch

Puoi utilizzare l’API Batch per produrre più comunicazioni interattive da un modello. Il modello è una comunicazione interattiva senza alcun dato. L’API Batch combina i dati con un modello per produrre una comunicazione interattiva. L&#39;API è utile nella produzione di massa di comunicazioni interattive. Ad esempio, bollette telefoniche, estratti conto della carta di credito per più clienti.

[Ulteriori informazioni sull’API di generazione batch](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Questo articolo fornisce risorse di esempio per generare documenti di comunicazioni interattive utilizzando l’API Batch.

## Generazione batch utilizzando la cartella osservata

* Importa [Modello di comunicazione interattiva](assets/Beneficiaries-confirmation.zip) nel server AEM Forms.
* Importa [configurazione delle cartelle controllata](assets/batch-generation-api.zip). Verrà creata una cartella denominata `batchAPI` nella vostra unità C.

**Se AEM Forms è in esecuzione su un sistema operativo non Windows, segui i 3 passaggi indicati di seguito:**

1. [Apri cartella di controllo](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selezionare BatchAPIWatchedFolder e fare clic su Modifica.
3. Modificare il percorso in modo che corrisponda al sistema operativo in uso.

![path](assets/watched-folder-batch-api-basic.PNG)

* Scaricare ed estrarre il contenuto di [file zip](assets/jsonfile.zip). Il file zip contiene la cartella denominata `jsonfile` che contiene `beneficiaries.json` file. Questo file ha i dati per generare 3 documenti.

* Rilascia `jsonfile` nella cartella di input della cartella controllata.
* Una volta che la cartella è stata selezionata per l&#39;elaborazione, controlla la cartella dei risultati della cartella controllata. Dovresti visualizzare 3 file PDF generati

## Generazione batch utilizzando le richieste REST

È possibile richiamare [API Batch](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) tramite richieste REST. È possibile esporre gli endpoint REST per altre applicazioni per richiamare l’API per generare documenti.
Le risorse di esempio fornite espongono l’endpoint REST per la generazione di documenti di comunicazione interattiva. Il servlet accetta i seguenti parametri:

* fileName - Posizione del file di dati nel file system.
* templatePath - Percorso modello IC
* saveLocation - Percorso per salvare i documenti generati nel file system
* channelType - Stampa, Web o entrambi
* recordId - Percorso JSON dell&#39;elemento per impostare il nome di una comunicazione interattiva

La schermata seguente mostra i parametri e i relativi valori
![richiesta di esempio](assets/generate-ic-batch-servlet.PNG)

## Distribuire risorse di esempio sul server

* Importa [ICTemplate](assets/ICTemplate.zip) utilizzo [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa [Gestore di invio personalizzato](assets/BatchAPICustomSubmit.zip) utilizzo [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa [Modulo adattivo](assets/BatchGenerationAPIAF.zip) utilizzando [Interfaccia Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Distribuzione e avvio [Bundle OSGI personalizzato](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) utilizzo [Console web Felix](http://localhost:4502/system/console/bundles)
* [Attivazione della generazione in batch inviando il modulo](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
