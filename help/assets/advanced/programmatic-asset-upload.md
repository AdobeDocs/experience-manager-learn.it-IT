---
title: Caricamento di risorse programmatiche in AEM as a Cloud Service
description: Scopri come caricare le risorse in AEM as a Cloud Service utilizzando la libreria Node.js di @adobe/aem-upload.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer, Architect
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: bf996405c360c77475d9f76d5de9bcd4fde3c163
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 1%

---


# Caricamento di risorse programmatiche in AEM as a Cloud Service

Scopri come caricare le risorse nell’ambiente AEM as a Cloud Service utilizzando l’applicazione client che utilizza la libreria Node.js [aem-upload](https://github.com/adobe/aem-upload).

## Argomenti trattati

In questa esercitazione imparerai:

+ Come utilizzare l&#39;approccio _caricamento binario diretto_ per caricare le risorse nell&#39;ambiente AEM as a Cloud Service (RDE, Dev, Stage, Prod) utilizzando la libreria [aem-upload](https://github.com/adobe/aem-upload) Node.js.
+ Come configurare ed eseguire l&#39;applicazione [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) per caricare risorse nell&#39;ambiente AEM as a Cloud Service.
+ Esamina il codice dell’applicazione di esempio e comprendi i dettagli di implementazione.
+ Scopri le best practice per il caricamento programmatico delle risorse nell’ambiente AEM as a Cloud Service.

## Informazioni sull&#39;approccio _caricamento binario diretto_

L&#39;approccio di _caricamento binario diretto_ consente di caricare i file dal sistema di origine _direttamente nell&#39;archiviazione cloud_ nell&#39;ambiente AEM as a Cloud Service utilizzando un _URL preceduto_. Elimina la necessità di instradare i dati binari attraverso i processi Java di AEM, velocizzando i caricamenti e riducendo il carico del server.

Prima di eseguire l’applicazione di esempio, comprendiamo il flusso di caricamento binario diretto.

Nel flusso di caricamento binario diretto, i dati binari vengono caricati direttamente nell’archiviazione cloud con URL prefirmati. L’AEM as a Cloud Service è responsabile dell’elaborazione leggera, ad esempio la generazione degli URL prefirmati e la notifica al servizio AEM Asset Compute del completamento del caricamento. Il seguente diagramma di flusso logico illustra il flusso di caricamento binario diretto.

![Flusso di caricamento binario diretto](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### Libreria di caricamento AEM

La libreria [aem-upload](https://github.com/adobe/aem-upload) Node.js astrae i dettagli di implementazione dell&#39;approccio _direct binary upload_. Fornisce due classi per orchestrare il processo di caricamento:

+ **FileSystemUpload** - Utilizzarlo per caricare i file dal file system locale, incluso il supporto per le strutture di directory
+ **DirectBinaryUpload** - Utilizzalo per un controllo più dettagliato sul processo di caricamento binario, ad esempio per il caricamento da flussi o buffer

>[!CAUTION]
>
>Non esiste un equivalente della libreria [aem-upload](https://github.com/adobe/aem-upload) in Java. L&#39;applicazione client deve essere scritta in Node.js per utilizzare l&#39;approccio _caricamento binario diretto_. Per ulteriori informazioni, consulta la pagina [API e operazioni di Experience Manager Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis).

## Applicazione di esempio

Utilizza l&#39;applicazione [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) per apprendere il processo di caricamento programmatico delle risorse. L&#39;applicazione di esempio illustra l&#39;utilizzo di entrambe le classi `FileSystemUpload` e `DirectBinaryUpload` della libreria [aem-upload](https://github.com/adobe/aem-upload).

### Prerequisiti

Prima di eseguire l&#39;applicazione di esempio, verificare di disporre dei seguenti prerequisiti:

+ Ambiente di authoring di AEM as a Cloud Service come ambiente di sviluppo rapido (RDE), ambiente di sviluppo, ecc.
+ Node.js (ultima versione LTS)
+ Node.js e npm: nozioni di base

>[!CAUTION]
>
> NON puoi utilizzare AEM as a Cloud Service SDK (o istanza AEM locale) per testare il processo di caricamento delle risorse a livello di programmazione. È necessario utilizzare un ambiente AEM as a Cloud Service come ambiente di sviluppo rapido (RDE, Rapid Development Environment), ambiente di sviluppo, ecc.

### Scarica l’applicazione di esempio

1. Scarica il file zip dell&#39;applicazione [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) ed estrailo.

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. Apri la cartella estratta nel tuo editor di codice preferito.

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. Utilizzando il terminale dell’editor di codice, installa le dipendenze.

   ```bash
   $ npm install
   ```

   ![Applicazione di esempio](./assets/programmatic-asset-upload/install-dependencies.png)

### Configurare l’applicazione di esempio

Prima di eseguire l&#39;applicazione di esempio, è necessario configurarla con i dettagli dell&#39;ambiente AEM as a Cloud Service necessari, come l&#39;URL di AEM Author, il _metodo di autenticazione_ e il percorso della cartella delle risorse.

Sono disponibili _più metodi di autenticazione_ supportati dalla libreria [aem-upload](https://github.com/adobe/aem-upload) Node.js. Nella tabella seguente sono riepilogati i _metodi di autenticazione_ supportati e il relativo scopo.

| | Autenticazione di base | [Token di sviluppo locale](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [Credenziali servizio](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [OAuth S2S](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [App Web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [SPA OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| È supportato? | &amp;check; | &amp;check; | &amp;check; | &amp;cross; | &amp;cross; | &amp;cross; |
| Scopo | Sviluppo locale | Sviluppo locale | Produzione | N/D | N/D | N/D |

Per configurare l’applicazione di esempio, effettua le seguenti operazioni:

1. Copiare il file `env.example` nel file `.env`.

   ```bash
   $ cp env.example .env
   ```

1. Apri il file `.env` e aggiorna la variabile di ambiente `AEM_URL` con l&#39;URL di authoring di AEM as a Cloud Service.

1. Scegli il metodo di autenticazione tra le seguenti opzioni e aggiorna le variabili di ambiente corrispondenti.

>[!BEGINTABS]

>[!TAB Autenticazione di base]

Per utilizzare l’autenticazione di base, è necessario creare un utente nell’ambiente AEM as a Cloud Service.

1. Accedi all’ambiente AEM as a Cloud Service.

1. Passa a **Strumenti** > **Sicurezza** > **Utenti** e fai clic sul pulsante **Crea**.

   ![Crea utente](./assets/programmatic-asset-upload/create-user.png)

1. Immetti i dettagli utente

   ![Dettagli utente](./assets/programmatic-asset-upload/user-details.png)

1. Nella scheda **Groups**, aggiungi il gruppo **DAM Users**. Fare clic sul pulsante **Salva e chiudi**.

   ![Aggiungi gruppo utenti DAM](./assets/programmatic-asset-upload/add-dam-users-group.png)

1. Aggiornare le variabili di ambiente `AEM_USERNAME` e `AEM_PASSWORD` con il nome utente e la password dell&#39;utente creato.

>[!TAB Token di sviluppo locale]

Per ottenere il token di sviluppo locale, è necessario utilizzare il Developer Console **AEM**. Il token generato è di tipo JSON Web Token (JWT).

1. Accedi a [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) e passa alla pagina dei dettagli **Ambiente** desiderata. Fare clic su **&quot;...&quot;** e selezionare **Developer Console**.

   ![Console per sviluppatori](./assets/programmatic-asset-upload/developer-console.png)

1. Accedi ad AEM Developer Console e utilizza il pulsante _Nuova console_ per passare alla console più recente.

1. Dalla sezione **Strumenti**, seleziona **Integrazioni** e fai clic sul pulsante **Ottieni token locale**.

   ![Ottieni token locale](./assets/programmatic-asset-upload/get-local-token.png)

1. Copiare il valore del token e aggiornare la variabile di ambiente `AEM_BEARER_TOKEN` con il valore del token.

Il token di sviluppo locale è valido per 24 ore ed è rilasciato per l’utente che lo ha generato.

>[!TAB Credenziali servizio]

Per ottenere le credenziali del servizio, è necessario utilizzare il Developer Console **AEM**. Viene utilizzato per generare il token del tipo JSON Web Token (JWT) utilizzando il modulo [jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm.

1. Accedi a [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) e passa alla pagina dei dettagli **Ambiente** desiderata. Fare clic su **&quot;...&quot;** e selezionare **Developer Console**.

   ![Console per sviluppatori](./assets/programmatic-asset-upload/developer-console.png)

1. Accedi ad AEM Developer Console e utilizza il pulsante _Nuova console_ per passare alla console più recente.

1. Dalla sezione **Strumenti**, seleziona **Integrazioni** e fai clic sul pulsante **Crea nuovo account tecnico**.

   ![Ottieni credenziali servizio](./assets/programmatic-asset-upload/get-service-credentials.png)

1. Fai clic sull&#39;opzione **Visualizza** per copiare le credenziali del servizio JSON.

   ![Credenziali servizio](./assets/programmatic-asset-upload/service-credentials.png)

1. Creare un file `service-credentials.json` nella radice dell&#39;applicazione di esempio e incollare le credenziali del servizio JSON nel file.

1. Aggiornare la variabile di ambiente `AEM_SERVICE_CREDENTIALS_FILE` con il percorso del file service-credentials.json.

1. Assicurati che l’utente con le credenziali del servizio disponga delle autorizzazioni necessarie per caricare le risorse nell’ambiente AEM as a Cloud Service. Per ulteriori informazioni, vedere [Configurare l&#39;accesso nella pagina AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem).

>[!ENDTABS]

Ecco il file `.env` completo con tutti e tre i metodi di autenticazione configurati.

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### Eseguire l’applicazione di esempio

L’applicazione di esempio mostra tre modi diversi per caricare le risorse di esempio nell’ambiente AEM as a Cloud Service.

1. **FileSystemUpload** - Carica i file da un file system locale con supporto per la struttura delle directory e creazione automatica delle cartelle
1. **DirectBinaryUpload** - Carica un [file remoto](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets). Il file binario viene inserito nel buffer della memoria prima di essere caricato nell’ambiente AEM as a Cloud Service.
1. **Caricamento batch** - Carica più file da un file system locale in batch con logica automatica di esecuzione di nuovi tentativi e recupero degli errori. Dietro le quinte, utilizza la classe `FileSystemUpload` per caricare i file dal file system locale.

Le risorse da caricare si trovano nella cartella `sample-assets` e contengono `img`, `video` e `doc` sottocartelle, ognuna contenente alcune risorse di esempio.

1. Per eseguire l&#39;applicazione di esempio, utilizzare il comando seguente:

```bash
$ npm start
```

1. Immetti l&#39;opzione desiderata _numero_ tra le seguenti scelte:

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

Le schede seguenti mostrano l’esecuzione dell’applicazione di esempio, il relativo output e le risorse caricate nell’ambiente AEM as a Cloud Service per ogni metodo di caricamento.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

1. L&#39;output dell&#39;applicazione di esempio per l&#39;opzione `FileSystemUpload`:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets caricato con l&#39;opzione `FileSystemUpload` nell&#39;ambiente AEM as a Cloud Service:

   ![Risorse caricate nell&#39;ambiente AEM as a Cloud Service utilizzando la classe FileSystemUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB DirectBinaryUpload]

1. L&#39;output dell&#39;applicazione di esempio per l&#39;opzione `DirectBinaryUpload`:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. Assets caricato con l&#39;opzione `DirectBinaryUpload` nell&#39;ambiente AEM as a Cloud Service:

![Risorse caricate nell&#39;ambiente AEM as a Cloud Service tramite la classe DirectBinaryUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB Caricamento batch]

1. L&#39;output dell&#39;applicazione di esempio per l&#39;opzione `Batch Upload`:

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets caricato con l&#39;opzione `Batch Upload` nell&#39;ambiente AEM as a Cloud Service:

![Risorse caricate nell&#39;ambiente AEM as a Cloud Service utilizzando la classe BatchUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## Esamina il codice dell’applicazione di esempio

Il punto di ingresso principale dell&#39;applicazione di esempio è il file `index.js`. Contiene la funzione `promptUser` che richiede all&#39;utente una scelta ed esegue l&#39;esempio selezionato.

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

Per il codice completo, fare riferimento al file `index.js` dell&#39;applicazione di esempio.

Le schede seguenti mostrano i dettagli di implementazione di ciascun metodo di caricamento.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

La classe `FileSystemUpload` viene utilizzata per caricare file dal file system locale con supporto per la struttura delle directory e creazione automatica delle cartelle.

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

Per il codice completo, fare riferimento al file `examples/filesystem-upload.js` dell&#39;applicazione di esempio.

>[!TAB DirectBinaryUpload]

La classe `DirectBinaryUpload` viene utilizzata per caricare un file remoto nell&#39;ambiente AEM as a Cloud Service.

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

Per il codice completo, fare riferimento al file `examples/direct-binary-upload.js` dell&#39;applicazione di esempio.

>[!TAB Caricamento batch]

Divide i file in batch e li carica in batch con logica automatica di esecuzione di nuovi tentativi e recupero degli errori. Dietro le quinte, utilizza la classe `FileSystemUpload` per caricare i file dal file system locale.

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

Per il codice completo, fare riferimento al file `examples/batch-upload.js` dell&#39;applicazione di esempio.

>[!ENDTABS]

Inoltre, il file `README.md` dell&#39;applicazione di esempio contiene la documentazione dettagliata dell&#39;applicazione di esempio.

## Best practice

1. **Scegliere il metodo di autenticazione corretto:**
Utilizza le credenziali del servizio solo per gli ambienti di produzione, il token di sviluppo locale e l’autenticazione di base per lo sviluppo e i test. Assicurati che l’utente con le credenziali del servizio disponga delle autorizzazioni necessarie per caricare le risorse nell’ambiente AEM as a Cloud Service.

1. **Scegliere il metodo di caricamento corretto:**
Utilizzare FileSystemUpload per i file locali con creazione automatica della cartella, DirectBinaryUpload per flussi/buffer/URL remoti con controllo dettagliato e Batch Upload pattern per gli ambienti di produzione con più di 1000 file che richiedono una logica di ripetizione dei tentativi.

1. **Struttura correttamente gli oggetti file DirectBinaryUpload**
Utilizza la proprietà blob (non il buffer) con i campi obbligatori: { fileName, fileSize, blob: buffer, targetFolder } e ricorda che DirectBinaryUpload NON crea automaticamente cartelle.

1. **Applicazione di esempio come riferimento:**
L’applicazione di esempio è un buon riferimento per i dettagli di implementazione del processo di caricamento delle risorse a livello di programmazione. Puoi utilizzarlo come punto di partenza per la tua implementazione.
