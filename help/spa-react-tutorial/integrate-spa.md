---
title: Integrazione di un'area SPA | Guida introduttiva all'editor SPA AEM e React
description: Scoprite come il codice sorgente per un’applicazione SPA (Single Page Application) scritta in React può essere integrato con un progetto Adobe Experience Manager (AEM). Scoprite come utilizzare i moderni strumenti front-end, come un server di sviluppo webpack, per sviluppare rapidamente l'SPA rispetto all'API AEM modello JSON.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '2104'
ht-degree: 1%

---


# Integrazione di un&#39;area SPA {#integrate-spa}

Scoprite come il codice sorgente per un’applicazione SPA (Single Page Application) scritta in React può essere integrato con un progetto Adobe Experience Manager (AEM). Scoprite come utilizzare i moderni strumenti front-end, come un server di sviluppo webpack, per sviluppare rapidamente l&#39;SPA rispetto all&#39;API AEM modello JSON.

## Obiettivo

1. Scoprite come il progetto SPA è integrato con AEM con librerie lato client.
2. Scoprite come utilizzare un server di sviluppo webpack per lo sviluppo front-end dedicato.
3. Esplorare l&#39;utilizzo di un file **proxy** e **mock** statico per lo sviluppo rispetto all&#39;API AEM modello JSON

## Cosa verrà creato

In questo capitolo verrà aggiunto un semplice `Header` componente all&#39;area di protezione. Nel processo di costruzione di questo `Header` componente statico saranno utilizzati diversi approcci allo sviluppo AEM SPA.

![Nuova intestazione in AEM](./assets/integrate-spa/final-header-component.png)

*L’area SPA è estesa per aggiungere un`Header`componente statico*

## Prerequisiti

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

### Ottenere il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Distribuire la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzate [AEM 6.x](overview.md#compatibility) , aggiungete il `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) o estrarre il codice localmente passando al ramo `React/integrate-spa-solution`.

## Metodo di integrazione {#integration-approach}

Nel progetto AEM sono stati creati due moduli: `ui.apps` e `ui.frontend`.

