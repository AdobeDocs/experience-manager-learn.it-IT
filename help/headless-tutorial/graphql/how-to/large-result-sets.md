---
title: Come lavorare con grandi set di risultati in AEM Headless
description: Scopri come lavorare con grandi set di risultati con AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
source-git-commit: 9eb706e49f12a3ebd5222e733f540db4cf2c8748
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 1%

---


# Set di risultati grandi AEM Headless

AEM le query GraphQL headless possono restituire risultati di grandi dimensioni. Questo articolo descrive come lavorare con grandi risultati in AEM Headless per garantire le migliori prestazioni per la tua applicazione.

AEM Headless supporta un [offset/limite](#list-query) e [impaginazione basata su cursore](#paginated-query) query a sottoinsiemi più piccoli di un set di risultati più grande. È possibile effettuare più richieste per raccogliere tutti i risultati necessari.

Gli esempi seguenti utilizzano piccoli sottoinsiemi di risultati (quattro record per richiesta) per illustrare le tecniche. In un&#39;applicazione reale, puoi utilizzare un numero maggiore di record per richiesta per migliorare le prestazioni. 50 record per richiesta è una buona linea di base.

## Modello per frammenti di contenuto

L’impaginazione e l’ordinamento possono essere utilizzati per qualsiasi modello di frammento di contenuto.

## Query persistenti GraphQL

Quando si utilizzano set di dati di grandi dimensioni, è possibile utilizzare sia l’impaginazione offset e limite che quella basata su cursore per recuperare un sottoinsieme specifico di dati. Tuttavia, vi sono alcune differenze tra le due tecniche che possono rendere una più appropriata rispetto all&#39;altra in determinate situazioni.

### Offset/limit

Elencare query, utilizzando `limit` e `offset` forniscono un approccio semplice che specifica il punto iniziale (`offset`) e il numero di record da recuperare (`limit`). Questo approccio consente di selezionare un sottoinsieme di risultati da qualsiasi punto all’interno dell’intero set di risultati, ad esempio saltando a una pagina specifica di risultati. Anche se è facile da implementare, può essere lento e inefficiente quando si tratta di risultati di grandi dimensioni, in quanto il recupero di molti record richiede la scansione di tutti i record precedenti. Questo approccio può anche causare problemi di prestazioni quando il valore di offset è alto, in quanto potrebbe richiedere il recupero e l&#39;eliminazione di molti risultati.

#### Query GraphQL

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### Variabili di query

```json
{
  "offset": 1,
  "limit": 4
}
```

#### Risposta GraphQL

La risposta JSON risultante contiene le avventure più costose 2°, 3°, 4° e 5°. Le prime due avventure nei risultati hanno lo stesso prezzo (`4500` quindi [query elenco](#list-queries) specifica le avventure con lo stesso prezzo e quindi ordinate per titolo in ordine crescente.)

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### Query impaginata

L’impaginazione basata su cursore, disponibile nelle query impaginate, comporta l’utilizzo di un cursore (un riferimento a un record specifico) per recuperare il successivo set di risultati. Questo approccio è più efficiente in quanto evita la necessità di analizzare tutti i record precedenti per recuperare il sottoinsieme di dati richiesto. Le query impaginate sono ideali per l’iterazione attraverso set di risultati di grandi dimensioni dall’inizio, fino a un punto intermedio o fino alla fine. Elencare query, utilizzando `limit` e `offset` forniscono un approccio semplice che specifica il punto iniziale (`offset`) e il numero di record da recuperare (`limit`). Questo approccio consente di selezionare un sottoinsieme di risultati da qualsiasi punto all’interno dell’intero set di risultati, ad esempio saltando a una pagina specifica di risultati. Anche se è facile da implementare, può essere lento e inefficiente quando si tratta di risultati di grandi dimensioni, in quanto il recupero di molti record richiede la scansione di tutti i record precedenti. Questo approccio può anche causare problemi di prestazioni quando il valore di offset è alto, in quanto potrebbe richiedere il recupero e l&#39;eliminazione di molti risultati.

#### Query GraphQL

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### Variabili di query

```json
{
  "first": 3
}
```

#### Risposta GraphQL

La risposta JSON risultante contiene le avventure più costose 2°, 3°, 4° e 5°. Le prime due avventure nei risultati hanno lo stesso prezzo (`4500` quindi [query elenco](#list-queries) specifica le avventure con lo stesso prezzo e quindi ordinate per titolo in ordine crescente.)

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### Successivo set di risultati impaginati

Il successivo set di risultati può essere recuperato utilizzando `after` e `endCursor` dalla query precedente. Se non ci sono più risultati da recuperare, `hasNextPage` è `false`.

##### Variabili di query

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## Esempi di reazione

Di seguito sono riportati alcuni esempi di React che mostrano come utilizzare [offset e limite](#offset-and-limit) e [impaginazione basata su cursore](#cursor-based-pagination) avvicinamenti. In genere il numero di risultati per richiesta è maggiore, tuttavia, ai fini di questi esempi, il limite è impostato su 5.

### Esempio di offset e limite

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

Utilizzando offset e limite, è possibile recuperare e visualizzare facilmente sottoinsiemi di risultati.

#### gancio useEffect

La `useEffect` gancio richiama una query persistente (`adventures-by-offset-and-limit`) che recupera un elenco di Avventure. La query utilizza la variabile `offset` e `limit` parametri per specificare il punto iniziale e il numero di risultati da recuperare. La `useEffect` viene richiamato quando `page` cambia valore.


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### Componente

Il componente utilizza il `useOffsetLimitAdventures` gancio per recuperare un elenco di Avventure. La `page` viene incrementato e decrementato per recuperare il set di risultati successivo e precedente. La `hasMore` viene utilizzato per determinare se il pulsante della pagina successiva deve essere abilitato.

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### Esempio impaginato

![Esempio impaginato](./assets/large-results/paginated-example.png)

_Ogni casella rossa rappresenta una query GraphQL HTTP dedicata impaginata._

Utilizzando l’impaginazione basata sul cursore, è possibile recuperare e visualizzare facilmente grandi set di risultati, raccogliendo in modo incrementale i risultati e concatenandoli ai risultati esistenti.


#### Gancio UseEffect

La `useEffect` gancio richiama una query persistente (`adventures-by-paginated`) che recupera un elenco di Avventure. La query utilizza la variabile `first` e `after` parametri per specificare il numero di risultati da recuperare e il cursore da cui iniziare. `fetchData` continua, raccogliendo il successivo set di risultati impaginati, fino a quando non vi sono più risultati da recuperare.

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### Componente

Il componente utilizza il `usePaginatedAdventures` gancio per recuperare un elenco di Avventure. La `queryCount` viene utilizzato per visualizzare il numero di richieste HTTP effettuate per recuperare l’elenco delle Avventure.

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
