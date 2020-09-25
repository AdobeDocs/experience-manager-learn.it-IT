---
title: Integrazione di un'area SPA | Guida introduttiva all'editor SPA AEM e Angular
description: Scoprite come il codice sorgente per un’applicazione SPA (Single Page Application) scritta in Angular può essere integrato con un progetto Adobe Experience Manager (AEM). Scopri come utilizzare i moderni strumenti front-end, come lo strumento CLI di Angular, per sviluppare rapidamente l'SPA rispetto all'API AEM modello JSON.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 0%

---


# Integrazione di un&#39;area SPA {#integrate-spa}

Scoprite come il codice sorgente per un’applicazione SPA (Single Page Application) scritta in Angular può essere integrato con un progetto Adobe Experience Manager (AEM). Scoprite come utilizzare i moderni strumenti front-end, come un server di sviluppo webpack, per sviluppare rapidamente l&#39;SPA rispetto all&#39;API AEM modello JSON.

## Obiettivo

1. Scoprite come il progetto SPA è integrato con AEM con librerie lato client.
2. Scoprite come utilizzare un server di sviluppo locale per lo sviluppo front-end dedicato.
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
   $ git checkout Angular/integrate-spa-start
   ```

2. Distribuire la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzate [AEM 6.x](overview.md#compatibility) , aggiungete il `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) o estrarre il codice localmente passando al ramo `Angular/integrate-spa-solution`.

## Metodo di integrazione {#integration-approach}

Nel progetto AEM sono stati creati due moduli: `ui.apps` e `ui.frontend`.

