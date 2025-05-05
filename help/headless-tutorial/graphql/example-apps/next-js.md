---
title: Next.js - Esempio di AEM headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Next.js illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 210
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# App Next.js

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Next.js illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti. Il client AEM Headless per JavaScript viene utilizzato per eseguire le query persistenti GraphQL che alimentano l’app.

![App Next.js con AEM Headless](./assets/next-js/next-js.png)

Visualizza il [codice sorgente in GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## Requisiti di AEM

L’app Next.js funziona con le seguenti opzioni di distribuzione di AEM. Tutte le distribuzioni richiedono l&#39;installazione di [Sito WKND condiviso v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sito WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) nell&#39;ambiente AEM as a Cloud Service.

Questa app Next.js di esempio è progettata per connettersi al servizio __AEM Publish__.

### Requisiti di authoring di AEM

Next.js è progettato per connettersi al servizio __Pubblicazione AEM__ e accedere a contenuto non protetto. Il file Next.js può essere configurato per la connessione ad AEM Author tramite le proprietà `.env` descritte di seguito. Le immagini fornite da AEM Author richiedono l’autenticazione e, pertanto, anche l’utente che accede all’app Next.js deve essere registrato in AEM Author.

## Come usare

1. Clona l&#39;archivio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modificare il file `aem-guides-wknd-graphql/next-js/.env.local` e impostare `NEXT_PUBLIC_AEM_HOST` sul servizio AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Se ti connetti al servizio di authoring di AEM, è necessario fornire l’autenticazione in quanto il servizio di authoring di AEM è sicuro per impostazione predefinita.

   Per utilizzare un set di account AEM locale `AEM_AUTH_METHOD=basic` e fornire il nome utente e la password nelle proprietà `AEM_AUTH_USER` e `AEM_AUTH_PASSWORD`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Per utilizzare un token di sviluppo locale [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=it#generating-the-access-token), impostare `AEM_AUTH_METHOD=dev-token` e fornire il valore del token di sviluppo completo nella proprietà `AEM_AUTH_DEV_TOKEN`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Modificare il file `aem-guides-wknd-graphql/next-js/.env.local` e convalidare `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` è impostato sull&#39;endpoint AEM GraphQL appropriato.

   Quando si utilizza [Sito WKND condiviso](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sito WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), utilizzare l&#39;endpoint API di GraphQL `wknd-shared`.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. Apri un prompt dei comandi e avvia l’app Next.js utilizzando i seguenti comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Una nuova finestra del browser apre l&#39;app Next.js in [http://localhost:3000](http://localhost:3000)
1. L’app Next.js mostra un elenco di avventure. Quando si seleziona un’avventura, i relativi dettagli vengono aperti in una nuova pagina.

## Il codice

Di seguito è riportato un riepilogo della modalità di creazione dell’app Next.js, della connessione ad AEM Headless per il recupero di contenuti tramite query persistenti GraphQL e della modalità di presentazione dei dati. Il codice completo si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Query persistenti

Seguendo le best practice di AEM Headless, l’app Next.js utilizza le query persistenti di AEM GraphQL per eseguire query sui dati dell’avventura. L’app utilizza due query persistenti:

+ Query persistente `wknd/adventures-all`, che restituisce tutte le avventure in AEM con un set ridotto di proprietà. Questa query persistente guida l’elenco di avventure della visualizzazione iniziale.

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

+ Query persistente `wknd/adventure-by-slug`, che restituisce una singola avventura di `slug` (una proprietà personalizzata che identifica in modo univoco un&#39;avventura) con un set completo di proprietà. Questa query persistente attiva le visualizzazioni dei dettagli dell’avventura.

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Esegui query persistente GraphQL

Le query persistenti di AEM vengono eseguite tramite HTTP GET, pertanto il [client AEM Headless per JavaScript](https://github.com/adobe/aem-headless-client-js) viene utilizzato per [eseguire le query GraphQL persistenti](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) su AEM e caricare il contenuto dell&#39;avventura nell&#39;app.

Ogni query persistente ha una funzione corrispondente in `src/lib//aem-headless-client.js`, che chiama l&#39;endpoint di AEM GraphQL e restituisce i dati dell&#39;avventura.

Ogni funzione a sua volta richiama `aemHeadlessClient.runPersistedQuery(...)`, eseguendo la query GraphQL persistente.

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### Pagine

L’app Next.js utilizza due pagine per presentare i dati dell’avventura.

+ `src/pages/index.js`

  Utilizza getServerSideProps() [&#128279;](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) di Next.js per chiamare `getAllAdventures()` e visualizza ogni avventura come una scheda.

  L&#39;utilizzo di `getServerSiteProps()` consente il rendering lato server della pagina Next.js.

+ `src/pages/adventures/[...slug].js`

  Percorso dinamico [Next.js](https://nextjs.org/docs/routing/dynamic-routes) che visualizza i dettagli di una singola avventura. Questa route dinamica preacquisisce i dati di ogni avventura utilizzando getStaticProps() [&#128279;](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) di Next.js tramite una chiamata a `getAdventureBySlug(slug, queryVariables)` tramite il parametro `slug` trasmesso tramite la selezione dell&#39;avventura nella pagina `adventures/index.js` e `queryVariables` per controllare il formato, la larghezza e la qualità dell&#39;immagine.

  La route dinamica è in grado di prerecuperare i dettagli per tutte le avventure utilizzando [GetStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) di Next.js e popolando tutte le permutazioni di route possibili in base all&#39;elenco completo delle avventure restituite dalla query GraphQL `getAdventurePaths()`

  L&#39;utilizzo di `getStaticPaths()` e `getStaticProps(..)` ha consentito la generazione statica del sito di queste pagine Next.js.

## Configurazione della distribuzione

Le app Next.js, soprattutto nel contesto del rendering lato server (SSR) e della generazione lato server (SSG), non richiedono configurazioni di sicurezza avanzate come la condivisione delle risorse tra le origini (CORS).

Tuttavia, se Next.js effettua richieste HTTP ad AEM dal contesto del client, potrebbero essere necessarie configurazioni di sicurezza in AEM. Per ulteriori dettagli, consulta l&#39;[esercitazione sulla distribuzione di app a pagina singola AEM Headless](../deployment/spa.md).
