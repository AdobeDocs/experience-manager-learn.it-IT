---
title: Sviluppa un Asset compute di processo di lavoro per metadati
description: Scopri come creare un processo di lavoro dei metadati di Asset compute che deriva i colori più comunemente utilizzati in una risorsa di immagine e riscrive i nomi dei colori nei metadati della risorsa in AEM.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 461
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# Sviluppa un Asset compute di processo di lavoro per metadati

Gli Asset compute personalizzati possono produrre dati XMP (XML) che vengono rimandati all&#39;AEM e memorizzati come metadati su una risorsa.

I casi d’uso comuni includono:

+ Integrazioni con sistemi di terze parti, ad esempio un sistema PIM (Product Information Management system), in cui è necessario recuperare e memorizzare nella risorsa metadati aggiuntivi
+ Integrazioni con i servizi Adobe, ad esempio IA per l’analisi di contenuti e commercio, per migliorare i metadati delle risorse con attributi di apprendimento automatico aggiuntivi
+ Derivazione dei metadati della risorsa dal suo binario e memorizzazione come metadati della risorsa in AEM as a Cloud Service

## Come procedere

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

In questo tutorial verrà creato un processo di lavoro dei metadati di Asset compute che deriva i colori più comunemente utilizzati in una risorsa immagine e riscrive i nomi dei colori nei metadati della risorsa in AEM. Anche se il worker è di base, questo tutorial lo utilizza per esplorare il modo in cui i worker Asset compute possono essere utilizzati per riscrivere i metadati nelle risorse in AEM as a Cloud Service.

## Flusso logico di una chiamata di lavoro di metadati Asset compute

Il richiamo dei processi di lavoro dei metadati Asset compute è quasi identico a quello di [rendering binario che genera i processi di lavoro](../develop/worker.md), dove la differenza principale è il tipo restituito è una rappresentazione XMP (XML) i cui valori vengono scritti anche nei metadati della risorsa.

Asset compute worker implementa il contratto Asset compute SDK worker API, nel `renditionCallback(...)` funzione, che è concettualmente:

+ __Input:__ Parametri binari e del profilo di elaborazione originali di una risorsa AEM
+ __Output:__ Una rappresentazione XMP (XML) è persistita nella risorsa AEM come rappresentazione e nei metadati della risorsa

![Flusso logico di lavoro metadati di Asset compute](./assets/metadata/logical-flow.png)

1. Il servizio di authoring AEM richiama il processo di lavoro metadati Asset compute, fornendo il __(1 bis)__ binario originale e __(1 ter)__ eventuali parametri definiti nel profilo di elaborazione.
1. L’SDK di Asset compute orchestra l’esecuzione del processo di lavoro dei metadati di Asset compute personalizzato `renditionCallback(...)` funzione, derivando una rappresentazione XMP (XML), in base al binario della risorsa __(1 bis)__ ed eventuali parametri del profilo di elaborazione __(1 ter)__.
1. Il lavoratore Asset compute salva la rappresentazione XMP (XML) in `rendition.path`.
1. Dati XMP (XML) scritti in `rendition.path` viene trasportato tramite l’SDK di Asset compute al servizio di authoring AEM e lo espone come __(4 bis)__ una rappresentazione del testo e __(4 ter)__ persistente nel nodo di metadati della risorsa.

## Configurare manifest.yml{#manifest}

Tutti i lavoratori Asset compute devono essere registrati nel [manifest.yml](../develop/manifest.md).

Apri il file `manifest.yml` e aggiungere una voce di lavoratore che configura il nuovo lavoratore, in questo caso `metadata-colors`.

_Ricorda `.yml` fa distinzione tra spazi._

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

`function` punta all&#39;implementazione del lavoratore creata in [passaggio successivo](#metadata-worker). Denomina i lavoratori in modo semantico (ad esempio, il `actions/worker/index.js` potrebbe essere stato chiamato meglio `actions/rendition-circle/index.js`), come mostrato nella [URL del lavoratore](#deploy) e determinano anche [nome cartella suite di test del lavoratore](#test).

Il `limits` e `require-adobe-auth` sono configurate in modo discreto per lavoratore. In questo lavoratore, `512 MB` viene allocata una quantità di memoria in quanto il codice controlla (potenzialmente) i dati di grandi dimensioni dell’immagine binaria. L&#39;altro `limits` vengono rimossi per utilizzare i valori predefiniti.

