---
title: Esportare le risorse
description: Scopri come esportare e scaricare in blocco le risorse nel computer locale.
feature: Asset Management
version: Experience Manager as a Cloud Service
topic: Content Management
role: Developer
level: Experienced
last-substantial-update: 2025-04-28T00:00:00Z
doc-type: Tutorial
jira: KT-15313
thumbnail: KT-15313.jpeg
exl-id: d04c3316-6f8f-4fd1-9df1-3fe09d44f735
duration: 256
source-git-commit: 107a9a77a1bf2337f309d503a4a310d8d0781f0d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# Esportare le risorse

Scopri come esportare le risorse nel computer locale utilizzando uno script Node.js personalizzabile. Questo script di esportazione fornisce un esempio di come scaricare in modo programmatico le risorse da AEM utilizzando [API HTTP di AEM Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), concentrandosi in particolare sulle rappresentazioni originali per garantire la massima qualità. È progettato per replicare la struttura di cartelle di AEM Assets sull&#39;unità locale, semplificando il backup o la migrazione delle risorse.

Lo script scarica solo le rappresentazioni originali della risorsa, senza metadati associati, a meno che tali metadati non siano stati incorporati nella risorsa come XMP. Ciò significa che eventuali informazioni descrittive, categorizzazioni o tag memorizzati in AEM ma non integrati nei file delle risorse non saranno inclusi nel download. È possibile scaricare anche altre rappresentazioni modificando lo script in modo da includerle. Assicurati di disporre di spazio sufficiente per archiviare le risorse esportate.

Lo script viene in genere eseguito con AEM Author, ma può essere eseguito anche con AEM Publish, purché gli endpoint API HTTP di AEM Assets e le rappresentazioni delle risorse siano accessibili tramite Dispatcher.

Prima di eseguire lo script è necessario configurarlo con l’URL dell’istanza di AEM, le credenziali utente (token di accesso) e il percorso della cartella da esportare.

## Esporta script

Lo script, scritto come modulo di JavaScript, fa parte di un progetto Node.js, in quanto ha una dipendenza da `node-fetch` e `p-limit`. È possibile copiare lo script seguente in un progetto Node.js vuoto di tipo `module` ed eseguire `npm install node-fetch p-limit` per installare la dipendenza.

Questo script descrive la struttura delle cartelle di AEM Assets e scarica risorse e cartelle in una cartella locale sul computer. Utilizza l&#39;[API HTTP di AEM Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) per recuperare i dati della cartella e delle risorse e scarica le rappresentazioni originali delle risorse.

