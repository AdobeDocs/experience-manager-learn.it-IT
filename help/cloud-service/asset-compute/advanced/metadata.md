---
title: Sviluppare un processo di lavoro per metadati Asset Compute
description: Scopri come creare un processo di lavoro per metadati Asset Compute che derivi colori più comunemente utilizzati in una risorsa immagine e scrive i nomi dei colori nei metadati della risorsa in AEM.
feature: Microservizi Asset Compute
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrazioni, Sviluppo
role: Developer (Sviluppatore)
level: Intermedio, esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1442'
ht-degree: 1%

---


# Sviluppare un processo di lavoro per metadati Asset Compute

I processi di lavoro di Asset Compute personalizzati possono produrre dati XMP (XML) che vengono ritrasmessi ad AEM e memorizzati come metadati su una risorsa.

I casi d’uso comuni includono:

+ Integrazioni con sistemi di terze parti, ad esempio un sistema PIM (Product Information Management System), in cui è necessario recuperare e memorizzare metadati aggiuntivi sulla risorsa
+ Integrazioni con i servizi Adobe, ad esempio Content and Commerce AI per migliorare i metadati delle risorse con attributi di apprendimento automatico aggiuntivi
+ Derivazione dei metadati della risorsa dal suo binario e memorizzazione come metadati della risorsa in AEM as a Cloud Service

## Cosa farai

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

In questa esercitazione creeremo un processo di lavoro per metadati Asset Compute che deriverà i colori più comunemente utilizzati in una risorsa immagine e riscriverà i nomi dei colori nei metadati della risorsa in AEM. Sebbene il processo di lavoro sia di base, questa esercitazione lo utilizza per esplorare come utilizzare i processi di lavoro Asset Compute per scrivere i metadati sulle risorse in AEM as a Cloud Service.

## Flusso logico della chiamata di un processo di lavoro per metadati di Asset Compute

La chiamata dei processi di lavoro per metadati di Asset Compute è quasi identica a quella di [processi di rendering binari che generano processi di lavoro](../develop/worker.md), con la differenza principale che il tipo di ritorno è un rendering XMP (XML) i cui valori sono scritti anche nei metadati della risorsa.

I processi di lavoro di Asset Compute implementano il contratto API di lavoro Asset Compute SDK, nella funzione `renditionCallback(...)` , che è concettualmente:

+ __Input:__ Parametri binari e del profilo di elaborazione originali di una risorsa AEM
+ __Output:__ un rendering XMP (XML) persisteva nella risorsa AEM come rendering e nei metadati della risorsa

![Flusso logico del processo di lavoro dei metadati di Asset Compute](./assets/metadata/logical-flow.png)

1. Il servizio AEM Author richiama il processo di lavoro per metadati Asset Compute, fornendo il __(1a)__ binario originale della risorsa e __(1b)__ eventuali parametri definiti nel profilo di elaborazione.
1. L’SDK di Asset Compute orchestra l’esecuzione della funzione `renditionCallback(...)` del processo di lavoro per metadati Asset Compute personalizzato, derivando un rendering XMP (XML), in base al binario della risorsa __(1a)__ e a eventuali parametri del profilo di elaborazione __(1b)__.
1. Il processo di lavoro Asset Compute salva la rappresentazione XMP (XML) in `rendition.path`.
1. I dati XMP (XML) scritti in `rendition.path` vengono trasportati tramite Asset Compute SDK ad AEM Author Service e li espongono come __(4a)__ una rappresentazione testuale e __(4b)__ persistenti nel nodo di metadati della risorsa.

## Configura il file manifest.yml{#manifest}

Tutti i processi di lavoro di Asset Compute devono essere registrati in [manifest.yml](../develop/manifest.md).

Apri il `manifest.yml` del progetto e aggiungi una voce di lavoro che configuri il nuovo processo di lavoro, in questo caso `metadata-colors`.

