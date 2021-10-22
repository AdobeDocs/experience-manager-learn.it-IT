---
title: React App - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Viene fornita un’applicazione React che illustra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. Il client AEM Headless per JavaScript viene utilizzato per eseguire le query GraphQL che alimentano l'app.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 2%

---


# Reagire all&#39;app

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Viene fornita un’applicazione React che illustra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. Il client AEM Headless per JavaScript viene utilizzato per eseguire le query GraphQL che alimentano l&#39;app.

Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

![React Application](./assets/react-screenshot.png)

È disponibile un’esercitazione dettagliata . [qui](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html).

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/it/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Requisiti AEM

L&#39;applicazione è progettata per connettersi a un AEM **Autore** o **Pubblica** ambiente con la versione più recente del [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) installato.

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
1. Viene caricata una nuova finestra del browser [http://localhost:3000](Http://localhost:3000)
1. Nell’applicazione deve essere visualizzato un elenco delle avventure del sito di riferimento WKND.

## Il codice

Di seguito è riportato un breve riepilogo dei file importanti e del codice utilizzati per alimentare l&#39;applicazione. Il codice completo è disponibile all&#39;indirizzo [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### Client AEM headless per JavaScript

La [Client headless AEM](https://github.com/adobe/aem-headless-client-js) viene utilizzato per eseguire la query GraphQL. Il client headless AEM fornisce due metodi per l&#39;esecuzione delle query, [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) e [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` esegue una query GraphQL standard per AEM contenuto ed è il tipo più comune di esecuzione di query.

[Query persistenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) sono una funzione in AEM che memorizza in cache i risultati di una query GraphQL e quindi rende disponibile il risultato in GET. Le query persistenti devono essere utilizzate per le query comuni che verranno eseguite più e più volte. In questa applicazione l&#39;elenco di Avventure è la prima query eseguita sulla schermata iniziale. Si tratta di una query molto popolare e pertanto è necessario utilizzare una query persistente. `runPersistedQuery` esegue una richiesta su un endpoint di query persistente.

`src/api/useGraphQL.js` è un [Aggancio a effetto reattivo](https://reactjs.org/docs/hooks-overview.html#effect-hook) che ascolta le modifiche al parametro `query` e `path`. Se `query` è vuoto, quindi viene utilizzata una query persistente in base al `path`. Qui è dove viene costruito e utilizzato il client headless AEM per recuperare i dati.

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

L&#39;app visualizza principalmente un elenco di Avventure e offre agli utenti la possibilità di fare clic nei dettagli di un Avventura.

`Adventures.js` - Visualizza un elenco di schede delle Avventure.  Lo stato iniziale utilizza [Query persistenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) che [preconfezionato](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) con il sito di riferimento WKND. L&#39;endpoint è `/wknd/adventures-all`. Esistono diversi pulsanti che consentono a un utente di sperimentare con risultati di filtraggio basati su un’attività:

```javascript
function filterQuery(activity) {
  return `
    {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: "${activity}"
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
        adventurePrimaryImage {
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
  `;
}
```

`AdventureDetail.js` - Visualizza una visualizzazione dettagliata dell&#39;avventura. Esegue una query GraphQL basata sul percorso dell&#39;avventura analizzato dall&#39;url:

```javascript
//parse the content fragment from the url
const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
...
function adventureDetailQuery(_path) {
  return `{
    adventureByPath (_path: "${_path}") {
      item {
        _path
          adventureTitle
          adventureActivity
          adventureType
          adventurePrice
          adventureTripLength
          adventureGroupSize
          adventureDifficulty
          adventurePrice
          adventurePrimaryImage {
            ... on ImageRef {
              _path
              mimeType
              width
              height
            }
          }
          adventureDescription {
            html
          }
          adventureItinerary {
            html
          }
      }
    }
  }
  `;
}
```

### Variabili di ambiente

Diversi [variabili di ambiente](https://create-react-app.dev/docs/adding-custom-environment-variables) vengono utilizzati da questo progetto per connettersi a un ambiente AEM. L’impostazione predefinita si connette a un ambiente di authoring AEM in esecuzione su http://localhost:4502. Se desideri modificare questo comportamento, aggiorna la sezione `.env.development` di conseguenza:

* `REACT_APP_HOST_URI=http://localhost:4502` - Imposta su AEM host di destinazione
* `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json` - Imposta il percorso dell&#39;endpoint GraphQL
* `REACT_APP_AUTH_METHOD=` - Il metodo di autenticazione preferito. Facoltativo, per impostazione predefinita non viene utilizzata alcuna autenticazione.
   * `service-token` - utilizzare lo scambio token di servizio per Cloud Env PROD
   * `dev-token` - utilizzare il token di sviluppo per lo sviluppo locale con Cloud Env
   * `basic` - utilizzare user/pass per lo sviluppo locale con Local Author Env
   * lasciare vuoto per non utilizzare alcun metodo di autenticazione
* `REACT_APP_AUTHORIZATION=admin:admin` - imposta le credenziali di autenticazione di base da utilizzare per la connessione a un ambiente AEM Author (solo per lo sviluppo). Se ci si connette a un ambiente di pubblicazione, questa impostazione non è necessaria.
* `REACT_APP_DEV_TOKEN` - Stringa del token di sviluppo. Per connettersi all’istanza remota, oltre all’autenticazione di base (user:pass) puoi utilizzare l’autenticazione Bearer con il token DEV dalla console Cloud
* `REACT_APP_SERVICE_TOKEN` - Percorso del file token di servizio. Per connettersi all’istanza remota, è possibile eseguire l’autenticazione anche con il token del servizio (file di download dalla console Cloud)

### Richieste API proxy

Quando si utilizza il server di sviluppo del webpack (`npm start`) il progetto si basa su un [configurazione proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) utilizzo `http-proxy-middleware`. Il file è configurato in [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) e si basa su diverse variabili di ambiente personalizzate impostate in `.env` e `.env.development`.

Se ci si connette a un ambiente di authoring AEM, è necessario configurare il metodo di autenticazione corrispondente.

### CORS - Condivisione risorse tra le origini

Questo progetto si basa su una configurazione CORS in esecuzione nell’ambiente AEM di destinazione e presuppone che l’app sia in esecuzione su http://localhost:3000 in modalità di sviluppo. La [Configurazione dei COR](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) fa parte del [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd).

![Configurazione CORS](assets/cross-origin-resource-sharing-configuration.png)

*Configurazione CORS di esempio per l’ambiente di authoring*