```javascript
// export-assets.js

import fetch from 'node-fetch';
import { promises as fs } from 'fs';
import path from 'path';
import pLimit from 'p-limit';

// Do not process the contents of these well-known AEM system folders
const SKIP_FOLDERS = ['/content/dam/appdata', '/content/dam/projects', '/content/dam/_CSS', '/content/dam/_DMSAMPLE' ];

/**
 * Determine if the folder should be processed based on the entity and AEM path.
 * 
 * @param {Object} entity the AEM entity that should represent a folder returned from AEM Assets HTTP API
 * @param {String} aemPath the path in AEM of this source
 * @returns true if the entity should be processed, false otherwise
 */
function isValidFolder(entity, aemPath) {
    if (aemPath === '/content/dam') {
        return true;
    } else if (!entity.class.includes('assets/folder')) {
        return false;
    } else if (SKIP_FOLDERS.find((path) => path === aemPath)) {
        return false;
    } else if (entity.properties.hidden) {
        return false;
    }
    
    return true;
}

/**
 * Determine if the entity is downloadable.
 * @param {Object} entity the AEM entity that should represent an asset returned from AEM Assets HTTP API
 * @returns true if the entity is downloadable, false otherwise
 */
function isDownloadable(entity) {
    if (entity.class.includes('assets/folder')) {
        return false;
    } else if (entity.properties.contentFragment) {
        return false;
    }
    return true;
}

/**
 * Helper function to get the link from the entity based on the relationship name.
 * @param {Object} entity the entity from the AEM Assets HTTP API
 * @param {String} rel the relationship name
 * @returns {String} link URL
 */
function getLink(entity, rel) {
    return entity.links.find(link => link.rel.includes(rel));
}

/**
 * Helper function to fetch JSON data from the AEM Assets HTTP API.
 * @param {String} url the AEM Assets HTTP API URL to fetch data from
 * @returns {Object} the JSON response
 */
async function fetchJSON(url) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
        }
    });

    if (!response.ok) {
        throw new Error(`Error fetching ${url}: ${response.status}`);
    }

    return response.json();
}

/**
 * Helper function to download a file from AEM Assets.
 * @param {String} url the URL of the asset rendition to download
 * @param {String} outputPath the local path to save the downloaded file
 */
async function downloadFile(url, outputPath) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
        }
    });

    if (!response.ok) {
        throw new Error(`Failed to download file: ${response.statusText}`);
    }

    const arrayBuffer = await response.arrayBuffer();
    await fs.writeFile(outputPath, Buffer.from(arrayBuffer));

    console.log(`Downloaded asset: ${outputPath}`);
}

/**
 * Main entry point to download assets from AEM.
 * 
 * @param {Object} options 
 * @param {String} options.apiUrl (optional) the direct AEM Assets HTTP API URL
 * @param {String} options.localPath local filesystem path to save the assets
 * @param {String} options.aemPath AEM folder path
 */
async function downloadAssets({ apiUrl, localPath = LOCAL_DOWNLOAD_FOLDER, aemPath = '/content/dam' }) {
    if (!apiUrl) {
        const prefix = "/content/dam/";
        let apiPath = aemPath.startsWith(prefix) ? aemPath.substring(prefix.length) : aemPath;    

        if (!apiPath.startsWith('/')) {
            apiPath = '/' + apiPath;
        }

        apiUrl = `${AEM_HOST}/api/assets.json${apiPath}`;
    }
    
    const data = await fetchJSON(apiUrl);
    const entities = data.entities || [];

    // First, process folders
    for (const folder of entities.filter(entity => entity.class.includes('assets/folder'))) {
        const newLocalPath = path.join(localPath, folder.properties.name);
        const newAemPath = path.join(aemPath, folder.properties.name);

        if (!isValidFolder(folder, newAemPath)) {
            continue;
        }

        await fs.mkdir(newLocalPath, { recursive: true });

        await downloadAssets({
            apiUrl: getLink(folder, 'self')?.href,
            localPath: newLocalPath,
            aemPath: newAemPath
        });
    }

    // Now, process assets with concurrency limit
    const limit = pLimit(MAX_CONCURRENT_DOWNLOADS);
    const downloads = [];

    for (const asset of entities.filter(entity => entity.class.includes('assets/asset'))) {
        const assetLocalPath = path.join(localPath, asset.properties.name);
        if (isDownloadable(asset)) {
            downloads.push(limit(() => downloadFile(getLink(asset, 'content')?.href, assetLocalPath)));
        }
    }

    await Promise.all(downloads);

    // Handle pagination
    const nextUrl = getLink(data, 'next');
    if (nextUrl) {
        await downloadAssets({
            apiUrl: nextUrl?.href,
            localPath,
            aemPath
        });
    }
}

/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './exported-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server.
const MAX_CONCURRENT_DOWNLOADS = 10;

/***** SCRIPT ENTRY POINT *****/

console.time('Download AEM assets');

await downloadAssets({
    aemPath: AEM_ASSETS_FOLDER,
    localPath: LOCAL_DOWNLOAD_FOLDER
}).catch(console.error);

console.timeEnd('Download AEM assets');
```

## Configurare l’esportazione

Con lo script scaricato, aggiorna le variabili di configurazione nella parte inferiore dello script.

È possibile ottenere `AEM_ACCESS_TOKEN` seguendo i passaggi descritti nell&#39;esercitazione sull&#39;autenticazione basata su token per AEM as a Cloud Service[&#128279;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview).  Spesso il token sviluppatore di 24 ore è sufficiente, purché il completamento dell’esportazione richieda meno di 24 ore e l’utente che genera il token abbia accesso in lettura alle risorse da esportare.

```javascript
...
/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './export-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;
```

## Esportare le risorse

Esegui lo script utilizzando Node.js per esportare le risorse nel computer locale.

A seconda del numero di risorse e delle loro dimensioni, il completamento dello script potrebbe richiedere un po’ di tempo. Durante l&#39;esecuzione, lo script [registra l&#39;avanzamento](#output) nella console.

```shell
$ node export-assets.js
```

## Esporta output

Lo script di esportazione registra l’avanzamento nella console, indicando le risorse che vengono scaricate. Al termine dello script, le risorse vengono salvate nella cartella locale specificata nella configurazione e il registro termina con il tempo totale impiegato per scaricare le risorse.

```plaintext
...
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring3sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring5sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring6sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/wa_camping_adobe.pdf
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobestock-156407519.jpeg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3094.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3851.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a7083.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a6978.jpg
Download AEM assets: 24.770s
```

Le risorse esportate si trovano nella cartella locale specificata nella configurazione `LOCAL_DOWNLOAD_FOLDER`. La struttura di cartelle rispecchia la struttura di cartelle di AEM Assets, con le risorse scaricate nelle sottocartelle appropriate. Questi file possono essere caricati in [provider di archiviazione cloud supportati](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/assets-view/bulk-import-assets-view), per [importazione in massa](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/migration/bulk-import) in altre istanze di AEM o per scopi di backup.

![Risorse esportate](./assets/export/exported-assets.png)
