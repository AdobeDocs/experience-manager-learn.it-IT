---
title: Utilizzo dell'SDK AEM Headless
description: Scopri come creare query GraphQL utilizzando l’SDK AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 595d990b7d8ed3c801a085892fef38d780082a15
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 4%

---

# AEM Headless SDK

L’AEM SDK headless è un set di librerie che possono essere utilizzate dai client per interagire rapidamente e facilmente con le API headless AEM tramite HTTP.

L’SDK AEM Headless è disponibile per varie piattaforme:

+ [SDK AEM Headless per browser lato client (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless SDK per server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM Headless SDK per Java™](https://github.com/adobe/aem-headless-client-java)

## Query GraphQL persistenti

Query AEM utilizzando GraphQL utilizzando query persistenti (anziché [query GraphQL definite dal client](#graphl-queries)) consente agli sviluppatori di persistere una query (ma non i relativi risultati) in AEM, quindi di richiedere l’esecuzione della query per nome. Le query persistenti sono simili al concetto di stored procedure nei database SQL.

Le query persistenti sono più performanti rispetto alle query GraphQL definite dal client, in quanto vengono eseguite query persistenti tramite HTTP GET, che è memorizzabile nella cache nei livelli CDN e Dispatcher AEM. Le query persistenti, inoltre, definiscono un’API e dissociano la necessità per lo sviluppatore di comprendere i dettagli di ciascun modello di frammento di contenuto.

### Esempi di codice{#persisted-graphql-queries-code-examples}

Di seguito sono riportati alcuni esempi di codice per eseguire una query persistente GraphQL rispetto a AEM.

+++ Esempio JavaScript

Installa il [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) eseguendo la `npm install` dalla directory principale del progetto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Questo esempio di codice mostra come eseguire query AEM utilizzando [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) modulo npm che utilizza `async/await` sintassi. L&#39;SDK AEM Headless per JavaScript supporta anche [Sintassi delle promesse](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Questo codice presuppone una query persistente con il nome `wknd/adventureNames` è stato creato su AEM Author e pubblicato su AEM Publish.

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

Installa il [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) eseguendo la `npm install` dalla radice del progetto React.

```
$ npm i @adobe/aem-headless-client-js
```

Questo esempio di codice mostra come utilizzare il [React useEffect(..) gancio](https://reactjs.org/docs/hooks-effect.html) per eseguire una chiamata asincrona a AEM GraphQL.

Utilizzo `useEffect` per effettuare la chiamata GraphQL asincrona in React è utile perché:

1. Fornisce il wrapper sincrono per la chiamata asincrona a AEM.
1. Riduce i AEM che richiedono inutilmente.

Questo codice presuppone una query persistente con il nome `wknd-shared/adventure-by-slug` è stato creato su AEM Author e pubblicato su AEM Publish utilizzando GraphiQL.

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

Richiamare il React personalizzato `useEffect` agganciarsi da un altro punto di un componente React.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Nuovo `useEffect` È possibile creare hook per ogni query persistente utilizzata dall&#39;app React.

+++

<p> </p>

## Query GraphQL

AEM supporta le query GraphQL definite dal client, tuttavia è AEM best practice utilizzare [query GraphQL persistenti](#persisted-graphql-queries).

