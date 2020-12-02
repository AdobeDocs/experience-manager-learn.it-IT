---
title: Integrare un SPA | Guida introduttiva all'editor SPA AEM e React
description: Scoprite come il codice sorgente per un’applicazione a pagina singola (SPA) scritta in React può essere integrato con un progetto Adobe Experience Manager (AEM). Scoprite come utilizzare i moderni strumenti front-end, come un server di sviluppo webpack, per sviluppare rapidamente il SPA rispetto all'API del modello AEM JSON.
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


# Integrare un SPA {#integrate-spa}

Scoprite come il codice sorgente per un’applicazione a pagina singola (SPA) scritta in React può essere integrato con un progetto Adobe Experience Manager (AEM). Scoprite come utilizzare i moderni strumenti front-end, come un server di sviluppo webpack, per sviluppare rapidamente il SPA rispetto all&#39;API del modello AEM JSON.

## Obiettivo

1. Scoprite come il progetto SPA è integrato con AEM con librerie lato client.
2. Scoprite come utilizzare un server di sviluppo webpack per lo sviluppo front-end dedicato.
3. Esplorare l&#39;utilizzo di un file **proxy** e **mock** statico per lo sviluppo rispetto all&#39;API AEM modello JSON

## Cosa verrà creato

In questo capitolo verrà aggiunto un semplice componente `Header` al SPA. Nel processo di creazione di questo componente statico `Header` verranno utilizzati diversi approcci per AEM sviluppo SPA.

![Nuova intestazione in AEM](./assets/integrate-spa/final-header-component.png)

*Il SPA è esteso per aggiungere un  `Header` componente statico*

