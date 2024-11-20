---
title: Come richiamare le API AEM basate su OpenAPI
description: Scopri come configurare e richiamare le API AEM basate su OpenAPI su AEM as a Cloud Service da applicazioni personalizzate.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Tutorial
jira: KT-16516
thumbnail: KT-16516.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 24c641e7-ab4b-45ee-bbc7-bf6b88b40276
source-git-commit: 316e08e6647d6fd731cd49ae1bc139ce57c3a7f4
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# Come richiamare le API AEM basate su OpenAPI{#invoke-openapi-based-aem-apis}

Scopri come configurare e richiamare le API AEM basate su OpenAPI su AEM as a Cloud Service da applicazioni personalizzate.

>[!AVAILABILITY]
>
>Le API AEM basate su OpenAPI sono disponibili come parte di un programma di accesso anticipato. Se ti interessa accedervi, ti invitiamo a inviare un&#39;e-mail a [aem-apis@adobe.com](mailto:aem-apis@adobe.com) con una descrizione del tuo caso d&#39;uso.

In questo tutorial imparerai a:

- Abilita l’accesso alle API AEM basate su OpenAPI per il tuo ambiente AEM as a Cloud Service.
- Crea e configura un progetto Adobe Developer Console (ADC) per accedere alle API AEM utilizzando l’autenticazione server-to-server OAuth.
- Sviluppa un’applicazione NodeJS di esempio che chiama l’API Assets Author per recuperare i metadati di una risorsa specifica.

