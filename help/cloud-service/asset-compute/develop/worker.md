---
title: Sviluppare un processo di lavoro Asset Compute
description: I processi di lavoro Asset Compute sono alla base dei progetti Asset Compute in quanto forniscono funzionalità personalizzate che eseguono o orchestrano il lavoro eseguito su una risorsa per crearne una nuova rappresentazione.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 451
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# Sviluppare un processo di lavoro Asset Compute

I processi di lavoro Asset Compute sono alla base di un progetto Asset Compute in quanto forniscono funzionalità personalizzate che eseguono o orchestrano il lavoro eseguito su una risorsa per crearne una nuova rappresentazione.

Il progetto Asset Compute genera automaticamente un semplice processo di lavoro che copia il binario originale della risorsa in una rappresentazione denominata, senza alcuna trasformazione. In questo tutorial modificheremo questo processo di lavoro per creare una rappresentazione più interessante, per illustrare la potenza dei processi di lavoro Asset Compute.

Creeremo un processo di lavoro Asset Compute che genera una nuova rappresentazione orizzontale dell’immagine, che copre lo spazio vuoto a sinistra e a destra della rappresentazione della risorsa con una versione sfocata della risorsa. La larghezza, l&#39;altezza e la sfocatura della rappresentazione finale sono parametrizzate.

## Flusso logico di una chiamata di un processo di lavoro Asset Compute

I processi di lavoro Asset Compute implementano il contratto API del processo di lavoro Asset Compute SDK nella funzione `renditionCallback(...)`, concettualmente:

+ __Input:__ Parametri binari ed elaborazione originali del profilo di una risorsa AEM
+ __Output:__ una o più rappresentazioni da aggiungere alla risorsa AEM

![Flusso logico di lavoro Asset Compute](./assets/worker/logical-flow.png)

1. Il servizio AEM Author richiama il processo di lavoro Asset Compute, fornendo il binario originale __(1a)__ della risorsa (`source` parametro) e __(1b)__ eventuali parametri definiti nel profilo di elaborazione (`rendition.instructions` parametro).
1. Asset Compute SDK orchestra l&#39;esecuzione della funzione `renditionCallback(...)` del processo di lavoro metadati di Asset Compute personalizzato, generando una nuova rappresentazione binaria basata sul file binario originale della risorsa __(1a)__ ed eventuali parametri __(1b)__.

   + In questa esercitazione la rappresentazione viene creata &quot;in process&quot;, ovvero il processo di lavoro compone la rappresentazione, tuttavia il binario di origine può essere inviato anche ad altre API di servizi Web per la generazione della rappresentazione.

1. Il processo di lavoro Asset Compute salva i dati binari della nuova rappresentazione in `rendition.path`.
1. I dati binari scritti in `rendition.path` vengono trasportati tramite Asset Compute SDK al servizio AEM Author ed esposti come __(4a)__ una rappresentazione di testo e __(4b)__ persistenti al nodo di metadati della risorsa.

