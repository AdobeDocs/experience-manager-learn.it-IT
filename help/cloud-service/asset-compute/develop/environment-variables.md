---
title: Configurare le variabili di ambiente per  estensibilità del Asset compute
description: Le variabili di ambiente vengono mantenute nel file .env per lo sviluppo locale e vengono utilizzate per fornire  credenziali Adobe I/O e credenziali di archiviazione cloud richieste per lo sviluppo locale.
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

Prima di iniziare lo sviluppo di  Asset compute di lavoro, accertati che il progetto sia configurato con  informazioni di Adobe I/O e archiviazione cloud. Queste informazioni vengono memorizzate in `.env` del progetto, che viene utilizzato solo per lo sviluppo locale e non per il salvataggio in Git. Il file `.env` fornisce un modo pratico per esporre le coppie chiave/valori all&#39;ambiente di sviluppo locale del Asset compute  locale. Se [distribuisci](../deploy/runtime.md)  Asset compute in Adobe I/O Runtime, il file `.env` non viene utilizzato, ma viene passato un sottoinsieme di valori tramite variabili di ambiente. Altri parametri e segreti personalizzati possono essere memorizzati anche nel file `.env`, come le credenziali di sviluppo per i servizi Web di terze parti.

## Fare riferimento a `private.key`

![chiave privata](assets/environment-variables/private-key.png)

Aprite il file `.env`, rimuovete il commento dalla chiave `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e fornite il percorso assoluto sul file system alla `private.key` che viene associata al certificato pubblico aggiunto al progetto Adobe I/O FireFly .

+ Se la coppia di chiavi è stata generata da  Adobe I/O, è stata scaricata automaticamente come parte del `config.zip`.
+ Se hai fornito la chiave pubblica per  Adobe I/O, devi anche essere in possesso della chiave privata corrispondente.
+ Se non disponete di queste coppie di chiavi, potete generare nuove coppie di chiavi o caricare nuove chiavi pubbliche nella parte inferiore di:
   [https://console.adobe.com](https://console.adobe.io) > Your  Asset compute Firefly project > Workspaces @ Development > Service Account (JWT).

Ricorda che il file `private.key` non deve essere archiviato in Git perché contiene segreti, ma deve essere memorizzato in un luogo sicuro al di fuori del progetto.

Ad esempio, in macOS potrebbe essere:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurare le credenziali di archiviazione cloud

Lo sviluppo locale di  Asset compute richiede l&#39;accesso a [cloud storage](../set-up/accounts-and-services.md#cloud-storage). Le credenziali di archiviazione cloud utilizzate per lo sviluppo locale vengono fornite nel file `.env`.

Questa esercitazione preferisce l&#39;uso di Azure Blob Storage, tuttavia  Amazon S3 e le relative chiavi nel file `.env` possono essere utilizzate.

### Utilizzo dell&#39;archiviazione BLOB di Azure

Rimuovete i commenti e compilate le seguenti chiavi nel file `.env`, quindi inseritele con i valori per l&#39;archiviazione cloud di cui è stato effettuato il provisioning trovati in Azure Portal.

![Archiviazione BLOB di Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valore per la chiave `AZURE_STORAGE_CONTAINER_NAME`
1. Valore per la chiave `AZURE_STORAGE_ACCOUNT`
1. Valore per la chiave `AZURE_STORAGE_KEY`

Ad esempio, potrebbe essere simile a (solo valori per illustrazione):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Il file `.env` risultante si presenta come segue:

![Credenziali archiviazione BLOB di Azure](assets/environment-variables/cloud-storage-credentials.png)

Se NON si utilizza l&#39;archiviazione BLOB di Microsoft Azure, rimuovere o lasciare i commenti (prefissandoli con `#`).

### Utilizzo  Amazon S3 cloud storage{#amazon-s3}

Se si utilizza &#39;archiviazione cloud Amazon S3, rimuovere il commento e compilare le seguenti chiavi nel file `.env`.

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

Una volta configurato il progetto  Asset compute generato, convalidate la configurazione prima di apportare modifiche al codice per garantire il provisioning dei servizi di supporto, nei file `.env`.

Per avviare  Strumento di sviluppo Asset compute per il progetto del Asset compute :

1. Aprite una riga di comando nella radice del progetto del Asset compute  (in Codice VS questo può essere aperto direttamente nell&#39;IDE tramite Terminal > Nuovo terminale) ed eseguite il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo  Asset compute locale si aprirà nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![avvio app aio](assets/environment-variables/aio-app-run.png)

1. Osservate l&#39;output della riga di comando e il browser Web per visualizzare i messaggi di errore durante l&#39;inizializzazione dello strumento di sviluppo.
1. Per arrestare lo strumento di sviluppo  Asset compute, toccare `Ctrl-C` nella finestra che ha eseguito `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Impossibile avviare lo strumento di sviluppo a causa di private.key mancante](../troubleshooting.md#missing-private-key)
