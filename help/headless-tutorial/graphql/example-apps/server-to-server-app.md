---
title: App server-to-server Node.js - Esempio di AEM headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Node.js lato server illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM headless as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 203
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# App Node.js server-to-server

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione server-to-server illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti e stamparle sul terminale.

![App server-to-server Node.js con AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

Visualizza [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L’applicazione Node.js funziona con le seguenti opzioni di distribuzione AEM. Tutte le distribuzioni richiedono [Sito WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) da installare.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Facoltativamente [credenziali del servizio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) se si autorizzano richieste (ad esempio, connessione al servizio di authoring AEM).

Questa applicazione Node.js può connettersi a AEM Author o AEM Publish in base ai parametri della riga di comando.

## Come usare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri un terminale ed esegui i comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. L’app può essere eseguita utilizzando il comando:

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   Ad esempio, per eseguire l’app su Pubblicazione AEM senza autorizzazione:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   Per eseguire l’app su AEM Author con autorizzazione:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Nel terminale deve essere stampato un elenco JSON di avventure provenienti dal sito di riferimento WKND.

## Il codice

Di seguito è riportato un riepilogo della modalità di creazione dell’applicazione Node.js server-to-server, della connessione a AEM Headless per il recupero di contenuti tramite query persistenti GraphQL e della modalità di presentazione di tali dati. Il codice completo è disponibile su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

Il caso d’uso comune per le app AEM headless server-to-server prevede la sincronizzazione dei dati dei frammenti di contenuto dall’AEM in altri sistemi. Tuttavia questa applicazione è intenzionalmente semplice e stampa i risultati JSON dalla query persistente.

### Query persistenti

Seguendo le best practice di AEM Headless, l’applicazione utilizza le query persistenti AEM GraphQL per eseguire query sui dati dell’avventura. L’applicazione utilizza due query persistenti:

+ `wknd/adventures-all` query persistente, che restituisce tutte le avventure in AEM con un set abbreviato di proprietà. Questa query persistente guida l’elenco di avventure della visualizzazione iniziale.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

### Creare un client AEM headless

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### Esegui query persistente GraphQL

Le query persistenti dell’AEM vengono eseguite tramite HTTP GET, pertanto [Client AEM headless per Node.js](https://github.com/adobe/aem-headless-client-nodejs) viene utilizzato per [eseguire le query GraphQL persistenti;](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) contro l’AEM e recupera il contenuto dell’avventura.

La query persistente viene richiamata chiamando `aemHeadlessClient.runPersistedQuery(...)`e passando il nome della query GraphQL persistente. Una volta che GraphQL restituisce i dati, trasmettili al `doSomethingWithDataFromAEM(..)` , che stampa i risultati, ma in genere invia i dati a un altro sistema o genera un output in base ai dati recuperati.

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