Il diagramma precedente illustra le preoccupazioni degli sviluppatori di Asset Compute e il flusso logico alla chiamata di Asset Compute worker. Per curiosità, sono disponibili [i dettagli interni dell&#39;esecuzione di Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html?lang=it), tuttavia solo i contratti API pubblici di Asset Compute SDK possono dipendere da.

## Anatomia di un lavoratore

Tutti i lavoratori Asset Compute seguono la stessa struttura di base e lo stesso contratto di input/output.

```javascript
'use strict';

// Any npm module imports used by the worker
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

/**
Exports the worker implemented by a custom rendition callback function, which parametrizes the input/output contract for the worker.
 + `source` represents the asset's original binary used as the input for the worker.
 + `rendition` represents the worker's output, which is the creation of a new asset rendition.
 + `params` are optional parameters, which map to additional key/value pairs, including a sub `auth` object that contains Adobe I/O access credentials.
**/
exports.main = worker(async (source, rendition, params) => {
    // Perform any necessary source (input) checks
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        // Throw appropriate errors whenever an erring condition is met
        throw new SourceCorruptError('source file is empty');
    }

    // Access any custom parameters provided via the Processing Profile configuration
    let param1 = rendition.instructions.exampleParam;

    /** 
    Perform all work needed to transform the source into the rendition.
    
    The source data can be accessed:
        + In the worker via a file available at `source.path`
        + Or via a presigned GET URL at `source.url`
    **/
    if (success) {
        // A successful worker must write some data back to `renditions.path`. 
        // This example performs a trivial 1:1 copy of the source binary to the rendition
        await fs.copyFile(source.path, rendition.path);
    } else {
        // Upon failure an Asset Compute Error (exported by @adobe/asset-compute-commons) should be thrown.
        throw new GenericError("An error occurred!", "example-worker");
    }
});

/**
Optionally create helper classes or functions the worker's rendition callback function invokes to help organize code.

Code shared across workers, or to complex to be managed in a single file, can be broken out across supporting JavaScript files in the project and imported normally into the worker. 
**/
function customHelperFunctions() { ... }
```

## Apertura di index.js per il processo di lavoro

![Indice generato automaticamente.js](./assets/worker/autogenerated-index-js.png)

1. Assicurati che il progetto Asset Compute sia aperto in VS Code
1. Passa alla cartella `/actions/worker`
1. Apri il file `index.js`

Questo è il file JavaScript di lavoro che modificheremo in questa esercitazione.

## Installazione e importazione dei moduli npm di supporto

Essendo basati su Node.js, i progetti Asset Compute beneficiano del solido ecosistema di moduli [npm](https://npmjs.com). Per sfruttare i moduli npm, devi prima installarli nel nostro progetto Asset Compute.

In questo processo di lavoro sfrutta [jimp](https://www.npmjs.com/package/jimp) per creare e manipolare l&#39;immagine di rendering direttamente nel codice Node.js.

>[!WARNING]
>
>Asset Compute non supporta tutti i moduli npm per la manipolazione delle risorse. I moduli npm che si basano sull’esistenza di applicazioni come ImageMagick o altre librerie dipendenti dal sistema operativo non sono supportati. È consigliabile limitare l’utilizzo dei moduli npm solo per JavaScript.

1. Apri la riga di comando nella directory principale del progetto Asset Compute (è possibile eseguire questa operazione in VS Code tramite __Terminal > New Terminal__) ed esegui il comando:

   ```
   $ npm install jimp
   ```

1. Importare il modulo `jimp` nel codice di lavoro in modo che possa essere utilizzato tramite l&#39;oggetto JavaScript `Jimp`.
Aggiorna le direttive `require` nella parte superiore di `index.js` del processo di lavoro per importare l&#39;oggetto `Jimp` dal modulo `jimp`:

   ```javascript
   'use strict';
   
   const Jimp = require('jimp');
   const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
   const fs = require('fs').promises;
   
   exports.main = worker(async (source, rendition, params) => {
       // Check handle a corrupt input source
       const stats = await fs.stat(source.path);
       if (stats.size === 0) {
           throw new SourceCorruptError('source file is empty');
       }
   
       // Do work here
   });
   ```

## Leggi parametri

I processi di lavoro di Asset Compute possono leggere i parametri che possono essere trasmessi tramite i Profili di elaborazione definiti nel servizio AEM as a Cloud Service Author. I parametri vengono passati nel processo di lavoro tramite l&#39;oggetto `rendition.instructions`.

È possibile leggerli accedendo a `rendition.instructions.<parameterName>` nel codice del processo di lavoro.

Qui verranno letti i `SIZE`, `BRIGHTNESS` e `CONTRAST` della rappresentazione configurabile, fornendo i valori predefiniti se non ne è stato fornito alcuno tramite il profilo di elaborazione. Tieni presente che `renditions.instructions` vengono passati come stringhe quando vengono richiamati dai profili di elaborazione di AEM as a Cloud Service, quindi assicurati che vengano trasformati nei tipi di dati corretti nel codice di lavoro.

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    // Processing Profiles pass in instructions as Strings, so make sure to parse to correct data types
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    // Do work here
}
```

## Generazione di errori{#errors}

I processi di lavoro di Asset Compute possono riscontrare situazioni che generano errori. Adobe Asset Compute SDK fornisce [una suite di errori predefiniti](https://github.com/adobe/asset-compute-commons#asset-compute-errors) che è possibile generare quando si verificano tali situazioni. Se non viene applicato alcun tipo di errore specifico, è possibile utilizzare `GenericError` oppure definire `ClientErrors` personalizzato specifico.

Prima di iniziare a elaborare la rappresentazione, verifica che tutti i parametri siano validi e supportati nel contesto di questo processo di lavoro:

+ Verificare che i parametri delle istruzioni di rendering per `SIZE`, `CONTRAST` e `BRIGHTNESS` siano validi. In caso contrario, generare un errore personalizzato `RenditionInstructionsError`.
   + Nella parte inferiore del file è definita una classe `RenditionInstructionsError` personalizzata che estende `ClientError`. L&#39;utilizzo di un errore specifico personalizzato è utile quando si scrivono [test](../test-debug/test.md) per il processo di lavoro.

```javascript
'use strict';

const Jimp = require('jimp');
// Import the Asset Compute SDK provided `ClientError` 
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        // Ensure size is within allowable bounds
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Do work here
}

// Create a new ClientError to handle invalid rendition.instructions values
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        // Provide a:
        // + message: describing the nature of this erring condition
        // + name: the name of the error; usually same as class name
        // + reason: a short, searchable, unique error token that identifies this error
        super(message, "RenditionInstructionsError", "rendition_instructions_error");

        // Capture the strack trace
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Creazione della rappresentazione

Con i parametri letti, bonificati e convalidati, viene scritto il codice per generare la rappresentazione. Lo pseudo codice per la generazione della rappresentazione è il seguente:

1. Crea una nuova area di lavoro `renditionImage` nelle dimensioni quadrate specificate tramite il parametro `size`.
1. Crea un oggetto `image` dal file binario della risorsa di origine
1. Utilizza la libreria __Jimp__ per trasformare l&#39;immagine:
   + Ritaglia l&#39;immagine originale in un quadrato centrato
   + Taglia un cerchio dal centro dell&#39;immagine al quadrato
   + Adatta alle dimensioni definite dal valore del parametro `SIZE`
   + Regola contrasto in base al valore del parametro `CONTRAST`
   + Regola luminosità in base al valore del parametro `BRIGHTNESS`
1. Posiziona `image` trasformato al centro di `renditionImage` che ha uno sfondo trasparente
1. Scrivi il composto `renditionImage` in `rendition.path` in modo che possa essere salvato nuovamente in AEM come rappresentazione di una risorsa.

Questo codice utilizza le [API Jimp](https://github.com/oliver-moran/jimp#jimp) per eseguire queste trasformazioni di immagini.

I processi di lavoro di Asset Compute devono terminare in modo sincrono e `rendition.path` deve essere completamente riscritto su prima del completamento di `renditionCallback`. È necessario che le chiamate di funzioni asincrone siano rese sincrone utilizzando l&#39;operatore `await`. Se non hai familiarità con le funzioni asincrone di JavaScript e su come farle eseguire in modo sincrono, acquisisci familiarità con l&#39;[operatore await di JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

Il processo di lavoro completato `index.js` dovrebbe essere simile al seguente:

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read/parse and validate parameters
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image 
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness
    image.crop(
            image.bitmap.width < image.bitmap.height ? 0 : (image.bitmap.width - image.bitmap.height) / 2,
            image.bitmap.width < image.bitmap.height ? (image.bitmap.height - image.bitmap.width) / 2 : 0,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height
        )   
        .circle()
        .scaleToFit(SIZE, SIZE)
        .contrast(CONTRAST)
        .brightness(BRIGHTNESS);

    // Place the transformed image onto the transparent renditionImage to save as PNG
    renditionImage.composite(image, 0, 0)

    // Write the final transformed image to the asset's rendition
    await renditionImage.writeAsync(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Esecuzione del processo di lavoro

Ora che il codice del processo di lavoro è stato completato ed è stato precedentemente registrato e configurato in [manifest.yml](./manifest.md), può essere eseguito utilizzando lo strumento di sviluppo Asset Compute locale per visualizzare i risultati.

1. Dalla directory principale del progetto Asset Compute
1. Esegui `aio app run`
1. Attendere che lo strumento di sviluppo Asset Compute si apra in una nuova finestra
1. Nel menu a discesa __Seleziona un file...__, seleziona un&#39;immagine di esempio da elaborare
   + Seleziona un file di immagine di esempio da utilizzare come binario della risorsa di origine
   + Se non ne esiste ancora alcuna, tocca __(+)__ a sinistra, carica un file di [immagine di esempio](../assets/samples/sample-file.jpg) e aggiorna la finestra del browser Strumenti di sviluppo
1. Aggiorna `"name": "rendition.png"` come lavoratore per generare un PNG trasparente.
   + Questo parametro &quot;name&quot; viene utilizzato solo per lo strumento di sviluppo e non deve essere utilizzato.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png"
           }
       ]
   }
   ```

1. Tocca __Esegui__ e attendi la generazione della rappresentazione
1. La sezione __Rendering__ visualizza in anteprima la rappresentazione generata. Tocca l’anteprima della rappresentazione per scaricarla completamente

   ![Rendering PNG predefinito](./assets/worker/default-rendition.png)

### Esegui il processo di lavoro con i parametri

I parametri, trasmessi tramite le configurazioni del profilo di elaborazione, possono essere simulati negli strumenti di sviluppo Asset Compute fornendoli come coppie chiave/valore nel parametro JSON della rappresentazione.

>[!WARNING]
>
>Durante lo sviluppo locale, i valori possono essere trasmessi utilizzando vari tipi di dati, quando vengono trasmessi da AEM as Cloud Service Processing Profiles come stringhe, quindi assicurati che i tipi di dati corretti vengano analizzati, se necessario.
> Ad esempio, la funzione `crop(width, height)` di Jimp richiede che i relativi parametri siano di `int`. Se `parseInt(rendition.instructions.size)` non viene analizzato in un int, la chiamata a `jimp.crop(SIZE, SIZE)` non riesce perché i parametri sono di tipo &quot;String&quot; incompatibile.

Il nostro codice accetta parametri per:

+ `size` definisce la dimensione della rappresentazione (altezza e larghezza in numeri interi)
+ `contrast` definisce la regolazione del contrasto, deve essere compreso tra -1 e 1, come virgolette
+ `brightness` definisce la regolazione luminosa, deve essere compresa tra -1 e 1, come in virgola mobile

Letti nel processo di lavoro `index.js` tramite:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Aggiorna i parametri della rappresentazione per personalizzare le dimensioni, il contrasto e la luminosità.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png",
               "size": "450",
               "contrast": "0.30",
               "brightness": "0.15"
           }
       ]
   }
   ```

1. Tocca di nuovo __Esegui__
1. Tocca l’anteprima della rappresentazione per scaricarla e rivederla. Notate le dimensioni e come il contrasto e la luminosità sono stati modificati rispetto al rendering predefinito.

   ![Rendering PNG con parametri](./assets/worker/parameterized-rendition.png)

1. Caricare altre immagini nel menu a discesa __File Source__ e provare a eseguire il processo di lavoro utilizzando parametri diversi.

## Worker index.js su Github

Il `index.js` finale è disponibile su Github all&#39;indirizzo:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Risoluzione dei problemi

+ [La rappresentazione ha restituito un disegno parziale/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
