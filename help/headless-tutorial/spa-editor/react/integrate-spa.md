---
title: Integrare un SPA | Guida introduttiva all'editor di SPA AEM e React
description: Scopri come il codice sorgente per un’applicazione a pagina singola (SPA) scritta in React può essere integrato con un progetto Adobe Experience Manager (AEM). Scopri come utilizzare strumenti front-end moderni, come un server di sviluppo webpack, per sviluppare rapidamente il SPA rispetto all’API del modello JSON AEM.
sub-product: sites
feature: SPA Editor
version: Cloud Service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 1%

---

# Integrare il SPA {#developer-workflow}

Scopri come il codice sorgente per un’applicazione a pagina singola (SPA) scritta in React può essere integrato con un progetto Adobe Experience Manager (AEM). Scopri come utilizzare strumenti front-end moderni, come un server di sviluppo webpack, per sviluppare rapidamente il SPA rispetto all’API del modello JSON AEM.

## Obiettivo

1. Scopri come il progetto SPA è integrato con AEM con librerie lato client.
2. Scopri come utilizzare un server di sviluppo webpack per lo sviluppo front-end dedicato.
3. Esplorare l&#39;utilizzo di un **proxy** e statici **deriso** file da sviluppare con l’API del modello JSON AEM.

## Cosa verrà creato

In questo capitolo si apporteranno diverse piccole modifiche al SPA per capire come è integrato con AEM.
Questo capitolo aggiunge un semplice `Header` al SPA. Nel processo di costruzione di questo **statico** `Header` componenti vengono utilizzati diversi approcci allo sviluppo SPA AEM.

![Nuova intestazione in AEM](./assets/integrate-spa/final-header-component.png)

*L’SPA viene estesa per aggiungere un `Header` component*

## Prerequisiti

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment). Il presente capitolo costituisce la continuazione del [Crea progetto](create-project.md) capitolo, tuttavia per seguire tutto ciò che serve è un progetto AEM funzionante SPA abilitato.

## Metodo di integrazione {#integration-approach}

Nell’ambito del progetto AEM sono stati creati due moduli: `ui.apps` e `ui.frontend`.