Prima di iniziare, assicurati di aver rivisto la sezione [Accesso alle API Adobe e ai concetti correlati](overview.md#accessing-adobe-apis-and-related-concepts).

## Prerequisiti

Per completare questa esercitazione, è necessario:

- L’ambiente AEM as a Cloud Service è stato modernizzato con:
   - Versione AEM `2024.10.18459.20241031T210302Z` o successiva.
   - Nuovi profili di prodotto (se l’ambiente è stato creato prima di novembre 2024)

- Il progetto [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) di esempio deve essere distribuito su di esso.

- Accesso a [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- Installa [Node.js](https://nodejs.org/it/) nel computer locale per eseguire l&#39;applicazione NodeJS di esempio.

## Passaggi di sviluppo

Le fasi di sviluppo di alto livello sono:

1. Modernizzazione dell’ambiente AEM as a Cloud Service.
1. Abilita l’accesso alle API AEM.
1. Crea progetto Adobe Developer Console (ADC).
1. Configura progetto ADC
   1. Aggiungere le API AEM desiderate
   1. Configurarne l’autenticazione
   1. Associare il profilo di prodotto alla configurazione di autenticazione
1. Configura l’istanza AEM per abilitare la comunicazione del progetto ADC
1. Sviluppare un esempio di applicazione NodeJS
1. Verificare il flusso end-to-end

## Modernizzazione dell’ambiente AEM as a Cloud Service

Iniziamo con la modernizzazione dell’ambiente AEM as a Cloud Service. Questo passaggio è necessario solo se l&#39;ambiente non è modernizzato.

La modernizzazione dell’ambiente AEM as a Cloud Service è un processo in due fasi:

- Aggiornamento all’ultima versione dell’AEM
- Aggiungi nuovi profili di prodotto.

### Aggiorna istanza AEM

Per aggiornare l&#39;istanza AEM, nella sezione _Ambienti_ di Cloud Manager[Adobe](https://my.cloudmanager.adobe.com/), seleziona l&#39;icona _puntini di sospensione_ accanto al nome dell&#39;ambiente e seleziona l&#39;opzione **Aggiorna**.

![Aggiorna istanza AEM](assets/update-aem-instance.png)

Quindi fai clic sul pulsante **Invia** ed esegui la pipeline full stack suggerita.

![Seleziona la versione più recente dell&#39;AEM](assets/select-latest-aem-release.png)

Nel mio caso, il nome della pipeline full stack è _Dev :: Fullstack-Deploy_ e il nome dell&#39;ambiente AEM è _wknd-program-dev_. Il nome può variare nel tuo caso.

### Aggiungere nuovi profili di prodotto

Per aggiungere nuovi profili di prodotto all&#39;istanza AEM Adobe, nella sezione _Ambienti_ di Cloud Manager[1}, seleziona l&#39;icona _puntini di sospensione_ accanto al nome dell&#39;ambiente e seleziona l&#39;opzione **Aggiungi profili di prodotto**.](https://my.cloudmanager.adobe.com/)

![Aggiungi nuovi profili di prodotto](assets/add-new-product-profiles.png)

Puoi rivedere i profili di prodotto appena aggiunti facendo clic sull&#39;icona _puntini di sospensione_ accanto al nome dell&#39;ambiente e selezionando **Gestisci accesso** > **Profili autore**.

Nella finestra _Admin Console_ sono visualizzati i profili di prodotto appena aggiunti.

![Verifica nuovi profili di prodotto](assets/review-new-product-profiles.png)

I passaggi precedenti completano la modernizzazione dell’ambiente AEM as a Cloud Service.

## Abilitare l’accesso alle API AEM

I nuovi profili di prodotto consentono l’accesso API AEM basato su OpenAPI in Adobe Developer Console (ADC).

I nuovi profili di prodotto aggiunti sono associati ai _Servizi_ che rappresentano gruppi di utenti AEM con elenchi di controllo di accesso (ACL) predefiniti. I _Servizi_ vengono utilizzati per controllare il livello di accesso alle API AEM.

Puoi anche selezionare o deselezionare i _servizi_ associati al profilo prodotto per ridurre o aumentare il livello di accesso.

Rivedi l&#39;associazione facendo clic sull&#39;icona _Visualizza dettagli_ accanto al nome del profilo di prodotto.

![Servizi di revisione associati al profilo di prodotto](assets/review-services-associated-with-product-profile.png)

Per impostazione predefinita, il servizio **Utenti API AEM Assets** non è associato ad alcun profilo di prodotto. Associamolo al profilo di prodotto **Amministratori AEM - Autore - Programma XXX - Ambiente XXX** appena aggiunto. Dopo questa associazione, l&#39;_API Asset Author_ del progetto ADC può configurare l&#39;autenticazione server-to-server OAuth e associare l&#39;account di autenticazione al profilo di prodotto.

![Associa il servizio Utenti API di AEM Assets al profilo di prodotto](assets/associate-aem-assets-api-users-service-with-product-profile.png)

È importante notare che prima della modernizzazione, nell&#39;istanza Autore AEM, erano disponibili due profili di prodotto: **Amministratori AEM-XXX** e **Utenti AEM-XXX**. È inoltre possibile associare questi profili di prodotto esistenti ai nuovi servizi.

## Crea progetto Adobe Developer Console (ADC)

Quindi, crea un progetto ADC per accedere alle API AEM.

1. Accedi a [Adobe Developer Console](https://developer.adobe.com/console) utilizzando il tuo Adobe ID.

   ![Adobe Developer Console](assets/adobe-developer-console.png)

1. Dalla sezione _Guida rapida_, fare clic sul pulsante **Crea nuovo progetto**.

   ![Crea nuovo progetto](assets/create-new-project.png)

1. Crea un nuovo progetto con il nome predefinito.

   ![Nuovo progetto creato](assets/new-project-created.png)

1. Modifica il nome del progetto facendo clic sul pulsante **Modifica progetto** nell&#39;angolo in alto a destra. Specifica un nome significativo e fai clic su **Salva**.

   ![Modifica nome progetto](assets/edit-project-name.png)

## Configura progetto ADC

Quindi, configura il progetto ADC per aggiungere API AEM, configurarne l’autenticazione e associare il profilo di prodotto.

1. Per aggiungere le API AEM, fare clic sul pulsante **Aggiungi API**.

   ![Aggiungi API](assets/add-api.png)

1. Nella finestra di dialogo _Aggiungi API_, filtra per _Experience Cloud_, seleziona la scheda **API Autore AEM Assets** e fai clic su **Avanti**.

   ![Aggiungi API AEM](assets/add-aem-api.png)

1. Nella finestra di dialogo _Configura API_, selezionare l&#39;opzione di autenticazione **Server-to-Server** e fare clic su **Avanti**. L’autenticazione server-to-server è ideale per i servizi back-end che richiedono accesso API senza interazione da parte dell’utente.

   ![Seleziona autenticazione](assets/select-authentication.png)

1. Rinominare le credenziali per semplificarne l&#39;identificazione (se necessario) e fare clic su **Avanti**. A scopo dimostrativo, viene utilizzato il nome predefinito.

   ![Rinomina credenziali](assets/rename-credential.png)

1. Seleziona il profilo di prodotto **Amministratori AEM - Autore - Programma XXX - Ambiente XXX** e fai clic su **Salva**. Come puoi vedere, è disponibile per la selezione solo il profilo di prodotto associato al servizio Utenti API di AEM Assets.

   ![Seleziona profilo prodotto](assets/select-product-profile.png)

1. Controlla l’API AEM e la configurazione dell’autenticazione.

   ![Configurazione API AEM](assets/aem-api-configuration.png)

   ![Configurazione autenticazione](assets/authentication-configuration.png)


## Configura istanza AEM per abilitare la comunicazione del progetto ADC

Per abilitare le credenziali server-to-server OAuth del progetto ADC ClientID per la comunicazione con l&#39;istanza AEM, è necessario configurare l&#39;istanza AEM.

Questa operazione viene eseguita definendo la configurazione nel file `config.yaml` nel progetto AEM. Quindi, distribuisci il file `config.yaml` utilizzando la pipeline di configurazione in Cloud Manager.

1. In Progetto AEM individuare o creare il file `config.yaml` dalla cartella `config`.

   ![Individua YAML configurazione](assets/locate-config-yaml.png)

1. Aggiungi la seguente configurazione al file `config.yaml`.

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's OAuth Server-to-Server credential ClientID>"
   ```

   Sostituire `<ADC Project's OAuth Server-to-Server credential ClientID>` con l&#39;ID client effettivo delle credenziali server-to-server OAuth del progetto ADC. L&#39;endpoint API utilizzato in questa esercitazione è disponibile solo sul livello di authoring, ma per altre API la configurazione yaml può avere anche un nodo _publish_ o _preview_.

1. Esegui il commit delle modifiche di configurazione nell’archivio Git e invia le modifiche all’archivio remoto.

1. Distribuisci le modifiche precedenti utilizzando la pipeline di configurazione in Cloud Manager. Il file `config.yaml` può essere installato anche in un RDE, utilizzando gli strumenti della riga di comando.

   ![Distribuisci config.yaml](assets/config-pipeline.png)

## Sviluppare un esempio di applicazione NodeJS

Sviluppiamo un’applicazione NodeJS di esempio che chiama l’API Assets Author.

Per sviluppare l’applicazione puoi utilizzare altri linguaggi di programmazione come Java, Python, ecc.

A scopo di test, puoi utilizzare [Postman](https://www.postman.com/), [curl](https://curl.se/) o qualsiasi altro client REST per richiamare le API AEM.

### Rivedere l’API

Prima di sviluppare l&#39;applicazione, esaminiamo [distribuire l&#39;endpoint dei metadati](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) della risorsa specificata dall&#39;_API Assets Author_. La sintassi API è:

```http
GET https://{bucket}.adobeaemcloud.com/adobe/assets/{assetId}/metadata
```

Per recuperare i metadati di una risorsa specifica, sono necessari i valori `bucket` e `assetId`. `bucket` è il nome dell&#39;istanza AEM senza il nome di dominio Adobe (.adobeaemcloud.com), ad esempio `author-p63947-e1420428`.

`assetId` è l&#39;UUID JCR della risorsa con il prefisso `urn:aaid:aem:`, ad esempio `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`. Esistono diversi modi per ottenere `assetId`:

- Aggiungere l&#39;estensione del percorso della risorsa AEM `.json` per ottenere i metadati della risorsa. `https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json` e cercare la proprietà `jcr:uuid`.

- In alternativa, è possibile ottenere `assetId` esaminando la risorsa nella finestra di ispezione degli elementi del browser. Cercare l&#39;attributo `data-id="urn:aaid:aem:..."`.

  ![risorsa Inspect](assets/inspect-asset.png)

### Richiama l’API tramite il browser

Prima di sviluppare l&#39;applicazione, richiamiamo l&#39;API utilizzando la funzionalità **Prova** nella [documentazione API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata).

1. Apri la [documentazione dell&#39;API Assets Author](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author) nel browser.

1. Espandi la sezione _Metadati_ e fai clic sull&#39;opzione **Distribuisce i metadati della risorsa specificata**.

1. Nel riquadro destro fare clic sul pulsante **Prova**.
   ![Documentazione API](assets/api-documentation.png)

1. Immetti i seguenti valori:
   1. Il valore `bucket` è il nome dell&#39;istanza AEM senza il nome di dominio Adobe (.adobeaemcloud.com), ad esempio `author-p63947-e1420428`.

   1. I valori `Bearer Token` e `X-Api-Key` relativi alla sezione **Security** sono ottenuti dalle credenziali server-to-server OAuth del progetto ADC. Fare clic su **Genera token di accesso** per ottenere il valore `Bearer Token` e utilizzare il valore `ClientID` come `X-Api-Key`.
      ![Genera token di accesso](assets/generate-access-token.png)

   1. Il valore `assetId` relativo alla sezione **Parameters** è l&#39;identificatore univoco della risorsa in AEM. `X-Adobe-Accept-Experimental` è impostato su 1.

      ![Richiama API - valori di input](assets/invoke-api-input-values.png)

1. Fai clic su **Invia** per richiamare l&#39;API.

1. Controlla la scheda **Risposta** per visualizzare la risposta API.

   ![Richiama API - risposta](assets/invoke-api-response.png)

I passaggi precedenti confermano la modernizzazione dell’ambiente AEM as a Cloud Service, consentendo l’accesso alle API AEM. Conferma inoltre la corretta configurazione del progetto ADC e la comunicazione delle credenziali server-to-server OAuth ClientID con l’istanza di authoring AEM.

### Esempio di applicazione NodeJS

Creiamo un esempio di applicazione NodeJS.

Per sviluppare l&#39;applicazione, puoi utilizzare _Esegui l&#39;applicazione campione_ o le istruzioni _Sviluppo_.


>[!BEGINTABS]

>[!TAB Esegui l&#39;applicazione campione]

1. Scarica il file zip dell&#39;applicazione [demo-nodejs-app-to-invoke-aem-openapi](assets/demo-nodejs-app-to-invoke-aem-openapi.zip) di esempio ed estrailo.

1. Passa alla cartella estratta e installa le dipendenze.

   ```bash
   $ npm install
   ```

1. Sostituire i segnaposto nel file `.env` con i valori effettivi delle credenziali server-to-server OAuth del progetto ADC.

1. Sostituire `<BUCKETNAME>` e `<ASSETID>` nel file `src/index.js` con i valori effettivi.

1. Esegui l’applicazione NodeJS.

   ```bash
   $ node src/index.js
   ```

>[!TAB Sviluppo guidato]

1. Crea un nuovo progetto NodeJS.

   ```bash
   $ mkdir demo-nodejs-app-to-invoke-aem-openapi
   $ cd demo-nodejs-app-to-invoke-aem-openapi
   $ npm init -y
   ```

1. Installa le librerie _fetch_ e _dotenv_ per effettuare richieste HTTP e leggere le variabili di ambiente rispettivamente.

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. Apri il progetto nel tuo editor di codice preferito e aggiorna il file `package.json` per aggiungere `type` a `module`.

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. Crea il file `.env` e aggiungi la seguente configurazione. Sostituisci i segnaposto con i valori effettivi delle credenziali server-to-server OAuth del progetto ADC.

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. Creare il file `src/index.js` e aggiungere il codice seguente e sostituire `<BUCKETNAME>` e `<ASSETID>` con i valori effettivi.

   ```javascript
   // Import the dotenv configuration to load environment variables from the .env file
   import "dotenv/config";
   
   // Import the fetch function to make HTTP requests
   import fetch from "node-fetch";
   
   // REPLACE THE FOLLOWING VALUES WITH YOUR OWN
   const bucket = "<BUCKETNAME>"; // Bucket name is the AEM instance name (e.g. author-p63947-e1420428)
   const assetId = "<ASSETID>"; // Asset ID is the unique identifier for the asset in AEM (e.g. urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da). You can get it by inspecting the asset in browser's element inspector, look for data-id="urn:aaid:aem:..."
   
   // Load environment variables for authentication
   const clientId = process.env.CLIENT_ID; // Adobe IMS client ID
   const clientSecret = process.env.CLIENT_SECRET; // Adobe IMS client secret
   const scopes = process.env.SCOPES; // Scope for the API access
   
   // Adobe IMS endpoint for obtaining an access token
   const adobeIMSV3TokenEndpointURL =
   "https://ims-na1.adobelogin.com/ims/token/v3";
   
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
       console.log("Getting access token from IMS"); // Log process initiation
       //console.log("Client ID: " + clientId); // Display client ID for debugging purposes
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       //console.log("Response status: " + response.status); // Log the HTTP status for debugging
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       console.log("Access token received"); // Log success message
   
       // Return the access token
       return responseJSON.access_token;
   };
   
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   
   // Call the getAssets function to start the process
   getAssetMetadat();
   ```

1. Esegui l’applicazione NodeJS.

   ```bash
   $ node src/index.js
   ```

>[!ENDTABS]

### Risposta API

Dopo la corretta esecuzione, la risposta API viene visualizzata nella console. La risposta contiene i metadati della risorsa specificata.

```json
{
  "assetId": "urn:aaid:aem:9c09ff70-9ee8-4b14-a5fa-ec37baa0d1b3",
  "assetMetadata": {    
    ...
    "dc:title": "A Young Mountain Biking Couple Takes A Minute To Take In The Scenery",
    "xmp:CreatorTool": "Adobe Photoshop Lightroom Classic 7.5 (Macintosh)",
    ...
  },
  "repositoryMetadata": {
    ...
    "repo:name": "adobestock-221043703.jpg",
    "repo:path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg",
    "repo:state": "ACTIVE",
    ...
  }
}
```

Congratulazioni Le API AEM basate su OpenAPI sono state richiamate dall’applicazione personalizzata mediante l’autenticazione server-to-server di OAuth.

### Rivedi il codice dell’applicazione

I callout chiave dal codice di esempio dell’applicazione NodeJS sono:

1. **Autenticazione IMS**: recupera un token di accesso utilizzando l&#39;impostazione delle credenziali da server a server OAuth nel progetto ADC.

   ```javascript
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token from Adobe IMS token endpoint https://ims-na1.adobelogin.com/ims/token/v3
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       // Return the access token
       return responseJSON.access_token;
   };
   ...
   ```

1. **Chiamata API**: richiama l&#39;API Assets Author per recuperare i metadati per una risorsa specifica fornendo il token di accesso per l&#39;autorizzazione.

   ```javascript
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   ...
   ```

## Riepilogo

In questo tutorial hai imparato a richiamare le API AEM basate su OpenAPI da applicazioni personalizzate. Hai abilitato l’accesso alle API AEM, hai creato e configurato un progetto Adobe Developer Console (ADC).
Nel progetto ADC, hai aggiunto le API AEM, ne hai configurato il tipo di autenticazione e hai associato il profilo di prodotto. Hai anche configurato l’istanza AEM per abilitare la comunicazione del progetto ADC e sviluppato un’applicazione NodeJS di esempio che chiama l’API Assets Author.
