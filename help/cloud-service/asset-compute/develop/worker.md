---
title: Sviluppa un lavoratore Asset compute
description: I processi di lavoro Asset compute sono alla base di un progetto Asset compute in quanto forniscono una funzionalità personalizzata che esegue o orchestra il lavoro eseguito su una risorsa per crearne una nuova rappresentazione.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 572
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# Sviluppa un lavoratore Asset compute

I processi di lavoro Asset compute sono alla base di un progetto Asset compute in quanto forniscono una funzionalità personalizzata che esegue o orchestra il lavoro eseguito su una risorsa per creare una nuova rappresentazione.

Il progetto di Asset compute genera automaticamente un semplice processo di lavoro che copia il binario originale della risorsa in una rappresentazione denominata, senza alcuna trasformazione. In questo tutorial verrà modificato il processo di lavoro per creare una rappresentazione più interessante, per illustrare la potenza dei processi di lavoro Asset compute.

Verrà creato un processo di lavoro Asset compute che genera una nuova rappresentazione orizzontale dell’immagine, che copre lo spazio vuoto a sinistra e a destra della rappresentazione della risorsa con una versione sfocata della risorsa. La larghezza, l&#39;altezza e la sfocatura della rappresentazione finale sono parametrizzate.

## Flusso logico di una chiamata di un lavoratore Asset compute

Asset compute worker implementa il contratto Asset compute SDK worker API, nel `renditionCallback(...)` funzione, che è concettualmente:

+ __Input:__ Parametri binari e del profilo di elaborazione originali di una risorsa AEM
+ __Output:__ Una o più rappresentazioni da aggiungere alla risorsa AEM

![Flusso logico lavoratore Asset compute](./assets/worker/logical-flow.png)

1. Il servizio di authoring AEM richiama il lavoratore Asset compute, fornendo il __(1 bis)__ binario originale (`source` parametro ), e __(1 ter)__ qualsiasi parametro definito nel profilo di elaborazione (`rendition.instructions` parametro ).
1. L’SDK di Asset compute orchestra l’esecuzione del processo di lavoro dei metadati di Asset compute personalizzato `renditionCallback(...)` , che genera una nuova rappresentazione binaria basata sul file binario originale della risorsa __(1 bis)__ ed eventuali parametri __(1 ter)__.

   + In questa esercitazione la rappresentazione viene creata &quot;in process&quot;, ovvero il processo di lavoro compone la rappresentazione, tuttavia il binario di origine può essere inviato anche ad altre API di servizi Web per la generazione della rappresentazione.

1. Il processo di lavoro Asset compute salva i dati binari della nuova rappresentazione in `rendition.path`.
1. Dati binari scritti in `rendition.path` viene trasportato tramite l’SDK di Asset compute al servizio di authoring AEM ed esposto come __(4 bis)__ una rappresentazione del testo e __(4 ter)__ persistente nel nodo di metadati della risorsa.

Il diagramma precedente illustra le preoccupazioni degli sviluppatori Asset compute e il flusso logico alla chiamata del lavoratore Asset compute. Per i curiosi, il [dettagli interni sull’esecuzione dell’Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) Tuttavia, solo i contratti API SDK Asset compute pubblici possono dipendere da.

## Anatomia di un lavoratore

Tutti i lavoratori Asset compute seguono la stessa struttura di base e lo stesso contratto di input/output.

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

![Index.js generato automaticamente](./assets/worker/autogenerated-index-js.png)

1. Verifica che il progetto Asset compute sia aperto in Codice VS
1. Accedi a `/actions/worker` cartella
1. Apri `index.js` file

Questo è il file JavaScript di lavoro che modificheremo in questa esercitazione.

## Installazione e importazione dei moduli npm di supporto

