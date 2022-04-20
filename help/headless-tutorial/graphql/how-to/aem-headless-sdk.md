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
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# AEM Headless SDK

L’AEM SDK headless è un set di librerie che possono essere utilizzate dai client per interagire rapidamente e facilmente con le API headless AEM tramite HTTP.

L’SDK AEM Headless è disponibile per varie piattaforme:

+ [SDK AEM Headless per browser lato client (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless SDK per server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM Headless SDK per Java™](https://github.com/adobe/aem-headless-client-java)

## Query GraphQL

Query AEM utilizzando GraphQL utilizzando query (anziché [query GraphQL persistenti](#persisted-graphql-queries)) consente agli sviluppatori di definire le query nel codice, specificando esattamente quale contenuto richiedere da AEM.

Le query GraphQL tendono ad essere meno performanti rispetto alle query persistenti, in quanto vengono eseguite utilizzando HTTP POST, che è meno cache-able nei livelli CDN e Dispatcher AEM.

### Esempi di codice{#graphql-queries-code-examples}

Di seguito sono riportati alcuni esempi di codice per eseguire una query GraphQL rispetto a AEM.

+++ Esempio JavaScript

Installa il [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) eseguendo la `npm install` dalla directory principale del progetto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Questo esempio di codice mostra come eseguire query AEM utilizzando [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) modulo npm che utilizza `async/await` sintassi. L&#39;SDK AEM Headless per JavaScript supporta anche [Sintassi delle promesse](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
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

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

Importa e utilizza `useGraphQL` aggancia il componente React per eseguire query AEM.

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## Query GraphQL persistenti

Query AEM utilizzando GraphQL utilizzando query persistenti (anziché [normali query GraphQL](#graphl-queries)) consente agli sviluppatori di persistere una query (ma non i relativi risultati) in AEM, quindi di richiedere l’esecuzione della query per nome. Le query persistenti sono simili al concetto di stored procedure nei database SQL.

Le query persistenti tendono ad essere più performanti delle normali query GraphQL, in quanto le query persistenti vengono eseguite tramite HTTP GET, che è più cache-able nei livelli CDN e Dispatcher AEM. Le query persistenti, inoltre, definiscono un’API e dissociano la necessità per lo sviluppatore di comprendere i dettagli di ciascun modello di frammento di contenuto.

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
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

Questo codice presuppone una query persistente con il nome `wknd/adventureNames` è stato creato su AEM Author e pubblicato su AEM Publish.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

E richiama questo codice da altrove nel codice React.

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
