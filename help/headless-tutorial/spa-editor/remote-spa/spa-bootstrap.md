---
title: Bootstrap l’SPA remoto per l’editor SPA
description: Scopri come avviare un SPA remoto per garantire la compatibilità con l’Editor SPA dell’AEM.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 478
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# Bootstrap l’SPA remoto per l’editor SPA

Prima di poter aggiungere le aree modificabili all’SPA remoto, è necessario avviarle con l’SDK JavaScript dell’editor SPA dell’AEM e con alcune altre configurazioni.

## Installare le dipendenze npm dell’SDK JS per l’editor SPA AEM

In primo luogo, esaminare AEM SPA dipendenze npm per il progetto React e installarle.

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : fornisce l’API per recuperare i contenuti dall’AEM.
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : fornisce l’API che mappa il contenuto dell’AEM ai componenti SPA.
+ [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) : fornisce un’API per la creazione di componenti SPA personalizzati e fornisce implementazioni comuni, come `AEMPage` Componente React.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## Rivedere le variabili di ambiente dell’SPA

Diverse variabili di ambiente devono essere esposte all’SPA remoto in modo che sappia come interagire con l’AEM.

1. Apri progetto SPA remoto all’indirizzo `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` nell’IDE
1. Apri il file `.env.development`
1. Nel file, presta particolare attenzione alle chiavi e, se necessario, aggiorna:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![Variabili di ambiente SPA remote](./assets/spa-bootstrap/env-variables.png)

   *Ricorda che le variabili di ambiente personalizzate in React devono avere il prefisso `REACT_APP_`.*

   + `REACT_APP_HOST_URI`: lo schema e l’host del servizio AEM al quale l’SPA remoto si connette.
      + Questo valore cambia in base all’ambiente AEM (locale, di sviluppo, stage o produzione) e al tipo di servizio AEM (authoring vs. pubblicazione)
   + `REACT_APP_USE_PROXY`: questo evita i problemi CORS durante lo sviluppo dicendo al server di sviluppo react di proxy delle richieste AEM come `/content, /graphql, .model.json` utilizzo `http-proxy-middleware` modulo.
   + `REACT_APP_AUTH_METHOD`: metodo di autenticazione per le richieste AEM servite, le opzioni sono &quot;service-token&quot;, &quot;dev-token&quot;, &quot;basic&quot; o lascia vuoto per i casi di utilizzo senza autenticazione
      + Necessario per l’utilizzo con AEM Author
      + Possibile utilizzo con Pubblicazione AEM (se il contenuto è protetto)
      + Lo sviluppo rispetto all’SDK dell’AEM supporta gli account locali tramite Autenticazione di base. Questo è il metodo utilizzato in questa esercitazione.
      + Durante l’integrazione con AEM as a Cloud Service, utilizzare [token di accesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`: AEM __nome utente__ dall’SPA per l’autenticazione durante il recupero del contenuto AEM.
   + `REACT_APP_BASIC_AUTH_PASS`: AEM __password__ dall’SPA per l’autenticazione durante il recupero del contenuto AEM.

## Integrare l’API ModelManager

Con le dipendenze npm dell’SPA dell’AEM disponibili per l’app, inizializza AEM `ModelManager` nel file del progetto `index.js` prima di `ReactDOM.render(...)` viene richiamato.

Il [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) è responsabile della connessione all’AEM per recuperare contenuti modificabili.

1. Aprire il progetto SPA remoto nell&#39;IDE
1. Apri il file `src/index.js`
1. Aggiungi importazione `ModelManager` e inizializzarlo prima del `root.render(..)` invocazione,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

Il `src/index.js` il file dovrebbe avere un aspetto simile a:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Configurare un proxy SPA interno

Quando si crea un SPA modificabile, è meglio impostare un’ [delega interna all&#39;SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), configurato per instradare le richieste appropriate all’AEM. Questa operazione viene eseguita utilizzando [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) modulo npm, già installato dall’app GraphQL WKND di base.

1. Aprire il progetto SPA remoto nell&#39;IDE
1. Apri il file in `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Aggiorna il file con il seguente codice:

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') || 
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   Il `setupProxy.spa-editor.auth.basic.js` il file dovrebbe avere un aspetto simile a:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Questa configurazione proxy esegue due operazioni principali:

   1. Invia richieste specifiche per approssimazione all&#39;SPA (`http://localhost:3000`) all&#39;AEM `http://localhost:4502`
      + Vengono proxy solo le richieste i cui percorsi corrispondono a pattern che indicano che devono essere servite dall’AEM, come definito in `toAEM(path, req)`.
      + Riscrive i percorsi dell’SPA sulle relative pagine AEM, come definito in `pathRewriteToAEM(path, req)`
   1. Aggiunge intestazioni CORS a tutte le richieste per consentire l’accesso al contenuto AEM, come definito da `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + Se non viene aggiunto, si verificano errori CORS durante il caricamento del contenuto AEM nell’SPA.

1. Apri il file `src/setupProxy.js`
1. Rivedi la riga che punta al `setupProxy.spa-editor.auth.basic` file di configurazione proxy:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Eventuali modifiche apportate al `src/setupProxy.js` oppure i file a cui si fa riferimento richiedono il riavvio dell&#39;SPA.

## Risorsa SPA statica

Le risorse SPA statiche, come il logo WKND e la grafica Loading, devono avere gli URL src aggiornati per forzarne il caricamento dall’host SPA remoto. Se relativo, quando l’SPA viene caricato nell’editor SPA per la creazione di, per impostazione predefinita questi URL utilizzano l’host AEM anziché l’SPA, generando 404 richieste come illustrato nell’immagine seguente.

![Risorse statiche interrotte](./assets/spa-bootstrap/broken-static-resource.png)

Per risolvere questo problema, crea una risorsa statica ospitata dall’SPA remoto utilizzando percorsi assoluti che includano l’origine SPA remota.

1. Aprire il progetto SPA nell’IDE
1. Aprire il file delle variabili di ambiente SPA `src/.env.development` e aggiungi una variabile per l’URI pubblico dell’SPA:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Quando si distribuisce a AEM as a Cloud Service, è necessario che lo stesso avvenga per il `.env` file._

1. Apri il file `src/App.js`
1. Importare l’URI pubblico dell’SPA dalle variabili di ambiente SPA

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Aggiungi il prefisso al logo WKND `<img src=.../>` con `REACT_APP_PUBLIC_URI` imporre la risoluzione delle crisi contro l&#39;SPA.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Effettua la stessa operazione per caricare l’immagine in `src/components/Loading.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. E per __due istanze__ del pulsante indietro in `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

Il `App.js`, `Loading.js`, e `AdventureDetails.js` i file dovrebbero avere un aspetto simile a:

![Risorse statiche](./assets/spa-bootstrap/static-resources.png)

## Griglia reattiva AEM

Per supportare la modalità di layout dell’Editor SPA per le aree modificabili nell’SPA, è necessario integrare i CSS della griglia reattiva dell’AEM nell’SPA. Non preoccuparti: questo sistema di griglia è applicabile solo ai contenitori modificabili e puoi utilizzare il sistema di griglia desiderato per gestire il layout del resto dell’SPA.

Aggiungere all’SPA i file SCSS della griglia reattiva dell’AEM.

1. Aprire il progetto SPA nell’IDE
1. Scarica e copia i due file seguenti in `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + Il generatore di SCSS della griglia reattiva dell&#39;AEM
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Richiama `_grid.scss` utilizzando le colonne (12) e i punti di interruzione specifici dell’SPA (desktop e mobile).
1. Apri `src/App.scss` e importa `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

Il `_grid.scss` e `_grid-init.scss` i file dovrebbero avere un aspetto simile a:

![SCSS della griglia reattiva dell’AEM](./assets/spa-bootstrap/aem-responsive-grid.png)

Ora l’SPA include il CSS necessario per supportare la modalità di layout AEM per i componenti aggiunti a un contenitore AEM.

## Classi di utilità

Copia le seguenti classi di utilità nel progetto dell’app React.

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![Classi di utilità SPA remote](./assets/spa-bootstrap/utility-classes.png)

## Avviare l’SPA

Ora che l&#39;SPA è stato avviato per l&#39;integrazione con l&#39;AEM, facciamo partire l&#39;SPA e vediamo come si presenta!

1. Dalla riga di comando, passa alla directory principale del progetto SPA
1. Avviare l’SPA con i comandi normali (se non lo si è già fatto)

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. Sfoglia SPA su [http://localhost:3000](http://localhost:3000). Tutto dovrebbe avere un bell&#39;aspetto!

![SPA in esecuzione su http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Aprire l’SPA nell’Editor SPA dell’AEM

Con l&#39;SPA in funzione [http://localhost:3000](http://localhost:3000), apriamolo utilizzando l’Editor SPA dell’AEM. Non è ancora possibile modificare nulla nell’SPA, il che convalida solo l’SPA nell’AEM.

1. Accedi a AEM Author
1. Accedi a __Sites > App WKND > us > it__
1. Seleziona la __Home page dell’app WKND__ e tocca __Modifica__ e viene visualizzato l&#39;SPA.

   ![Modifica home page dell’app WKND](./assets/spa-bootstrap/edit-home.png)

1. Passa a __Anteprima__ utilizzo del commutatore modalità in alto a destra
1. Fai clic intorno all’SPA

   ![SPA in esecuzione su http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Congratulazioni.

Hai avviato l’SPA remoto affinché sia compatibile con l’Editor SPA dell’AEM. Ora sai come:

+ Aggiungere al progetto SPA le dipendenze npm dell’SDK JS per l’editor SPA dell’AEM
+ Configurare le variabili di ambiente SPA
+ Integrare l’API ModelManager con l’SPA
+ Imposta un proxy interno per l’SPA in modo che instradi le richieste di contenuti appropriate all’AEM
+ Risolvere i problemi relativi alla risoluzione di risorse SPA statiche nel contesto dell’Editor SPA
+ Aggiungere CSS griglia reattiva AEM per supportare il layout nei contenitori modificabili AEM

## Passaggi successivi

Ora che abbiamo raggiunto una linea di base di compatibilità con l&#39;AEM SPA Editor, possiamo iniziare a introdurre aree modificabili. Dapprima esaminiamo come posizionare un [componente modificabile fisso](./spa-fixed-component.md) dell&#39;SPA.
