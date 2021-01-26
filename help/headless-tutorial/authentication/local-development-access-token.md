---
title: Token di accesso allo sviluppo locale
description: AEM Token di accesso allo sviluppo locale vengono utilizzati per accelerare lo sviluppo di integrazioni con AEM come Cloud Service che interagiscono in modo programmatico con i servizi AEM Author o Publish via HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
translation-type: tm+mt
source-git-commit: eabd8650886fa78d9d177f3c588374a443ac1ad6
workflow-type: tm+mt
source-wordcount: '1044'
ht-degree: 0%

---


# Token di accesso allo sviluppo locale

Gli sviluppatori che creano integrazioni che richiedono l&#39;accesso programmatico alle AEM come Cloud Service necessitano di un modo semplice e rapido per ottenere token di accesso temporaneo per AEM per facilitare le attività di sviluppo locale. Per soddisfare questa esigenza, AEM Developer Console consente agli sviluppatori di generare autonomamente token di accesso temporaneo che possono essere utilizzati per accedere ai AEM a livello di programmazione.

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## Genera un token di accesso allo sviluppo locale

![Ottenimento di un token di accesso allo sviluppo locale](assets/local-development-access-token/getting-a-local-development-access-token.png)

Il Token di accesso allo sviluppo locale fornisce l&#39;accesso ai servizi AEM Author e Publish come utente che ha generato il token, insieme alle relative autorizzazioni. Nonostante questo sia un token di sviluppo, non condividete questo token.