## Prerequisiti

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

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

   Se si utilizza [AEM 6.x](overview.md#compatibility) aggiungere il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) o estrarre il codice localmente passando al ramo `React/integrate-spa-solution`.

## Metodo di integrazione {#integration-approach}

Nel progetto AEM sono stati creati due moduli: `ui.apps` e `ui.frontend`.

Il modulo `ui.frontend` è un progetto [webpack](https://webpack.js.org/) che contiene tutto il codice sorgente SPA. La maggior parte dello sviluppo SPA e dei test sarà fatto nel progetto webpack. Quando viene attivata una build di produzione, la SPA viene creata e compilata utilizzando il webpack. Gli artifact compilati (CSS e Javascript) vengono copiati nel modulo `ui.apps` che viene quindi distribuito nel runtime AEM.

![architettura di alto livello ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Una rappresentazione di alto livello dell&#39;integrazione SPA.*

Ulteriori informazioni sulla build front-end sono disponibili [qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

##  Inspect l&#39;integrazione SPA {#inspect-spa-integration}

Quindi, ispezionare il modulo `ui.frontend` per comprendere il SPA generato automaticamente dal [AEM Project archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. Nell’IDE di vostra scelta, aprite il progetto AEM per il SPA WKND. Questa esercitazione utilizza il codice [Visual Studio Code IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM progetto SPA WKND](./assets/integrate-spa/vscode-ide-openproject.png)

2. Espandete ed esaminate la cartella `ui.frontend`. Aprire il file `ui.frontend/package.json`

3. Sotto la `dependencies` dovrebbero essere visualizzate diverse informazioni correlate a `react` incluso `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   `ui.frontend` è un&#39;applicazione React basata su [Create React App](https://create-react-app.dev/) o CRA per farla breve. La versione `react-scripts` indica quale versione di CRA viene utilizzata.

4. Sono inoltre presenti tre dipendenze con il prefisso `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   I moduli di cui sopra costituiscono l&#39; [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) e forniscono le funzionalità necessarie per la mappatura SPA Componenti su AEM Componenti.

5. Nel file `package.json` sono definiti diversi `scripts`:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Si tratta di script di build standard resi [disponibili](https://create-react-app.dev/docs/available-scripts) dall&#39;app Create React.

   L&#39;unica differenza è l&#39;aggiunta di `&& clientlib` allo script `build`. Questa istruzione aggiuntiva è responsabile della copia del SPA compilato nel modulo `ui.apps` come libreria lato client durante una build.

   Il modulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) è utilizzato per facilitare questo processo.

6.  Inspect il file `ui.frontend/clientlib.config.js`. Questo file di configurazione è utilizzato da [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) per determinare come generare la libreria client.

7.  Inspect il file `ui.frontend/pom.xml`. Questo file trasforma la cartella `ui.frontend` in un [modulo Paradiso](http://maven.apache.org/guides/mini/guide-multiple-modules.html). Il file `pom.xml` è stato aggiornato per utilizzare il [front-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) a **test** e **build** il SPA durante una build Maven.

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

   `index.js` è il punto di ingresso del SPA. `ModelManager` è fornito dall’SDK JS dell’editor SPA AEM. È responsabile della chiamata e dell&#39;invio di `pageModel` (il contenuto JSON) nell&#39;applicazione.

## Aggiunta di un componente Intestazione {#header-component}

Quindi, aggiungete un nuovo componente al SPA e distribuite le modifiche a un’istanza AEM locale.

1. Nel modulo `ui.frontend`, sotto `ui.frontend/src/components` create una nuova cartella denominata `Header`.
2. Create un file denominato `Header.js` sotto la cartella `Header`.

   ![Cartella e file di intestazione](assets/integrate-spa/header-folder-js.png)

3. Compilare `Header.js` con i seguenti elementi:

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
5. Effettuate i seguenti aggiornamenti a `App.js` per includere la `Header` statica:

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

6. Aprite un nuovo terminale e accedete alla cartella `ui.frontend`, quindi eseguite il comando `npm run build`:

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

7. Andate alla cartella `ui.apps`. Sotto `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` è possibile vedere i file SPA compilati copiati dalla cartella`ui.frontend/build`.

   ![Libreria client generata in ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

8. Tornate al terminale e individuate la cartella `ui.apps`. Eseguite il seguente comando Maven:

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

   Questo distribuirà il pacchetto `ui.apps` in un&#39;istanza locale in esecuzione di AEM.

9. Aprite una scheda del browser e andate a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). È ora necessario visualizzare il contenuto del componente `Header` nel SPA.

   ![Implementazione intestazione iniziale](./assets/integrate-spa/initial-header-implementation.png)

   I passaggi da 6 a 8 vengono eseguiti automaticamente quando si attiva una build Maven dalla radice del progetto (ovvero `mvn clean install -PautoInstallSinglePackage`). È ora necessario comprendere le basi dell&#39;integrazione tra le librerie lato SPA e AEM lato client. È comunque possibile modificare e aggiungere componenti `Text` in AEM sotto il componente statico `Header`.

## Webpack Dev Server - Proxy API JSON {#proxy-json}

Come mostrato negli esercizi precedenti, l&#39;esecuzione di una build e la sincronizzazione della libreria client con un&#39;istanza locale di AEM richiede alcuni minuti. Questo è accettabile per il test finale, ma non è ideale per la maggior parte dello sviluppo SPA.

Per sviluppare rapidamente il SPA è possibile utilizzare un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/). Il SPA è guidato da un modello JSON generato da AEM. In questo esercizio il contenuto JSON da un&#39;istanza in esecuzione di AEM sarà **proxy** nel server di sviluppo.

1. Tornare all&#39;IDE e aprire il file `ui.frontend/package.json`.

   Cercare una linea come la seguente:

   ```json
   "proxy": "http://localhost:4502",
   ```

   [Create React App](https://create-react-app.dev/docs/proxying-api-requests-in-development) fornisce un meccanismo semplice per le richieste API proxy. Tutte le richieste sconosciute verranno proxy tramite `localhost:4502`, il AEM rapido locale.

2. Aprite una finestra del terminale e passate alla cartella `ui.frontend`. Eseguire il comando `npm start`:

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

4. Tornate all&#39;IDE e create una nuova cartella denominata `media` in `ui.frontend/src/media`.
5. Scaricate e aggiungete il seguente logo WKND alla cartella `media`:

   ![Logo WKND](./assets/integrate-spa/wknd-logo-dk.png)

6. Aprite `Header.js` in `ui.frontend/src/components/Header/Header.js` e importate il logo:

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. Effettuate i seguenti aggiornamenti a `Header.js` per includere il logo nell&#39;intestazione:

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

   È possibile continuare a eseguire aggiornamenti dei contenuti in AEM e visualizzarli visualizzati in **webpack-dev-server**, dal momento che il contenuto viene proxy.

9. Arrestate il server di sviluppo webpack con `ctrl+c` nel terminale.

## Webpack Dev Server - Mock JSON API {#mock-json}

Un altro approccio al rapido sviluppo consiste nell&#39;utilizzare un file JSON statico per agire come modello JSON. &quot;prendendo in giro&quot; il JSON, rimuoviamo la dipendenza da un&#39;istanza AEM locale. Consente inoltre a uno sviluppatore front-end di aggiornare il modello JSON al fine di testare la funzionalità e di apportare modifiche all&#39;API JSON che sarebbe poi implementata da uno sviluppatore back-end.

L&#39;impostazione iniziale del JSON fittizio **richiede un&#39;istanza AEM locale**.

1. Nel browser, andate a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   È il JSON esportato da AEM che guida l&#39;applicazione. Copiate l’output JSON.

2. Tornate all&#39;IDE, andate a `ui.frontend/public` e aggiungete una nuova cartella denominata `mock-content`.
3. Crea un nuovo file denominato `mock.model.json` sotto a `ui.frontend/public/mock-content`. Incollate l&#39;output JSON da **Passo 1** qui.

   ![File Json modello Mock](./assets/integrate-spa/mock-model-json-created.png)

4. Aprire il file `index.html` in `ui.frontend/public/index.html`. Aggiornare la proprietà metadata per il modello di pagina AEM in modo che punti a una variabile `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   L&#39;utilizzo di una variabile per il valore di `cq:pagemodel_root_url` renderà più semplice passare dal modello proxy a quello json.

5. Apri il file `ui.frontend/.env.development` ed effettua i seguenti aggiornamenti per commentare il valore precedente per `REACT_APP_PAGE_MODEL_PATH`:

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

   Andate a http://localhost:3000/content/wknd-spa-react/us/en/home.html[ e visualizzate il SPA con lo stesso contenuto utilizzato nel file json ](http://localhost:3000/content/wknd-spa-react/us/en/home.html)proxy **.**

7. Apportate una piccola modifica al file `mock.model.json` creato in precedenza. Dovresti vedere il contenuto aggiornato immediatamente riflesso nel **webpack-dev-server**.

   ![aggiornamento json modello](./assets/integrate-spa/webpack-mock-model.gif)

La possibilità di manipolare il modello JSON e visualizzare gli effetti su un SPA live può aiutare uno sviluppatore a comprendere l&#39;API del modello JSON. Consente inoltre lo sviluppo front-end e back-end in parallelo.

Ora potete attivare o disattivare la posizione di utilizzo del contenuto JSON attivando le voci nel file `env.development`:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Aggiungi stili con ass

Una best practice React consiste nel mantenere ogni componente modulare e autonomo. Una raccomandazione generale consiste nell&#39;evitare di riutilizzare lo stesso nome di classe CSS tra i componenti, il che rende l&#39;uso dei preprocessori meno potente. Questo progetto utilizzerà [Sass](https://sass-lang.com/) per alcune funzioni utili come le variabili. Questo progetto seguirà inoltre le [convenzioni di denominazione CSS SUIT](https://github.com/suitcss/suit/blob/master/doc/components.md). SUIT è una variante di notazione BEM, Modificatore elemento blocco, utilizzata per creare regole CSS coerenti.

1. Aprire una finestra del terminale e arrestare il **webpack-dev-server** se avviato. Dall&#39;interno della cartella `ui.frontend` immettere il comando seguente per [installare Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   Installate `sass` come dipendenza peer:

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

4. Tornate all&#39;IDE e, sotto `ui.frontend/src`, create una nuova cartella denominata `styles`.
5. Create un nuovo file sotto `ui.frontend/src/styles` denominato `_variables.scss` e compilatelo con le seguenti variabili:

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

6. Rinominare l&#39;estensione del file `index.css` in `ui.frontend/src/index.css` in **`index.scss`**. Sostituite il contenuto con quanto segue:

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

7. Aggiornate `ui.frontend/src/index.js` per includere il `index.scss` rinominato:

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

9. Includere `Header.scss` aggiornando `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. Tornate al browser e al **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Intestazione con stile - webpack dev server](./assets/integrate-spa/styled-header.png)

   È ora possibile visualizzare gli stili aggiornati aggiunti al componente `Header`.

## Implementare SPA aggiornamenti a AEM

Le modifiche apportate alla `Header` sono attualmente visibili solo attraverso il **webpack-dev-server**. Distribuite il SPA aggiornato per AEM per visualizzare le modifiche.

1. Andate alla directory principale del progetto (`aem-guides-wknd-spa`) e distribuite il progetto da AEM utilizzando Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Andate a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Dovresti vedere la `Header` aggiornata con il logo e gli stili applicati.

   ![Intestazione aggiornata in AEM](./assets/integrate-spa/final-header-component.png)

   Ora che la SPA aggiornata è AEM, l’authoring può continuare.

## Risoluzione di errori nodo-sass

Nel corso dello sviluppo è possibile che si verifichi il seguente errore:

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

Ciò può verificarsi quando la versione locale di **Node.js** e **npm** è diversa da quella utilizzata dal [front-maven-plugin](https://github.com/eirslett/frontend-maven-plugin). L&#39;esecuzione del comando `npm rebuild node-sass` può risolvere temporaneamente il problema o rimuovere la cartella `ui.frontend/node_modules` e reinstallare.

Ci sono anche alcuni modi per affrontare questo problema in modo più permanente.

* Assicurarsi che la versione locale di npm e Node.js corrisponda alle versioni utilizzate dalla build [Maven](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* Aggiungete il seguente passaggio di esecuzione a `ui.frontend/pom.xml` prima del passaggio `npm run build`:

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

Congratulazioni, avete aggiornato il SPA ed esplorato l&#39;integrazione con AEM! Ora conoscete due approcci diversi per sviluppare il SPA rispetto all&#39;API del modello AEM JSON utilizzando un **webpack-dev-server**.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) o estrarre il codice localmente passando al ramo `React/integrate-spa-solution`.

### Passaggi successivi {#next-steps}

[Mappatura SPA componenti a AEM componenti](map-components.md)  - Scoprite come mappare i componenti React a componenti Adobe Experience Manager (AEM) con l’SDK JS AEM Editor. La mappatura dei componenti consente agli utenti di effettuare aggiornamenti dinamici ai componenti SPA nell’editor SPA AEM, in modo simile all’authoring AEM tradizionale.
