---
title: App Node.js server-to-server - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Node.js lato server dimostra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 3%

---

# App Node.js server-to-server

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione server-to-server illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti e stamparle sul terminale.

![App Node.js server-to-server con AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

+ [Node.js v10+](https://nodejs.org/it/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L&#39;applicazione Node.js funziona con le seguenti opzioni di distribuzione AEM. Tutte le implementazioni richiedono l’ [Sito WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) da installare.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Facoltativamente, [credenziali del servizio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) se si autorizzano le richieste (ad esempio, se ci si connette al servizio AEM Author).

Questa applicazione Node.js può connettersi ad AEM Author o AEM Publish in base ai parametri della riga di comando.

## Come utilizzare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri un terminale ed esegui i comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. L’app può essere eseguita utilizzando il comando :

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   Ad esempio, per eseguire l’app su AEM Publish senza autorizzazione:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   Per eseguire l’app su AEM Author con autorizzazione:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Nel terminale deve essere stampato un elenco JSON delle avventure del sito di riferimento WKND.

## Il codice

Di seguito è riportato un riepilogo di come viene creata l&#39;applicazione Node.js server-to-server, di come si connette a AEM Headless per recuperare il contenuto utilizzando le query persistenti GraphQL e di come tali dati vengono presentati. Il codice completo è disponibile all&#39;indirizzo [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app).

Il caso d&#39;uso comune per le app senza intestazione da server a server è quello di sincronizzare i dati dei frammenti di contenuto da AEM ad altri sistemi, tuttavia questa applicazione è intenzionalmente semplice e stampa i risultati JSON dalla query persistente.

### Query persistenti

Seguendo AEM best practice headless, l’applicazione utilizza query persistenti AEM GraphQL per eseguire query sui dati di avventura. L&#39;applicazione utilizza due query persistenti:

+ `wknd/adventures-all` query persistente, che restituisce tutte le avventure in AEM con un set abbreviato di proprietà. Questa query persistente guida l&#39;elenco di avventura della visualizzazione iniziale.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

### Crea AEM client headless

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

Le query persistenti AEM vengono eseguite su HTTP GET e quindi, il [AEM client headless per Node.js](https://github.com/adobe/aem-headless-client-nodejs) è utilizzato per [eseguire le query GraphQL persistenti](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) contro AEM e recupera il contenuto dell’avventura.

La query persistente viene richiamata chiamando `aemHeadlessClient.runPersistedQuery(...)`e passando il nome della query GraphQL persistente. Una volta che GraphQL restituisce i dati, passali alla versione semplificata `doSomethingWithDataFromAEM(..)` funzione che stampa i risultati, ma in genere invia i dati a un altro sistema, o genera un output in base ai dati recuperati.

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
