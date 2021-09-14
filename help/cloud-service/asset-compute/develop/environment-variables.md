---
title: Configura le variabili di ambiente per Asset compute l’estensibilità
description: Le variabili di ambiente vengono mantenute nel file .env per lo sviluppo locale e vengono utilizzate per fornire le credenziali di Adobe I/O e di archiviazione cloud richieste per lo sviluppo locale.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---

# Configurare le variabili di ambiente

![file env punto](assets/environment-variables/dot-env-file.png)

Prima di iniziare lo sviluppo dei processi di lavoro di Asset compute, assicurati che il progetto sia configurato con informazioni di Adobe I/O e archiviazione cloud. Queste informazioni vengono memorizzate in `.env` del progetto, utilizzato solo per lo sviluppo locale, e non in Git. Il file `.env` offre un modo semplice per esporre coppie chiave/valori all’ambiente di sviluppo locale di Asset compute locale. Quando [distribuisci](../deploy/runtime.md) Asset compute worker in Adobe I/O Runtime, il file `.env` non viene utilizzato, ma viene passato un sottoinsieme di valori tramite variabili di ambiente. È possibile memorizzare altri parametri e segreti personalizzati anche nel file `.env` , ad esempio le credenziali di sviluppo per i servizi Web di terze parti.

## Fai riferimento a `private.key`

![chiave privata](assets/environment-variables/private-key.png)

Apri il file `.env` , rimuovi il commento dalla chiave `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e fornisci il percorso assoluto del file system al `private.key` che corrisponde al certificato pubblico aggiunto al progetto FireFly di Adobe I/O.

+ Se la coppia di chiavi è stata generata da Adobe I/O, è stata scaricata automaticamente come parte del `config.zip`.
+ Se hai fornito la chiave pubblica all&#39;Adobe I/O, dovresti anche essere in possesso della chiave privata corrispondente.
+ Se non disponi di queste coppie di chiavi, puoi generare nuove coppie di chiavi o caricare nuove chiavi pubbliche nella parte inferiore di:
   [https://console.adobe.com](https://console.adobe.io) > Il tuo progetto Firefly Asset compute > Aree di lavoro @ Sviluppo > Account di servizio (JWT).

Ricorda che il file `private.key` non deve essere archiviato in Git in quanto contiene segreti, ma deve essere memorizzato in un luogo sicuro al di fuori del progetto.

Ad esempio, in macOS potrebbe essere così:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurare le credenziali di archiviazione cloud

Lo sviluppo locale dei processi di lavoro di Asset compute richiede l&#39;accesso a [cloud storage](../set-up/accounts-and-services.md#cloud-storage). Le credenziali di archiviazione cloud utilizzate per lo sviluppo locale vengono fornite nel file `.env` .

Questa esercitazione preferisce l&#39;utilizzo di Azure Blob Storage, tuttavia è possibile utilizzare Amazon S3 e le relative chiavi nel file `.env` .

### Utilizzo dell’archiviazione BLOB di Azure

Rimuovi il commento e compila le seguenti chiavi nel file `.env` e compila tali chiavi con i valori per l’archiviazione cloud di cui è stato eseguito il provisioning presenti sul portale di Azure.

![Archiviazione BLOB di Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valore della chiave `AZURE_STORAGE_CONTAINER_NAME`
1. Valore della chiave `AZURE_STORAGE_ACCOUNT`
1. Valore della chiave `AZURE_STORAGE_KEY`

Ad esempio, potrebbe essere simile a (solo valori a scopo illustrativo):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Il file `.env` risultante si presenta come segue:

![Credenziali di archiviazione BLOB di Azure](assets/environment-variables/cloud-storage-credentials.png)

Se NON utilizzi l’archiviazione BLOB di Microsoft Azure, rimuovi o lascia questi commenti (eseguendo il prefisso `#`).

### Utilizzo dell’archiviazione cloud Amazon S3{#amazon-s3}

Se utilizzi Amazon S3 cloud storage, rimuovi il commento e compila le seguenti chiavi nel file `.env` .

Ad esempio, potrebbe essere simile a (solo valori a scopo illustrativo):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Convalida della configurazione del progetto

Una volta configurato il progetto di Asset compute generato, convalida la configurazione prima di apportare modifiche al codice per garantire il provisioning dei servizi di supporto nei file `.env` .

Per avviare Asset compute Development Tool per il progetto Asset compute:

1. Apri una riga di comando nella directory principale del progetto di Asset compute (in Codice VS che può essere aperta direttamente nell’IDE tramite Terminal > Nuovo terminale) ed esegui il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo Asset compute locale si aprirà nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![esecuzione di app aio](assets/environment-variables/aio-app-run.png)

1. Osserva l’output della riga di comando e il browser Web per verificare la presenza di messaggi di errore durante l’inizializzazione dello strumento di sviluppo.
1. Per interrompere lo strumento di sviluppo Asset compute, tocca `Ctrl-C` nella finestra che ha eseguito `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Impossibile avviare lo strumento di sviluppo a causa della mancanza di private.key](../troubleshooting.md#missing-private-key)