Essendo basati su Node.js, i progetti Asset compute beneficiano del solido [ecosistema del modulo npm](https://npmjs.com). Per sfruttare i moduli npm, devi prima installarli nel nostro progetto Asset compute.

In questo lavoratore, sfruttiamo [scivolare](https://www.npmjs.com/package/jimp) per creare e manipolare l’immagine della rappresentazione direttamente nel codice Node.js.

>[!WARNING]
>
>Asset compute non supporta tutti i moduli npm per la manipolazione delle risorse. I moduli npm che si basano sull’esistenza di applicazioni come ImageMagick o altre librerie dipendenti dal sistema operativo non sono supportati. È consigliabile limitare l’utilizzo dei moduli npm solo per JavaScript.

1. Apri la riga di comando nella directory principale del progetto di Asset compute (è possibile farlo in VS Code tramite __Terminale > Nuovo terminale__) ed eseguire il comando:

   ```
   $ npm install jimp
   ```

1. Importa `jimp` nel codice del processo di lavoro in modo che possa essere utilizzato tramite il `Jimp` Oggetto JavaScript.
Aggiornare il `require` direttive nella parte superiore del `index.js` per importare `Jimp` oggetto da `jimp` modulo:

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

I lavoratori Asset compute possono leggere i parametri che possono essere trasmessi tramite i Profili di elaborazione definiti nel servizio di authoring as a Cloud Service dell’AEM. I parametri vengono trasmessi al lavoratore tramite `rendition.instructions` oggetto.

Per leggerle, accedi a `rendition.instructions.<parameterName>` nel codice del lavoratore.

Qui troveremo i nella sezione del `SIZE`, `BRIGHTNESS` e `CONTRAST`, fornendo i valori predefiniti, se non ne è stato fornito alcuno tramite il Profilo di elaborazione. Tieni presente che `renditions.instructions` vengono passati come stringhe quando vengono richiamati dai profili di elaborazione as a Cloud Service dell’AEM, in modo che vengano trasformati nei tipi di dati corretti nel codice del processo di lavoro.

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

I lavoratori Asset compute possono trovarsi in situazioni che generano errori. L’SDK dell’Asset compute di Adobe fornisce [una suite di errori predefiniti](https://github.com/adobe/asset-compute-commons#asset-compute-errors) che possono essere lanciati quando si verificano tali situazioni. Se non si applica alcun tipo di errore specifico, la `GenericError` può essere utilizzato, o personalizzato `ClientErrors` possono essere definiti.

Prima di iniziare a elaborare la rappresentazione, verifica che tutti i parametri siano validi e supportati nel contesto di questo processo di lavoro:

+ Verificare i parametri delle istruzioni di rendering per `SIZE`, `CONTRAST`, e `BRIGHTNESS` sono validi. In caso contrario, genera un errore personalizzato `RenditionInstructionsError`.
   + Una `RenditionInstructionsError` classe che estende `ClientError` è definito nella parte inferiore di questo file. L’utilizzo di un errore specifico e personalizzato è utile quando [test di scrittura](../test-debug/test.md) per il lavoratore.

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

1. Crea un nuovo `renditionImage` area di lavoro in dimensioni quadrate specificate tramite `size` parametro.
1. Creare un `image` oggetto dal binario della risorsa sorgente
1. Utilizza il __Jimp__ libreria per trasformare l&#39;immagine:
   + Ritaglia l&#39;immagine originale in un quadrato centrato
   + Taglia un cerchio dal centro dell&#39;immagine al quadrato
   + Scalabilità per adattarsi alle dimensioni definite dalla `SIZE` valore parametro
   + Regola contrasto in base al `CONTRAST` valore parametro
   + Regola la luminosità in base al `BRIGHTNESS` valore parametro
1. Posizionare il `image` al centro del `renditionImage` con sfondo trasparente
1. Scrivi il composto, `renditionImage` a `rendition.path` in modo che possa essere salvato nuovamente in AEM come rappresentazione delle risorse.

Questo codice utilizza [API Jimp](https://github.com/oliver-moran/jimp#jimp) per eseguire queste trasformazioni dell&#39;immagine.

I lavoratori Asset compute devono terminare il lavoro in modo sincrono e `rendition.path` deve essere completamente riscritto su prima del `renditionCallback` completati. Ciò richiede che le chiamate di funzioni asincrone siano rese sincrone utilizzando `await` operatore. Se non conosci le funzioni asincrone JavaScript e come farle eseguire in modo sincrono, impara a [Operatore await di JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

Il lavoratore finito `index.js` dovrebbe essere simile a:

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

Ora che il codice del lavoratore è completo ed è stato precedentemente registrato e configurato in [manifest.yml](./manifest.md), può essere eseguito utilizzando lo strumento di sviluppo Asset compute locale per visualizzare i risultati.

1. Dalla directory principale del progetto Asset compute
1. Esegui `aio app run`
1. Attendi l’apertura dello strumento di sviluppo Asset compute in una nuova finestra
1. In __Seleziona un file...__ , seleziona un&#39;immagine di esempio da elaborare
   + Seleziona un file di immagine di esempio da utilizzare come binario della risorsa di origine
   + Se non ne esiste ancora nessuno, tocca il __(+)__ a sinistra, quindi carica un [immagine di esempio](../assets/samples/sample-file.jpg) e aggiorna la finestra del browser Strumenti di sviluppo
1. Aggiorna `"name": "rendition.png"` come questo processo di lavoro genera un PNG trasparente.
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
1. Il __Rappresentazioni__ sezione visualizza un’anteprima della rappresentazione generata. Tocca l’anteprima della rappresentazione per scaricarla completamente

   ![Rendering PNG predefinito](./assets/worker/default-rendition.png)

### Esegui il processo di lavoro con i parametri

I parametri, trasmessi tramite le configurazioni del profilo di elaborazione, possono essere simulati in Strumenti di sviluppo Asset compute fornendoli come coppie chiave/valore nel parametro JSON della rappresentazione.

>[!WARNING]
>
>Durante lo sviluppo locale, i valori possono essere trasmessi utilizzando vari tipi di dati, quando vengono trasmessi dall’AEM come profili di elaborazione del Cloud Service come stringhe, quindi assicurati che i tipi di dati corretti vengano analizzati se necessario.
> Ad esempio, Jimp `crop(width, height)` la funzione richiede che i relativi parametri siano `int`di. Se `parseInt(rendition.instructions.size)` non viene analizzato in un int, la chiamata a `jimp.crop(SIZE, SIZE)` ha esito negativo perché i parametri sono di tipo &quot;String&quot; incompatibile.

Il nostro codice accetta parametri per:

+ `size` definisce le dimensioni della rappresentazione (altezza e larghezza in numeri interi)
+ `contrast` definisce la regolazione del contrasto; deve essere compresa tra -1 e 1, come valori mobili
+ `brightness`  definisce la regolazione luminosa, deve essere compresa tra -1 e 1, come fluttuazioni

Questi vengono letti nel processo di lavoro `index.js` tramite:

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

1. Tocca __Esegui__ di nuovo
1. Tocca l’anteprima della rappresentazione per scaricarla e rivederla. Notate le dimensioni e come il contrasto e la luminosità sono stati modificati rispetto al rendering predefinito.

   ![Rendering PNG con parametri](./assets/worker/parameterized-rendition.png)

1. Carica altre immagini in __File di origine__ e provare a eseguire il processo di lavoro con parametri diversi.

## Worker index.js su Github

La versione finale `index.js` è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Risoluzione dei problemi

+ [La rappresentazione ha restituito un disegno parziale/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
