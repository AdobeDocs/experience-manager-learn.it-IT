---
title: Configurare le variabili di ambiente per l’estensibilità di Asset Compute
description: Le variabili di ambiente vengono mantenute nel file .env per lo sviluppo locale e vengono utilizzate per fornire  credenziali di I/O Adobe e le credenziali di archiviazione cloud richieste per lo sviluppo locale.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---


# Configurare le variabili di ambiente

![file .env punto](assets/environment-variables/dot-env-file.png)

Prima di iniziare lo sviluppo di Asset Compute Workers, assicurarsi che il progetto sia configurato con  Adobe I/O e informazioni sull&#39;archiviazione cloud. Queste informazioni vengono memorizzate nel progetto `.env` che viene utilizzato solo per lo sviluppo locale, e non salvate in Git. Il `.env` file fornisce un modo pratico per esporre le coppie chiave/valori all&#39;ambiente di sviluppo locale di elaborazione risorse. Quando si [distribuiscono](../deploy/runtime.md) i lavoratori di calcolo delle risorse in Adobe I/O Runtime, il `.env` file non viene utilizzato, ma un sottoinsieme di valori viene trasmesso tramite variabili di ambiente. Altri parametri e segreti personalizzati possono essere memorizzati anche nel `.env` file, come le credenziali di sviluppo per i servizi Web di terze parti.

## Fare riferimento al `private.key`

![chiave privata](assets/environment-variables/private-key.png)

Aprite il `.env` file, rimuovete il commento dalla `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` chiave e fornite il percorso assoluto sul file system al `private.key` quale si associano le coppie con il certificato pubblico aggiunto al progetto I/O FireFly del Adobe .

+ Se la coppia di chiavi è stata generata da  I/O Adobe, è stata scaricata automaticamente come parte del `config.zip`.
+ Se hai fornito la chiave pubblica per  I/O Adobe, dovresti anche essere in possesso della chiave privata corrispondente.
+ Se non disponete di queste coppie di chiavi, potete generare nuove coppie di chiavi o caricare nuove chiavi pubbliche nella parte inferiore di:
   [https://console.adobe.com](https://console.adobe.io) > Your Asset Compute Firefly project > Workspaces @ Development > Service Account (JWT).

Ricorda che il `private.key` file non deve essere archiviato in Git in quanto contiene segreti, ma dovrebbe essere memorizzato in un luogo sicuro al di fuori del progetto.

Ad esempio, in macOS potrebbe essere:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurare le credenziali di archiviazione cloud

Lo sviluppo locale di Asset Compute Workers richiede l&#39;accesso all&#39;archiviazione [](../set-up/accounts-and-services.md#cloud-storage)cloud. Le credenziali di archiviazione cloud utilizzate per lo sviluppo locale vengono fornite nel `.env` file.

Questa esercitazione preferisce l&#39;uso di Azure Blob Storage, tuttavia  Amazon S3 e le relative chiavi nel `.env` file possono essere utilizzate.

### Utilizzo dell&#39;archiviazione BLOB di Azure

Rimuovete i commenti e compilate le seguenti chiavi nel `.env` file, quindi inseritele con i valori per l&#39;archiviazione cloud di cui è stato effettuato il provisioning trovati in Azure Portal.

![Archiviazione BLOB di Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valore per la `AZURE_STORAGE_CONTAINER_NAME` chiave
1. Valore per la `AZURE_STORAGE_ACCOUNT` chiave
1. Valore per la `AZURE_STORAGE_KEY` chiave

Ad esempio, potrebbe essere simile a (solo valori per illustrazione):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Il `.env` file risultante si presenta come segue:

![Credenziali archiviazione BLOB di Azure](assets/environment-variables/cloud-storage-credentials.png)

Se NON si utilizza l&#39;archiviazione BLOB di Microsoft Azure, rimuovere o lasciare i commenti (prefissandoli con `#`).

### Utilizzo  Amazon S3 cloud storage{#amazon-s3}

Se utilizzate &#39;archiviazione cloud Amazon S3, rimuovete il commento e inserite le seguenti chiavi nel `.env` file.

Ad esempio, potrebbe essere simile a (solo valori per illustrazione):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Convalida della configurazione del progetto

Una volta configurato il progetto Asset Compute generato, convalidate la configurazione prima di apportare modifiche al codice in modo da garantire il provisioning dei servizi di supporto nei `.env` file.

Per avviare lo strumento di sviluppo del calcolo delle risorse per il progetto Asset Compute:

1. Aprite una riga di comando nella directory principale del progetto Asset Compute (in Codice VS questo può essere aperto direttamente nell’IDE tramite Terminal > Nuovo terminale) ed eseguite il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo del calcolo risorse locale si aprirà nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![avvio app aio](assets/environment-variables/aio-app-run.png)

1. Osservate l&#39;output della riga di comando e il browser Web per visualizzare i messaggi di errore durante l&#39;inizializzazione dello strumento di sviluppo.
1. Per arrestare lo strumento di sviluppo di calcolo risorse, toccate `Ctrl-C` nella finestra che è stata eseguita `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Impossibile avviare lo strumento di sviluppo a causa di private.key mancante](../troubleshooting.md#missing-private-key)
