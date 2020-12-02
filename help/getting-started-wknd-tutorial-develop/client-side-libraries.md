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

Scoprite come le librerie o i clientlibs lato client vengono utilizzati per distribuire e gestire CSS e Javascript per un&#39;implementazione di Adobe Experience Manager (AEM) Sites. Questa esercitazione illustra inoltre come il modulo [ui.frontend](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html), un progetto [webpack ](https://webpack.js.org/) sganciato, possa essere integrato nel processo di compilazione end-to-end.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

È inoltre consigliabile rivedere l&#39;esercitazione [Component Basics](component-basics.md#client-side-libraries) per comprendere le nozioni di base delle librerie e delle AEM lato client.

### Progetto iniziale

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Duplicare il repository [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `client-side-libraries/start`

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

In questo capitolo verranno aggiunti alcuni stili di base per il sito WKND e per il modello di pagina dell&#39;articolo, allo scopo di avvicinare l&#39;implementazione ai [modelli di progettazione dell&#39;interfaccia utente](assets/pages-templates/wknd-article-design.xd). Utilizzerete un flusso di lavoro front-end avanzato per integrare un progetto webpack in una libreria client AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## Sfondo {#background}

Le librerie lato client forniscono un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un&#39;implementazione AEM Sites . Gli obiettivi di base per le librerie lato client o i client sono:

1. Archiviare CSS/JS in piccoli file discreti per semplificare lo sviluppo e la manutenzione
1. Gestire le dipendenze da framework di terze parti in modo organizzato
1. Riducete al minimo il numero di richieste lato client concatenando CSS/JS in una o due richieste.

Ulteriori informazioni sull&#39;utilizzo delle [librerie lato client sono disponibili qui.](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html)

Le librerie lato client presentano alcune limitazioni. In particolare, il supporto per i linguaggi front-end più comuni, come Sass, LESS e TypeScript, è limitato. Nell&#39;esercitazione verrà illustrato come il modulo **ui.frontend** può contribuire a risolvere questo problema.

Distribuire la base del codice iniziale in un&#39;istanza AEM locale e passare a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Questa pagina non ha uno stile. Implementeremo in seguito le librerie lato client per il marchio WKND per aggiungere CSS e Javascript alla pagina.

## Organizzazione delle librerie lato client {#organization}

Verranno quindi esaminati l&#39;organizzazione dei clientlibs generati dall&#39; [AEM Project Archetype](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/overview.html).

![Organizzazione clientlibrary di alto livello](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramma di alto livello Organizzazione della libreria lato client e inclusione delle pagine*

>[!NOTE]
>
> La seguente organizzazione di libreria lato client viene generata da AEM Project Archetype, ma rappresenta solo un punto di partenza. Il modo in cui un progetto gestisce e distribuisce CSS e Javascript a un&#39;implementazione di Sites può variare notevolmente in base a risorse, competenze e requisiti.

1. Utilizzando Eclipse o un altro IDE, aprite il modulo **ui.apps**.
1. Espandete il percorso `/apps/wknd/clientlibs` per visualizzare i clientlibs generati dall&#39;archetype.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Esamineremo questi clientlibs più in dettaglio qui di seguito.

1.  Inspect le proprietà di `clientlibs/clientlib-base`.

   **clientlib-** basererappresenta il livello di base di CSS e JavaScript necessario per il funzionamento del sito WKND. Osservate la proprietà `categories` impostata su `wknd.base`. `categories` è un meccanismo di tag per clientlibs ed è il modo in cui è possibile farvi riferimento.

   Osservate la proprietà `embed` e la proprietà `String[]` dei valori. La proprietà `embed` include altri clientlibs in base alla loro categoria. **clientlib-** baseincluderà tutte le librerie AEM client dei componenti core necessarie. Sono inclusi artefatti come javascript per il funzionamento dei componenti Carosello e Ricerca rapida. **clientlib-** basenon includerà CSS e Javascript propri, ma incorporerà solo altre librerie client. **clientlib-** baseincorpora la  **clientlib-** gridclientlib con la categoria di  `wknd.grid`.

   Notare che la proprietà `allowProxy` è impostata su `true`. È consigliabile impostare sempre `allowProxy=true` su clientlibs. La proprietà `allowProxy` consente di memorizzare i clientlibs con il codice dell&#39;applicazione in `/apps` **ma**, quindi distribuisce i clientlibs su un percorso con il prefisso `/etc.clientlibs`, al fine di evitare l&#39;esposizione del codice dell&#39;applicazione agli utenti finali. Ulteriori informazioni sulla proprietà [allowProxy sono disponibili qui.](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1.  Inspect le proprietà di `clientlibs/clientlib-grid`.

   **clientlib-** gridis è responsabile dell&#39;inclusione/generazione del CSS necessario per il funzionamento del  [modello ](https://docs.adobe.com/content/help/it-IT/experience-manager-65/authoring/siteandpage/responsive-layout.html) Layout con l&#39;editor AEM Sites . **clientlib-** gridhad aveva una categoria impostata su  `wknd.grid` ed è incorporata tramite  **clientlib-base**.

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

   In questo modo i punti di interruzione verranno modificati in base ai punti di interruzione del modello impostati in `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`.

   Notate che questo file fa riferimento a un file `grid_base.less` in `/libs` che contiene un mixin personalizzato per generare la griglia.

1.  Inspect le proprietà di `clientlibs/clientlib-site`.

   **clientlib-** siteconterrà tutti gli stili specifici del sito per il marchio WKND. Prendete nota della categoria `wknd.site`. I CSS e Javascript che generano clientlib verranno effettivamente mantenuti nel modulo `ui.frontend`. Questa integrazione verrà esplorata in seguito.

1.  Inspect le proprietà di `clientlibs/clientlib-dependencies`.

   **clientlib-** dependenciesis destinato a incorporare eventuali dipendenze di terze parti. È una clientlib separata in modo che possa essere caricata nella parte superiore della pagina HTML, se necessario. Prendete nota della categoria `wknd.dependencies`. I CSS e Javascript che generano clientlib verranno effettivamente mantenuti nel modulo `ui.frontend`. Questa integrazione verrà esaminata più avanti nell&#39;esercitazione.

## Utilizzo del modulo ui.frontend {#ui-frontend}

Verrà quindi esaminato l&#39;utilizzo del modulo **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**.

### Motivazione

Le librerie lato client presentano alcune limitazioni per quanto riguarda il supporto di linguaggi come [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). È stata inoltre rilevata un&#39;esplosione di strumenti open-source come [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) che accelerano e ottimizzano lo sviluppo front-end.

L&#39;idea di base del modulo **ui.frontend** è quella di poter utilizzare strumenti straordinari come NPM e Webpack per gestire la maggior parte dello sviluppo front-end. Un elemento di integrazione chiave integrato nel modulo **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prende gli artifact CSS e JS compilati da un progetto webpack/npm e li trasforma in AEM librerie lato client. Questo offre agli sviluppatori front-end maggiore libertà di scegliere strumenti e tecnologie diversi.

![Integrazione con l&#39;architettura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

### Utilizzo

Ora verranno aggiunti alcuni stili di base per il marchio WKND aggiungendo alcuni file Sass (`.scss` estensione) tramite il modulo **ui.frontend**.

1. Aprire il modulo **ui.frontend** e passare a `src/main/webpack/base/sass`.

   ![modulo ui.frontend](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. Create un nuovo file denominato `_variables.scss` sotto la cartella `src/main/webpack/base/sass`.
1. Compilare `_variables.scss` con i seguenti elementi:

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

1. Create un altro file denominato `_elements.scss` sotto `src/main/webpack/base/sass` e compilatelo con i seguenti elementi:

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

   Tenere presente che il file `_elements.scss` utilizza le variabili presenti nella cartella `_variables.scss`.

1. Aggiornare `_shared.scss` sotto `src/main/webpack/base/sass` per includere i file `_elements.scss` e `_variables.scss`.

   ```css
   @import './variables';
   @import './elements';
   ```

1. Aprite un terminale della riga di comando e installate il modulo **ui.frontend** utilizzando il comando `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` deve essere eseguito una sola volta, dopo un nuovo clone o generazione del progetto.

1. Nello stesso terminale, creare e distribuire il modulo **ui.frontend** utilizzando il comando `npm run dev`:

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   Il comando `npm run dev` deve creare e compilare il codice sorgente per il progetto Webpack e compilare infine il **clientlib-site** e **clientlib-dependencies** nel modulo **ui.apps**.

   >[!NOTE]
   >
   >È inoltre disponibile un profilo `npm run prod` che minificherà JS e CSS. Questa è la compilazione standard ogni volta che la build webpack viene attivata tramite Maven. Ulteriori dettagli sul modulo [ui.frontend sono disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1.  Inspect il file `site.css` sotto `ui.frontend/dist/clientlib-site/css/site.css`. Tenere presente che il CSS è costituito principalmente da contenuti del file `_elements.scss` creato in precedenza, ma le variabili sono state sostituite con valori effettivi.

   ![Cs sito distribuito](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1.  Inspect il file `ui.frontend/clientlib.config.js`. Questo è il file di configurazione per un plugin npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-** generatoris lo strumento responsabile per la trasformazione dei CSS/JavaScript compilati e per copiarli nel file  **ui.** appsmoModule.

1.  Inspect il file `site.css` nel modulo **ui.apps** all&#39;indirizzo `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Deve trattarsi di una copia identica del file `site.css` dal modulo **ui.frontend**. Ora che si trova nel modulo **ui.apps** è possibile distribuirlo per AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Poiché **clientlib-site** viene effettivamente compilata durante il tempo di creazione, utilizzando **npm** o **maven**, può essere ignorata dal controllo del codice sorgente nel modulo **ui.apps**.  Inspect il file `.gitignore` sotto **ui.apps**.

>[!CAUTION]
>
> L&#39;utilizzo del modulo **ui.frontend** potrebbe non essere necessario per tutti i progetti. Il modulo **ui.frontend** aggiunge ulteriore complessità e, se non è necessario/si desidera utilizzare alcuni di questi strumenti front-end avanzati (Sass, webpack, npm...), potrebbe essere un errore eccessivo. Per questo motivo è considerata una parte opzionale del Project Archetype AEM e l&#39;uso di librerie lato client standard e vaniglia CSS e JavaScript continua ad essere pienamente supportato.

## Inclusione di pagine e modelli {#page-inclusion}

Verrà quindi esaminata la modalità di configurazione del progetto per includere i clientlibs in AEM modelli/pagine. Una procedura ottimale comune nello sviluppo Web consiste nell&#39;includere CSS nell&#39;intestazione HTML `<head>` e in JavaScript immediatamente prima della chiusura del tag `</body>`.

1. Nel modulo **ui.apps** andate a `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Componente pagina struttura](assets/client-side-libraries/customheaderlibs-html.png)

   Si tratta del componente `page` utilizzato per eseguire il rendering di tutte le pagine nell&#39;implementazione WKND.

1. Aprire il file `customheaderlibs.html`. Osservate le righe `${clientlib.css @ categories='wknd.base'}`. Questo indica che il CSS per clientlib con una categoria di `wknd.base` sarà incluso tramite questo file, includendo in modo efficace **clientlib-base** nell&#39;intestazione di tutte le nostre pagine.

1. Aggiornate `customheaderlibs.html` per includere un riferimento agli stili di font Google precedentemente specificati nel modulo **ui.frontend**. Per il momento, inoltre, faremo commenti su ContextHub...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1.  Inspect il file `customfooterlibs.html`. Questo file, come `customheaderlibs.html`, deve essere sovrascritto mediante l&#39;implementazione di progetti. Qui la riga `${clientlib.js @ categories='wknd.base'}` indica che il codice JavaScript di **clientlib-base** sarà incluso nella parte inferiore di tutte le nostre pagine.

1. Create e implementate il progetto in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Passate ai modelli WKND all&#39;indirizzo [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Selezionare e aprire il **Modello pagina articolo** nell&#39;Editor modelli.

   ![Seleziona modello pagina articolo](assets/client-side-libraries/open-article-page-template.png)

1. Fare clic sull&#39;icona **Informazioni pagina** e, nel menu, selezionare **Criteri pagina** per aprire la finestra di dialogo **Criteri pagina**.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy.png)

   *Informazioni pagina > Criteri pagina*

1. Tenere presente che le categorie per `wknd.dependencies` e `wknd.site` sono elencate qui. Per impostazione predefinita, i clientlibs configurati tramite il criterio pagina sono suddivisi per includere il CSS nell&#39;intestazione della pagina e il codice JavaScript all&#39;estremità del corpo. Se desiderato, potete elencare esplicitamente che clientlib JavaScript deve essere caricato nell&#39;intestazione Pagina. Questo è il caso di `wknd.dependencies`.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > È inoltre possibile fare riferimento direttamente allo script `wknd.site` o `wknd.dependencies` dal componente della pagina, utilizzando lo script `customheaderlibs.html` o `customfooterlibs.html`, come abbiamo visto in precedenza per la clientlib `wknd.base`. L’utilizzo del modello offre una certa flessibilità in quanto consente di scegliere e scegliere quali clientlibs vengono utilizzati per ogni modello. Ad esempio, se disponete di una libreria JavaScript molto pesante che verrà utilizzata solo su un modello selezionato.

1. Andate alla pagina **LA Skateparks** creata utilizzando il **Modello pagina articolo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). È opportuno visualizzare una differenza nei font e alcuni stili di base applicati per indicare che il CSS creato nel modulo **ui.frontend** funziona correttamente.

1. Fate clic sull&#39;icona **Informazioni pagina** e, nel menu, selezionate **Visualizza come pubblicato** per aprire la pagina dell&#39;articolo all&#39;esterno dell&#39;editor AEM.

   ![Visualizza come pubblicato](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizzare l&#39;origine pagina di [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e dovrebbe essere possibile visualizzare i seguenti riferimenti clientlib in `<head>`:

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

   Tenere presente che i clientlibs utilizzano l&#39;endpoint proxy `/etc.clientlibs`. Dovresti anche vedere le seguenti clientlib incluse nella parte inferiore della pagina:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >Dal lato della pubblicazione è fondamentale che le librerie client siano **non** servite da **/apps** in quanto il percorso deve essere limitato per motivi di sicurezza utilizzando la sezione [Dispatcher filter](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [proprietà allowProxy](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) della libreria client garantisce che i file CSS e JS vengano serviti da **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

Nel paio di esercizi precedenti siamo stati in grado di aggiornare diversi file Sass nel modulo **ui.frontend** e attraverso un processo di compilazione, alla fine vedere queste modifiche riflesse in AEM. In seguito, cercheremo di utilizzare un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) per sviluppare rapidamente i nostri stili front-end.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

Di seguito sono riportati i passaggi ad alto livello mostrati nel video:

1. Avviare il server di sviluppo webpack eseguendo il comando seguente dall&#39;interno del modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Questa opzione consente di aprire una nuova finestra del browser in [http://localhost:8080/](Http://localhost:8080/) con tag statici.
1. Copiate l&#39;origine della pagina dell&#39;articolo dello skatepark della LA all&#39;indirizzo [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Incollare la marcatura copiata da AEM nel modulo `index.html`ui.frontend **sotto**.`src/main/webpack/static`
1. Modificate la marcatura copiata e rimuovete eventuali riferimenti a **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   È possibile rimuovere tali riferimenti perché il server di sviluppo webpack genererà automaticamente questi artefatti.

1. Modificate i file `.scss` e visualizzate le modifiche automaticamente riportate nel browser.
1. Rivedete il file `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contiene la configurazione del webpack utilizzata per avviare il webpack-dev-server. Si noti che esegue il proxy dei percorsi `/content` e `/etc.clientlibs` da un&#39;istanza di AEM eseguita localmente. In questo modo vengono rese disponibili le immagini e altri clientlibs (non gestiti dal codice **ui.frontend**).

   >[!CAUTION]
   >
   > L’immagine src della marcatura statica punta a un componente immagine live in un’istanza AEM locale. Le immagini risulteranno interrotte se il percorso dell&#39;immagine cambia, se AEM non viene avviato o se il browser utilizza un collegamento non collegato all&#39;istanza AEM locale.
1. È possibile **arrestare** il server webpack dalla riga di comando digitando `CTRL+C`.

## Inserirlo insieme {#putting-it-together}

Questa esercitazione è incentrata sulle librerie lato client e sui potenziali flussi di lavoro front-end da integrare con AEM. A tal fine, l&#39;implementazione verrà accelerata installando [client-side-libraries-final-Styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip), che fornisce alcuni stili predefiniti per i componenti core utilizzati nel modello di pagina dell&#39;articolo:

* [Breadcrumb](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/breadcrumb.html)
* [Scarica](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Immagine](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)
* [Elenco](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/list.html)
* [Navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)
* [Ricerca rapida](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/quick-search.html)
* [Separatore](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

Di seguito sono riportati i passaggi ad alto livello mostrati nel video:

1. Scaricate [client-side-libraries-final-Styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) e decomprimete il contenuto sotto `ui.frontend/src/main/webpack`. Il contenuto del file ZIP deve sovrascrivere le cartelle seguenti:

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

Congratulazioni, la pagina dell&#39;articolo ora ha alcuni stili coerenti che corrispondono al marchio WKND e si è familiarizzati con il modulo **ui.frontend**!

### Passaggi successivi {#next-steps}

Scopri come implementare singoli stili e riutilizzare i componenti core con  Experience Manager  Style System. [Lo sviluppo con Style ](style-system.md) Systemcover utilizzando Style System per estendere i componenti core con CSS specifici del marchio e configurazioni avanzate di criteri dell&#39;Editor modelli.

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in corrispondenza del blocco Git `client-side-libraries/solution`.

1. Duplicare il repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `client-side-libraries/solution`.

## Strumenti e risorse aggiuntivi {#additional-resources}

### aemfeed {#develop-aemfed}

[**aemfeis**](https://aemfed.io/) uno strumento open-source da riga di comando che può essere utilizzato per accelerare lo sviluppo front-end. È alimentato da [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) e [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

A un livello elevato **aemfeed** è progettato per ascoltare le modifiche ai file all&#39;interno del modulo **ui.apps** e sincronizzarle automaticamente direttamente in un&#39;istanza AEM in esecuzione. In base alle modifiche, un browser locale si aggiorna automaticamente, accelerando così lo sviluppo front-end. È inoltre progettato per lavorare con Sling Log Tracer per visualizzare automaticamente eventuali errori sul lato server direttamente nel terminale.

Se si sta lavorando molto all&#39;interno del modulo **ui.apps**, modificando gli script HTL e creando componenti personalizzati, **aemfeed** può essere uno strumento molto potente da utilizzare. [La documentazione completa è disponibile qui.](https://github.com/abmaonline/aemfed).

### Debug delle librerie lato client {#debugging-clientlibs}

Con metodi diversi di **category** e **embeds** per includere più librerie client, la risoluzione dei problemi può essere complessa. AEM espone diversi strumenti per aiutarlo. Uno degli strumenti più importanti è **Ricostruisci librerie client**, che costringerà AEM a ricompilare qualsiasi file LESS e generare il CSS.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - Elenca tutte le librerie client registrate nell&#39;istanza AEM.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Output**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  test: consente all&#39;utente di visualizzare l&#39;output HTML previsto di clientlib in base alla categoria.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Convalida**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  delle dipendenze delle librerie - evidenzia eventuali dipendenze o categorie incorporate che non è possibile trovare.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rigenerare le librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  client: consente all&#39;utente di forzare la AEM per ricreare tutte le librerie client o di annullare la validità della cache delle librerie client. Questo strumento è particolarmente efficace quando si sviluppa con MENO, in quanto può costringere AEM a ricompilare il CSS generato. In generale è più efficace annullare la validità delle cache, quindi eseguire un aggiornamento delle pagine anziché ricreare tutte le librerie. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![rigenerare la libreria client](assets/client-side-libraries/rebuild-clientlibs.png)