Il `ui.frontend` modulo è un progetto [webpack](https://webpack.js.org/) che contiene tutto il codice sorgente SPA. La maggior parte delle attività di sviluppo e test SPA sarà realizzata nel progetto webpack. Quando viene attivata una build di produzione, l&#39;SPA viene creata e compilata utilizzando il webpack. Gli artifact compilati (CSS e Javascript) vengono copiati nel `ui.apps` modulo che viene quindi distribuito al runtime AEM.

![architettura di alto livello ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Una rappresentazione di alto livello dell&#39;integrazione SPA.*

Ulteriori informazioni sulla build Front-end sono [disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

##  l&#39;integrazione SPA di Inspect {#inspect-spa-integration}

Quindi, ispezionate il `ui.frontend` modulo per comprendere la SPA generata automaticamente dal [AEM archetipo](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)del progetto.

1. Nell’IDE di vostra scelta, aprite il progetto AEM per l’area SPA WKND. Questa esercitazione utilizzerà l&#39;IDE [di codice di](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)Visual Studio.

   ![VSCode - AEM progetto SPA WKND](./assets/integrate-spa/vscode-ide-openproject.png)

2. Espandete e ispezionate la `ui.frontend` cartella. Aprire il file `ui.frontend/package.json`

3. Sotto la sezione `dependencies` dovrebbero essere visualizzate diverse informazioni correlate a `@angular`:

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   Il `ui.frontend` modulo è un&#39;applicazione [](https://angular.io) angolare generata utilizzando lo strumento [CLI](https://angular.io/cli) angolare che include il routing.

4. Sono inoltre presenti tre dipendenze con il prefisso `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   I moduli di cui sopra costituiscono l’SDK [JS di](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) AEM SPA Editor e forniscono le funzionalità necessarie per la mappatura dei componenti SPA su AEM componenti.

5. Nel `package.json` file `scripts` sono definiti diversi elementi:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Questi script sono basati su comandi [CLI](https://angular.io/cli/build) angolare comuni, ma sono stati leggermente modificati per lavorare con il progetto AEM più grande.

   `start` - esegue l&#39;app Angular localmente utilizzando un server Web locale. È stato aggiornato per il proxy del contenuto dell&#39;istanza AEM locale.

   `build` - compila l&#39;app Angular per la distribuzione di produzione. L&#39;aggiunta di `&& clientlib` è responsabile della copia dell&#39;SPA compilata nel `ui.apps` modulo come libreria lato client durante una build. Il modulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) è utilizzato per facilitare questo.

   More details about the available scripts can be found [here](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6.  Inspect il file `ui.frontend/clientlib.config.js`. Questo file di configurazione viene utilizzato da [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) per determinare come generare la libreria client.

7.  Inspect il file `ui.frontend/pom.xml`. Questo file trasforma la `ui.frontend` cartella in un modulo [](http://maven.apache.org/guides/mini/guide-multiple-modules.html)Paradiso. Il `pom.xml` file è stato aggiornato per utilizzare il plugin [frontend-maven](https://github.com/eirslett/frontend-maven-plugin) per **testare** e **costruire** l&#39;SPA durante una build Maven.

8.  Inspect il file `app.component.ts` in `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` è il punto di ingresso della SPA. `ModelManager` è fornito dall’SDK JS AEM SPA Editor. È responsabile della chiamata e dell&#39;inserimento del `pageModel` (il contenuto JSON) nell&#39;applicazione.

## Aggiunta di un componente Intestazione {#header-component}

Quindi, aggiungete un nuovo componente all’area di protezione e distribuite le modifiche a un’istanza AEM locale per visualizzare l’integrazione.

1. Aprite una nuova finestra del terminale e passate alla `ui.frontend` cartella:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installare [Angular CLI](https://angular.io/cli#installing-angular-cli) a livello globale Questo viene utilizzato per generare componenti Angular e per creare e servire l&#39;applicazione Angular tramite il comando **ng** .

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > La versione di **@angular/cli** utilizzata da questo progetto è **9.1.7**. Si consiglia di mantenere sincronizzate le versioni CLI angolare.

3. Crea un nuovo `Header` componente eseguendo il comando CLI angolare `ng generate component` dall’interno della `ui.frontend` cartella.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   In questo modo verrà creato uno scheletro per il nuovo componente Intestazione angolare in `ui.frontend/src/app/components/header`.

4. Apri il `aem-guides-wknd-spa` progetto nell’IDE desiderato. Passate alla `ui.frontend/src/app/components/header` cartella.

   ![Percorso componente intestazione nell’IDE](assets/integrate-spa/header-component-path.png)

5. Aprite il file `header.component.html` e sostituite il contenuto con quanto segue:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Notate che viene visualizzato contenuto statico, pertanto questo componente Angular non richiede alcuna regolazione per il contenuto generato predefinito `header.component.ts`.

6. Apri il file **app.component.html** in `ui.frontend/src/app/app.component.html`. Aggiungete `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Questo include il `header` componente al di sopra di tutto il contenuto della pagina.

7. Aprite un nuovo terminale, individuate la `ui.frontend` cartella ed eseguite il `npm run build` comando:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Passate alla `ui.apps` cartella. Sotto di `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` seguito dovresti vedere che i file SPA compilati sono stati copiati dalla`ui.frontend/build` cartella.

   ![Libreria client generata in ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Tornate al terminale e individuate la `ui.apps` cartella. Eseguite il seguente comando Maven:

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

10. Aprite una scheda del browser e andate a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). È ora possibile visualizzare il contenuto del `Header` componente nell’area di protezione.

   ![Implementazione intestazione iniziale](assets/integrate-spa/initial-header-implementation.png)

   I passaggi **7-9** vengono eseguiti automaticamente quando si attiva una build Maven dalla radice del progetto (ovvero `mvn clean install -PautoInstallSinglePackage`). È ora necessario comprendere le basi dell&#39;integrazione tra SPA e AEM librerie lato client. È comunque possibile modificare e aggiungere `Text` componenti in AEM, ma il `Header` componente non è modificabile.

## Webpack Dev Server - Proxy API JSON {#proxy-json}

Come mostrato negli esercizi precedenti, l&#39;esecuzione di una build e la sincronizzazione della libreria client con un&#39;istanza locale di AEM richiede alcuni minuti. Questo è accettabile per il test finale, ma non è ideale per la maggior parte dello sviluppo SPA.

Un server [di sviluppo](https://webpack.js.org/configuration/dev-server/) webpack può essere utilizzato per sviluppare rapidamente l&#39;SPA. La SPA è guidata da un modello JSON generato da AEM. In questo esercizio il contenuto JSON da un&#39;istanza in esecuzione di AEM verrà **proxy** nel server di sviluppo configurato dal progetto [](https://angular.io/guide/build)Angular.

1. Tornate all&#39;IDE e aprite il file **proxy.conf.json** all&#39;indirizzo `ui.frontend/proxy.conf.json`.

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   L&#39;app [](https://angular.io/guide/build#proxying-to-a-backend-server) Angular fornisce un meccanismo semplice per le richieste API proxy. I pattern specificati in `context` vengono proxy attraverso `localhost:4502`, il AEM rapido locale.

2. Aprite il file **index.html** in `ui.frontend/src/index.html`. Si tratta del file HTML principale utilizzato dal server di sviluppo.

   Nota: è presente una voce per `base href="/"`. Il tag [](https://angular.io/guide/deployment#the-base-tag) base è fondamentale perché l&#39;app risolva gli URL relativi.

   ```html
   <base href="/">
   ```

3. Aprite una finestra del terminale e passate alla `ui.frontend` cartella. Eseguire il comando `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. Aprite una nuova scheda del browser (se non è già aperta) e andate a [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Server di sviluppo webpack - json proxy](assets/integrate-spa/webpack-dev-server-1.png)

   Dovresti visualizzare lo stesso contenuto come in AEM, ma senza che nessuna delle funzionalità di authoring sia abilitata.

5. Tornate all’IDE e create una nuova cartella denominata `img` in `ui.frontend/src/assets`.
6. Scaricate e aggiungete il seguente logo WKND alla `img` cartella:

   ![Logo WKND](./assets/integrate-spa/wknd-logo-dk.png)

7. Aprite **header.component.html** in `ui.frontend/src/app/components/header/header.component.html` e includete il logo:

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   Save the changes to **header.component.html**.

8. Tornate al browser. Dovresti vedere immediatamente le modifiche all&#39;app riflesse.

   ![Logo aggiunto all’intestazione](assets/integrate-spa/added-logo-localhost.png)

   È possibile continuare a eseguire aggiornamenti di contenuto in **AEM** e visualizzarli visualizzati nel server **di sviluppo** webpack, dal momento che il contenuto viene proxy. Le modifiche al contenuto sono visibili solo nel server **di sviluppo** webpack.

9. Arrestate il server Web locale con `ctrl+c` il terminale.

## Webpack Dev Server - API Mock JSON {#mock-json}

Un altro approccio al rapido sviluppo consiste nell&#39;utilizzare un file JSON statico per agire come modello JSON. &quot;prendendo in giro&quot; il JSON, rimuoviamo la dipendenza da un&#39;istanza AEM locale. Consente inoltre a uno sviluppatore front-end di aggiornare il modello JSON al fine di testare la funzionalità e di apportare modifiche all&#39;API JSON che sarebbe poi implementata da uno sviluppatore back-end.

La configurazione iniziale del JSON fittizio non **richiede un&#39;istanza** AEM locale.

1. Nel browser, andate a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   È il JSON esportato da AEM che guida l&#39;applicazione. Copiate l’output JSON.

2. Tornate all&#39;IDE andate a `ui.frontend/src` e aggiungete nuove cartelle denominate **mocks** e **json** in modo che corrispondano alla seguente struttura di cartelle:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Create a new file named **en.model.json** beneath `ui.frontend/public/mocks/json`. Incolla qui l&#39;output JSON dal **passaggio 1** .

   ![File Json modello Mock](assets/integrate-spa/mock-model-json-created.png)

4. Create a new file **proxy.mock.conf.json** beneath `ui.frontend`. Compilate il file con le seguenti opzioni:

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   Questa configurazione proxy riscrive le richieste che iniziano con `/content/wknd-spa-angular/us` e `/mocks/json` servono il file JSON statico corrispondente, ad esempio:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Apri il file **angular.json**. Aggiungete una nuova configurazione **dev** con un array di **risorse** aggiornato per fare riferimento alla cartella **mocks** creata.

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Cartella di aggiornamento per le risorse per sviluppatori JSON angolare](assets/integrate-spa/dev-assets-update-folder.png)

   La creazione di una configurazione **dev** dedicata garantisce che la cartella **mocks** venga utilizzata solo durante lo sviluppo e non venga mai distribuita per AEM in una build di produzione.

6. Nel file **angular.json** , aggiorna di nuovo la configurazione **browserTarget** per utilizzare la nuova configurazione **dev** :

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Angular JSON build dev update](assets/integrate-spa/angular-json-build-dev-update.png)

7. Aprite il file `ui.frontend/package.json` e aggiungete un nuovo comando **start:mock** per fare riferimento al file **proxy.mock.conf.json** .

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   L&#39;aggiunta di un nuovo comando facilita l&#39;alternanza tra le configurazioni proxy.

8. Se è in esecuzione, arrestare il server **di sviluppo** webpack. Avviate il server **di sviluppo** webpack utilizzando lo script **start:mock** :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Andate a [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) e dovreste visualizzare lo stesso SPA, ma il contenuto ora viene estratto dal file JSON **fittizio** .

9. Apportate una piccola modifica al file **en.model.json** creato in precedenza. Il contenuto aggiornato deve riflettersi immediatamente nel server **di sviluppo** webpack.

   ![aggiornamento json modello](./assets/integrate-spa/webpack-mock-model.gif)

   La possibilità di manipolare il modello JSON e visualizzare gli effetti su un&#39;app SPA live può aiutare uno sviluppatore a comprendere l&#39;API del modello JSON. Consente inoltre lo sviluppo front-end e back-end in parallelo.

## Aggiungi stili con ass

Successivamente, al progetto verrà aggiunto uno stile aggiornato. Questo progetto aggiunge il supporto [Sass](https://sass-lang.com/) per alcune funzioni utili come le variabili.

1. Aprire una finestra terminale e arrestare il server **di sviluppo** webpack se avviato. Dall’interno della `ui.frontend` cartella immettete il comando seguente per aggiornare l’app Angular ai file **.scss** .

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Il `angular.json` file verrà aggiornato con una nuova voce nella parte inferiore del file:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Installate `normalize-scss` per normalizzare gli stili tra i vari browser:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Tornate all’IDE e `ui.frontend/src` create una nuova cartella denominata `styles`.
4. Create un nuovo file sotto `ui.frontend/src/styles` nome `_variables.scss` e compilatelo con le seguenti variabili:

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
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. Rinominare l&#39;estensione del file **Styles.css** in corrispondenza `ui.frontend/src/styles.css` di **Styles.scss**. Sostituite il contenuto con quanto segue:

   ```scss
   /* styles.scss * /
   
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
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. Aggiorna **angular.json** e rinomina tutti i riferimenti a **style.css** con **Styles.scss**. Ci dovrebbero essere 3 riferimenti.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Aggiorna stili di intestazione

Quindi aggiungere al componente **Intestazione** stili specifici per il marchio utilizzando Sass.

1. Avviate il server **di sviluppo** webpack per visualizzare gli stili aggiornati in tempo reale:

   ```shell
   $ npm run start:mock
   ```

2. In `ui.frontend/src/app/components/header` ri-name **header.component.css** to **header.component.scss**. Compilate il file con le seguenti opzioni:

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. Aggiorna **header.component.js** per fare riferimento a **header.component.scss**:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. Tornate al browser e al server **di sviluppo** webpack:

   ![Intestazione con stile - webpack dev server](assets/integrate-spa/styled-header.png)

   È ora possibile visualizzare gli stili aggiornati aggiunti al componente **Intestazione** .

## Implementare gli aggiornamenti SPA per AEM

Le modifiche apportate a **Header** sono attualmente visibili solo attraverso il server **di sviluppo del** webpack. Distribuite l&#39;app SPA aggiornata per AEM vedere le modifiche.

1. Arrestate il server **di sviluppo** webpack.
2. Andate alla radice del progetto `/aem-guides-wknd-spa` e distribuite il progetto da AEM utilizzando Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Andate a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Dovresti vedere l’ **intestazione** aggiornata con il logo e gli stili applicati:

   ![Intestazione aggiornata in AEM](assets/integrate-spa/final-header-component.png)

   Ora che l’app SPA aggiornata è AEM, l’authoring può continuare.

## Congratulazioni! {#congratulations}

Congratulazioni, avete aggiornato l&#39;SPA ed esplorato l&#39;integrazione con AEM! Ora conoscete due approcci diversi per lo sviluppo dell&#39;SPA rispetto all&#39;API modello JSON AEM utilizzando un server **di sviluppo** webpack.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) o estrarre il codice localmente passando al ramo `Angular/integrate-spa-solution`.

### Passaggi successivi {#next-steps}

[Mappatura di componenti SPA a componenti](map-components.md) AEM - Scoprite come mappare componenti Angular a componenti Adobe Experience Manager (AEM) con l’SDK JS AEM SPA Editor. La mappatura dei componenti consente agli autori di effettuare aggiornamenti dinamici ai componenti SPA nell’editor AEM SPA, in modo simile all’authoring AEM tradizionale.
