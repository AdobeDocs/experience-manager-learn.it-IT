---
title: Utilizzo dell’API Batch per generare documenti di comunicazione interattiva
description: Risorse di esempio per la generazione di documenti del canale di stampa tramite API batch
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 90
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---

# API Batch

Puoi utilizzare l’API Batch per produrre più comunicazioni interattive da un modello. Il modello è una comunicazione interattiva senza alcun dato. L’API Batch combina i dati con un modello per produrre una comunicazione interattiva. L’API è utile nella produzione di massa di comunicazioni interattive. Ad esempio, bollette telefoniche, estratti conto delle carte di credito per più clienti.

[Ulteriori informazioni sull’API di generazione batch](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Questo articolo fornisce risorse di esempio per generare documenti di comunicazione interattiva utilizzando l’API Batch.

## Generazione batch tramite cartella controllata

* Importa [Modello di comunicazione interattiva](assets/Beneficiaries-confirmation.zip) nel server AEM Forms.
* Importa [configurazione cartella controllata](assets/batch-generation-api.zip). Verrà creata una cartella denominata `batchAPI` nell&#39;unità C.

**Se AEM Forms è in esecuzione su sistemi operativi non Windows, attenersi ai tre passaggi indicati di seguito:**

1. [Apri cartella controllata](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selezionare BatchAPIWatchedFolder e fare clic su Modifica.
3. Modifica il Percorso in modo che corrisponda al sistema operativo in uso.

![percorso](assets/watched-folder-batch-api-basic.PNG)

* Scarica ed estrai il contenuto di [file zip](assets/jsonfile.zip). Il file zip contiene una cartella denominata `jsonfile` che contiene `beneficiaries.json` file. Questo file contiene i dati per generare 3 documenti.

* Rilascia il `jsonfile` cartella nella cartella di input della cartella controllata.
* Una volta selezionata la cartella per l’elaborazione, controlla la cartella dei risultati della cartella controllata. Dovresti vedere 3 file PDF generati

## Generazione batch con richieste REST

È possibile richiamare [API Batch](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) tramite richieste REST. È possibile esporre gli endpoint REST per altre applicazioni per richiamare l’API per generare documenti.
Le risorse di esempio fornite espongono l’endpoint REST per la generazione di documenti di comunicazione interattiva. Il servlet accetta i seguenti parametri:

* fileName: posizione del file di dati nel file system.
* templatePath - Percorso modello IC
* saveLocation: percorso in cui salvare i documenti generati nel file system
* channelType - Stampa, Web o entrambi
* recordId: percorso JSON dell’elemento per impostare il nome di una comunicazione interattiva

La schermata seguente mostra i parametri e i relativi valori
![richiesta di esempio](assets/generate-ic-batch-servlet.PNG)

## Distribuire risorse di esempio sul server

* Importa [Modello ICT](assets/ICTemplate.zip) utilizzo [gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa [Gestore invio personalizzato](assets/BatchAPICustomSubmit.zip) utilizzo [gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa [Modulo adattivo](assets/BatchGenerationAPIAF.zip) utilizzando [Interfaccia Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Implementare e avviare [Bundle OSGi personalizzato](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) utilizzo [Console web Felix](http://localhost:4502/system/console/bundles)
* [Attiva generazione batch inviando il modulo](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
