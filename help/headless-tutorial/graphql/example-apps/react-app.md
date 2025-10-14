---
title: App React - Esempio di AEM Headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione React illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
duration: 256
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 1%

---

# App React{#react-app}

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione React illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti. Il client AEM Headless per JavaScript viene utilizzato per eseguire le query persistenti GraphQL che alimentano l’app.

![App React con AEM Headless](./assets/react-app/react-app.png)

Visualizza il [codice sorgente in GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

È disponibile un [tutorial completo](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it) che descrive come è stata generata l&#39;app React.

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Node.js v18](https://nodejs.org/it/)
+ [Git](https://git-scm.com/)

## Requisiti di AEM

L’applicazione React funziona con le seguenti opzioni di distribuzione di AEM. Tutte le distribuzioni richiedono l&#39;installazione del sito [WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=it)
+ Configurazione locale con [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)
   + Richiede [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)

L&#39;applicazione React è progettata per connettersi a un ambiente __AEM Publish__, tuttavia può creare contenuto da AEM Author se l&#39;autenticazione viene fornita nella configurazione dell&#39;applicazione React.

## Come usare

1. Clona l&#39;archivio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifica il file `aem-guides-wknd-graphql/react-app/.env.development` e imposta `REACT_APP_HOST_URI` in modo che punti all&#39;AEM di destinazione.

   Se ti connetti a un’istanza Autore, aggiorna il metodo di autenticazione.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. Apri un terminale ed esegui i comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Una nuova finestra del browser deve essere caricata su [http://localhost:3000](http://localhost:3000)
1. Nell’applicazione deve essere visualizzato un elenco di avventure dal sito di riferimento WKND.

## Il codice

Di seguito è riportato un riepilogo di come viene creata l’applicazione React, di come si connette ad AEM Headless per recuperare contenuti utilizzando query persistenti GraphQL e di come vengono presentati tali dati. Il codice completo si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Query persistenti

Seguendo le best practice di AEM Headless, l’applicazione React utilizza query persistenti di AEM GraphQL per eseguire query sui dati di avventura. L’applicazione utilizza due query persistenti:

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

A ogni query persistente corrisponde un hook React [useEffect](https://reactjs.org/docs/hooks-effect.html) in `src/api/usePersistedQueries.js`, che chiama in modo asincrono l&#39;endpoint della query persistente AEM HTTP GET e restituisce i dati relativi all&#39;avventura.

Ogni funzione a sua volta richiama `aemHeadlessClient.runPersistedQuery(...)`, eseguendo la query GraphQL persistente.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
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
```

### Viste

L’applicazione React utilizza due visualizzazioni per presentare i dati dell’avventura nell’esperienza web.

+ `src/components/Adventures.js`

  Richiama `getAdventuresByActivity(..)` da `src/api/usePersistedQueries.js` e visualizza le avventure restituite in un elenco.

+ `src/components/AdventureDetail.js`

  Richiama `getAdventureBySlug(..)` utilizzando il parametro `slug` trasmesso tramite la selezione di avventura nel componente `Adventures` e visualizza i dettagli di una singola avventura.

### Variabili di ambiente

Diverse [variabili di ambiente](https://create-react-app.dev/docs/adding-custom-environment-variables) sono utilizzate per connettersi a un ambiente AEM. Il valore predefinito si connette ad AEM Publish in esecuzione alle `http://localhost:4503`. Aggiornare il file `.env.development` per modificare la connessione AEM:

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`: impostato sull&#39;host di destinazione AEM
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: impostare il percorso dell&#39;endpoint GraphQL. Questa app React non la utilizza, in quanto utilizza solo query persistenti.
+ `REACT_APP_AUTH_METHOD=`: metodo di autenticazione preferito. Facoltativo, per impostazione predefinita non viene utilizzata alcuna autenticazione.
   + `service-token`: usa le credenziali del servizio per ottenere un token di accesso su AEM as a Cloud Service
   + `dev-token`: usa il token di sviluppo per lo sviluppo locale su AEM as a Cloud Service
   + `basic`: utilizza utente/passaggio per lo sviluppo locale con AEM Author locale
   + Lascia vuoto questo campo per connetterti ad AEM senza autenticazione
+ `REACT_APP_AUTHORIZATION=admin:admin`: impostare le credenziali di autenticazione di base da utilizzare per la connessione a un ambiente AEM Author (solo per lo sviluppo). Se ci si connette a un ambiente di pubblicazione, questa impostazione non è necessaria.
+ `REACT_APP_DEV_TOKEN`: stringa token di sviluppo. Per connettersi all’istanza remota, oltre all’autenticazione di base (utente:passaggio) puoi utilizzare l’autenticazione Bearer con il token DEV dalla console Cloud
+ `REACT_APP_SERVICE_TOKEN`: percorso del file delle credenziali del servizio. Per connettersi all’istanza remota, l’autenticazione può essere eseguita anche con il token di servizio (scarica il file da Developer Console).

### Richieste proxy di AEM

Quando si utilizza il server di sviluppo Webpack (`npm start`), il progetto si basa su una [configurazione proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) che utilizza `http-proxy-middleware`. Il file è configurato in [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) e si basa su diverse variabili di ambiente personalizzate impostate in `.env` e `.env.development`.

Se ci si connette a un ambiente di authoring AEM, è necessario configurare il metodo di autenticazione [corrispondente](#environment-variables).

### Condivisione delle risorse tra le origini (CORS)

Questa applicazione React si basa su una configurazione CORS basata su AEM in esecuzione nell&#39;ambiente AEM di destinazione e presuppone che l&#39;app React venga eseguita su `http://localhost:3000` in modalità di sviluppo.  Consulta la[documentazione sulla distribuzione AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html?lang=it) per ulteriori informazioni su come configurare CORS.