1. In [ Adobe AdminConsole](https://adminconsole.adobe.com/) assicurati che lo sviluppatore sia membro di:
   + __Cloud Manager - Profilo di prodotto__ DeveloperIMS (concede l&#39;accesso a AEM Developer Console)
   + Il profilo di prodotto __AEM Administrators__ o __AEM Utenti__ IMS per il servizio dell&#39;ambiente AEM con cui verrà integrato il token di accesso
   + La AEM sandbox come ambiente di Cloud Service richiede solo l&#39;iscrizione al profilo di prodotto __AEM Administrators__ o __AEM Users__
1. Accedi a [ Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Aprire il programma contenente il AEM come ambiente Cloud Service da integrare con
1. Toccate il __puntini di sospensione__ accanto all&#39;ambiente nella sezione __Ambienti__, quindi selezionate __Developer Console__
1. Toccate la scheda __Integrazioni__
1. Toccare il pulsante __Get Local Development Token__
1. Toccate il __pulsante di download__ nell&#39;angolo in alto a sinistra per scaricare il file JSON contenente il valore `accessToken` e salvate il file JSON in una posizione sicura nel computer di sviluppo.
   + Questo è il token di accesso sviluppatore di 24 ore al AEM come ambiente di Cloud Service.

![AEM Developer Console - Integrazioni - Ottieni token di sviluppo locale](./assets/local-development-access-token/developer-console.png)

## Scarica il token di accesso allo sviluppo locale{#download-local-development-access-token}

![Token di accesso allo sviluppo locale - Applicazione esterna](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Scaricare il token di accesso allo sviluppo locale temporaneo da AEM Developer Console
   + Il token di accesso allo sviluppo locale scade ogni 24 ore, pertanto gli sviluppatori dovranno scaricare i nuovi token di accesso ogni giorno
1. È in corso di sviluppo un&#39;applicazione esterna che interagisce in modo programmatico con AEM come Cloud Service
1. L&#39;applicazione esterna viene letta nel token di accesso allo sviluppo locale
1. L&#39;applicazione esterna crea richieste HTTP da AEM come Cloud Service, aggiungendo il Token di accesso allo sviluppo locale come token del portatore all&#39;intestazione Autorizzazione delle richieste HTTP
1. AEM come Cloud Service riceve la richiesta HTTP, autentica la richiesta ed esegue il lavoro richiesto dalla richiesta HTTP, quindi restituisce una risposta HTTP all’applicazione esterna

### Applicazione esterna

Verrà creata una semplice applicazione JavaScript esterna per illustrare come accedere in modo programmatico AEM come Cloud Service tramite HTTPS utilizzando il token di accesso per gli sviluppatori locali. Questo spiega come _qualsiasi applicazione o sistema_ che viene eseguito al di fuori di AEM, indipendentemente dal framework o dalla lingua, possa utilizzare il token di accesso per l&#39;autenticazione e l&#39;accesso a livello di programmazione AEM come Cloud Service. Nella sezione [successiva](./service-credentials.md) verrà aggiornato questo codice applicazione per supportare l&#39;approccio per la generazione di un token per l&#39;utilizzo in produzione.

Questa applicazione viene eseguita dalla riga di comando e aggiorna i metadati AEM risorse utilizzando  API HTTP AEM Assets, utilizzando il seguente flusso:

1. Legge i parametri dalla riga di comando (`getCommandLineParams()`)
1. Ottiene il token di accesso utilizzato per l&#39;autenticazione AEM come Cloud Service (`getAccessToken(...)`)
1. Elenca tutte le risorse in una cartella di risorse AEM specificata in un parametro della riga di comando (`listAssetsByFolder(...)`)
1. Aggiornare i metadati delle risorse elencate con i valori specificati nei parametri della riga di comando (`updateMetadata(...)`)

L’elemento chiave per l’autenticazione a livello di programmazione a AEM mediante il token di accesso è l’aggiunta di un’intestazione di richiesta HTTP di autorizzazione a tutte le richieste HTTP effettuate a AEM nel seguente formato:

+ `Authorization: Bearer ACCESS_TOKEN`

## Esecuzione dell&#39;applicazione esterna

1. Assicurarsi che [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) sia installato nel computer di sviluppo locale, che verrà utilizzato per eseguire l&#39;applicazione esterna
1. Scaricare e decomprimere l&#39;applicazione esterna di esempio [](./assets/aem-guides_token-authentication-external-application.zip)
1. Dalla riga di comando, nella cartella del progetto eseguire `npm install`
1. Copiare il [scaricato Local Development Access Token](#download-local-development-access-token) in un file denominato `local_development_token.json` nella directory principale del progetto
   + Ma ricorda, non fare mai credenziali a Git!
1. Aprire `index.js` e rivedere il codice e i commenti dell&#39;applicazione esterna.

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   Rivedete le chiamate `fetch(..)` in `listAssetsByFolder(...)` e `updateMetadata(...)` e notate che `headers` definiscono l&#39;intestazione della richiesta HTTP `Authorization` con un valore di `Bearer <ACCESS TOKEN>`. Questo è il modo in cui la richiesta HTTP proveniente dall’applicazione esterna si autentica AEM come Cloud Service.

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json?configid=ims`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   Qualsiasi richiesta HTTP da AEM come Cloud Service, deve impostare il token di accesso del portatore nell’intestazione Autorizzazione. Ricordate, ogni AEM come ambiente di Cloud Service richiede un token di accesso personale. Il token di accesso dello sviluppo non funzionerà su Stage o Produzione, lo Stage non funzionerà su Sviluppo o Produzione, e la Produzione non funzionerà su Sviluppo o Stage!

1. Tramite la riga di comando, dalla radice del progetto viene eseguita l&#39;applicazione, trasmettendo i seguenti parametri:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Vengono passati i seguenti parametri:

   + `aem`: Lo schema e il nome host del AEM come ambiente Cloud Service con cui l&#39;applicazione interagisce (ad esempio).  `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: Percorso della cartella di risorse le cui risorse verranno aggiornate con  `propertyValue`; NON aggiungere il  `/content/dam` prefisso (ad es.  `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`: Nome della proprietà della risorsa da aggiornare, relativo a  `[dam:Asset]/jcr:content` (ad esempio).  `metadata/dc:rights`).
   + `propertyValue`: Il valore su cui impostare  `propertyName` il valore; i valori con spazi devono essere racchiusi con  `"` (es.  `"WKND Limited Use"`)
   + `file`: Il percorso relativo del file JSON scaricato da AEM Developer Console.

   Esecuzione corretta dei risultati dell&#39;applicazione per ogni risorsa aggiornata:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Verifica aggiornamento metadati in AEM

Verificare che i metadati siano stati aggiornati, effettuando l&#39;accesso al AEM come ambiente di Cloud Service (assicurarsi che lo stesso host passato al parametro della riga di comando `aem` sia accessibile).

1. Accedere al AEM come ambiente di Cloud Service con cui l&#39;applicazione esterna ha interagito (utilizzare lo stesso host fornito nel parametro della riga di comando `aem`)
1. Passa a __Risorse__ > __File__
1. Spostatevi nella cartella delle risorse specificata dal parametro della riga di comando `folder`, ad esempio __WKND__ > __English__ > __Adventures__ > __Napa Wine Tasting__
1. Aprire la cartella __Properties__ per qualsiasi risorsa (frammento non di contenuto)
1. Toccate la scheda __Avanzate__
1. Rivedete il valore della proprietà aggiornata, ad esempio __Copyright__ mappato alla proprietà JCR `metadata/dc:rights` aggiornata, che riflette il valore fornito nel parametro `propertyValue`, ad esempio __Uso limitato WKND__

![Aggiornamento dei metadati di utilizzo limitato WKND](./assets/local-development-access-token/asset-metadata.png)

## Passaggi successivi

Ora che abbiamo eseguito l&#39;accesso a livello di programmazione AEM come Cloud Service utilizzando il token di sviluppo locale, dobbiamo aggiornare il codice a.