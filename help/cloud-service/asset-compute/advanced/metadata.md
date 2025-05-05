---
title: Sviluppare un processo di lavoro per metadati Asset Compute
description: Scopri come creare un processo di lavoro dei metadati di Asset Compute che ricava i colori più comunemente utilizzati in una risorsa di immagini e riscrive i nomi dei colori nei metadati della risorsa in AEM.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 432
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# Sviluppare un processo di lavoro per metadati Asset Compute

I processi di lavoro personalizzati di Asset Compute possono produrre dati XMP (XML) che vengono rimandati ad AEM e memorizzati come metadati in una risorsa.

I casi d’uso comuni includono:

+ Integrazioni con sistemi di terze parti, ad esempio un sistema PIM (Product Information Management system), in cui è necessario recuperare e memorizzare nella risorsa metadati aggiuntivi
+ Integrazioni con i servizi Adobe, come IA per contenuti e Commerce, per migliorare i metadati delle risorse con attributi di apprendimento automatico aggiuntivi
+ Derivazione dei metadati della risorsa dal suo binario e memorizzazione come metadati della risorsa in AEM as a Cloud Service

## Come procedere

>[!VIDEO](https://video.tv.adobe.com/v/3437019?quality=12&learn=on&captions=ita)

In questo tutorial verrà creato un processo di lavoro sui metadati di Asset Compute che deriva i colori più comunemente utilizzati in una risorsa di immagine e riscrive i nomi dei colori nei metadati della risorsa in AEM. Anche se il processo di lavoro è di base, questo tutorial lo utilizza per esplorare come i processi di lavoro di Asset Compute possono essere utilizzati per riscrivere i metadati nelle risorse in AEM as a Cloud Service.

## Flusso logico di una chiamata di un processo di lavoro di metadati Asset Compute

Il richiamo dei processi di lavoro dei metadati di Asset Compute è quasi identico a quello di [rendering binario che genera processi di lavoro](../develop/worker.md). La differenza principale è che il tipo restituito è un rendering XMP (XML) i cui valori vengono scritti anche nei metadati della risorsa.

I processi di lavoro Asset Compute implementano il contratto API del processo di lavoro Asset Compute SDK nella funzione `renditionCallback(...)`, concettualmente:

+ __Input:__ Parametri binari ed elaborazione originali del profilo di una risorsa AEM
+ __Output:__ una copia trasformata XMP (XML) è persistita nella risorsa AEM come copia trasformata e nei metadati della risorsa

![Flusso logico del processo di lavoro metadati di Asset Compute](./assets/metadata/logical-flow.png)

1. Il servizio AEM Author richiama il processo di lavoro metadati di Asset Compute, fornendo il binario originale __(1a)__ della risorsa e __(1b)__ eventuali parametri definiti nel profilo di elaborazione.
1. Asset Compute SDK orchestra l&#39;esecuzione della funzione `renditionCallback(...)` del processo di lavoro metadati di Asset Compute personalizzato, derivando una rappresentazione XMP (XML) basata sul file binario della risorsa __(1a)__ e su eventuali parametri del profilo di elaborazione __(1b)__.
1. Asset Compute worker salva la rappresentazione XMP (XML) in `rendition.path`.
1. I dati XMP (XML) scritti in `rendition.path` vengono trasportati tramite Asset Compute SDK al servizio AEM Author e li espone come __(4a)__ una rappresentazione di testo e __(4b)__ persistenti al nodo di metadati della risorsa.

## Configurare manifest.yml{#manifest}

Tutti i processi di lavoro di Asset Compute devono essere registrati in [manifest.yml](../develop/manifest.md).

Aprire `manifest.yml` del progetto e aggiungere una voce di processo che configura il nuovo processo di lavoro, in questo caso `metadata-colors`.

_Ricorda che `.yml` è sensibile agli spazi._

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

`function` punta all&#39;implementazione di lavoro creata nel [passaggio successivo](#metadata-worker). Denomina i processi di lavoro in modo semantico (ad esempio, `actions/worker/index.js` potrebbe essere stato denominato `actions/rendition-circle/index.js`), in quanto vengono visualizzati nell&#39;URL del [processo di lavoro](#deploy) e determinano anche il nome della cartella della suite di test del [processo di lavoro](#test).

`limits` e `require-adobe-auth` sono configurati in modo discreto per lavoratore. In questo processo di lavoro, `512 MB` di memoria viene allocata in quanto il codice controlla (potenzialmente) i dati di immagini binarie di grandi dimensioni. Gli altri `limits` vengono rimossi per utilizzare i valori predefiniti.

## Sviluppare un processo di lavoro metadati{#metadata-worker}

Crea un nuovo file JavaScript del processo di lavoro metadati nel progetto Asset Compute nel percorso [defined manifest.yml per il nuovo processo di lavoro](#manifest), in `/actions/metadata-colors/index.js`

### Installare i moduli npm

Installa i moduli npm aggiuntivi ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors) e [color-namer](https://www.npmjs.com/package/color-namer)) utilizzati in questo processo di lavoro Asset Compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Codice processo metadati

Questo processo di lavoro è molto simile al [processo di lavoro che genera le rappresentazioni](../develop/worker.md). La differenza principale è che scrive i dati di XMP (XML) in `rendition.path` per essere salvato nuovamente in AEM.


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

Una volta completato, il codice del processo di lavoro può essere eseguito utilizzando lo strumento di sviluppo Asset Compute locale.

Poiché il progetto Asset Compute contiene due processi di lavoro (il precedente [rendering circolare](../develop/worker.md) e questo processo di lavoro `metadata-colors`), la definizione del profilo [Strumento di sviluppo Asset Compute](../develop/development-tool.md) elenca i profili di esecuzione per entrambi i processi di lavoro. La seconda definizione di profilo punta al nuovo processo di lavoro `metadata-colors`.

![Rendering metadati XML](./assets/metadata/metadata-rendition.png)

1. Dalla directory principale del progetto Asset Compute
1. Esegui `aio app run` per avviare lo strumento di sviluppo Asset Compute
1. Nel menu a discesa __Seleziona un file...__, scegli un [immagine di esempio](../assets/samples/sample-file.jpg) da elaborare
1. Nella seconda configurazione della definizione del profilo, che punta al processo di lavoro `metadata-colors`, aggiornare `"name": "rendition.xml"` mentre questo processo di lavoro genera una rappresentazione XMP (XML). Facoltativamente, aggiungere un parametro `colorsFamily` (valori supportati `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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
   + Poiché entrambi i processi di lavoro sono elencati nella definizione del profilo, verranno generate entrambe le rappresentazioni. Facoltativamente, è possibile eliminare la definizione del profilo superiore che punta al [processo di lavoro copia trasformata circolare](../develop/worker.md), per evitare di eseguirla dallo strumento di sviluppo.
1. La sezione __Rendering__ visualizza in anteprima la rappresentazione generata. Tocca `rendition.xml` per scaricarlo e aprirlo in VS Code (o nel tuo editor XML/di testo preferito) per rivederlo.

## Verifica il lavoratore{#test}

I processi di lavoro dei metadati possono essere testati utilizzando [lo stesso framework di test di Asset Compute delle rappresentazioni binarie](../test-debug/test.md). L&#39;unica differenza è che il file `rendition.xxx` nel test case deve essere il rendering XMP (XML) previsto.

1. Crea la seguente struttura nel progetto Asset Compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilizza il [file di esempio](../assets/samples/sample-file.jpg) come `file.jpg` del test case.
3. Aggiungi il seguente JSON a `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Nota che `"fmt": "xml"` è necessario per istruire la suite di test a generare una rappresentazione basata su testo di `.xml`.

4. Fornire l&#39;XML previsto nel file `rendition.xml`. Tale risultato può essere ottenuto:
   + Esecuzione del file di input di prova tramite lo strumento di sviluppo e salvataggio della rappresentazione XML (convalidata).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Esegui `aio app test` dalla directory principale del progetto Asset Compute per eseguire tutte le suite di test.

### Distribuire il processo di lavoro in Adobe I/O Runtime{#deploy}

Per richiamare questo nuovo processo di lavoro metadati da AEM Assets, è necessario distribuirlo a Adobe I/O Runtime utilizzando il comando:

```
$ aio app deploy
```

![distribuzione app aio](./assets/metadata/aio-app-deploy.png)

Tieni presente che verranno distribuiti tutti i processi di lavoro nel progetto. Rivedi le [istruzioni di distribuzione non abbreviate](../deploy/runtime.md) per informazioni su come distribuire nelle aree di lavoro di staging e produzione.

### Integrare con i profili di elaborazione di AEM{#processing-profile}

Richiama il processo di lavoro da AEM creando un nuovo servizio Profilo di elaborazione personalizzato esistente o modificandone uno esistente che richiama il processo di lavoro distribuito.

![Elaborazione profilo](./assets/metadata/processing-profile.png)

1. Accedi al servizio AEM as a Cloud Service Author come __amministratore AEM__
1. Passa a __Strumenti > Assets > Profili elaborazione__
1. __Crea__ un profilo di elaborazione nuovo o __modifica__ ed esistente
1. Tocca la scheda __Personalizzato__ e tocca __Aggiungi nuovo__
1. Definisci il nuovo servizio
   + __Crea rappresentazione metadati__: attiva/disattiva
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + URL del processo di lavoro ottenuto durante la [distribuzione](#deploy) o utilizzando il comando `aio app get-url`. Assicurati che l’URL punti nell’area di lavoro corretta in base all’ambiente AEM as a Cloud Service.
   + __Parametri del servizio__
      + Tocca __Aggiungi parametro__
         + Chiave: `colorFamily`
         + Valore: `pantone`
            + Valori supportati: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipi MIME__
      + __Include:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Questi sono gli unici tipi MIME supportati dai moduli npm di terze parti utilizzati per derivare i colori.
      + __Esclusioni:__ `Leave blank`
1. Tocca __Salva__ in alto a destra
1. Se non lo hai già fatto, applica il profilo di elaborazione a una cartella AEM Assets

### Aggiornare lo schema metadati{#metadata-schema}

Per rivedere i metadati dei colori, mappa due nuovi campi nello schema di metadati dell’immagine con le nuove proprietà dei dati metadati che il processo di lavoro compila.

![Schema metadati](./assets/metadata/metadata-schema.png)

1. Nel servizio AEM Author, passa a __Strumenti > Assets > Schemi di metadati__
1. Passa a __default__, seleziona e modifica __image__ e aggiungi campi modulo di sola lettura per esporre i metadati colore generati
1. Aggiungi un __testo a riga singola__
   + __Etichetta campo__: `Colors Family`
   + __Mappa sulla proprietà__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regole > Campo > Disattiva modifica__: selezionato
1. Aggiungi __Testo con più valori__
   + __Etichetta campo__: `Colors`
   + __Mappa sulla proprietà__: `./jcr:content/metadata/wknd:colors`
1. Tocca __Salva__ in alto a destra

## Elaborazione delle risorse

![Dettagli risorsa](./assets/metadata/asset-details.png)

1. Nel servizio AEM Author, passa a __Assets > File__
1. Passa alla cartella o sottocartella a cui viene applicato il profilo di elaborazione
1. Carica una nuova immagine (JPEG, PNG, GIF o SVG) nella cartella o rielabora le immagini esistenti utilizzando il [profilo di elaborazione](#processing-profile) aggiornato
1. Al termine dell&#39;elaborazione, seleziona la risorsa e tocca __proprietà__ nella barra delle azioni superiore per visualizzarne i metadati
1. Esaminare i `Colors Family` e i `Colors` [campi di metadati](#metadata-schema) per i metadati scritti dal processo di lavoro metadati Asset Compute personalizzato.

Con i metadati del colore scritti nei metadati della risorsa, nella risorsa `[dam:Asset]/jcr:content/metadata`, questi metadati vengono indicizzati in modo da aumentare la capacità di individuazione della risorsa utilizzando questi termini tramite ricerca e possono anche essere riscritti nel file binario della risorsa se viene richiamato il flusso di lavoro __Writeback di metadati DAM__.

### Rendering dei metadati in AEM Assets

![File di rendering metadati AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

Anche il file XMP effettivo generato da Asset Compute metadata worker viene memorizzato come rappresentazione discreta sulla risorsa. Questo file generalmente non viene utilizzato, ma vengono utilizzati i valori applicati al nodo di metadati della risorsa, ma l’output XML non elaborato del worker è disponibile in AEM.

## codice di lavoro metadati-colori su Github

Il `metadata-colors/index.js` finale è disponibile su Github all&#39;indirizzo:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

La suite di test `test/asset-compute/metadata-colors` finale è disponibile su Github all&#39;indirizzo:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