_Ricorda  `.yml` che gli spazi bianchi sono sensibili._

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` fa riferimento all’implementazione del processo di lavoro creata nel  [passaggio](#metadata-worker) successivo. Denomina i processi di lavoro semanticamente (ad esempio, il `actions/worker/index.js` potrebbe essere stato meglio denominato `actions/rendition-circle/index.js`), come mostrato nell&#39; [URL del processo di lavoro](#deploy) e inoltre determina il [nome della cartella di test del processo di lavoro](#test).

Le `limits` e `require-adobe-auth` sono configurate in modo discreto per lavoratore. In questo processo di lavoro, `512 MB` di memoria viene allocata mentre il codice controlla (potenzialmente) grandi dati di immagine binari. Gli altri `limits` vengono rimossi per utilizzare i valori predefiniti.

## Sviluppare un processo di lavoro metadati{#metadata-worker}

Crea un nuovo file JavaScript di lavoro metadati nel progetto Asset Compute al percorso [definito manifest.yml per il nuovo processo di lavoro](#manifest), all&#39;indirizzo `/actions/metadata-colors/index.js`

### Installare i moduli npm

Installa i moduli npm aggiuntivi ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-color](https://www.npmjs.com/package/get-image-colors) e [color-namer](https://www.npmjs.com/package/color-namer)) che verranno utilizzati in questo processo di lavoro Asset Compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Codice lavoratore metadati

Questo processo di lavoro ha un aspetto molto simile al processo di lavoro [generazione rendering](../develop/worker.md), la differenza principale consiste nel scrivere i dati XMP (XML) nel `rendition.path` per essere salvati di nuovo in AEM.


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces will be automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## Esegui il processo di lavoro metadati localmente{#development-tool}

Completato il codice del processo di lavoro, è possibile eseguirlo utilizzando lo strumento di sviluppo Asset Compute locale.

Poiché il nostro progetto Asset Compute contiene due processi di lavoro (il precedente [rendering a cerchio](../develop/worker.md) e questo processo di lavoro `metadata-colors`), la definizione del profilo [Asset Compute Development Tool](../develop/development-tool.md) elenca i profili di esecuzione per entrambi i processi di lavoro. La seconda definizione del profilo punta al nuovo processo di lavoro `metadata-colors`.

![Rendering di metadati XML](./assets/metadata/metadata-rendition.png)

1. Dalla directory principale del progetto Asset Compute
1. Esegui `aio app run` per avviare Asset Compute Development Tool
1. In __Seleziona un file...A discesa__ , scegli un’ [immagine di esempio](../assets/samples/sample-file.jpg) da elaborare
1. Nella seconda configurazione di definizione del profilo, che punta al processo di lavoro `metadata-colors`, aggiorna `"name": "rendition.xml"` poiché questo processo di lavoro genera un rendering XMP (XML). Facoltativamente, aggiungi un parametro `colorsFamily` (valori supportati `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```
1. Tocca __Esegui__ e attendi che il rendering XML venga generato
   + Poiché entrambi i processi di lavoro sono elencati nella definizione del profilo, verranno generate entrambe le rappresentazioni. Facoltativamente, è possibile eliminare la definizione del profilo principale che punta al processo di lavoro [rendering del cerchio](../develop/worker.md), per evitare di eseguirlo dallo strumento di sviluppo.
1. La sezione __Rendering__ visualizza in anteprima il rendering generato. Toccare il `rendition.xml` per scaricarlo e aprirlo nel codice VS (o nell&#39;editor XML/text preferito) per la revisione.

## Test del processo di lavoro{#test}

I processi di lavoro dei metadati possono essere testati utilizzando lo [stesso framework di test di Asset Compute come rappresentazioni binarie](../test-debug/test.md). L’unica differenza è che il file `rendition.xxx` nel caso di test deve essere il rendering XMP (XML) previsto.

1. Crea la seguente struttura nel progetto Asset Compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilizza il [file di esempio](../assets/samples/sample-file.jpg) come case di prova `file.jpg`.
3. Aggiungi il seguente JSON al `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Tieni presente che per istruire la suite di test è necessario `.xml` generare un rendering basato su testo.`"fmt": "xml"`

4. Immetti il codice XML previsto nel file `rendition.xml` . Ciò può essere ottenuto:
   + Esecuzione del file di input di prova tramite lo strumento di sviluppo e salvataggio del rendering XML (convalidato).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Esegui `aio app test` dalla directory principale del progetto Asset Compute per eseguire tutte le suite di test.

### Distribuire il processo di lavoro in Adobe I/O Runtime{#deploy}

Per richiamare questo nuovo processo di lavoro metadati da AEM Assets, è necessario implementarlo in Adobe I/O Runtime tramite il comando :

```
$ aio app deploy
```

![distribuzione app aio](./assets/metadata/aio-app-deploy.png)

