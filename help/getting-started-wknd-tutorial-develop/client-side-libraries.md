---
title: Librerie lato client e flusso di lavoro front-end
description: Scoprite come le librerie o i clientlibs lato client vengono utilizzati per distribuire e gestire CSS e Javascript per un'implementazione di Adobe Experience Manager (AEM) Sites. Questa esercitazione descriverà anche come il modulo ui.frontend, un progetto webpack, può essere integrato nel processo di compilazione end-to-end.
sub-product: sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 5%

---


# Librerie lato client e flusso di lavoro front-end {#client-side-libraries}

Scoprite come le librerie o i clientlibs lato client vengono utilizzati per distribuire e gestire CSS e Javascript per un&#39;implementazione di Adobe Experience Manager (AEM) Sites. Questa esercitazione descriverà inoltre come il modulo [ui.frontend](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html) , un progetto di [webpack](https://webpack.js.org/) scollegato, possa essere integrato nel processo di compilazione end-to-end.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

Si consiglia inoltre di consultare l’esercitazione [Component Basics](component-basics.md#client-side-libraries) per comprendere le nozioni di base delle librerie e dei AEM lato client.

### Progetto iniziale

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Duplicate l&#39;archivio [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) .
1. Estrarre il `client-side-libraries/start` ramo

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) o estrarre il codice localmente passando al ramo `client-side-libraries/solution`.

## Obiettivo

1. Comprendere in che modo le librerie lato client vengono incluse in una pagina tramite un modello modificabile.
1. Scopri come utilizzare il modulo UI.Frontend e un server di sviluppo webpack per lo sviluppo front-end dedicato.
1. Comprendere il flusso di lavoro end-to-end per la distribuzione di CSS e JavaScript compilati a un&#39;implementazione di Sites.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo verranno aggiunti alcuni stili di base per il sito WKND e per il modello di pagina dell&#39;articolo, allo scopo di avvicinare l&#39;implementazione ai modelli di progettazione dell&#39; [interfaccia utente](assets/pages-templates/wknd-article-design.xd). Utilizzerete un flusso di lavoro front-end avanzato per integrare un progetto webpack in una libreria client AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## Sfondo {#background}

Le librerie lato client forniscono un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un&#39;implementazione AEM Sites . Gli obiettivi di base per le librerie lato client o i client sono:

1. Archiviare CSS/JS in piccoli file discreti per semplificare lo sviluppo e la manutenzione
1. Gestire le dipendenze da framework di terze parti in modo organizzato
1. Riducete al minimo il numero di richieste lato client concatenando CSS/JS in una o due richieste.

Ulteriori informazioni sull&#39;utilizzo delle librerie lato [client sono disponibili qui.](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html)

Le librerie lato client presentano alcune limitazioni. In particolare, il supporto per i linguaggi front-end più comuni, come Sass, LESS e TypeScript, è limitato. Nell&#39;esercitazione vedremo come il modulo **ui.frontend** può contribuire a risolvere questo problema.

Distribuite la base di codice iniziale in un&#39;istanza AEM locale e andate a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Questa pagina non ha uno stile. Implementeremo in seguito le librerie lato client per il marchio WKND per aggiungere CSS e Javascript alla pagina.

## Organizzazione delle librerie lato client {#organization}

Prossimo esploreremo l&#39;organizzazione di clientlibs generati dal [AEM Project Archetype](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/overview.html).

