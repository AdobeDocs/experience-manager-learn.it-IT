---
title: Sviluppare un processo di lavoro Asset compute
description: I processi di lavoro di Asset compute costituiscono il nucleo di un progetto di Asset compute, in quanto forniscono funzionalità personalizzate che eseguono o orchestrano il lavoro eseguito su una risorsa per creare un nuovo rendering.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Sviluppare un processo di lavoro Asset compute

I processi di lavoro di Asset compute costituiscono il nucleo di un progetto di Asset compute in quanto forniscono funzionalità personalizzate che eseguono o orchestrano il lavoro eseguito su una risorsa per creare un nuovo rendering.

Il progetto di Asset compute genera automaticamente un processo di lavoro semplice che copia il binario originale della risorsa in un rendering denominato, senza alcuna trasformazione. In questa esercitazione modificheremo questo processo di lavoro per creare una rappresentazione più interessante, per illustrare il potere dei lavoratori Asset compute.

Verrà creato un processo di lavoro di Asset compute che genera un nuovo rendering orizzontale dell’immagine, che copre lo spazio vuoto a sinistra e a destra del rendering della risorsa con una versione sfocata della risorsa. La larghezza, l&#39;altezza e la sfocatura del rendering finale sono parametrizzate.

## Flusso logico della chiamata di un processo di lavoro di Asset compute

I dipendenti di Asset compute implementano il contratto Asset compute SDK worker API nel `renditionCallback(...)` funzione concettualmente:

+ __Ingresso:__ Parametri binari e del profilo di elaborazione originali di una risorsa AEM
+ __Uscita:__ Uno o più rendering da aggiungere alla risorsa AEM

![Flusso logico del processo di lavoro di Asset compute](./assets/worker/logical-flow.png)

1. Il servizio di authoring di AEM richiama il processo di lavoro Asset compute, fornendo l’ __(1 bis)__ binario originale (`source` ), e __1 ter)__ eventuali parametri definiti nel profilo di elaborazione (`rendition.instructions` ).
1. L&#39;SDK di Asset compute orchestra l&#39;esecuzione del processo di lavoro dei metadati di Asset compute personalizzati `renditionCallback(...)` generare una nuova rappresentazione binaria basata sul binario originale della risorsa __(1 bis)__ e qualsiasi parametro __1 ter)__.

   + In questa esercitazione il rendering viene creato &quot;in process&quot;, ovvero il processo di lavoro compone il rendering, tuttavia il binario di origine può essere inviato ad altre API di servizi Web per la generazione del rendering.

1. Il processo di lavoro Asset compute salva i dati binari del nuovo rendering in `rendition.path`.
1. Dati binari scritti in `rendition.path` viene trasportato tramite Asset compute SDK a AEM Author Service ed esposto come __4 bis)__ una rappresentazione testuale e __4 ter)__ persistita nel nodo di metadati della risorsa.

