---
title: Bootstrap l’applicazione a pagina singola remota per l’editor di applicazioni a pagina singola
description: Scopri come avviare un’applicazione a pagina singola remota per garantire la compatibilità con AEM SPA Editor.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 2%

---

# Bootstrap l’applicazione a pagina singola remota per l’editor di applicazioni a pagina singola

{{spa-editor-deprecation}}

Prima di poter aggiungere le aree modificabili all’applicazione a pagina singola remota, è necessario avviarla con AEM SPA Editor JavaScript SDK e alcune altre configurazioni.

## Installare le dipendenze npm di AEM SPA Editor JS SDK

Innanzitutto, controlla le dipendenze npm SPA di AEM per il progetto React e installale.

* [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : fornisce l&#39;API per il recupero del contenuto da AEM.
* [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : fornisce l&#39;API che mappa il contenuto di AEM ai componenti SPA.
* [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) : fornisce un&#39;API per la creazione di componenti SPA personalizzati e implementazioni di uso comune, ad esempio il componente React `AEMPage`.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## Rivedere le variabili di ambiente delle applicazioni a pagina singola

È necessario esporre diverse variabili di ambiente all’applicazione a pagina singola remota in modo che sappia come interagire con AEM.

* Apri il progetto di applicazione a pagina singola remota in `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` nell&#39;IDE
* Apri il file in `.env.development`
* Nel file, presta particolare attenzione alle chiavi e, se necessario, aggiorna:

  ```
  REACT_APP_HOST_URI=http://localhost:4502
  
  REACT_APP_USE_PROXY=true
  
  REACT_APP_AUTH_METHOD=basic 
  
  REACT_APP_BASIC_AUTH_USER=admin
  REACT_APP_BASIC_AUTH_PASS=admin
  ```

  ![Variabili di ambiente di applicazioni a pagina singola remote](./assets/spa-bootstrap/env-variables.png)

  *Ricorda che le variabili di ambiente personalizzate in React devono avere il prefisso `REACT_APP_`.*

   * `REACT_APP_HOST_URI`: schema e host del servizio AEM a cui l&#39;applicazione a pagina singola remota si connette.
      * Questo valore cambia in base all’eventuale ambiente AEM (locale, di sviluppo, stage o produzione) e al tipo di servizio AEM (authoring o pubblicazione)
   * `REACT_APP_USE_PROXY`: questo evita i problemi CORS durante lo sviluppo indicando al server di sviluppo react di inoltrare le richieste di AEM come `/content, /graphql, .model.json` utilizzando il modulo `http-proxy-middleware`.
   * `REACT_APP_AUTH_METHOD`: metodo di autenticazione per le richieste servite da AEM. Le opzioni sono &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; o lascia vuoto per i casi di utilizzo senza autenticazione
      * Obbligatorio per l’utilizzo con AEM Author
      * Possibile utilizzo con AEM Publish (se il contenuto è protetto)
      * Lo sviluppo rispetto a AEM SDK supporta gli account locali tramite Autenticazione di base. Questo è il metodo utilizzato in questa esercitazione.
      * Durante l&#39;integrazione con AEM as a Cloud Service, utilizza [token di accesso](https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview)
   * `REACT_APP_BASIC_AUTH_USER`: AEM __username__ dall&#39;applicazione a pagina singola da autenticare durante il recupero del contenuto di AEM.
   * `REACT_APP_BASIC_AUTH_PASS`: l&#39;AEM __password__ dall&#39;applicazione a pagina singola da autenticare durante il recupero del contenuto di AEM.

## Integrare l’API ModelManager

Con le dipendenze npm dell&#39;applicazione a pagina singola AEM disponibili per l&#39;app, inizializza `ModelManager` di AEM nel `index.js` del progetto prima che venga richiamato `ReactDOM.render(...)`.

[ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) è responsabile della connessione ad AEM per il recupero del contenuto modificabile.

1. Apri il progetto di applicazione a pagina singola remota nell’IDE
1. Apri il file in `src/index.js`
1. Aggiungi l&#39;importazione `ModelManager` e inizializzala prima della chiamata `root.render(..)`,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

Il file `src/index.js` deve essere simile al seguente:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Configurare un proxy SPA interno

Durante la creazione di un&#39;applicazione a pagina singola modificabile, è consigliabile impostare un proxy interno [nell&#39;applicazione a pagina singola](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), configurato per instradare le richieste appropriate ad AEM. Questa operazione viene eseguita utilizzando il modulo [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm, già installato dall&#39;app GraphQL WKND di base.

1. Apri il progetto di applicazione a pagina singola remota nell’IDE
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

   Il file `setupProxy.spa-editor.auth.basic.js` deve essere simile al seguente:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Questa configurazione proxy esegue due operazioni principali:

   1. Invia richieste specifiche effettuate all&#39;applicazione a pagina singola (`http://localhost:3000`) ad AEM `http://localhost:4502`
      * Esegue il proxy solo delle richieste i cui percorsi corrispondono a pattern che indicano che devono essere servite da AEM, come definito in `toAEM(path, req)`.
      * Riscrive i percorsi SPA nelle pagine AEM corrispondenti, come definito in `pathRewriteToAEM(path, req)`
   1. Aggiunge intestazioni CORS a tutte le richieste per consentire l&#39;accesso al contenuto di AEM, come definito da `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      * Se non viene aggiunto, si verificano errori CORS durante il caricamento del contenuto AEM nell’applicazione a pagina singola.

1. Apri il file in `src/setupProxy.js`
1. Rivedere la riga che punta al file di configurazione proxy `setupProxy.spa-editor.auth.basic`:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Eventuali modifiche apportate a `src/setupProxy.js` o ai relativi file di riferimento richiedono il riavvio dell&#39;applicazione a pagina singola.

## Risorsa SPA statica

Le risorse statiche di applicazioni a pagina singola, come il logo WKND e la grafica Loading, devono avere gli URL src aggiornati per forzarne il caricamento dall’host dell’applicazione a pagina singola remota. Se relativo, quando l’applicazione a pagina singola viene caricata nell’editor di applicazioni a pagina singola per l’authoring, per impostazione predefinita questi URL utilizzano l’host di AEM anziché l’applicazione a pagina singola, generando 404 richieste come illustrato nell’immagine seguente.

![Risorse statiche interrotte](./assets/spa-bootstrap/broken-static-resource.png)

Per risolvere questo problema, fai in modo che una risorsa statica ospitata dall’applicazione a pagina singola remota utilizzi percorsi assoluti che includono l’origine dell’applicazione a pagina singola remota.

1. Apri il progetto SPA nell’IDE
1. Aprire il file delle variabili di ambiente dell&#39;applicazione a pagina singola `src/.env.development` e aggiungere una variabile per l&#39;URI pubblico dell&#39;applicazione a pagina singola:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Durante la distribuzione in AEM as a Cloud Service, è necessario fare lo stesso per i file `.env` corrispondenti._

1. Apri il file in `src/App.js`
1. Importare l’URI pubblico dell’applicazione a pagina singola dalle variabili di ambiente dell’applicazione a pagina singola

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Aggiungi al logo WKND `<img src=.../>` il prefisso `REACT_APP_PUBLIC_URI` per forzare la risoluzione rispetto all&#39;applicazione a pagina singola.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Eseguire la stessa operazione per il caricamento dell&#39;immagine in `src/components/Loading.js`

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

1. E per le __due istanze__ del pulsante Indietro in `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

I file `App.js`, `Loading.js` e `AdventureDetails.js` dovrebbero avere un aspetto simile a:

![Risorse statiche](./assets/spa-bootstrap/static-resources.png)

## Griglia reattiva AEM

Per supportare la modalità di layout dell’Editor SPA per le aree modificabili nell’SPA, è necessario integrare i CSS Griglia reattiva di AEM nell’SPA. Non preoccuparti: questo sistema di griglia è applicabile solo ai contenitori modificabili e puoi utilizzare il sistema di griglia desiderato per gestire il layout del resto dell’applicazione a pagina singola.

Aggiungi i file AEM Responsive Grid SCSS all’applicazione a pagina singola.

1. Apri il progetto SPA nell’IDE
1. Scaricare e copiare i due file seguenti in `src/styles`
   * [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      * Il generatore SCSS di AEM Responsive Grid
   * [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      * Richiama `_grid.scss` utilizzando i punti di interruzione specifici dell&#39;applicazione a pagina singola (desktop e mobile) e le colonne (12).
1. Apri `src/App.scss` e importa `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

I file `_grid.scss` e `_grid-init.scss` dovrebbero avere un aspetto simile a:

![SCSS griglia reattiva AEM](./assets/spa-bootstrap/aem-responsive-grid.png)

Ora l’applicazione a pagina singola include il CSS necessario per supportare la modalità di layout di AEM per i componenti aggiunti a un contenitore AEM.

## Classi di utilità

Copia le seguenti classi di utilità nel progetto dell’app React.

* [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
* [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) in `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
* [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
* [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![Classi di utilità di applicazioni a pagina singola remote](./assets/spa-bootstrap/utility-classes.png)

## Avviare l’applicazione a pagina singola

Ora che l’applicazione a pagina singola è stata avviata per l’integrazione con AEM, eseguiamola e vediamo come si presenta.

1. Dalla riga di comando, passa alla directory principale del progetto SPA
1. Avvia l’applicazione a pagina singola utilizzando i comandi normali (se non l’hai già fatto)

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. Sfoglia l&#39;applicazione a pagina singola in [http://localhost:3000](http://localhost:3000). Tutto dovrebbe avere un bell&#39;aspetto!

![SPA in esecuzione su http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Aprire l’applicazione a pagina singola nell’Editor SPA di AEM

Con l&#39;applicazione a pagina singola in esecuzione su [http://localhost:3000](http://localhost:3000), apriamola utilizzando l&#39;editor di applicazioni a pagina singola di AEM. Non è ancora possibile modificare nulla nell’applicazione a pagina singola; questa operazione convalida solo l’applicazione a pagina singola in AEM.

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND > us > en__
1. Seleziona la __home page dell&#39;app WKND__ e tocca __Modifica__ per visualizzare l&#39;applicazione a pagina singola.

   ![Modifica home page app WKND](./assets/spa-bootstrap/edit-home.png)

1. Passa a __Anteprima__ utilizzando il commutatore modalità in alto a destra
1. Fai clic intorno all’applicazione a pagina singola

   ![SPA in esecuzione su http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Congratulazioni.

Hai avviato l’applicazione a pagina singola remota per renderla compatibile con AEM SPA Editor. Ora sai come:

* Aggiungere le dipendenze npm JS SDK dell’Editor SPA di AEM al progetto SPA
* Configurare le variabili di ambiente dell’applicazione a pagina singola
* Integrare l’API ModelManager con l’applicazione a pagina singola
* Imposta un proxy interno per l’applicazione a pagina singola in modo che instradi le richieste di contenuto appropriate ad AEM
* Risolvere i problemi relativi alla risoluzione di risorse SPA statiche nel contesto dell’Editor SPA
* Aggiungi CSS griglia reattiva di AEM per supportare il layout nei contenitori modificabili di AEM

## Passaggi successivi

Ora che abbiamo raggiunto una linea di base di compatibilità con l’Editor SPA di AEM, possiamo iniziare a introdurre aree modificabili. Viene innanzitutto esaminato come inserire un [componente modificabile fisso](./spa-fixed-component.md) nell&#39;applicazione a pagina singola.
