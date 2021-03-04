---
title: Librerie lato client e flusso di lavoro front-end
description: Scopri come le librerie lato client o le librerie client vengono utilizzate per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites. Questa esercitazione illustra anche come il modulo ui.frontend, un progetto webpack, può essere integrato nel processo di compilazione end-to-end.
sub-product: sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
feature: Componenti core, AEM Project Archetype
topic: Gestione dei contenuti, sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3301'
ht-degree: 3%

---


# Librerie lato client e flusso di lavoro front-end {#client-side-libraries}

Scopri come le librerie lato client o le librerie client vengono utilizzate per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites. Questa esercitazione riguarderà anche come il modulo [ui.frontend](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html), un progetto [webpack](https://webpack.js.org/) disaccoppiato, può essere integrato nel processo di compilazione end-to-end.

## Prerequisiti {#prerequisites}

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

È inoltre consigliabile rivedere l&#39;esercitazione [Nozioni di base sui componenti](component-basics.md#client-side-libraries) per comprendere i fondamenti delle librerie lato client e di AEM.

### Progetto iniziale

>[!NOTE]
>
> Se hai completato con successo il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Estrai il ramo `tutorial/client-side-libraries-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Distribuisci la base di codice in un’istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o estrarre il codice localmente passando al ramo `tutorial/client-side-libraries-solution`.

## Obiettivo

1. Scopri come le librerie lato client vengono incluse in una pagina tramite un modello modificabile.
1. Scopri come utilizzare il modulo UI.Frontend e un server di sviluppo webpack per lo sviluppo front-end dedicato.
1. Comprendi il flusso di lavoro end-to-end della distribuzione di CSS e JavaScript compilati a un’implementazione di Sites.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo verranno aggiunti alcuni stili di base per il sito WKND e il modello di pagina dell&#39;articolo per avvicinare l&#39;implementazione ai [modelli di progettazione dell&#39;interfaccia utente](assets/pages-templates/wknd-article-design.xd). Utilizza un flusso di lavoro front-end avanzato per integrare un progetto webpack in una libreria client AEM.

![Stili completati](assets/client-side-libraries/finished-styles.png)

*Pagina articolo con stili di linea di base applicati*

## Sfondo {#background}

Le librerie lato client forniscono un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un’implementazione di AEM Sites. Gli obiettivi di base per le librerie lato client o le librerie client sono:

1. Archiviare CSS/JS in piccoli file discreti per facilitarne lo sviluppo e la manutenzione
1. Gestire le dipendenze da framework di terze parti in modo organizzato
1. Riduci al minimo il numero di richieste lato client concatenando CSS/JS in una o due richieste.

Ulteriori informazioni sull&#39;utilizzo delle [librerie lato client sono disponibili qui.](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html)

Le librerie lato client presentano alcune limitazioni. In particolare, il supporto limitato per i linguaggi front-end popolari come Sass, LESS e TypeScript. Nell’esercitazione verrà illustrato come il modulo **ui.frontend** può contribuire a risolvere il problema.

Distribuisci la base di codice iniziale in un&#39;istanza AEM locale e passa a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Questa pagina non è attualmente formattata. Implementeremo quindi le librerie lato client per il marchio WKND per aggiungere CSS e Javascript alla pagina.

## Organizzazione delle librerie lato client {#organization}

Ora esploreremo l’organizzazione delle clientlib generate da [AEM Project Archetype](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/developing/archetype/overview.html).

![Organizzazione di librerie client di alto livello](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramma di alto livello Organizzazione della libreria lato client e inclusione della pagina*

>[!NOTE]
>
> La seguente organizzazione di librerie lato client viene generata da AEM Project Archetype, ma rappresenta solo un punto di partenza. Il modo in cui un progetto gestisce e distribuisce CSS e JavaScript a un’implementazione di Sites può variare notevolmente in base a risorse, competenze e requisiti.

1. Utilizzando VSCode o un altro IDE, apri il modulo **ui.apps** .
1. Espandi il percorso `/apps/wknd/clientlibs` per visualizzare le clientlibs generate dall’archetipo.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Esamineremo questi clientlibs in modo più dettagliato di seguito.

1. Nella tabella seguente sono riepilogate le librerie client. Ulteriori dettagli su [incluse le librerie client sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrizione | Note |
   |-------------------| ------------| ------|
   | `clientlib-base` | Livello di base di CSS e JavaScript necessari per il funzionamento del sito WKND | incorpora le librerie client dei componenti core |
   | `clientlib-grid` | Genera il CSS necessario per il funzionamento di [Modalità layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html). | I punti di interruzione Mobile/Tablet possono essere configurati qui |
   | `clientlib-site` | Contiene un tema specifico per il sito WKND | Generato dal modulo `ui.frontend` |
   | `clientlib-dependencies` | Incorpora tutte le dipendenze di terze parti | Generato dal modulo `ui.frontend` |

1. Tenere presente che `clientlib-site` e `clientlib-dependencies` vengono ignorati dal controllo del codice sorgente. Questo è di progettazione, in quanto verranno generate in fase di creazione dal modulo `ui.frontend` .

## Aggiorna gli stili di base {#base-styles}

Quindi, aggiorna gli stili di base definiti nel modulo **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** . I file nel modulo `ui.frontend` genereranno le librerie `clientlib-site` e `clientlib-dependecies` che contengono il tema del sito ed eventuali dipendenze di terze parti.

Le librerie lato client hanno alcune limitazioni quando si tratta di supportare linguaggi come [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Esistono diversi strumenti open-source come [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) che accelerano e ottimizzano lo sviluppo front-end. L&#39;obiettivo del modulo **ui.frontend** è quello di essere in grado di utilizzare questi strumenti per gestire la maggior parte dei file sorgente front-end.

1. Apri il modulo **ui.frontend** e passa a `src/main/webpack/site`.
1. Aprire il file `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` è il punto di ingresso di tutti i file Sass nel  `ui.frontend` modulo. Includerà il file `_variables.scss`, che contiene una serie di variabili di marchio da utilizzare in diversi file Sass del progetto. Anche il file `_base.scss` è incluso e definisce alcuni stili di base per gli elementi HTML. Un&#39;espressione regolare include tutti gli stili per i singoli stili dei componenti in `src/main/webpack/components`. Un&#39;altra espressione regolare include tutti i file sotto `src/main/webpack/site/styles`.

1. Ispeziona il file `main.ts`. `main.ts` include  `main.scss` e include un’espressione regolare per raccogliere qualsiasi  `.js` o  `.ts` file nel progetto. Questo punto di ingresso verrà utilizzato dai [file di configurazione del webpack](https://webpack.js.org/configuration/) come punto di ingresso per l&#39;intero modulo `ui.frontend`.

1. Ispeziona i file sotto `src/main/webpack/site/styles`:

   ![File di stile](assets/client-side-libraries/style-files.png)

   Questi file sono stili per elementi globali nel modello, come Intestazione, Piè di pagina e contenitore di contenuto principale. Le regole CSS in questi file hanno come target diversi elementi HTML `header`, `main` e `footer`. Questi elementi HTML sono stati definiti dai criteri nel capitolo precedente [Pagine e Modelli](./pages-templates.md).

1. Espandi la cartella `components` in `src/main/webpack` ed esamina i file.

   ![File Sass dei componenti](assets/client-side-libraries/component-sass-files.png)

   Ogni file viene mappato su un componente core come [Componente pannello a soffietto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Ogni componente di base è generato con [modificatore elemento di blocco](https://getbem.com/) o notazione BEM per facilitare il targeting di classi CSS specifiche con regole di stile. I file sotto `/components` sono stati sovrapposti da AEM Project Archetype con le diverse regole BEM per ciascun componente.

1. Scarica il file Stili di base WKND **[wknd-base-Styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** e **decomprimi**.

   ![Stili di base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Per accelerare l’esercitazione, abbiamo fornito i diversi file Sass che implementano il brand WKND basato su componenti core e sulla struttura del modello di pagina dell’articolo.

1. Sovrascrivi il contenuto di `ui.frontend/src` con i file del passaggio precedente. Il contenuto dello zip deve sovrascrivere le seguenti cartelle:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![File modificati](assets/client-side-libraries/changed-files-uifrontend.png)

   Ispeziona i file modificati per visualizzare i dettagli dell’implementazione dello stile WKND.

## Ispezionare l&#39;integrazione ui.frontend {#ui-frontend-integration}

Un elemento di integrazione chiave integrato nel modulo **ui.frontend** , [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prende gli artefatti CSS e JS compilati da un progetto webpack/npm e li trasforma in librerie lato client AEM.

![Integrazione dell&#39;architettura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Archetype imposta automaticamente questa integrazione. Poi, esplora come funziona.


1. Apri un terminale della riga di comando e installa il modulo **ui.frontend** utilizzando il comando `npm install` :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` deve essere eseguito una sola volta, dopo un nuovo clone o generazione del progetto.

1. Nello stesso terminale, genera e distribuisci il modulo **ui.frontend** utilizzando il comando `npm run dev` :

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > Potresti ricevere un errore come &quot;ERROR in ./src/main/webpack/site/main.scss&quot;.
   > Questo accade solitamente perché l&#39;ambiente è cambiato dall&#39;esecuzione di `npm install`.
   > Esegui `npm rebuild node-sass` per risolvere il problema. Questo accade se la versione di `npm` installata sul computer di sviluppo locale è diversa dalla versione utilizzata da Maven `frontend-maven-plugin` nel file `aem-guides-wknd/pom.xml`. È possibile correggere definitivamente questo problema modificando la versione nel file pom in modo che corrisponda alla versione locale o viceversa.

1. Il comando `npm run dev` deve generare e compilare il codice sorgente per il progetto Webpack e, in ultima analisi, compilare i **clientlib-site** e **clientlib-dependencies** nel modulo **ui.apps** .

   >[!NOTE]
   >
   >Esiste anche un profilo `npm run prod` che minimizzerà JS e CSS. Questa è la compilazione standard ogni volta che la build del webpack viene attivata tramite Maven. Ulteriori dettagli sul modulo [ui.frontend sono disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Ispeziona il file `site.css` sotto `ui.frontend/dist/clientlib-site/css/site.css`. Questo è il CSS compilato in base ai file di origine Sass.

   ![Css sito distribuito](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Ispeziona il file `ui.frontend/clientlib.config.js`. Questo è il file di configurazione per un plugin npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) che trasforma il contenuto di `/dist` in una libreria client e lo sposta nel modulo `ui.apps`.

1. Ispeziona il file `site.css` nel modulo **ui.apps** in `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Questa deve essere una copia identica del file `site.css` dal modulo **ui.frontend** . Ora che si trova nel modulo **ui.apps** , può essere implementato in AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Poiché **clientlib-site** viene compilato durante il tempo di compilazione, utilizzando **npm** o **maven**, può essere ignorato dal controllo del codice sorgente nel modulo **ui.apps** . Ispeziona il file `.gitignore` sotto **ui.apps**.

1. Sincronizza la libreria `clientlib-site` con un&#39;istanza locale di AEM utilizzando gli strumenti per sviluppatori o le competenze Maven.

   ![Sincr. sito clientlib](assets/client-side-libraries/sync-clientlib-site.png)

1. Apri l&#39;articolo LO Skatepark in AEM all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Stili di base aggiornati per l’articolo](assets/client-side-libraries/updated-base-styles.png)

   Ora dovresti visualizzare gli stili aggiornati per l’articolo. Potrebbe essere necessario eseguire un aggiornamento per cancellare tutti i file CSS memorizzati nella cache dal browser.

   Sta iniziando a guardare molto più vicino ai mascalzoni!

   >[!NOTE]
   >
   > I passaggi eseguiti in precedenza per generare e distribuire il codice ui.frontend ad AEM verranno eseguiti automaticamente quando una build Maven viene attivata dalla directory principale del progetto `mvn clean install -PautoInstallSinglePackage`.

>[!CAUTION]
>
> L&#39;utilizzo del modulo **ui.frontend** potrebbe non essere necessario per tutti i progetti. Il modulo **ui.frontend** aggiunge ulteriore complessità e, se non è necessario/si desidera utilizzare alcuni di questi strumenti front-end avanzati (Sass, webpack, npm..), potrebbe non essere necessario.

## Inclusione di pagine e modelli {#page-inclusion}

Ora, esaminiamo come viene fatto riferimento alle clientlibs nella pagina AEM. Una best practice comune nello sviluppo web è quella di includere i CSS nel tag HTML Header `<head>` e JavaScript immediatamente prima di chiudere il tag `</body>` .

1. Nel modulo **ui.apps** , passa a `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Componente Pagina struttura](assets/client-side-libraries/customheaderlibs-html.png)

   Si tratta del componente `page` utilizzato per il rendering di tutte le pagine nell’implementazione WKND.

1. Aprire il file `customheaderlibs.html`. Osserva le righe `${clientlib.css @ categories='wknd.base'}`. Questo indica che il CSS per clientlib con una categoria di `wknd.base` sarà incluso tramite questo file, includendo in modo efficace **clientlib-base** nell&#39;intestazione di tutte le nostre pagine.

1. Aggiorna `customheaderlibs.html` per includere un riferimento agli stili di font Google specificati in precedenza nel modulo **ui.frontend** .

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Ispeziona il file `customfooterlibs.html`. Questo file, come `customheaderlibs.html`, deve essere sovrascritto mediante l’implementazione di progetti. In questo caso la riga `${clientlib.js @ categories='wknd.base'}` indica che il JavaScript di **clientlib-base** verrà incluso nella parte inferiore di tutte le nostre pagine.

1. Esporta il componente `page` nel server AEM utilizzando gli strumenti per sviluppatori o le tue competenze Maven.

1. Passa al modello Pagina articolo all&#39;indirizzo [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Fai clic sull&#39;icona **Informazioni pagina** e nel menu seleziona **Criterio pagina** per aprire la finestra di dialogo **Criterio pagina**.

   ![Criterio pagina del menu del modello di pagina dell’articolo](assets/client-side-libraries/template-page-policy.png)

   *Informazioni pagina > Criterio pagina*

1. Si noti che le categorie per `wknd.dependencies` e `wknd.site` sono elencate qui. Per impostazione predefinita, le clientlibs configurate tramite i Criteri di pagina sono suddivise in modo da includere il CSS nell’intestazione della pagina e il JavaScript all’estremità del corpo. Se lo desideri, puoi elencare esplicitamente che il JavaScript clientlib deve essere caricato nell’intestazione della pagina. Questo è il caso di `wknd.dependencies`.

   ![Criterio pagina del menu del modello di pagina dell’articolo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > È anche possibile fare riferimento direttamente al `wknd.site` o `wknd.dependencies` dal componente della pagina, utilizzando lo script `customheaderlibs.html` o `customfooterlibs.html`, come abbiamo visto in precedenza per la clientlib `wknd.base`. L’utilizzo del modello offre una certa flessibilità in che consente di scegliere e scegliere quali clientlibs vengono utilizzati per modello. Ad esempio, se disponi di una libreria JavaScript molto pesante che verrà utilizzata solo su un modello selezionato.

1. Passa alla pagina **LA Skatepark** creata utilizzando il **Modello pagina articolo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Dovresti vedere una differenza nei font.

1. Fai clic sull&#39;icona **Informazioni pagina** e nel menu seleziona **Visualizza come pubblicato** per aprire la pagina dell&#39;articolo all&#39;esterno dell&#39;editor AEM.

   ![Visualizza come pubblicato](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizza l&#39;origine pagina di [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e dovresti essere in grado di visualizzare i seguenti riferimenti clientlib nel `<head>`:

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   Tieni presente che le clientlibs utilizzano l’endpoint proxy `/etc.clientlibs` . Dovresti anche vedere i seguenti inclusioni clientlib nella parte inferiore della pagina:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Se segue la versione 6.5/6.4, le librerie lato client non verranno minimizzate automaticamente. Per abilitare la minimizzazione (consigliato)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors), consulta la documentazione su [Gestione libreria HTML .

   >[!WARNING]
   >
   >Dal lato della pubblicazione è fondamentale che le librerie client siano **non** servite da **/apps** in quanto questo percorso deve essere limitato per motivi di sicurezza utilizzando la sezione [Filtro Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [proprietà allowProxy](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) della libreria client assicura che i CSS e JS siano serviti da **/etc.clientlibs**.

## Webpack DevServer - Markup statico {#webpack-dev-static}

Nel paio di esercizi precedenti siamo stati in grado di aggiornare diversi file Sass nel modulo **ui.frontend** e attraverso un processo di compilazione, vedere alla fine queste modifiche riflesse in AEM. Ora esamineremo le tecniche che sfruttano un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) per sviluppare rapidamente i nostri stili front-end rispetto a **static** HTML.

Questa tecnica è utile se la maggior parte degli stili e del codice front-end viene eseguita da uno sviluppatore Front End dedicato che potrebbe non avere un facile accesso a un ambiente AEM. Questa tecnica consente anche al FED di apportare modifiche direttamente al codice HTML, che può quindi essere consegnato a uno sviluppatore AEM per l’implementazione come componenti.

1. Copia la pagina sorgente della pagina dell&#39;articolo sullo skatepark LA all&#39;indirizzo [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Riapri l’IDE. Incolla il markup copiato da AEM nel modulo `index.html` nel modulo **ui.frontend** sotto `src/main/webpack/static`.
1. Modifica il markup copiato e rimuovi eventuali riferimenti a **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Possiamo rimuovere questi riferimenti perché il server di sviluppo del webpack genererà questi artefatti automaticamente.

1. Avviare il server di sviluppo del webpack da un nuovo terminale eseguendo il seguente comando dall&#39;interno del modulo **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Questo dovrebbe aprire una nuova finestra del browser in [http://localhost:8080/](Http://localhost:8080/) con marcatura statica.

1. Modifica il file `src/main/webpack/site/_variables.scss`. Sostituisci la regola `$text-color` con quanto segue:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salva le modifiche.

1. Dovresti visualizzare automaticamente le modifiche riportate automaticamente nel browser su [http://localhost:8080](Http://localhost:8080).

   ![Modifiche al server di sviluppo del webpack locale](assets/client-side-libraries/local-webpack-dev-server.png)

1. Esamina il file `/aem-guides-wknd.ui.frontend/webpack.dev.js` . Contiene la configurazione del webpack utilizzata per avviare il webpack-dev-server. Si noti che proxy i percorsi `/content` e `/etc.clientlibs` da un&#39;istanza di AEM in esecuzione locale. In questo modo vengono rese disponibili le immagini e altre clientlib (non gestite dal codice **ui.frontend** ).

   >[!CAUTION]
   >
   > L’immagine src del markup statico punta a un componente immagine live su un’istanza AEM locale. Le immagini appariranno interrotte se il percorso dell’immagine cambia, se AEM non viene avviato o se il browser non ha effettuato l’accesso all’istanza AEM locale. Se si passa a una risorsa esterna, è anche possibile sostituire le immagini con riferimenti statici.

1. È possibile **arrestare** il server webpack dalla riga di comando digitando `CTRL+C`.

## Webpack DevServer - Watch e aemsync {#webpack-dev-watch}

Un&#39;altra tecnica è quella di far sì che Node.js controlli eventuali modifiche ai file src nel modulo `ui.frontend`. Ogni volta che un file cambia, compila rapidamente la libreria client e utilizza il modulo [aemsync](https://www.npmjs.com/package/aemsync) npm per sincronizzare le modifiche a un server AEM in esecuzione.

1. Avviare il server di sviluppo del webpack in modalità **watch** da un nuovo terminale eseguendo il seguente comando dall&#39;interno del modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. Questo compilerà i file `src` e sincronizzerà le modifiche con AEM all&#39;indirizzo [http://localhost:4502](Http://localhost:4502)

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. Passa all&#39;articolo AEM e LA Skatepark: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Modifiche implementate in AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Le modifiche devono essere distribuite in AEM. Si verifica un leggero ritardo e dovrai aggiornare il browser manualmente per vedere gli aggiornamenti. Tuttavia, la visualizzazione delle modifiche direttamente in AEM è utile se lavori con nuovi componenti e con l’authoring tramite finestra di dialogo.

1. Ripristinare la modifica in `_variables.scss` e salvare le modifiche. Le modifiche devono essere nuovamente sincronizzate con l’istanza locale di AEM dopo un leggero ritardo.

1. Arrestare il server di sviluppo del webpack ed eseguire una build Maven completa dalla radice del progetto:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Anche in questo caso il modulo `ui.frontend` viene compilato, trasformato in clientlibrerie e implementato in AEM tramite il modulo `ui.apps` . Ma questa volta Maven esegue tutto per noi.

## Congratulazioni! {#congratulations}

Congratulazioni, la pagina Articolo ora ha alcuni stili coerenti che corrispondono al marchio WKND e hai acquisito familiarità con il modulo **ui.frontend**!

### Passaggi successivi {#next-steps}

Scopri come implementare i singoli stili e riutilizzare i componenti core utilizzando il sistema di stili di Experience Manager. [Lo sviluppo con il ](style-system.md) sistema di stili prevede l’utilizzo del sistema di stili per estendere i componenti core con configurazioni CSS specifiche per il marchio e criteri avanzati dell’Editor modelli.

Visualizza il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente in sulla barra Git `tutorial/client-side-libraries-solution`.

1. Clona il repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Estrai il ramo `tutorial/client-side-libraries-solution`.

## Strumenti e risorse aggiuntivi {#additional-resources}

### aemfeed {#develop-aemfed}

[****](https://aemfed.io/) aemfedis è uno strumento open-source a riga di comando che può essere utilizzato per accelerare lo sviluppo front-end. È alimentato da [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) e [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

Ad un livello elevato **aemfed** è progettato per ascoltare le modifiche ai file all&#39;interno del modulo **ui.apps** e sincronizzarle automaticamente direttamente in un&#39;istanza AEM in esecuzione. In base alle modifiche, un browser locale si aggiorna automaticamente, accelerando così lo sviluppo front-end. È inoltre progettato per lavorare con Sling Log Tracer per visualizzare automaticamente eventuali errori lato server direttamente nel terminale.

Se lavori molto nel modulo **ui.apps**, modifichi gli script HTL e crea componenti personalizzati, **aemfeed** può essere uno strumento molto potente da utilizzare. [La documentazione completa è disponibile qui.](https://github.com/abmaonline/aemfed).

### Debug delle librerie lato client {#debugging-clientlibs}

Con diversi metodi di **categories** e **incorpora** per includere più librerie client, può essere difficile risolvere i problemi. AEM espone diversi strumenti per aiutarti. Uno degli strumenti più importanti è **Ricostruire le librerie client** che forzerà AEM a ricompilare qualsiasi file LESS e a generare il CSS.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - Elenca tutte le librerie client registrate nell&#39;istanza AEM.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Output del test**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : consente a un utente di visualizzare l&#39;output HTML previsto di clientlib include in base alla categoria.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Convalida delle dipendenze delle librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : evidenzia tutte le dipendenze o le categorie incorporate che non è possibile trovare.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rigenera librerie client**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : consente a un utente di forzare AEM a ricostruire tutte le librerie client o di annullare la validità della cache delle librerie client. Questo strumento è particolarmente efficace quando si sviluppa con Meno, in quanto può forzare AEM a ricompilare i CSS generati. In generale, è più efficace annullare la validità delle cache, quindi eseguire un aggiornamento della pagina anziché ricostruire tutte le librerie. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![rigenerare la libreria client](assets/client-side-libraries/rebuild-clientlibs.png)
