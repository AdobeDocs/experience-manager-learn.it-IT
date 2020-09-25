---
title: Utilizzo dell'API Batch per la generazione di documenti di comunicazione interattiva
description: Risorse di esempio per la generazione di documenti del canale di stampa mediante l’API batch
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 3dc1bd3f2f7b6324c53640f01a263fa0728d439c
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# API batch

Potete utilizzare l&#39;API Batch per produrre più comunicazioni interattive da un modello. Il modello è una comunicazione interattiva senza alcun dato. L&#39;API Batch combina i dati con un modello per produrre una comunicazione interattiva. L&#39;API è utile nella produzione di massa di comunicazioni interattive. Ad esempio, bollette telefoniche, estratti conto della carta di credito per più clienti.

[Ulteriori informazioni sull&#39;API di generazione batch](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Questo articolo fornisce risorse di esempio per generare documenti di comunicazione interattiva mediante l&#39;API Batch.

## Generazione batch con cartella esaminata

* Importa il modello [di comunicazione](assets/Beneficiaries-confirmation.zip) interattiva nel server AEM Forms .
* Importa la configurazione [della cartella](assets/batch-generation-api.zip)controllata. In questo modo verrà creata una cartella chiamata `batchAPI` nell&#39;unità C.

**Se si esegue  AEM Forms su un sistema operativo non Windows, seguire i 3 passaggi indicati di seguito:**

1. [Apri cartella esaminata](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selezionate BatchAPIWatchedFolder e fate clic su Modifica.
3. Modificare il percorso in modo che corrisponda al sistema operativo in uso.

![path](assets/watched-folder-batch-api-basic.PNG)

* Scaricate ed estraete il contenuto del file [](assets/jsonfile.zip)zip. Il file zip contiene una cartella denominata `jsonfile` che contiene `beneficiaries.json` il file. Questo file ha i dati per generare 3 documenti.

* Trascinate la `jsonfile` cartella nella cartella di input della cartella esaminata.
* Una volta che la cartella è stata scelta per l’elaborazione, controllate la cartella dei risultati della cartella esaminata. Dovresti vedere 3 file PDF generati

## Generazione batch con richieste REST

Potete richiamare l&#39;API [](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) Batch tramite richieste REST. Potete esporre gli endpoint REST per altre applicazioni affinché richiamino l&#39;API per generare documenti.
Le risorse di esempio fornite espongono l’endpoint REST per la generazione di documenti di comunicazione interattiva. Il servlet accetta i seguenti parametri:

* fileName - Posizione del file di dati nel file system.
* templatePath - Percorso modello IC
* saveLocation - Posizione in cui salvare i documenti generati nel file system
* channelType - Stampa, Web o entrambi
* recordId - percorso JSON all&#39;elemento per impostare il nome di una comunicazione interattiva

La schermata seguente mostra i parametri e i relativi valori![di richiesta di esempio](assets/generate-ic-batch-servlet.PNG)

## Distribuzione di risorse campione sul server

* Importare [ICTemplate](assets/ICTemplate.zip) utilizzando [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Importa gestore [invio](assets/BatchAPICustomSubmit.zip) personalizzato tramite [gestore pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa modulo [](assets/BatchGenerationAPIAF.zip) adattivo utilizzando l&#39;interfaccia [Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Implementare e avviare il bundle OSGI [personalizzato](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) utilizzando la console Web [Felix](http://localhost:4502/system/console/bundles)
* [Attiva generazione batch inviando il modulo](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