Il diagramma riportato sopra descrive le preoccupazioni rivolte agli sviluppatori di Asset compute e il flusso logico verso la chiamata del lavoratore Asset compute. Per i curiosi, il [dettagli interni sull&#39;esecuzione degli Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) sono disponibili, ma solo i contratti API SDK per Asset compute pubblici possono essere dipendenti.

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

## Apertura di index.js del processo di lavoro

![Index.js generato automaticamente](./assets/worker/autogenerated-index-js.png)

1. Assicurati che il progetto di Asset compute sia aperto nel codice VS
1. Passa a `/actions/worker` cartella
1. Apri `index.js` file

Questo è il file JavaScript di lavoro che modificheremo in questa esercitazione.

## Installare e importare i moduli di supporto npm

Essendo basati su Node.js, i progetti di Asset compute beneficiano del [ecosistema del modulo npm](https://npmjs.com). Per sfruttare i moduli npm, dobbiamo prima installarli nel nostro progetto Asset compute.

In questo lavoratore, sfruttiamo il [scintillare](https://www.npmjs.com/package/jimp) per creare e manipolare l&#39;immagine di rendering direttamente nel codice Node.js.

>[!WARNING]
>
>Non tutti i moduli npm per la manipolazione delle risorse sono supportati da Asset compute. I moduli npm che si basano sull&#39;esistenza di applicazioni come ImageMagick o altre librerie dipendenti dal sistema operativo non sono supportati. È consigliabile limitare l’utilizzo di moduli npm solo JavaScript.

1. Apri la riga di comando nella directory principale del progetto di Asset compute (questa operazione può essere eseguita in Codice VS tramite __Terminale > Nuovo terminale__) ed esegui il comando:

   ```
   $ npm install jimp
   ```

1. Importa `jimp` nel codice del lavoratore in modo che possa essere utilizzato tramite il `Jimp` Oggetto JavaScript.
Aggiorna `require` direttive in cima alla `index.js` per importare `Jimp` dell&#39;oggetto `jimp` modulo:

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

## Parametri di lettura

I processi di lavoro di Asset compute possono leggere i parametri che possono essere trasmessi tramite Profili di elaborazione definiti AEM servizio Author as a Cloud Service. I parametri vengono trasmessi al processo di lavoro tramite il `rendition.instructions` oggetto.

Queste possono essere lette accedendo a `rendition.instructions.<parameterName>` nel codice lavoratore.

Qui si legge nel rendering configurabile `SIZE`, `BRIGHTNESS` e `CONTRAST`, fornendo valori predefiniti se non ne è stato fornito alcuno tramite il profilo di elaborazione. Tieni presente che `renditions.instructions` vengono passate come stringhe quando vengono richiamate da AEM profili di elaborazione as a Cloud Service, in modo che vengano trasformate nei tipi di dati corretti nel codice del processo di lavoro.

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

## Errori di generazione{#errors}

I lavoratori Asset compute possono trovarsi in situazioni che generano errori. L’SDK per Asset compute di Adobe fornisce [una suite di errori predefiniti](https://github.com/adobe/asset-compute-commons#asset-compute-errors) che possono essere lanciati quando si verificano situazioni del genere. Se non si applica un tipo di errore specifico, la `GenericError` può essere utilizzato o personalizzato `ClientErrors` può essere definito.

Prima di iniziare a elaborare il rendering, controlla che tutti i parametri siano validi e supportati nel contesto di questo processo di lavoro:

+ Assicurati i parametri delle istruzioni di rendering per `SIZE`, `CONTRAST`e `BRIGHTNESS` sono validi. In caso contrario, genera un errore personalizzato `RenditionInstructionsError`.
   + Personalizzazione `RenditionInstructionsError` classe che si estende `ClientError` è definito in fondo a questo file. L&#39;utilizzo di un errore personalizzato specifico è utile quando [prove di scrittura](../test-debug/test.md) per il lavoratore.

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

## Creazione del rendering

Con i parametri letti, igienizzati e convalidati, il codice viene scritto per generare il rendering. Lo pseudo codice per la generazione del rendering è il seguente:

1. Crea un nuovo `renditionImage` tela in dimensioni quadrate specificate tramite `size` parametro .
1. Crea un `image` oggetto dal binario della risorsa di origine
1. Utilizza la __Grembiule__ libreria per trasformare l&#39;immagine:
   + Ritaglia l&#39;immagine originale in un quadrato centrato
   + Taglia un cerchio dal centro dell&#39;immagine &quot;al quadrato&quot;
   + Adatta alle dimensioni definite dal `SIZE` valore parametro
   + Regola il contrasto in base al `CONTRAST` valore parametro
   + Regolare la luminosità in base al `BRIGHTNESS` valore parametro
1. Posiziona la trasformazione `image` nel centro del `renditionImage` che ha uno sfondo trasparente
1. Scrivi il composto, `renditionImage` a `rendition.path` in modo che possa essere salvato nuovamente in AEM come rendering di una risorsa.

Questo codice utilizza il [API di Jimp](https://github.com/oliver-moran/jimp#jimp) per eseguire queste trasformazioni immagine.

I lavoratori Asset compute devono finire il lavoro in modo sincrono e il `rendition.path` deve essere riscritto completamente prima del `renditionCallback` completo. Ciò richiede che le chiamate alle funzioni asincrone siano effettuate in modo sincrono utilizzando il `await` operatore. Se non hai familiarità con le funzioni asincrone di JavaScript e con come eseguirle in modo sincrono, acquisisci familiarità con [Operatore di attesa di JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

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

## Esecuzione del lavoratore

Ora che il codice del lavoratore è completo ed è stato precedentemente registrato e configurato nel [manifest.yml](./manifest.md), può essere eseguito utilizzando lo strumento di sviluppo Asset compute locale per visualizzare i risultati.

1. Dalla directory principale del progetto Asset compute
1. Esegui `aio app run`
1. Attendi l&#39;apertura dello strumento di sviluppo di Asset compute in una nuova finestra
1. In __Seleziona un file...__ a discesa, seleziona un’immagine di esempio da elaborare
   + Seleziona un file immagine di esempio da utilizzare come binario della risorsa sorgente
   + Se non è ancora presente, tocca il pulsante __(+)__ a sinistra e carica un [immagine campione](../assets/samples/sample-file.jpg) e aggiornare la finestra del browser Strumenti di sviluppo
1. Aggiorna `"name": "rendition.png"` come questo lavoratore per generare un PNG trasparente.
   + Questo parametro &quot;name&quot; viene utilizzato solo per lo strumento di sviluppo e non deve fare affidamento su di esso.

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

1. Tocca __Esegui__ e attendere che il rendering venga generato
1. La __Rendering__ visualizza in anteprima il rendering generato. Tocca l’anteprima del rendering per scaricare il rendering completo

   ![Rendering PNG predefinito](./assets/worker/default-rendition.png)

### Esegui il processo di lavoro con i parametri

I parametri passati tramite le configurazioni del profilo di elaborazione possono essere simulati in Asset compute Development Tools fornendo loro come coppie chiave/valore sul parametro di rendering JSON.

>[!WARNING]
>
>Durante lo sviluppo locale, i valori possono essere passati utilizzando vari tipi di dati, quando passati da AEM come profili di elaborazione del Cloud Service come stringhe, quindi assicurati che i tipi di dati corretti siano analizzati se necessario.
> Ad esempio, Jimp `crop(width, height)` la funzione richiede che i relativi parametri siano `int`S. Se `parseInt(rendition.instructions.size)` non viene analizzato in un valore int, quindi la chiamata a `jimp.crop(SIZE, SIZE)` non riesce perché i parametri sono di tipo &quot;String&quot; incompatibile.

Il nostro codice accetta parametri per:

+ `size` definisce le dimensioni del rendering (altezza e larghezza come numeri interi)
+ `contrast` definisce la regolazione del contrasto, deve essere compresa tra -1 e 1, come floats
+ `brightness`  definisce la regolazione luminosa, deve essere compresa tra -1 e 1, come galleggianti

Queste vengono lette nel lavoratore `index.js` tramite:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Aggiorna i parametri di rendering per personalizzare le dimensioni, il contrasto e la luminosità.

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
1. Tocca l’anteprima del rendering per scaricare e rivedere il rendering generato. Nota le dimensioni e come sono stati modificati il contrasto e la luminosità rispetto al rendering predefinito.

   ![Rendering PNG con parametri](./assets/worker/parameterized-rendition.png)

1. Carica altre immagini in __File di origine__ a discesa, e prova a eseguire il processo di lavoro rispetto a loro con parametri diversi!

## Index.js di Worker su Github

Il finale `index.js` è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Risoluzione dei problemi

+ [Rendering restituito parzialmente disegnato/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