Il `ui.frontend` modulo è un progetto [webpack](https://webpack.js.org/) che contiene tutto il codice sorgente SPA. La maggior parte delle attività di sviluppo e test SPA sarà realizzata nel progetto webpack. Quando viene attivata una build di produzione, l&#39;SPA viene creata e compilata utilizzando il webpack. Gli artifact compilati (CSS e Javascript) vengono copiati nel `ui.apps` modulo che viene quindi distribuito al runtime AEM.

![architettura di alto livello ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Una rappresentazione di alto livello dell&#39;integrazione SPA.*

Ulteriori informazioni sulla build Front-end sono [disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

##  l&#39;integrazione SPA di Inspect {#inspect-spa-integration}

Quindi, ispezionate il `ui.frontend` modulo per comprendere la SPA generata automaticamente dal [AEM archetipo](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)del progetto.

1. Nell’IDE di vostra scelta, aprite il progetto AEM per l’area SPA WKND. Questa esercitazione utilizzerà l&#39;IDE [di codice di](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)Visual Studio.

   ![VSCode - AEM progetto SPA WKND](./assets/integrate-spa/vscode-ide-openproject.png)

2. Espandete e ispezionate la `ui.frontend` cartella. Aprire il file `ui.frontend/package.json`

3. Sotto `dependencies` dovresti vedere diversi collegamenti `react` `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   Si `ui.frontend` tratta di un’applicazione React basata su [Create React App](https://create-react-app.dev/) o CRA (Crea appReact). La `react-scripts` versione indica quale versione di CRA viene utilizzata.

4. Sono inoltre presenti tre dipendenze con il prefisso `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   I moduli di cui sopra costituiscono l’SDK [JS di](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) AEM SPA Editor e forniscono le funzionalità necessarie per la mappatura dei componenti SPA su AEM componenti.

5. Nel `package.json` file sono `scripts` definiti diversi elementi:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Si tratta di script di build standard resi [disponibili](https://create-react-app.dev/docs/available-scripts) dall&#39;app Create React.

   L&#39;unica differenza è l&#39;aggiunta `&& clientlib` allo `build` script. Questa istruzione supplementare è responsabile della copia dell&#39;SPA compilata nel `ui.apps` modulo come libreria lato client durante una build.

   Il modulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) è utilizzato per facilitare questo.

6.  Inspect il file `ui.frontend/clientlib.config.js`. Questo file di configurazione viene utilizzato da [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) per determinare come generare la libreria client.

7.  Inspect il file `ui.frontend/pom.xml`. Questo file trasforma la `ui.frontend` cartella in un modulo [](http://maven.apache.org/guides/mini/guide-multiple-modules.html)Paradiso. Il `pom.xml` file è stato aggiornato per utilizzare il plugin [frontend-maven](https://github.com/eirslett/frontend-maven-plugin) per **testare** e **costruire** l&#39;SPA durante una build Maven.

8.  Inspect il file `index.js` in `ui.frontend/src/index.js`:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` è il punto di ingresso della SPA. `ModelManager` è fornito dall’SDK JS AEM SPA Editor. È responsabile della chiamata e dell&#39;inserimento del `pageModel` (il contenuto JSON) nell&#39;applicazione.

## Aggiunta di un componente Intestazione {#header-component}

Quindi, aggiungete un nuovo componente all’area di protezione e distribuite le modifiche a un’istanza AEM locale.

1. Nel `ui.frontend` modulo, sotto `ui.frontend/src/components` create una nuova cartella denominata `Header`.
2. Create un file denominato `Header.js` sotto la `Header` cartella.

   ![Cartella e file di intestazione](assets/integrate-spa/header-folder-js.png)

3. Compilate `Header.js` con le seguenti opzioni:

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   Sopra è presente un componente React standard che genera una stringa di testo statica.

4. Aprire il file `ui.frontend/src/App.js`. Questo è il punto di ingresso dell&#39;applicazione.
5. Effettuate i seguenti aggiornamenti `App.js` per includere l&#39;elemento statico `Header`:

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

6. Aprite un nuovo terminale, individuate la `ui.frontend` cartella ed eseguite il `npm run build` comando:

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

7. Passate alla `ui.apps` cartella. Sotto di `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` seguito dovresti vedere che i file SPA compilati sono stati copiati dalla`ui.frontend/build` cartella.

   ![Libreria client generata in ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

8. Tornate al terminale e individuate la `ui.apps` cartella. Eseguite il seguente comando Maven:

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   Questo distribuirà il `ui.apps` pacchetto in un&#39;istanza locale in esecuzione di AEM.

9. Aprite una scheda del browser e andate a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). È ora possibile visualizzare il contenuto del `Header` componente nell’area di protezione.

   ![Implementazione intestazione iniziale](./assets/integrate-spa/initial-header-implementation.png)

   I passaggi da 6 a 8 vengono eseguiti automaticamente quando si attiva una build Maven dalla radice del progetto (ovvero `mvn clean install -PautoInstallSinglePackage`). È ora necessario comprendere le basi dell&#39;integrazione tra SPA e AEM librerie lato client. È comunque possibile modificare e aggiungere `Text` componenti in AEM sotto il `Header` componente statico.

## Webpack Dev Server - Proxy API JSON {#proxy-json}

Come mostrato negli esercizi precedenti, l&#39;esecuzione di una build e la sincronizzazione della libreria client con un&#39;istanza locale di AEM richiede alcuni minuti. Questo è accettabile per il test finale, ma non è ideale per la maggior parte dello sviluppo SPA.

Un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) può essere utilizzato per sviluppare rapidamente l&#39;SPA. La SPA è guidata da un modello JSON generato da AEM. In questo esercizio il contenuto JSON da un&#39;istanza in esecuzione di AEM verrà **proxy** nel server di sviluppo.

1. Tornare all’IDE e aprire il file `ui.frontend/package.json`.

   Cercare una linea come la seguente:

   ```json
   "proxy": "http://localhost:4502",
   ```

   L&#39;app [Create React App](https://create-react-app.dev/docs/proxying-api-requests-in-development) fornisce un meccanismo semplice per le richieste API proxy. Tutte le richieste sconosciute verranno proxy attraverso `localhost:4502`, il AEM rapido locale.

2. Aprite una finestra del terminale e passate alla `ui.frontend` cartella. Eseguire il comando `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

3. Aprite una nuova scheda del browser (se non è già aperta) e andate a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Server di sviluppo webpack - json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   Dovresti visualizzare lo stesso contenuto come in AEM, ma senza che nessuna delle funzionalità di authoring sia abilitata.

   >[!NOTE]
   >
   > A causa dei requisiti di sicurezza di AEM, dovrete accedere all&#39;istanza AEM locale (http://localhost:4502) nello stesso browser, ma in una scheda diversa.

4. Tornate all’IDE e create una nuova cartella denominata `media` in `ui.frontend/src/media`.
5. Scaricate e aggiungete il seguente logo WKND alla `media` cartella:

   ![Logo WKND](./assets/integrate-spa/wknd-logo-dk.png)

6. Aprite `Header.js` in `ui.frontend/src/components/Header/Header.js` e importate il logo:

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. Per includere il logo `Header.js` nell’intestazione, effettuate le seguenti operazioni:

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   Salva le modifiche apportate a `Header.js`.

8. Tornate al browser all&#39;indirizzo [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Dovresti vedere immediatamente le modifiche all&#39;app riflesse.

   ![Logo aggiunto all’intestazione](./assets/integrate-spa/added-logo-localhost.png)

   È possibile continuare a effettuare aggiornamenti dei contenuti in AEM e visualizzarli visualizzati nel **webpack-dev-server**, dal momento che i contenuti vengono proxy.

9. Arrestate il server di sviluppo webpack con `ctrl+c` nel terminale.

## Webpack Dev Server - API Mock JSON {#mock-json}

Un altro approccio al rapido sviluppo consiste nell&#39;utilizzare un file JSON statico per agire come modello JSON. &quot;prendendo in giro&quot; il JSON, rimuoviamo la dipendenza da un&#39;istanza AEM locale. Consente inoltre a uno sviluppatore front-end di aggiornare il modello JSON al fine di testare la funzionalità e di apportare modifiche all&#39;API JSON che sarebbe poi implementata da uno sviluppatore back-end.

La configurazione iniziale del JSON fittizio non **richiede un&#39;istanza** AEM locale.

1. Nel browser, andate a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   È il JSON esportato da AEM che guida l&#39;applicazione. Copiate l’output JSON.

2. Tornate all&#39;IDE, individuate `ui.frontend/public` e aggiungete una nuova cartella denominata `mock-content`.
3. Crea un nuovo file denominato `mock.model.json` sotto a `ui.frontend/public/mock-content`. Incolla qui l&#39;output JSON dal **passaggio 1** .

   ![File Json modello Mock](./assets/integrate-spa/mock-model-json-created.png)

4. Open the file `index.html` at `ui.frontend/public/index.html`. Aggiornate la proprietà metadata per il modello di pagina AEM in modo che punti a una variabile `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   L&#39;utilizzo di una variabile per il valore del `cq:pagemodel_root_url` file renderà più semplice passare dal modello proxy a quello json.

5. Aprite il file `ui.frontend/.env.development` ed effettuate le seguenti operazioni per commentare il valore precedente per `REACT_APP_PAGE_MODEL_PATH`:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. Se è in esecuzione, arrestare il **webpack-dev-server**. Avviare il **webpack-dev-server** dal terminale:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Andate a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) e visualizzate l&#39;SPA con lo stesso contenuto utilizzato nel json **proxy** .

7. Apportate una piccola modifica al `mock.model.json` file creato in precedenza. Dovresti vedere immediatamente il contenuto aggiornato riflesso nel **webpack-dev-server**.

   ![aggiornamento json modello](./assets/integrate-spa/webpack-mock-model.gif)

La possibilità di manipolare il modello JSON e visualizzare gli effetti su un&#39;app SPA live può aiutare uno sviluppatore a comprendere l&#39;API del modello JSON. Consente inoltre lo sviluppo front-end e back-end in parallelo.

Ora potete scegliere dove utilizzare il contenuto JSON attivando o disattivando le voci nel `env.development` file:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Aggiungi stili con ass

Una best practice React consiste nel mantenere ogni componente modulare e autonomo. Una raccomandazione generale consiste nell&#39;evitare di riutilizzare lo stesso nome di classe CSS tra i componenti, il che rende l&#39;uso dei preprocessori meno potente. Questo progetto utilizzerà [Sass](https://sass-lang.com/) per alcune funzioni utili come le variabili. Il progetto seguirà inoltre le convenzioni [di denominazione CSS](https://github.com/suitcss/suit/blob/master/doc/components.md)SUIT. SUIT è una variante di notazione BEM, Modificatore elemento blocco, utilizzata per creare regole CSS coerenti.

1. Aprire una finestra terminale e arrestare il **webpack-dev-server** se avviato. Dall’interno della `ui.frontend` cartella immettete il comando seguente per [installare Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   Installa `sass` come dipendenza peer:

   ```shell
   $ npm install sass --save
   ```

2. Installate `normalize-scss` per normalizzare gli stili tra i vari browser:

   ```shell
   $ npm install normalize-scss
   ```

3. Avviare il **webpack-dev-server** in modo da visualizzare gli stili aggiornati in tempo reale:

   ```shell
   $ npm start
   ```

   Utilizzate l&#39;approccio Proxy of Mock per gestire l&#39;API del modello JSON.

4. Tornate all’IDE e `ui.frontend/src` create una nuova cartella denominata `styles`.
5. Create un nuovo file sotto `ui.frontend/src/styles` nome `_variables.scss` e compilatelo con le seguenti variabili:

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. Rinominare l’estensione del file `index.css` in corrispondenza `ui.frontend/src/index.css` di **`index.scss`**. Sostituite il contenuto con quanto segue:

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. Aggiorna `ui.frontend/src/index.js` per includere il nome `index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. Crea un nuovo file denominato `Header.scss` sotto a `ui.frontend/src/components/Header`. Compilate il file con le seguenti opzioni:

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. Includi `Header.scss` mediante aggiornamento `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. Tornate al browser e al **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Intestazione con stile - webpack dev server](./assets/integrate-spa/styled-header.png)

   A questo punto dovrebbero essere visualizzati gli stili aggiornati aggiunti al `Header` componente.

## Implementare gli aggiornamenti SPA per AEM

Le modifiche apportate al `Header` Web al momento sono visibili solo attraverso il **webpack-dev-server**. Distribuite l&#39;app SPA aggiornata per AEM vedere le modifiche.

1. Andate alla radice del progetto (`aem-guides-wknd-spa`) e distribuite il progetto da AEM utilizzando Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Andate a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Dovresti vedere gli aggiornamenti `Header` con logo e stili applicati.

   ![Intestazione aggiornata in AEM](./assets/integrate-spa/final-header-component.png)

   Ora che l’app SPA aggiornata è AEM, l’authoring può continuare.

## Risoluzione di errori nodo-sass

Nel corso dello sviluppo è possibile che si verifichi il seguente errore:

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

Ciò può verificarsi quando la versione locale di **Node.js** e **npm** è diversa da quella utilizzata dal [front-maven-plugin](https://github.com/eirslett/frontend-maven-plugin). L&#39;esecuzione del comando `npm rebuild node-sass` può risolvere temporaneamente il problema, rimuovere la `ui.frontend/node_modules` cartella e reinstallare.

Ci sono anche alcuni modi per affrontare questo problema in modo più permanente.

* Verificate che la versione locale di npm e Node.js corrisponda alle versioni utilizzate dalla build [Maven](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* Aggiungete il seguente passaggio di esecuzione `ui.frontend/pom.xml` prima del `npm run build` passaggio:

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## Congratulazioni! {#congratulations}

Congratulazioni, avete aggiornato l&#39;SPA ed esplorato l&#39;integrazione con AEM! Ora conoscete due approcci diversi per lo sviluppo dell&#39;SPA rispetto all&#39;API modello JSON AEM utilizzando un **webpack-dev-server**.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) o estrarre il codice localmente passando al ramo `React/integrate-spa-solution`.

### Passaggi successivi {#next-steps}

[Mappatura dei componenti SPA su componenti](map-components.md) AEM - Scoprite come mappare i componenti React su componenti Adobe Experience Manager (AEM) con l’SDK JS AEM SPA Editor. La mappatura dei componenti consente agli utenti di effettuare aggiornamenti dinamici ai componenti SPA nell’editor AEM SPA, in modo simile all’authoring AEM tradizionale.
