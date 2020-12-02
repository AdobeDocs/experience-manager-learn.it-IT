---
title: 'Sviluppare un Asset compute '
description: ' Asset compute sono il nucleo di un progetto di Asset compute  per fornire funzionalità personalizzate che eseguono, o orchestrano, il lavoro svolto su una risorsa per creare una nuova rappresentazione.'
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


# Sviluppare un Asset compute 

 Asset compute sono il nucleo di un progetto di Asset compute  per fornire funzionalità personalizzate che eseguono, o orchestrano, il lavoro svolto su una risorsa per creare una nuova rappresentazione.

Il progetto di Asset compute  genera automaticamente un semplice lavoratore che copia il binario originale della risorsa in una rappresentazione con nome, senza alcuna trasformazione. In questa esercitazione modificheremo questo lavoratore per creare una rappresentazione più interessante, per illustrare il potere dei lavoratori  Asset compute.

Verrà creato un Asset compute  che genera una nuova rappresentazione immagine orizzontale, che copre lo spazio vuoto a sinistra e a destra della rappresentazione della risorsa con una versione sfocata della risorsa. Larghezza, altezza e sfocatura della rappresentazione finale verranno parametrizzati.

## Flusso logico di una chiamata di lavoro di un Asset compute 

 Asset compute implementano il contratto di  Asset compute SDK Worker API, nella funzione `renditionCallback(...)`, concettualmente:

+ __Input:__ Parametri binari e profilo di elaborazione originali di una risorsa AEM
+ __Output:__ Uno o più rendering da aggiungere alla risorsa AEM

![Flusso logico lavoratore Asset compute ](./assets/worker/logical-flow.png)

1. Il servizio AEM Author richiama il lavoratore del Asset compute , fornendo i __(1a)__ binari originali (`source` parametro) e __(1b)__ eventuali parametri definiti nel profilo di elaborazione (`rendition.instructions` parametro).
1. L&#39;SDK del Asset compute  orchestra l&#39;esecuzione della funzione `renditionCallback(...)` del lavoratore di metadati del Asset compute  personalizzato, generando una nuova rappresentazione binaria, basata sul binario originale della risorsa __(1a)__ ed eventuali parametri __(1b)__.

   + In questa esercitazione la rappresentazione viene creata &quot;in processo&quot;, ovvero il lavoratore compone la rappresentazione, ma il binario di origine può essere inviato ad altre API del servizio Web per la generazione della rappresentazione.

1. Il Asset compute  salva i dati binari della nuova rappresentazione in `rendition.path`.
1. I dati binari scritti in `rendition.path` vengono trasportati tramite l&#39;SDK del Asset compute  in AEM Author Service ed esposti come __(4a)__ una rappresentazione di testo e __(4b)__ persistono nel nodo di metadati della risorsa.

