---
title: Configurare le variabili di ambiente per l’estensibilità di Asset Compute
description: Le variabili di ambiente vengono mantenute nel file .env per lo sviluppo locale e vengono utilizzate per fornire le credenziali di Adobe I/O e dell’archiviazione cloud richieste per lo sviluppo locale.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 121
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Configurare le variabili di ambiente

![dot env file](assets/environment-variables/dot-env-file.png)

Prima di iniziare lo sviluppo dei processi di lavoro di Asset Compute, assicurati che il progetto sia configurato con le informazioni di Adobe I/O e dell’archiviazione cloud. Queste informazioni sono memorizzate in `.env` del progetto, che viene utilizzato solo per lo sviluppo locale, e non vengono salvate in Git. Il file `.env` consente di esporre le coppie chiave/valori all&#39;ambiente di sviluppo locale Asset Compute locale. Quando [distribuisce](../deploy/runtime.md) processi di lavoro Asset Compute in Adobe I/O Runtime, il file `.env` non viene utilizzato, ma viene passato un sottoinsieme di valori tramite variabili di ambiente. Nel file `.env` possono essere archiviati anche altri parametri e segreti personalizzati, ad esempio le credenziali di sviluppo per i servizi Web di terze parti.

## Fai riferimento a `private.key`

![chiave privata](assets/environment-variables/private-key.png)

Apri il file `.env`, rimuovi il commento dalla chiave `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e fornisci nel file system il percorso assoluto di `private.key` che corrisponde al certificato pubblico aggiunto al progetto Adobe I/O App Builder.

+ Se la coppia di chiavi è stata generata da Adobe I/O, è stata scaricata automaticamente come parte di `config.zip`.
+ Se hai fornito la chiave pubblica ad Adobe I/O, dovresti anche essere in possesso della chiave privata corrispondente.
+ In caso contrario, puoi generare nuove coppie di chiavi o caricare nuove chiavi pubbliche nella parte inferiore di:
  [https://console.adobe.com](https://console.adobe.io) > Il tuo progetto Asset Compute App Builder > Aree di lavoro @ Sviluppo > Account di servizio (JWT).

Ricorda che il file `private.key` non deve essere archiviato in Git in quanto contiene segreti, ma deve essere archiviato in un luogo sicuro all&#39;esterno del progetto.

Ad esempio, in macOS potrebbe presentarsi così:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurare le credenziali di Cloud Storage

Lo sviluppo locale dei processi di lavoro di Asset Compute richiede l&#39;accesso all&#39;[archiviazione cloud](../set-up/accounts-and-services.md#cloud-storage). Le credenziali dell&#39;archiviazione cloud utilizzate per lo sviluppo locale sono fornite nel file `.env`.

Questa esercitazione preferisce l&#39;utilizzo dell&#39;archiviazione BLOB di Azure, tuttavia è possibile utilizzare Amazon S3 e le chiavi corrispondenti nel file `.env`.

### Utilizzo dell’archiviazione BLOB di Azure

Rimuovere il commento e popolare le chiavi seguenti nel file `.env` e popolarle con i valori per l&#39;archiviazione cloud con provisioning trovati nel portale di Azure.

![Archiviazione BLOB di Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valore per la chiave `AZURE_STORAGE_CONTAINER_NAME`
1. Valore per la chiave `AZURE_STORAGE_ACCOUNT`
1. Valore per la chiave `AZURE_STORAGE_KEY`

Ad esempio, potrebbe presentarsi così (valori solo a titolo illustrativo):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Il file `.env` risultante ha il seguente aspetto:

![Credenziali archiviazione BLOB di Azure](assets/environment-variables/cloud-storage-credentials.png)

Se NON utilizzi l’archiviazione BLOB di Microsoft Azure, rimuovi o lascia questi commenti (con il prefisso `#`).

### Utilizzo dell’archiviazione cloud Amazon S3{#amazon-s3}

Se utilizzi l&#39;archiviazione cloud Amazon S3, rimuovi il commento e popola le seguenti chiavi nel file `.env`.

Ad esempio, potrebbe presentarsi così (valori solo a titolo illustrativo):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Convalida della configurazione del progetto

Una volta configurato il progetto Asset Compute generato, convalidare la configurazione prima di apportare modifiche al codice per garantire il provisioning dei servizi di supporto nei file `.env`.

Per avviare Asset Compute Development Tool per il progetto Asset Compute:

1. Apri una riga di comando nella directory principale del progetto Asset Compute (in VS Code può essere aperta direttamente nell’IDE tramite Terminal > New Terminal) ed esegui il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo Asset Compute locale verrà aperto nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![esecuzione app aio](assets/environment-variables/aio-app-run.png)

1. Esaminare l&#39;output della riga di comando e il browser Web per i messaggi di errore durante l&#39;inizializzazione dello strumento di sviluppo.
1. Per arrestare lo strumento di sviluppo Asset Compute, tocca `Ctrl-C` nella finestra che ha eseguito `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Impossibile avviare lo strumento di sviluppo. Private.key mancante](../troubleshooting.md#missing-private-key)