![Organizzazione clientlibrary di alto livello](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramma di alto livello Organizzazione della libreria lato client e inclusione delle pagine*

>[!NOTE]
>
> La seguente organizzazione di libreria lato client viene generata da AEM Project Archetype, ma rappresenta solo un punto di partenza. Il modo in cui un progetto gestisce e distribuisce CSS e Javascript a un&#39;implementazione di Sites può variare notevolmente in base a risorse, competenze e requisiti.

1. Utilizzando Eclipse o un altro IDE, aprite il modulo **ui.apps** .
1. Espandete il percorso `/apps/wknd/clientlibs` per visualizzare i clientlibs generati dall&#39;archetype.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Esamineremo questi clientlibs più in dettaglio qui di seguito.

1.  le proprietà di Inspect `clientlibs/clientlib-base`.

   **clientlib-base** rappresenta il livello di base di CSS e JavaScript necessario per il funzionamento del sito WKND. Osservate la proprietà `categories` impostata su `wknd.base`. `categories` è un meccanismo di tag per clientlibs ed è il modo in cui è possibile farvi riferimento.

   Osservate la `embed` proprietà e il `String[]` valore dei valori. La `embed` proprietà incorpora altri clientlibs in base alla loro categoria. **clientlib-base** includerà tutte le librerie AEM client dei componenti core necessarie. Sono inclusi artefatti come javascript per il funzionamento dei componenti Carosello e Ricerca rapida. **clientlib-base** non includerà CSS e Javascript propri, ma incorporerà solo altre librerie client. **clientlib-base** incorpora la clientlib-grid **-** clientlib con la categoria di `wknd.grid`.

   Osservate la `allowProxy` proprietà impostata su `true`. È consigliabile impostare sempre `allowProxy=true` i clientlibs. La `allowProxy` proprietà consente di memorizzare i clientlibs con il codice dell&#39;applicazione in `/apps` ma **di distribuire i clientlibs su un percorso con il prefisso** `/etc.clientlibs` al fine di evitare l&#39;esposizione del codice dell&#39;applicazione agli utenti finali. More information about the [allowProxy property can be found here.](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1.  le proprietà di Inspect `clientlibs/clientlib-grid`.

   **clientlib-grid** è responsabile dell’inclusione/generazione del CSS necessario per l’utilizzo della modalità [](https://docs.adobe.com/content/help/it-IT/experience-manager-65/authoring/siteandpage/responsive-layout.html) Layout con l’editor AEM Sites . **clientlib-grid** aveva una categoria impostata su `wknd.grid` ed è incorporata tramite **clientlib-base**.

   La griglia può essere personalizzata per utilizzare diverse quantità di colonne e punti di interruzione. Verranno quindi aggiornati i punti di interruzione predefiniti generati.

1. Aggiornare il file `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   In questo modo i punti di interruzione verranno modificati per corrispondere ai punti di interruzione Modello impostati in `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`.

   Notate che questo file fa riferimento a un `grid_base.less` file al di sotto `/libs` del quale è contenuto un mixin personalizzato per generare la griglia.

1.  le proprietà di Inspect `clientlibs/clientlib-site`.

   **clientlib-site** conterrà tutti gli stili specifici per il sito per il marchio WKND. Prendete nota della categoria di `wknd.site`. I CSS e Javascript che generano clientlib verranno effettivamente mantenuti nel `ui.frontend` modulo. Questa integrazione verrà esplorata in seguito.

1.  le proprietà di Inspect `clientlibs/clientlib-dependencies`.

   **clientlib-dependencies** è destinato a incorporare eventuali dipendenze di terze parti. È una clientlib separata in modo che possa essere caricata nella parte superiore della pagina HTML, se necessario. Prendete nota della categoria di `wknd.dependencies`. I CSS e Javascript che generano clientlib verranno effettivamente mantenuti nel `ui.frontend` modulo. Questa integrazione verrà esaminata più avanti nell&#39;esercitazione.

## Utilizzo del modulo ui.frontend {#ui-frontend}

Successivamente esploreremo l&#39;uso del modulo **[ui.frontend](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html)** .

### Motivazione

Le librerie lato client presentano alcune limitazioni quando si tratta di supportare linguaggi come [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Sono stati inoltre introdotti strumenti open-source come [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) che accelerano e ottimizzano lo sviluppo front-end.

L&#39;idea di base dietro il modulo **ui.frontend** è quella di poter utilizzare ottimi strumenti come NPM e Webpack per gestire la maggior parte dello sviluppo front-end. Un elemento di integrazione chiave integrato nel modulo **ui.frontend** , [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prende gli artifact CSS e JS compilati da un progetto webpack/npm e li trasforma in librerie AEM lato client. Questo offre agli sviluppatori front-end maggiore libertà di scegliere strumenti e tecnologie diversi.

![Integrazione con l&#39;architettura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

### Utilizzo

Ora aggiungeremo alcuni stili di base per il marchio WKND aggiungendo alcuni file Sass (`.scss` estensione) tramite il modulo **ui.frontend** .

1. Aprite il modulo **ui.frontend** e passate a `src/main/webpack/base/sass`.

   ![modulo ui.frontend](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. Create a new file named `_variables.scss` beneath the folder `src/main/webpack/base/sass`.
1. Compilate `_variables.scss` con le seguenti opzioni:

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Sass ci consente di creare variabili, che possono essere poi utilizzate in file diversi per garantire la coerenza. Notate le Famiglie di font. Più avanti nell&#39;esercitazione vedremo come includere una chiamata ai font Web Google, per utilizzare questi font.

1. Create un altro file denominato `_elements.scss` in basso `src/main/webpack/base/sass` e compilatelo con i seguenti elementi:

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   Il `_elements.scss` file utilizza le variabili presenti nel `_variables.scss`.

1. Aggiorna `_shared.scss` sotto `src/main/webpack/base/sass` per includere i `_elements.scss` file e `_variables.scss` .

   ```css
   @import './variables';
   @import './elements';
   ```

1. Aprite un terminale della riga di comando e installate il modulo **ui.frontend** utilizzando il `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` deve essere eseguito una sola volta, dopo un nuovo clone o generazione del progetto.

1. Nello stesso terminale, create e implementate il modulo **ui.frontend** utilizzando il `npm run dev` comando:

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   Il comando `npm run dev` deve creare e compilare il codice sorgente per il progetto Webpack e compilare infine le dipendenze **clientlib-site** e **clientlib** nel modulo **ui.apps** .

   >[!NOTE]
   >
   >Esiste anche un `npm run prod` profilo che minificherà i codici JS e CSS. Questa è la compilazione standard ogni volta che la build webpack viene attivata tramite Maven. More details about the [ui.frontend module can be found here](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1.  Inspect il file `site.css` sottostante `ui.frontend/dist/clientlib-site/css/site.css`. Tenere presente che il CSS è costituito principalmente da contenuti del `_elements.scss` file creato in precedenza, ma le variabili sono state sostituite con valori effettivi.

   ![Cs sito distribuito](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1.  Inspect il file `ui.frontend/clientlib.config.js`. Questo è il file di configurazione per un plugin npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-generator** è lo strumento responsabile per la trasformazione dei CSS/JavaScript compilati e per copiarli nel modulo **ui.apps** .

1.  Inspect il file `site.css` nel modulo **ui.apps** all&#39;indirizzo `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Deve trattarsi di una copia identica del `site.css` file dal modulo **ui.frontend** . Ora che è nel modulo **ui.apps** può essere distribuito a AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Poiché **clientlib-site** viene effettivamente compilata durante il tempo di creazione, utilizzando **npm** o **maven**, può essere effettivamente ignorata dal controllo del codice sorgente nel modulo **ui.apps** .  Inspect il `.gitignore` file sotto **ui.apps**.

>[!CAUTION]
>
> L&#39;utilizzo del modulo **ui.frontend** potrebbe non essere necessario per tutti i progetti. Il modulo **ui.frontend** aggiunge ulteriore complessità e se non c&#39;è bisogno/desiderio di utilizzare alcuni di questi strumenti front-end avanzati (Sass, webpack, npm...) potrebbe essere un eccesso. Per questo motivo è considerata una parte opzionale del Project Archetype AEM e l&#39;uso di librerie lato client standard e vaniglia CSS e JavaScript continua ad essere pienamente supportato.

## Inclusione di pagine e modelli {#page-inclusion}

Verrà quindi esaminata la modalità di configurazione del progetto per includere i clientlibs in AEM modelli/pagine. Una procedura ottimale comune nello sviluppo Web consiste nell’includere CSS nell’intestazione HTML `<head>` e in JavaScript subito prima della chiusura `</body>` del tag.

1. In the **ui.apps** module navigate to `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Componente pagina struttura](assets/client-side-libraries/customheaderlibs-html.png)

   Questo è il `page` componente utilizzato per eseguire il rendering di tutte le pagine nell’implementazione WKND.

1. Aprire il file `customheaderlibs.html`. Osservate le linee `${clientlib.css @ categories='wknd.base'}`. Questo indica che il CSS per clientlib con una categoria di `wknd.base` sarà incluso tramite questo file, includendo efficacemente **clientlib-base** nell&#39;intestazione di tutte le nostre pagine.

1. Aggiornamento `customheaderlibs.html` per includere un riferimento agli stili di font Google precedentemente specificati nel modulo **ui.frontend** . Per il momento, inoltre, faremo commenti su ContextHub...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1.  Inspect il file `customfooterlibs.html`. Questo file, come `customheaderlibs.html` viene sovrascritto mediante l&#39;implementazione di progetti. Qui la riga `${clientlib.js @ categories='wknd.base'}` indica che il codice JavaScript di **clientlib-base** sarà incluso in fondo a tutte le nostre pagine.

1. Create e implementate il progetto in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Passate ai modelli WKND all&#39;indirizzo [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Selezionate e aprite il Modello **pagina** articolo nell&#39;Editor modelli.

   ![Seleziona modello pagina articolo](assets/client-side-libraries/open-article-page-template.png)

1. Fate clic sull’icona Informazioni **** pagina e nel menu selezionate Criteri **** pagina per aprire la finestra di dialogo Criteri **** pagina.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy.png)

   *Informazioni pagina > Criteri pagina*

1. Notate che le categorie per `wknd.dependencies` e `wknd.site` sono elencate qui. Per impostazione predefinita, i clientlibs configurati tramite il criterio pagina sono suddivisi per includere il CSS nell&#39;intestazione della pagina e il codice JavaScript all&#39;estremità del corpo. Se desiderato, potete elencare esplicitamente che clientlib JavaScript deve essere caricato nell&#39;intestazione Pagina. Questo è il caso per `wknd.dependencies`.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > È inoltre possibile fare riferimento direttamente al componente `wknd.site` o `wknd.dependencies` dal componente pagina, utilizzando lo `customheaderlibs.html` script o `customfooterlibs.html` , come già visto in precedenza per `wknd.base` clientlib. L’utilizzo del modello offre una certa flessibilità in quanto consente di scegliere e scegliere quali clientlibs vengono utilizzati per ogni modello. Ad esempio, se disponete di una libreria JavaScript molto pesante che verrà utilizzata solo su un modello selezionato.

1. Passate alla pagina **LO Skateparks** creata utilizzando il modello **di pagina** Articolo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Dovreste notare una differenza nei font e alcuni stili di base applicati per indicare che il CSS creato nel modulo **ui.frontend** funziona.

1. Fate clic sull&#39;icona Informazioni **** pagina e nel menu selezionate **Visualizza come pubblicato** per aprire la pagina dell&#39;articolo all&#39;esterno dell&#39;editor AEM.

   ![Visualizza come pubblicato](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizzate l&#39;origine Pagina di [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e dovreste essere in grado di visualizzare i seguenti riferimenti clientlib nel `<head>`:

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   Notate che i clientlibs utilizzano l&#39; `/etc.clientlibs` endpoint proxy. Dovresti anche vedere le seguenti clientlib incluse nella parte inferiore della pagina:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >Dal lato della pubblicazione è importante che le librerie client **non** vengano servite dalle **/app** , poiché questo percorso dovrebbe essere limitato per motivi di sicurezza tramite la sezione [Filtro](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)Dispatcher. La proprietà [](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) allowProxy della libreria client garantisce che i file CSS e JS vengano serviti da **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

Nel paio di esercizi precedenti siamo stati in grado di aggiornare diversi file Sass nel modulo **ui.frontend** e attraverso un processo di compilazione, alla fine vedere queste modifiche riflesse in AEM. In seguito, cercheremo di sfruttare un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) per sviluppare rapidamente i nostri stili front-end.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

Di seguito sono riportati i passaggi ad alto livello mostrati nel video:

1. Avviate il server webpack dev eseguendo il seguente comando dal modulo **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Questa opzione consente di aprire una nuova finestra del browser all&#39;indirizzo [http://localhost:8080/](Http://localhost:8080/) con marcatura statica.
1. Copiate l&#39;origine della pagina dell&#39;articolo sullo skatepark della LA all&#39;indirizzo [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Incollate la marcatura copiata da AEM nel `index.html` modulo **ui.frontend** sottostante `src/main/webpack/static`.
1. Modificate il markup copiato e rimuovete eventuali riferimenti a dipendenze **clientlib-site** e **clientlib**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   È possibile rimuovere tali riferimenti perché il server di sviluppo webpack genererà automaticamente questi artefatti.

1. Modificate i `.scss` file e visualizzate le modifiche automaticamente riportate nel browser.
1. Rivedete il `/aem-guides-wknd.ui.frontend/webpack.dev.js` file. Contiene la configurazione del webpack utilizzata per avviare il webpack-dev-server. Si noti che associa i percorsi `/content` e `/etc.clientlibs` da un&#39;istanza di AEM in esecuzione locale. In questo modo vengono rese disponibili le immagini e altri clientlibs (non gestiti dal codice **ui.frontend** ).

   >[!CAUTION]
   >
   > L’immagine src della marcatura statica punta a un componente immagine live in un’istanza AEM locale. Le immagini risulteranno interrotte se il percorso dell&#39;immagine cambia, se AEM non viene avviato o se il browser utilizza un collegamento non collegato all&#39;istanza AEM locale.
1. È possibile **arrestare** il server webpack dalla riga di comando digitando `CTRL+C`.

## Mettere insieme {#putting-it-together}

Questa esercitazione è incentrata sulle librerie lato client e sui potenziali flussi di lavoro front-end da integrare con AEM. A tal fine, l&#39;implementazione verrà accelerata installando [client-side-libraries-final-Styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip), che fornisce alcuni stili predefiniti per i componenti core utilizzati nel modello di pagina dell&#39;articolo:

* [Breadcrumb](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/breadcrumb.html)
* [Scarica](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Immagine](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/components/image.html)
* [Elenco](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/list.html)
* [Navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)
* [Ricerca rapida](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/quick-search.html)
* [Separatore](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

Di seguito sono riportati i passaggi ad alto livello mostrati nel video:

1. Scaricate [client-side-libraries-final-Styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) e decomprimete il contenuto sottostante `ui.frontend/src/main/webpack`. Il contenuto del file ZIP deve sovrascrivere le cartelle seguenti:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. Visualizzare l&#39;anteprima dei nuovi stili utilizzando il server di sviluppo webpack:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Distribuite la base di codice in un&#39;istanza AEM locale per visualizzare i nuovi stili applicati all&#39;articolo del parco di skate della LA:

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## Congratulazioni! {#congratulations}

Congratulazioni, la pagina dell&#39;articolo ora ha alcuni stili coerenti che corrispondono al marchio WKND e avete familiarità con il modulo **ui.frontend** !

### Passaggi successivi {#next-steps}

Scopri come implementare singoli stili e riutilizzare i componenti core con  Experience Manager  Style System. [Lo sviluppo con Style System](style-system.md) copre l&#39;utilizzo di Style System per estendere i componenti core con CSS specifici del marchio e configurazioni di criteri avanzati dell&#39;Editor modelli.

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in Git brach `client-side-libraries/solution`.

1. Duplicate l&#39;archivio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) .
1. Controlla il `client-side-libraries/solution` ramo.

## Strumenti e risorse aggiuntivi {#additional-resources}

### aemed {#develop-aemfed}

[**aemfeed**](https://aemfed.io/) è uno strumento open-source da riga di comando che può essere utilizzato per velocizzare lo sviluppo front-end. È alimentato da [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) e [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

A un livello **aemfeed** alto è progettato per ascoltare le modifiche ai file all&#39;interno del modulo **ui.apps** e sincronizzarle automaticamente direttamente a un&#39;istanza AEM in esecuzione. In base alle modifiche, un browser locale si aggiorna automaticamente, accelerando così lo sviluppo front-end. È inoltre progettato per lavorare con Sling Log Tracer per visualizzare automaticamente eventuali errori sul lato server direttamente nel terminale.

Se state facendo molto lavoro all&#39;interno del modulo **ui.apps** , modificando gli script HTL e creando componenti personalizzati, **aemfeed** può essere uno strumento molto potente da usare. [La documentazione completa è disponibile qui.](https://github.com/abmaonline/aemfed).

### Debug delle librerie lato client {#debugging-clientlibs}

Con diversi metodi di **categorie** e **incorpora** per includere più librerie client, la risoluzione dei problemi può essere complicata. AEM espone diversi strumenti per aiutarlo. Uno degli strumenti più importanti è **Ricostruire le librerie** client, che costringerà AEM a ricompilare qualsiasi file LESS e generare il CSS.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Elenca tutte le librerie client registrate nell&#39;istanza AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Output**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) test: consente a un utente di visualizzare l&#39;output HTML previsto di clientlib in base alla categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Convalida**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) delle dipendenze delle librerie - evidenzia eventuali dipendenze o categorie incorporate che non è possibile trovare. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rigenerare le librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) client: consente all&#39;utente di forzare la AEM per ricreare tutte le librerie client o di annullare la validità della cache delle librerie client. Questo strumento è particolarmente efficace quando si sviluppa con MENO, in quanto può costringere AEM a ricompilare il CSS generato. In generale è più efficace annullare la validità delle cache, quindi eseguire un aggiornamento delle pagine anziché ricreare tutte le librerie. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![rigenerare la libreria client](assets/client-side-libraries/rebuild-clientlibs.png)
