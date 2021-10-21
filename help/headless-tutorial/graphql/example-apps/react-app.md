---
title: React App - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). A React application is provided that demonstrates how to query content using the GraphQL APIs of AEM. The AEM Headless Client for JavaScript is used to execute the GraphQL queries that power the app.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 4%

---


# React App

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Viene fornita un’applicazione React che illustra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. Il client AEM Headless per JavaScript viene utilizzato per eseguire le query GraphQL che alimentano l&#39;app.

![React Application](./assets/react-screenshot.png)

È disponibile un’esercitazione dettagliata . [qui](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html).

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/it/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## AEM Requirements

The application is designed to connect to an AEM **Author** or **Publish** environment with the latest release of the [WKND Reference site](https://github.com/adobe/aem-guides-wknd/releases/latest) installed.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=it)

Consigliamo [distribuzione del sito WKND Reference in un ambiente di Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Impostazione locale tramite [l’SDK di AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) o [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) può essere utilizzato anche.

## Come utilizzare

1. Clona il `aem-guides-wknd-graphql` archivio:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifica le `aem-guides-wknd/react-app/.env.development` e assicurati che `REACT_APP_HOST_URI` punta alla tua istanza di AEM target. Aggiorna il metodo di autenticazione (se ti connetti a un&#39;istanza dell&#39;autore).

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. Apri un terminale ed esegui i comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. A new browser window should load on [http://localhost:3000](Http://localhost:3000)
1. Nell’applicazione deve essere visualizzato un elenco delle avventure del sito di riferimento WKND.

## Il codice

Di seguito è riportato un breve riepilogo dei file importanti e del codice utilizzati per alimentare l&#39;applicazione. Il codice completo è disponibile all&#39;indirizzo [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### Client AEM headless per JavaScript

La [Client headless AEM](https://github.com/adobe/aem-headless-client-js) viene utilizzato per eseguire la query GraphQL. Il client headless AEM fornisce due metodi per l&#39;esecuzione delle query, [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) e [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` esegue una query GraphQL standard per AEM contenuto ed è il tipo più comune di esecuzione di query.

[Query persistenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) sono una funzione in AEM che memorizza in cache i risultati di una query GraphQL e quindi rende disponibile il risultato in GET. Le query persistenti devono essere utilizzate per le query comuni che verranno eseguite più e più volte. In questa applicazione l&#39;elenco di Avventure è la prima query eseguita sulla schermata iniziale. Si tratta di una query molto popolare e pertanto è necessario utilizzare una query persistente. `runPersistedQuery` esegue una richiesta su un endpoint di query persistente.

`src/api/useGraphQL.js` è un [Aggancio a effetto reattivo](https://reactjs.org/docs/hooks-overview.html#effect-hook) che ascolta le modifiche al parametro `query` e `path`. If `query` is blank then a persisted query is used based on the `path`. Qui è dove viene costruito e utilizzato il client headless AEM per recuperare i dati.

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### Contenuto avventura

The app primarily displays a list of Adventures and gives the users an option to click into the details of an Adventure.

`Adventures.js` - Visualizza un elenco di schede delle Avventure.