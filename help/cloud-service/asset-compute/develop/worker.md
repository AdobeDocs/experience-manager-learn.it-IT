---
title: Sviluppare un lavoratore di elaborazione risorse
description: I lavoratori di calcolo delle risorse sono il nucleo di un progetto di calcolo delle risorse e forniscono funzionalità personalizzate che eseguono, o orchestrano, il lavoro eseguito su una risorsa per creare una nuova rappresentazione.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 0%

---


# Sviluppare un lavoratore di elaborazione risorse

I lavoratori di calcolo delle risorse sono il nucleo di un progetto di elaborazione delle risorse e forniscono funzionalità personalizzate che eseguono, o orchestrano, il lavoro eseguito su una risorsa per creare una nuova rappresentazione.

Il progetto Asset Compute genera automaticamente un semplice lavoratore che copia il binario originale della risorsa in una rappresentazione con nome, senza alcuna trasformazione. In questa esercitazione modificheremo questo lavoratore per creare una rappresentazione più interessante, per illustrare il potere dei lavoratori di calcolo delle risorse.

Verrà creato un lavoratore Asset Compute che genera una nuova rappresentazione immagine orizzontale, che copre lo spazio vuoto a sinistra e a destra della rappresentazione della risorsa con una versione sfocata della risorsa. Larghezza, altezza e sfocatura della rappresentazione finale verranno parametrizzati.

## Flusso logico di una chiamata al lavoro di calcolo risorse

I lavoratori di calcolo delle risorse implementano il contratto API del lavoratore SDK di calcolo delle risorse, nella `renditionCallback(...)` funzione, concettualmente:

+ __Ingresso:__ Parametri binari e profilo di elaborazione originali di una risorsa AEM
+ __Output:__ Una o più rappresentazioni da aggiungere alla risorsa AEM

![Flusso logico lavoratore di calcolo risorsa](./assets/worker/logical-flow.png)

1. Il servizio AEM Author richiama il lavoratore Asset Compute, fornendo il binario originale della risorsa __(1a)__ (`source` parametro) e __(1b)__ eventuali parametri definiti nel profilo di elaborazione (`rendition.instructions` parametro).
1. L’SDK Asset Compute orchestra l’esecuzione della `renditionCallback(...)` funzione personalizzata Asset Compute, che genera una nuova rappresentazione binaria basata sul binario originale della risorsa __(1a)__ e su eventuali parametri __(1b)__.

   + In questa esercitazione la rappresentazione viene creata &quot;in processo&quot;, ovvero il lavoratore compone la rappresentazione, ma il binario di origine può essere inviato ad altre API del servizio Web per la generazione della rappresentazione.

1. Il lavoratore di elaborazione risorse salva i dati binari della nuova rappresentazione in `rendition.path`.
1. I dati binari scritti in `rendition.path` vengono trasportati tramite Asset Compute SDK a AEM Author Service ed esposti come __(4a)__ una rappresentazione di testo e __(4b)__ persistenti nel nodo di metadati della risorsa.

Il diagramma riportato sopra descrive i problemi affrontati dagli sviluppatori di Asset Compute e il flusso logico verso l&#39;invocazione di Asset Compute Worker. Per curiosi, sono disponibili i dettagli [interni dell’esecuzione](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) di Asset Compute, ma solo i contratti API SDK per il calcolo delle risorse pubblici possono essere dipendenti.

## Anatomia di un lavoratore

Tutti i lavoratori di elaborazione delle risorse seguono la stessa struttura di base e lo stesso contratto di input/output.

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

## Apertura di index.js del lavoratore

![Generazione automatica di index.js](./assets/worker/autogenerated-index-js.png)

1. Verificare che il progetto Asset Compute sia aperto nel codice VS
1. Passa alla `/actions/worker` cartella
1. Open the `index.js` file

Questo è il file JavaScript di lavoro che verrà modificato in questa esercitazione.

## Installazione e importazione di moduli npm

