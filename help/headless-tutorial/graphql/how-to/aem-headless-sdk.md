---
title: Utilizzo dell’SDK headless dell’AEM
description: Scopri come eseguire query GraphQL utilizzando l’SDK headless dell’AEM.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
duration: 200
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 6%

---

# SDK headless per AEM

L’SDK headless dell’AEM è un set di librerie che possono essere utilizzate dai client per interagire in modo rapido e semplice con le API headless dell’AEM tramite HTTP.

L’SDK headless dell’AEM è disponibile per diverse piattaforme:

+ [SDK headless di AEM per browser lato client (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [SDK headless di AEM per lato server/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [SDK headless di AEM per Java™](https://github.com/adobe/aem-headless-client-java)

## Query GraphQL persistenti

L&#39;esecuzione di query AEM tramite GraphQL tramite query persistenti (anziché [query GraphQL definite dal client](#graphl-queries)) consente agli sviluppatori di rendere persistente una query (ma non i relativi risultati) nell&#39;AEM e quindi di richiedere l&#39;esecuzione della query per nome. Le query persistenti sono simili al concetto di stored procedure nei database SQL.

Le query persistenti sono più performanti delle query GraphQL definite dal client, in quanto vengono eseguite utilizzando HTTP GET, che può essere memorizzato nella cache ai livelli CDN e AEM Dispatcher. Anche le query persistenti diventano effettive, definiscono un’API e riducono la necessità per lo sviluppatore di comprendere i dettagli di ciascun modello per frammenti di contenuto.

### Esempi di codice{#persisted-graphql-queries-code-examples}

Di seguito sono riportati alcuni esempi di codice per l’esecuzione di una query persistente di GraphQL nei confronti dell’AEM.

+++ Esempio di JavaScript

Installa [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) eseguendo il comando `npm install` dalla radice del progetto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

In questo esempio di codice viene illustrato come eseguire query su AEM utilizzando il modulo npm [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) utilizzando la sintassi `async/await`. L&#39;SDK headless dell&#39;AEM per JavaScript supporta anche la sintassi [Promise](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Questo codice presuppone che una query persistente con il nome `wknd/adventureNames` sia stata creata in AEM Author e pubblicata in AEM Publish.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(..) esempio

Installa [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) eseguendo il comando `npm install` dalla radice del progetto React.

```
$ npm i @adobe/aem-headless-client-js
```

In questo esempio di codice viene illustrato come utilizzare [React useEffect(..) hook](https://reactjs.org/docs/hooks-effect.html) per eseguire una chiamata asincrona a AEM GraphQL.

L&#39;utilizzo di `useEffect` per effettuare la chiamata GraphQL asincrona in React è utile perché:

1. Fornisce il wrapper sincrono per la chiamata asincrona all&#39;AEM.
1. Riduce l’AEM che richiede inutilmente.

Questo codice presuppone che una query persistente con il nome `wknd-shared/adventure-by-slug` sia stata creata in AEM Author e pubblicata in AEM Publish utilizzando GraphiQL.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

Richiama l&#39;hook React `useEffect` personalizzato da un&#39;altra posizione in un componente React.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

È possibile creare nuovi hook di `useEffect` per ogni query persistente utilizzata dall&#39;app React.

+++

<p> </p>

## Query GraphQL

AEM supporta le query GraphQL definite dal client, tuttavia è consigliabile AEM utilizzare [query GraphQL persistenti](#persisted-graphql-queries).

## Webpack 5+

L&#39;SDK JS di AEM headless ha dipendenze da `util` che non è incluso in Webpack 5+ per impostazione predefinita. Se utilizzi Webpack 5+ e ricevi il seguente errore:

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

Aggiungi `devDependencies` seguente al file `package.json`:

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

Eseguire quindi `npm install` per installare le dipendenze.