In questo modo verranno distribuiti tutti i processi di lavoro del progetto. Consulta le [istruzioni di distribuzione senza limiti](../deploy/runtime.md) per informazioni su come distribuire nelle aree di lavoro di Stage e Produzione.

### Integrazione con i profili di elaborazione AEM{#processing-profile}

Richiama il lavoratore da AEM creando un nuovo servizio Profilo di elaborazione personalizzato esistente che richiama il lavoratore distribuito.

![Profilo di elaborazione](./assets/metadata/processing-profile.png)

1. Accedi al servizio Author di AEM as a Cloud Service come __Amministratore AEM__
1. Passa a __Strumenti > Risorse > Profili di elaborazione__
1. ____ Creare un profilo di elaborazione nuovo o  ____ modificabile ed esistente
1. Tocca la scheda __Personalizzato__ e tocca __Aggiungi nuovo__
1. Definire il nuovo servizio
   + __Crea rappresentazione__ metadati: Passa a attivo
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Si tratta dell&#39;URL del processo di lavoro ottenuto durante la [distribuzione](#deploy) o utilizzando il comando `aio app get-url`. Assicurati che i punti URL siano nell’area di lavoro corretta in base all’ambiente AEM as a Cloud Service.
   + __Parametri del servizio__
      + Tocca __Aggiungi parametro__
         + Chiave: `colorFamily`
         + Valore: `pantone`
            + Valori supportati: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipi mime__
      + __Include:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/svg`
         + Questi sono gli unici tipi MIME supportati dai moduli npm di terze parti utilizzati per ricavare i colori.
      + __Escludi:__ `Leave blank`
1. Tocca __Salva__ in alto a destra
1. Applica il profilo di elaborazione a una cartella di AEM Assets se non lo fai già

### Aggiornare lo schema metadati{#metadata-schema}

Per rivedere i metadati dei colori, mappare due nuovi campi dello schema metadati dell&#39;immagine alle nuove proprietà dei dati dei metadati che il processo di lavoro compila.

![Schema metadati](./assets/metadata/metadata-schema.png)

1. Nel servizio AEM Author, passa a __Strumenti > Risorse > Schemi di metadati__
1. Passa a __default__ e seleziona e modifica __image__ e aggiungi campi modulo di sola lettura per esporre i metadati colore generati
1. Aggiungi un __testo a riga singola__
   + __Etichetta campo__: `Colors Family`
   + __Mappa su proprietà__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regole > Campo > Disattiva modifica__: Selezionato
1. Aggiungi un __testo con più valori__
   + __Etichetta campo__: `Colors`
   + __Mappa su proprietà__: `./jcr:content/metadata/wknd:colors`
1. Tocca __Salva__ in alto a destra

## Elaborazione delle risorse

![Dettagli risorsa](./assets/metadata/asset-details.png)

1. Nel servizio AEM Author, passa a __Risorse > File__
1. Passa alla cartella o alla sottocartella a cui viene applicato il profilo di elaborazione
1. Carica una nuova immagine (JPEG, PNG, GIF o SVG) nella cartella oppure rielabora le immagini esistenti utilizzando il [Profilo di elaborazione aggiornato](#processing-profile)
1. Al termine dell’elaborazione, seleziona la risorsa e tocca __proprietà__ nella barra delle azioni superiore per visualizzarne i metadati
1. Esamina i `Colors Family` e `Colors` [campi di metadati](#metadata-schema) per i metadati riscritti dal processo di lavoro per metadati di Asset Compute personalizzato.

Con i metadati a colori scritti nei metadati della risorsa, sulla risorsa `[dam:Asset]/jcr:content/metadata` , questi metadati sono indicizzati e consentono di aumentare la possibilità di scoprire le risorse utilizzando questi termini tramite la ricerca, e possono anche essere riscritti nel file binario della risorsa se su di esso viene richiamato il flusso di lavoro __DAM Metadata Writeback__ .

### Rendering dei metadati in AEM Assets

![File di rendering dei metadati di AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

Anche il file XMP effettivo generato dal processo di lavoro metadati Asset Compute viene memorizzato come rendering discreto sulla risorsa. Questo file generalmente non viene utilizzato, ma i valori applicati al nodo di metadati della risorsa vengono utilizzati, ma l’output XML non elaborato del processo di lavoro è disponibile in AEM.

## codice lavoratore a colori metadati su Github

L’ ultima versione `metadata-colors/index.js` è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

La suite di test finale `test/asset-compute/metadata-colors` è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-color](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