Essendo basati su Node.js, i progetti Asset Compute beneficiano del solido ecosistema [del modulo](https://npmjs.com)npm. Per utilizzare i moduli npm, dobbiamo prima installarli nel nostro progetto Asset Compute.

In questo lavoro, utilizziamo lo [jimp](https://www.npmjs.com/package/jimp) per creare e manipolare l&#39;immagine di rappresentazione direttamente nel codice Node.js.

>[!WARNING]
>
>Non tutti i moduli npm per la manipolazione delle risorse sono supportati da Asset Compute. moduli npm che si basano su altre applicazioni esistenti come ImageMagick o librerie dipendenti dal sistema operativo. È consigliabile limitare l&#39;uso di moduli npm solo JavaScript.

1. Aprite la riga di comando nella directory principale del progetto Asset Compute (questa operazione può essere eseguita in Codice VS tramite __Terminal > Nuovo terminale__) ed eseguite il comando:

   ```
   $ npm install jimp
   ```

1. Importare il modulo nel codice del `jimp` lavoratore in modo che possa essere utilizzato tramite l&#39;oggetto `Jimp` JavaScript.
Aggiornare le `require` direttive nella parte superiore dell&#39;operatore `index.js` per importare l&#39; `Jimp` oggetto dal `jimp` modulo:

   ```javascript
   'use strict';
   
   const { Jimp } = require('jimp');
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

## Lettura parametri

I lavoratori di elaborazione delle risorse possono leggere in parametri che possono essere passati tramite Profili di elaborazione definiti in AEM come servizio di authoring Cloud Service. I parametri vengono passati al lavoratore tramite l&#39; `rendition.instructions` oggetto.

Questi possono essere letti accedendo `rendition.instructions.<parameterName>` nel codice del lavoratore.

In questa scheda verranno letti i rendering configurabili `SIZE`, `BRIGHTNESS` e `CONTRAST`, fornendo i valori predefiniti se non ne è stata fornita alcuna tramite il profilo di elaborazione. Tenere presente che `renditions.instructions` vengono trasmesse come stringhe quando vengono richiamate da AEM come profili di elaborazione Cloud Service, in modo che vengano trasformate nei tipi di dati corretti nel codice del lavoratore.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

I lavoratori di elaborazione delle risorse possono trovarsi in situazioni che generano errori. L’SDK per il calcolo delle risorse di Adobe  fornisce [una suite di errori](https://github.com/adobe/asset-compute-commons#asset-compute-errors) predefiniti che possono essere generati in caso di situazioni simili. Se non si applica un tipo di errore specifico, `GenericError` è possibile utilizzarlo, oppure `ClientErrors` è possibile definire una specifica personalizzazione.

Prima di iniziare l&#39;elaborazione del rendering, verificate che tutti i parametri siano validi e supportati nel contesto del lavoratore:

+ Assicurarsi che i parametri delle istruzioni di rappresentazione per `SIZE`, `CONTRAST`e `BRIGHTNESS` siano validi. In caso contrario, genera un errore personalizzato `RenditionInstructionsError`.
   + Una `RenditionInstructionsError` classe personalizzata che si estende `ClientError` viene definita nella parte inferiore del file. L&#39;uso di un errore personalizzato specifico è utile quando si [scrivono test](../test-debug/test.md) per il lavoratore.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

Con i parametri letti, igienizzati e convalidati, il codice viene scritto per generare la rappresentazione. Lo pseudo codice per la generazione della rappresentazione è il seguente:

1. Create un nuovo `renditionImage` quadro in dimensioni quadrate specificate tramite il `size` parametro.
1. Creare un `image` oggetto dal binario della risorsa di origine
1. Utilizzate la libreria __Jimp__ per trasformare l&#39;immagine:
   + Ritagliare l’immagine originale in un quadrato centrato
   + Taglia un cerchio dal centro dell’immagine al quadrato
   + Adatta alle dimensioni definite dal valore del `SIZE` parametro
   + Regola il contrasto in base al valore del `CONTRAST` parametro
   + Regolare la luminosità in base al valore del `BRIGHTNESS` parametro
1. Posizionare la trasformazione `image` al centro del `renditionImage` quale è presente uno sfondo trasparente
1. Scrivi il composto, `renditionImage` in `rendition.path` modo che possa essere salvato nuovamente in AEM come rappresentazione di una risorsa.

Questo codice utilizza le API [Jimp](https://github.com/oliver-moran/jimp#jimp) per eseguire queste trasformazioni di immagini.

I lavoratori di elaborazione risorse devono finire il lavoro in modo sincrono e `rendition.path` devono essere riscritti completamente prima del `renditionCallback` completamento del lavoro. Ciò richiede che le chiamate delle funzioni asincrone siano effettuate in modo sincrono utilizzando l&#39; `await` operatore. Se non si ha familiarità con le funzioni asincrone JavaScript e con come farle eseguire in modo sincrono, è necessario conoscere l&#39;operatore [await di](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)JavaScript.

Il lavoratore finito `index.js` deve essere:

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

    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 10,000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image of the target size with a transparent background (0x0)
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness using Jimp
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
    renditionImage.write(rendition.path);
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

Ora che il codice del lavoratore è completo, ed è stato precedentemente registrato e configurato nel file [manifest.yml](./manifest.md), può essere eseguito utilizzando lo strumento di sviluppo del calcolo risorse locale per vedere i risultati.

1. Dal livello principale del progetto Asset Compute
1. Esegui `aio app run`
1. Attendere l&#39;apertura dello strumento di sviluppo di calcolo risorse in una nuova finestra
1. In __Selezionare un file...__ a discesa, selezionate un&#39;immagine di esempio da elaborare
   + Selezionate un file immagine di esempio da usare come binario della risorsa sorgente
   + Se non ne sono ancora stati trovati, toccare __(+)__ a sinistra, caricare un file immagine [di](../assets/samples/sample-file.jpg) esempio e aggiornare la finestra del browser Strumenti di sviluppo
1. Aggiorna `"name": "rendition.png"` come lavoratore per generare un file PNG trasparente.
   + Nota: questo parametro &quot;name&quot; viene utilizzato solo per lo strumento di sviluppo e non deve basarsi su.

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
1. Toccate __Esegui__ e attendete che venga generata la rappresentazione
1. La sezione __Rendering__ visualizza l&#39;anteprima della rappresentazione generata. Toccate l&#39;anteprima della rappresentazione per scaricare la rappresentazione completa

   ![Rendering PNG predefinito](./assets/worker/default-rendition.png)

### Eseguire il lavoratore con i parametri

I parametri, passati attraverso le configurazioni del profilo di elaborazione, possono essere simulati in Asset Compute Development Tools fornendo loro come coppie chiave/valore sul parametro di rappresentazione JSON.

>[!WARNING]
>
>Durante lo sviluppo locale, i valori possono essere passati utilizzando vari tipi di dati, se passati da AEM come profili di elaborazione Cloud Service come stringhe, quindi assicurarsi che i tipi di dati corretti siano analizzati se necessario.
> Ad esempio, la `crop(width, height)` funzione di Jimp richiede che i relativi parametri siano `int`di tipo &quot;s&quot;. Se non `parseInt(rendition.instructions.size)` viene analizzato un valore int, la chiamata a `jimp.crop(SIZE, SIZE)` non riesce perché i parametri saranno di tipo &#39;String&#39; incompatibile.

Il nostro codice accetta parametri per:

+ `size` definisce le dimensioni della rappresentazione (altezza e larghezza come numeri interi)
+ `contrast` definisce la regolazione del contrasto, deve essere compresa tra -1 e 1, come galleggianti
+ `brightness`  definisce la regolazione luminosa, deve essere compresa tra -1 e 1, come galleggianti

Queste sono lette nel lavoratore `index.js` tramite:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Aggiornate i parametri di rappresentazione per personalizzare le dimensioni, il contrasto e la luminosità.

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
1. Toccate l&#39;anteprima della rappresentazione per scaricare e rivedere la rappresentazione generata. Osservate le dimensioni e come sono stati modificati contrasto e luminosità rispetto alla rappresentazione predefinita.

   ![Rappresentazioni PNG con parametri](./assets/worker/parameterized-rendition.png)

1. Caricate altre immagini nel menu a discesa del file __di__ origine e provate a eseguire il processo di lavoro con parametri diversi.

## Worker index.js su Github

La finale `index.js` è disponibile su Github al seguente indirizzo:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Risoluzione dei problemi

+ [Rendering restituito parzialmente disegnato/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