La `ui.frontend` un modulo [webpack](https://webpack.js.org/) progetto che contiene tutto il codice sorgente SPA. La maggior parte dello sviluppo e dei test SPA viene effettuata nel progetto webpack. Quando viene attivata una build di produzione, il SPA viene generato e compilato utilizzando il webpack. Gli artefatti compilati (CSS e Javascript) vengono copiati nel `ui.apps` che viene quindi distribuito al runtime AEM.

![architettura di alto livello ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Una descrizione di alto livello dell’integrazione SPA.*

Ulteriori informazioni sulla build front-end possono essere [qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Integrazione di Inspect SPA {#inspect-spa-integration}

Quindi, controlla il `ui.frontend` per comprendere il SPA generato automaticamente dal [Archetipo AEM progetto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. Nell’IDE che preferisci, apri il progetto AEM. Questa esercitazione utilizzerà il [IDE codice di Visual Studio](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM progetto di SPA WKND](./assets/integrate-spa/vscode-ide-openproject.png)

1. Espandi ed esamina le `ui.frontend` cartella. Aprire il file `ui.frontend/package.json`

1. Sotto la `dependencies` dovresti vedere diversi `react` compreso `react-scripts`

   La `ui.frontend` è un&#39;applicazione React basata su [Creare un&#39;app reattiva](https://create-react-app.dev/) o CRA in breve. La `react-scripts` version indica quale versione di CRA viene utilizzata.

1. Ci sono anche diverse dipendenze con prefisso `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   I moduli di cui sopra costituiscono il [AEM Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) e fornisce le funzionalità necessarie per mappare SPA Componenti su Componenti AEM.

   Sono incluse anche le [AEM componenti WCM - Implementazione di React Core](https://github.com/adobe/aem-react-core-wcm-components-base) e [AEM componenti WCM - Editor Spa - Implementazione react Core](https://github.com/adobe/aem-react-core-wcm-components-spa). Si tratta di un set di componenti dell’interfaccia utente riutilizzabili che vengono mappati su componenti AEM predefiniti. Sono progettati per essere utilizzati così come sono e formattati per soddisfare le esigenze del progetto.

1. In `package.json` file ce ne sono diversi `scripts` definito:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Questi sono script di build standard creati [disponibile](https://create-react-app.dev/docs/available-scripts) tramite Crea app per reazione.

   L&#39;unica differenza è l&#39;aggiunta di `&& clientlib` al `build` script. Questa istruzione aggiuntiva è responsabile della copia del SPA compilato nel `ui.apps` come libreria lato client durante una build.

   Modulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) è utilizzato per facilitare questa fase.

1. Inspect il file `ui.frontend/clientlib.config.js`. Questo file di configurazione viene utilizzato da [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) per determinare come generare la libreria client.

1. Inspect il file `ui.frontend/pom.xml`. Questo file trasforma il `ui.frontend` in una cartella [Modulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). La `pom.xml` è stato aggiornato per utilizzare il [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) a **test** e **build** il SPA durante una build Maven.

1. Inspect il file `index.js` a `ui.frontend/src/index.js`:

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

   `index.js` è il punto di ingresso del SPA. `ModelManager` è fornito dall’SDK JS AEM Editor SPA. È responsabile della chiamata e dell&#39;inserimento del `pageModel` (il contenuto JSON) nell’applicazione.

1. Inspect il file `import-components.js` a `ui.frontend/src/components/import-components.js`. Questo file importa il file pronto all&#39;uso **Reazione dei componenti core** e li rende disponibili al progetto. Nel capitolo successivo verrà esaminata la mappatura del contenuto AEM ai componenti SPA.

## Aggiungere un componente SPA statico {#static-spa-component}

Quindi, aggiungi un nuovo componente al SPA e distribuisci le modifiche a un&#39;istanza AEM locale. Questo è un semplice cambiamento, solo per illustrare come viene aggiornato il SPA.

1. In `ui.frontend` modulo, sotto `ui.frontend/src/components` crea una nuova cartella denominata `Header`.
1. Crea un file denominato `Header.js` sotto il `Header` cartella.

   ![Cartella e file di intestazione](assets/create-project/header-folder-js.png)

1. Popolare `Header.js` con le seguenti caratteristiche:

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

   Sopra c&#39;è un componente React standard che genera una stringa di testo statica.

1. Aprire il file `ui.frontend/src/App.js`. Questo è il punto di ingresso dell&#39;applicazione.
1. Effettua i seguenti aggiornamenti a `App.js` per includere l&#39;elemento statico `Header`:

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

1. Apri un nuovo terminale e naviga nel `ui.frontend` ed esegui la `npm run build` comando:

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

1. Passa a `ui.apps` cartella. Sotto `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` dovresti vedere che i file compilati SPA sono stati copiati dal`ui.frontend/build` cartella.

   ![Libreria client generata in ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

1. Torna al terminale e naviga nel `ui.apps` cartella. Esegui il seguente comando Maven:

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

   Verrà distribuito il `ui.apps` a un&#39;istanza locale in esecuzione di AEM.

1. Apri una scheda del browser e passa a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Ora dovresti visualizzare il contenuto della `Header` componente visualizzato nella SPA.

   ![Implementazione dell’intestazione iniziale](./assets/integrate-spa/initial-header-implementation.png)

   I passaggi precedenti vengono eseguiti automaticamente quando si attiva una build Maven dalla directory principale del progetto (ovvero `mvn clean install -PautoInstallSinglePackage`). È ora necessario comprendere le nozioni di base dell’integrazione tra le librerie lato client SPA e AEM. È comunque possibile modificare e aggiungere `Text` componenti in AEM sotto l’elemento statico `Header` componente.

## Webpack Dev Server - Proxy dell&#39;API JSON {#proxy-json}

Come visto negli esercizi precedenti, l’esecuzione di una build e la sincronizzazione della libreria client con un’istanza locale di AEM richiede alcuni minuti. Ciò è accettabile per i test finali, ma non è ideale per la maggior parte dello sviluppo SPA.

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) può essere utilizzato per sviluppare rapidamente il SPA. Il SPA è guidato da un modello JSON generato da AEM. In questo esercizio il contenuto JSON da un’istanza in esecuzione di AEM è **proxy** nel server di sviluppo.

1. Torna all’IDE e apri il file . `ui.frontend/package.json`.

   Cerca una riga come la seguente:

   ```json
   "proxy": "http://localhost:4502",
   ```

   La [Creare un&#39;app reattiva](https://create-react-app.dev/docs/proxying-api-requests-in-development) fornisce un meccanismo semplice per le richieste API proxy. Tutte le richieste sconosciute vengono trasmesse tramite proxy `localhost:4502`, l&#39;avvio rapido AEM locale.

1. Apri una finestra terminale e passa alla `ui.frontend` cartella. Esegui il comando `npm start`:

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

1. Apri una nuova scheda del browser (se non è già aperta) e passa a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Server di sviluppo del webpack - json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   Dovresti visualizzare lo stesso contenuto di AEM, ma senza che sia abilitata alcuna delle funzionalità di authoring.

   >[!NOTE]
   >
   > A causa dei requisiti di sicurezza di AEM, dovrai aver effettuato l’accesso all’istanza AEM locale (http://localhost:4502) nello stesso browser ma in una scheda diversa.

1. Torna all’IDE e crea un file denominato `Header.css` in `src/components/Header` cartella.
1. Popolare `Header.css` con le seguenti caratteristiche:

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![IDE VSCode](assets/integrate-spa/header-css-update.png)

1. Riaprire `Header.js` e aggiungi la seguente riga al riferimento `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   Salva le modifiche.

1. Passa a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) per visualizzare automaticamente le modifiche allo stile applicate.

1. Apri il file . `Page.css` a `ui.frontend/src/components/Page`. Apporta le seguenti modifiche per correggere la spaziatura:

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. Torna al browser in [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Dovresti vedere immediatamente le modifiche apportate all’app.

   ![Stile aggiunto all’intestazione](assets/integrate-spa/added-logo-localhost.png)

   Puoi continuare a eseguire aggiornamenti dei contenuti in AEM e visualizzarli riflessi in **webpack-dev-server**, dal momento che il contenuto viene proxy.

1. Arrestare il server di sviluppo del webpack con `ctrl+c` nel terminale.

## Distribuire aggiornamenti SPA a AEM

Le modifiche apportate al `Header` al momento sono visibili solo attraverso **webpack-dev-server**. Distribuisci il SPA aggiornato per AEM per visualizzare le modifiche.

1. Passa alla directory principale del progetto (`aem-guides-wknd-spa`) e implementa il progetto per AEM utilizzando Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Passa a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Dovresti visualizzare l&#39;aggiornamento `Header` e gli stili applicati.

   ![Intestazione aggiornata in AEM](assets/integrate-spa/final-header-component.png)

   Ora che il SPA aggiornato è in AEM, l’authoring può continuare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai aggiornato il SPA ed esplorato l&#39;integrazione con AEM! Sai come sviluppare il SPA rispetto all’API del modello AEM JSON utilizzando un **webpack-dev-server**.

### Passaggi successivi {#next-steps}

[Mappatura di componenti SPA per AEM componenti](map-components.md) - Scopri come mappare i componenti React ai componenti di Adobe Experience Manager (AEM) con l’SDK JS dell’editor di AEM SPA. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA all’interno dell’editor di SPA AEM, in modo simile all’authoring tradizionale AEM.

## (Bonus) Webpack Dev Server - Mock JSON API {#mock-json}

Un altro approccio per un rapido sviluppo è quello di utilizzare un file JSON statico per agire come modello JSON. &quot;prendendo in giro&quot; il JSON, rimuoviamo la dipendenza da un&#39;istanza AEM locale. Consente inoltre a uno sviluppatore front-end di aggiornare il modello JSON per testare la funzionalità e apportare modifiche all’API JSON che verranno implementate in seguito da uno sviluppatore back-end.

La configurazione iniziale del modello JSON **richiedere un&#39;istanza AEM locale**.

1. Torna all’IDE e passa a `ui.frontend/public` e aggiungi una nuova cartella denominata `mock-content`.
1. Crea un nuovo file denominato `mock.model.json` sotto a `ui.frontend/public/mock-content`.
1. Nel browser passa a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Si tratta del JSON esportato da AEM che guida l’applicazione. Copia l’output JSON.

1. Incolla l’output JSON del passaggio precedente nel file . `mock.model.json`.

   ![File Json modello maschera](./assets/integrate-spa/mock-model-json-created.png)

1. Apri il file . `index.html` a `ui.frontend/public/index.html`. Aggiorna la proprietà metadati per il modello di pagina AEM in modo che punti a una variabile `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Utilizzo di una variabile per il valore della variabile `cq:pagemodel_root_url` renderà più facile passare dal modello proxy a quello json.

1. Apri il file . `ui.frontend/.env.development` e apporta i seguenti aggiornamenti per commentare il valore precedente per `REACT_APP_PAGE_MODEL_PATH` e `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. Se è attualmente in esecuzione, interrompi la **webpack-dev-server**. Avvia la **webpack-dev-server** dal terminale:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Passa a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) e dovresti visualizzare il SPA con lo stesso contenuto utilizzato nel **proxy** json.

1. Effettuare una piccola modifica al `mock.model.json` file creato in precedenza. Dovresti visualizzare il contenuto aggiornato immediatamente riflesso nel **webpack-dev-server**.

   ![aggiornamento json del modello fittizio](./assets/integrate-spa/webpack-mock-model.gif)

La possibilità di manipolare il modello JSON e visualizzare gli effetti su un SPA live può aiutare uno sviluppatore a comprendere l’API del modello JSON. Consente inoltre lo sviluppo front-end e back-end in parallelo.

Ora puoi alternare il punto in cui utilizzare il contenuto JSON attivando le voci nel `env.development` file:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