Il diagramma riportato sopra descrive i problemi affrontati dallo sviluppatore  Asset compute e il flusso logico verso  chiamata del lavoratore Asset compute. Per curiosità, sono disponibili i [dettagli interni dell&#39;esecuzione  Asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html), ma solo i contratti dell&#39;API SDK del Asset compute  pubblico possono essere dipendenti.

## Anatomia di un lavoratore

Tutti  lavoratori Asset compute seguono la stessa struttura di base e lo stesso contratto input/output.

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

1. Verificare che il progetto di Asset compute  sia aperto nel codice VS
1. Passa alla cartella `/actions/worker`
1. Aprire il file `index.js`

Questo è il file JavaScript di lavoro che verrà modificato in questa esercitazione.

## Installazione e importazione di moduli npm

Essendo basati su Node.js,  progetti di Asset compute beneficiano del robusto ecosistema del modulo npm [](https://npmjs.com). Per utilizzare i moduli npm, dobbiamo prima installarli nel nostro progetto di Asset compute .

In questo processo di lavoro, sfruttiamo [jimp](https://www.npmjs.com/package/jimp) per creare e manipolare l&#39;immagine di rappresentazione direttamente nel codice Node.js.

>[!WARNING]
>
>Non tutti i moduli npm per la manipolazione delle risorse sono supportati dal Asset compute . moduli npm che si basano su altre applicazioni esistenti come ImageMagick o librerie dipendenti dal sistema operativo. È consigliabile limitare l&#39;uso di moduli npm solo JavaScript.

1. Aprite la riga di comando nella radice del progetto del Asset compute  (questo può essere fatto in Codice VS tramite __Terminal > Nuovo terminale__) ed eseguite il comando:

   ```
   $ npm install jimp
   ```

1. Importare il modulo `jimp` nel codice del lavoro in modo che possa essere utilizzato tramite l&#39;oggetto JavaScript `Jimp`.
Aggiornare le direttive `require` nella parte superiore dell&#39;oggetto `index.js` del lavoratore per importare l&#39;oggetto `Jimp` dal modulo `jimp`:

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

 Asset compute di lavoro possono leggere i parametri che possono essere passati tramite Profili di elaborazione definiti in AEM come un servizio Cloud Service Autore. I parametri vengono passati al processo di lavoro tramite l&#39;oggetto `rendition.instructions`.

Per leggerli, accedere a `rendition.instructions.<parameterName>` nel codice del lavoratore.

Qui si trovano i valori predefiniti `SIZE`, `BRIGHTNESS` e `CONTRAST` della rappresentazione configurabile, che forniscono i valori predefiniti se non ne sono stati forniti tramite il profilo di elaborazione. Tenere presente che `renditions.instructions` vengono trasmesse come stringhe quando vengono richiamate da AEM come profili di elaborazione Cloud Service, in modo che vengano trasformate nei tipi di dati corretti nel codice del lavoratore.

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

 lavoratori Asset compute possono trovarsi in situazioni che generano errori. Il  Adobe  Asset compute SDK fornisce [una suite di errori predefiniti](https://github.com/adobe/asset-compute-commons#asset-compute-errors) che possono essere generati quando tali situazioni vengono rilevate. Se non si applica un tipo di errore specifico, è possibile utilizzare la `GenericError` oppure definire `ClientErrors` personalizzato specifico.

Prima di iniziare l&#39;elaborazione del rendering, verificate che tutti i parametri siano validi e supportati nel contesto del lavoratore:

+ Assicurarsi che i parametri delle istruzioni di rappresentazione per `SIZE`, `CONTRAST` e `BRIGHTNESS` siano validi. In caso contrario, genera un errore personalizzato `RenditionInstructionsError`.
   + Una classe `RenditionInstructionsError` personalizzata che estende `ClientError` è definita nella parte inferiore del file. L&#39;uso di un errore personalizzato specifico è utile quando [test di scrittura](../test-debug/test.md) per il lavoratore.

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

1. Create un nuovo quadro `renditionImage` nelle dimensioni quadrate specificate mediante il parametro `size`.
1. Creare un oggetto `image` dal binario della risorsa di origine
1. Utilizzate la libreria __Jimp__ per trasformare l&#39;immagine:
   + Ritagliare l’immagine originale in un quadrato centrato
   + Taglia un cerchio dal centro dell’immagine al quadrato
   + Adatta alle dimensioni definite dal valore del parametro `SIZE`
   + Regola il contrasto in base al valore del parametro `CONTRAST`
   + Regolare la luminosità in base al valore del parametro `BRIGHTNESS`
1. Posizionare la `image` trasformata al centro della `renditionImage` con uno sfondo trasparente
1. Scrivi il composto, `renditionImage` in `rendition.path` in modo che possa essere salvato nuovamente in AEM come rappresentazione di una risorsa.

Questo codice utilizza le [API Jimp](https://github.com/oliver-moran/jimp#jimp) per eseguire queste trasformazioni di immagini.

 lavoratori del Asset compute devono terminare il lavoro in modo sincrono e il `rendition.path` deve essere riscritto completamente prima del completamento dell&#39;attività `renditionCallback` del lavoratore. Ciò richiede che le chiamate delle funzioni asincrone siano sincronizzate utilizzando l&#39;operatore `await`. Se non si ha familiarità con le funzioni asincrone JavaScript e con come farle eseguire in modo sincrono, è necessario conoscere l&#39;operatore di attesa di [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

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

Ora che il codice del lavoratore è completo, ed è stato precedentemente registrato e configurato in [manifest.yml](./manifest.md), può essere eseguito utilizzando lo strumento di sviluppo  Asset compute locale per vedere i risultati.

1. Dal livello principale del progetto del Asset compute 
1. Esegui `aio app run`
1. Attendi che  Strumento di sviluppo Asset compute si apra in una nuova finestra
1. In __Selezionare un file...__ a discesa, selezionare un&#39;immagine di esempio da elaborare
   + Selezionate un file immagine di esempio da usare come binario della risorsa sorgente
   + Se non ne è ancora presente, toccare il __(+)__ a sinistra, caricare un file di immagine di esempio [](../assets/samples/sample-file.jpg) e aggiornare la finestra del browser Strumenti di sviluppo
1. Aggiornate `"name": "rendition.png"` come lavoratore per generare un file PNG trasparente.
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

I parametri, passati tramite configurazioni di profilo di elaborazione, possono essere simulati in  Asset compute Development Tools fornendo come coppie chiave/valore sul parametro di rappresentazione JSON.

>[!WARNING]
>
>Durante lo sviluppo locale, i valori possono essere passati utilizzando vari tipi di dati, se passati da AEM come profili di elaborazione Cloud Service come stringhe, quindi assicurarsi che i tipi di dati corretti siano analizzati se necessario.
> Ad esempio, la funzione `crop(width, height)` di Jimp richiede che i parametri siano `int`. Se `parseInt(rendition.instructions.size)` non viene analizzato in base a un valore int, la chiamata a `jimp.crop(SIZE, SIZE)` avrà esito negativo in quanto i parametri saranno di tipo &#39;String&#39; incompatibile.

Il nostro codice accetta parametri per:

+ `size` definisce le dimensioni della rappresentazione (altezza e larghezza come numeri interi)
+ `contrast` definisce la regolazione del contrasto, deve essere compresa tra -1 e 1, come galleggianti
+ `brightness`  definisce la regolazione luminosa, deve essere compresa tra -1 e 1, come galleggianti

Queste vengono lette nel lavoratore `index.js` tramite:

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

1. Toccare di nuovo __Esegui__
1. Toccate l&#39;anteprima della rappresentazione per scaricare e rivedere la rappresentazione generata. Osservate le dimensioni e come sono stati modificati contrasto e luminosità rispetto alla rappresentazione predefinita.

   ![Rappresentazioni PNG con parametri](./assets/worker/parameterized-rendition.png)

1. Caricate altre immagini nel menu a discesa __File di origine__, quindi provate a eseguire il processo di lavoro rispetto a tali immagini con parametri diversi!

## Worker index.js su Github

L&#39;ultima `index.js` è disponibile su Github all&#39;indirizzo:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Risoluzione dei problemi

+ [Rendering restituito parzialmente disegnato/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