## Sviluppare un processo di lavoro metadati{#metadata-worker}

Crea un nuovo file JavaScript di metadata worker nel progetto Asset compute nel percorso [manifest.yml definito per il nuovo lavoratore](#manifest), in corrispondenza di `/actions/metadata-colors/index.js`

### Installare i moduli npm

Installare i moduli npm aggiuntivi ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors), e [color-namer](https://www.npmjs.com/package/color-namer)) utilizzato in questo processo di lavoro di Asset compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Codice processo metadati

Questo processo di lavoro è molto simile al [processo di lavoro che genera copie trasformate](../develop/worker.md), la differenza principale consiste nel fatto che scrive i dati XMP (XML) in `rendition.path` per essere salvati in AEM.


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
      // These namespaces are automatically registered in AEM if they do not yet exist
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

Una volta completato, il codice del processo di lavoro può essere eseguito utilizzando lo strumento di sviluppo Asset compute locale.

Perché il nostro progetto di Asset compute contiene due lavoratori (il precedente [copia trasformata circolare](../develop/worker.md) e questo `metadata-colors` lavoratore), [Strumenti per lo sviluppo Asset compute](../develop/development-tool.md) nella definizione del profilo sono elencati i profili di esecuzione per entrambi i processi di lavoro. La seconda definizione di profilo punta al nuovo `metadata-colors` lavoratore.

![Rendering dei metadati XML](./assets/metadata/metadata-rendition.png)

1. Dalla directory principale del progetto Asset compute
1. Esegui `aio app run` per avviare lo strumento di sviluppo Asset compute
1. In __Seleziona un file...__ a discesa, scegli un [immagine di esempio](../assets/samples/sample-file.jpg) da elaborare
1. Nella seconda configurazione di definizione del profilo, che punta al `metadata-colors` lavoratore, aggiornamento `"name": "rendition.xml"` durante la generazione del rendering XMP (XML) da parte di questo processo di lavoro. Se necessario, aggiungi un `colorsFamily` parametro (valori supportati) `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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

1. Tocca __Esegui__ e attendi la generazione della rappresentazione XML
   + Poiché entrambi i processi di lavoro sono elencati nella definizione del profilo, verranno generate entrambe le rappresentazioni. Facoltativamente, la definizione del profilo superiore che punta al [processo di lavoro copia trasformata circolare](../develop/worker.md) possono essere eliminate per evitare di eseguirle dallo strumento di sviluppo.
1. Il __Rappresentazioni__ sezione visualizza un’anteprima della rappresentazione generata. Tocca il `rendition.xml` per scaricarlo e aprirlo in VS Code (o nel tuo editor XML/di testo preferito) per rivederlo.

## Verifica il lavoratore{#test}

I processi di lavoro dei metadati possono essere testati utilizzando [stesso Asset compute di framework di test delle rappresentazioni binarie](../test-debug/test.md). L&#39;unica differenza è `rendition.xxx` Il file nel test case deve essere il rendering XMP (XML) previsto.

1. Crea la seguente struttura nel progetto Asset compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilizza il [file di esempio](../assets/samples/sample-file.jpg) come test case `file.jpg`.
3. Aggiungi il seguente JSON a `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Osserva `"fmt": "xml"` è necessario per istruire la suite di test a generare un `.xml` rendering basato su testo.

4. Fornisci il codice XML previsto in `rendition.xml` file. Tale risultato può essere ottenuto:
   + Esecuzione del file di input di prova tramite lo strumento di sviluppo e salvataggio della rappresentazione XML (convalidata).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Esegui `aio app test` dalla directory principale del progetto Asset compute per eseguire tutte le suite di test.

### Distribuire il processo di lavoro in Adobe I/O Runtime{#deploy}

Per richiamare questo nuovo processo di lavoro metadati da AEM Assets, è necessario distribuirlo a Adobe I/O Runtime utilizzando il comando:

```
$ aio app deploy
```

![distribuzione app aio](./assets/metadata/aio-app-deploy.png)

Tieni presente che verranno distribuiti tutti i processi di lavoro nel progetto. Rivedi [istruzioni di distribuzione non abbreviate](../deploy/runtime.md) come implementare nelle aree di lavoro di staging e produzione.

### Integrare con i profili di elaborazione dell’AEM{#processing-profile}

Richiama il lavoratore dall’AEM creando un nuovo servizio Profilo di elaborazione personalizzato esistente o modificandone uno esistente che richiama il lavoratore distribuito.

![Profilo di elaborazione](./assets/metadata/processing-profile.png)

1. Accedi al servizio di authoring as a Cloud Service dell’AEM come __Amministratore AEM__
1. Accedi a __Strumenti > Risorse > Profili elaborazione__
1. __Crea__ una nuova, oppure __modifica__ ed esistente, Profilo di elaborazione
1. Tocca il __Personalizzato__ , quindi tocca __Aggiungi nuovo__
1. Definisci il nuovo servizio
   + __Crea rappresentazione metadati__: attiva
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + URL del lavoratore ottenuto durante il [distribuire](#deploy) o utilizzando il comando `aio app get-url`. Assicurati che l’URL punti nell’area di lavoro corretta in base all’ambiente as a Cloud Service dall’AEM.
   + __Parametri del servizio__
      + Tocca __Aggiungi parametro__
         + Chiave: `colorFamily`
         + Valore: `pantone`
            + Valori supportati: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipi MIME__
      + __Include:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Questi sono gli unici tipi MIME supportati dai moduli npm di terze parti utilizzati per derivare i colori.
      + __Esclusi:__ `Leave blank`
1. Tocca __Salva__ in alto a destra
1. Se non lo hai già fatto, applica il profilo di elaborazione a una cartella AEM Assets

### Aggiornare lo schema metadati{#metadata-schema}

Per rivedere i metadati dei colori, mappa due nuovi campi nello schema di metadati dell’immagine con le nuove proprietà dei dati metadati che il processo di lavoro compila.

![Schema metadati](./assets/metadata/metadata-schema.png)

1. Nel servizio Author dell’AEM, passa a __Strumenti > Risorse > Schemi metadati__
1. Accedi a __predefinito__ e selezionare e modificare __immagine__ e aggiungere campi modulo di sola lettura per esporre i metadati colore generati
1. Aggiungi un __Testo su riga singola__
   + __Etichetta campo__: `Colors Family`
   + __Mappa su proprietà__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regole > Campo > Disattiva modifica__: selezionato
1. Aggiungi un __Testo con più valori__
   + __Etichetta campo__: `Colors`
   + __Mappa su proprietà__: `./jcr:content/metadata/wknd:colors`
1. Tocca __Salva__ in alto a destra

## Elaborazione delle risorse

![Dettagli risorsa](./assets/metadata/asset-details.png)

1. Nel servizio Author dell’AEM, passa a __Risorse > File__
1. Passa alla cartella o sottocartella a cui viene applicato il profilo di elaborazione
1. Carica una nuova immagine (JPEG, PNG, GIF o SVG) nella cartella o rielabora le immagini esistenti utilizzando il file aggiornato [Profilo di elaborazione](#processing-profile)
1. Al termine dell’elaborazione, seleziona la risorsa e tocca __proprietà__ nella barra delle azioni superiore per visualizzarne i metadati
1. Rivedi `Colors Family` e `Colors` [campi metadati](#metadata-schema) per i metadati riscritti dal processo di lavoro metadati Asset compute personalizzato.

Con i metadati del colore scritti nei metadati della risorsa, nella `[dam:Asset]/jcr:content/metadata` risorsa, questi metadati sono indicizzati per migliorarne la funzionalità di individuazione tramite ricerca e possono anche essere riscritti nel file binario della risorsa __Writeback di metadati DAM__ il flusso di lavoro viene richiamato.

### Rendering dei metadati in AEM Assets

![File di rappresentazione dei metadati di AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

Anche il file XMP effettivo generato da Asset compute Metadata Worker viene memorizzato come rappresentazione discreta sulla risorsa. Questo file generalmente non viene utilizzato, ma vengono utilizzati i valori applicati al nodo di metadati della risorsa, ma l’output XML non elaborato del processo di lavoro è disponibile in AEM.

## codice di lavoro metadati-colori su Github

La versione finale `metadata-colors/index.js` è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

La versione finale `test/asset-compute/metadata-colors` la suite di test è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
