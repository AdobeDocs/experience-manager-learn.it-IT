---
title: Next.js - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Next.js illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
source-git-commit: ae49fb45db6f075a34ae67475f2fcc5658cb0413
workflow-type: tm+mt
source-wordcount: '806'
ht-degree: 2%

---

# App Next.js

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Next.js illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti. Il client AEM Headless per JavaScript viene utilizzato per eseguire le query persistenti GraphQL che alimentano l&#39;app.

![App Next.js con AEM Headless](./assets/next-js/next-js.png)

Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

+ [Node.js v16+](https://nodejs.org/it/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L’app Next.js funziona con le seguenti opzioni di distribuzione AEM. Tutte le distribuzioni richiedono [WKND Shared v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sito WKND v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) da installare nell’ambiente as a Cloud Service AEM.

Questa app di esempio Next.js è progettata per connettersi a __Pubblicazione AEM__ servizio.

### Requisiti di AEM Author

Next.js è progettato per connettersi a __Pubblicazione AEM__ e accedere a contenuti non protetti. Next.js può essere configurato per connettersi ad AEM Author tramite `.env` proprietà descritte di seguito. Le immagini fornite da AEM Author richiedono l’autenticazione, e quindi l’utente che accede all’app Next.js deve anche essere connesso ad AEM Author.

## Come utilizzare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifica le `aem-guides-wknd-graphql/next-js/.env.local` file e set `NEXT_PUBLIC_AEM_HOST` al servizio AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Se ci si connette al servizio AEM Author, l’autenticazione deve essere fornita perché il servizio AEM Author è protetto per impostazione predefinita.

   Per utilizzare un set di account AEM locale `AEM_AUTH_METHOD=basic` e fornire il nome utente e la password nel `AEM_AUTH_USER` e `AEM_AUTH_PASSWORD` proprietà.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Per utilizzare un [Token di sviluppo locale as a Cloud Service AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` e fornire il valore completo del token di sviluppo nel `AEM_AUTH_DEV_TOKEN` proprietà.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Modifica le `aem-guides-wknd-graphql/next-js/.env.local` file e convalida  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` è impostato sull&#39;endpoint GraphQL AEM appropriato.

   Quando utilizzi [WKND condiviso](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sito WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), utilizza `wknd-shared` Endpoint API GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. Apri un prompt dei comandi e avvia l&#39;app Next.js utilizzando i seguenti comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Una nuova finestra del browser apre l’app Next.js in [http://localhost:3000](Http://localhost:3000)
1. L’app Next.js visualizza un elenco di avventure. Quando si seleziona un’avventura, i relativi dettagli vengono aperti in una nuova pagina.

## Il codice

Di seguito è riportato un riepilogo di come viene creata l&#39;app Next.js, di come si connette a AEM Headless per recuperare contenuto utilizzando query GraphQL persistenti e di come tali dati vengono presentati. Il codice completo è disponibile all&#39;indirizzo [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Query persistenti

Seguendo AEM best practice headless, l’app Next.js utilizza query persistenti AEM GraphQL per eseguire query sui dati di avventura. L’app utilizza due query persistenti:

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

+ `wknd/adventure-by-slug` query persistente, che restituisce una singola avventura per `slug` (proprietà personalizzata che identifica in modo univoco un’avventura) con un set completo di proprietà. Questa query persistente potenzia le visualizzazioni dei dettagli dell’avventura.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

Le query persistenti AEM vengono eseguite su HTTP GET e quindi, il [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js) è utilizzato per [eseguire le query GraphQL persistenti](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) su AEM e carica il contenuto dell’avventura nell’app.

Ogni query persistente ha una funzione corrispondente in `src/lib//aem-headless-client.js`, che chiama il punto finale AEM GraphQL e restituisce i dati relativi all’avventura.

Ogni funzione a sua volta richiama il `aemHeadlessClient.runPersistedQuery(...)`, esecuzione della query GraphQL persistente.

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

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### Pagine

L’app Next.js utilizza due pagine per presentare i dati dell’avventura.

+ `src/pages/index.js`

   Usi [getServerSideProps() di Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) chiamare `getAllAdventures()` e visualizza ogni avventura come una scheda.

   L&#39;uso di `getServerSiteProps()` consente il rendering lato server di questa pagina Next.js.

+ `src/pages/adventures/[...slug].js`

   A [Percorso dinamico Next.js](https://nextjs.org/docs/routing/dynamic-routes) che mostra i dettagli di una singola avventura. Questo percorso dinamico prerileva i dati di ogni avventura utilizzando [getStaticProps() di Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) tramite una chiamata a `getAdventureBySlug(..)` utilizzando `slug` Param passato attraverso la selezione dell&#39;avventura sul `adventures/index.js` pagina.

   Il percorso dinamico è in grado di preacquisire i dettagli di tutte le avventure utilizzando [getStaticPaths() di Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) e compilazione di tutte le permutazioni di percorso possibili in base all&#39;elenco completo delle avventure restituite dalla query GraphQL  `getAdventurePaths()`

   L&#39;uso di `getStaticPaths()` e `getStaticProps(..)` ha consentito la generazione statica del sito di queste pagine Next.js .

## Configurazione della distribuzione

Le app Next.js, soprattutto nel contesto del rendering lato server (SSR) e della generazione lato server (SSG), non richiedono configurazioni di sicurezza avanzate come la condivisione delle risorse tra origini diverse (CORS).

Tuttavia, se il file Next.js effettua richieste HTTP a AEM dal contesto del client, potrebbero essere necessarie configurazioni di sicurezza in AEM. Consulta la sezione [Esercitazione sulla distribuzione di app a pagina singola AEM headless](../deployment/spa.md) per ulteriori dettagli.

