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
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '3291'
ht-degree: 3%

---


# Librerie lato client e flusso di lavoro front-end {#client-side-libraries}

Scoprite come le librerie o i clientlibs lato client vengono utilizzati per distribuire e gestire CSS e Javascript per un&#39;implementazione di Adobe Experience Manager (AEM) Sites. Questa esercitazione illustra inoltre come il modulo [ui.frontend](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html), un progetto [webpack ](https://webpack.js.org/) sganciato, possa essere integrato nel processo di compilazione end-to-end.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

È inoltre consigliabile rivedere l&#39;esercitazione [Component Basics](component-basics.md#client-side-libraries) per comprendere le nozioni di base delle librerie e delle AEM lato client.

### Progetto iniziale

>[!NOTE]
>
> Se il capitolo precedente è stato completato correttamente, è possibile riutilizzare il progetto e ignorare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Estrarre il ramo `tutorial/client-side-libraries-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzate AEM 6.5 o 6.4, aggiungete il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o estrarre il codice localmente passando al ramo `tutorial/client-side-libraries-solution`.

## Obiettivo

1. Comprendere in che modo le librerie lato client vengono incluse in una pagina tramite un modello modificabile.
1. Scopri come utilizzare il modulo UI.Frontend e un server di sviluppo webpack per lo sviluppo front-end dedicato.
1. Comprendere il flusso di lavoro end-to-end per la distribuzione di CSS e JavaScript compilati a un&#39;implementazione di Sites.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo verranno aggiunti alcuni stili di base per il sito WKND e per il modello di pagina dell&#39;articolo, allo scopo di avvicinare l&#39;implementazione ai [modelli di progettazione dell&#39;interfaccia utente](assets/pages-templates/wknd-article-design.xd). Utilizzerete un flusso di lavoro front-end avanzato per integrare un progetto webpack in una libreria client AEM.

![Stili finiti](assets/client-side-libraries/finished-styles.png)

*Pagina articolo con stili di linea applicati*

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

1. Utilizzando VSCode o un altro IDE, aprite il modulo **ui.apps**.
1. Espandete il percorso `/apps/wknd/clientlibs` per visualizzare i clientlibs generati dall&#39;archetype.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Esamineremo questi clientlibs più in dettaglio qui di seguito.

1. Nella tabella seguente sono riepilogate le librerie client. Maggiori dettagli su [comprese le librerie client sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrizione | Note |
   |-------------------| ------------| ------|
   | `clientlib-base` | Livello di base di CSS e JavaScript necessari per il funzionamento del sito WKND | incorpora le librerie client dei componenti core |
   | `clientlib-grid` | Genera il CSS necessario per il funzionamento di [Modalità layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html). | I punti di interruzione Mobile/Tablet possono essere configurati qui |
   | `clientlib-site` | Contiene un tema specifico per il sito WKND | Generato dal modulo `ui.frontend` |
   | `clientlib-dependencies` | Incorpora eventuali dipendenze di terze parti | Generato dal modulo `ui.frontend` |

1. Tenere presente che `clientlib-site` e `clientlib-dependencies` vengono ignorati dal controllo del codice sorgente. Questo avviene per progettazione, in quanto verranno generati in fase di creazione dal modulo `ui.frontend`.

## Aggiorna stili di base {#base-styles}

Quindi, aggiornate gli stili di base definiti nel modulo **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. I file nel modulo `ui.frontend` genereranno le librerie `clientlib-site` e `clientlib-dependecies` che contengono il tema del sito ed eventuali dipendenze di terze parti.

Le librerie lato client presentano alcune limitazioni per quanto riguarda il supporto di linguaggi come [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Esistono diversi strumenti open-source come [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) che accelerano e ottimizzano lo sviluppo front-end. L&#39;obiettivo del modulo **ui.frontend** è quello di poter utilizzare questi strumenti per gestire la maggior parte dei file sorgente front-end.

1. Aprire il modulo **ui.frontend** e passare a `src/main/webpack/site`.
1. Aprire il file `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` è il punto di ingresso di tutti i file Sass nel  `ui.frontend` modulo. Includerà il file `_variables.scss`, che contiene una serie di variabili di marchio da utilizzare nei diversi file Sass del progetto. Il file `_base.scss` è incluso e definisce alcuni stili di base per gli elementi HTML. Un&#39;espressione regolare include tutti gli stili per i singoli stili di componenti in `src/main/webpack/components`. Un&#39;altra espressione regolare include tutti i file in `src/main/webpack/site/styles`.

1.  Inspect il file `main.ts`. `main.ts` include  `main.scss` e include un&#39;espressione regolare per raccogliere qualsiasi  `.js` o  `.ts` file nel progetto. Questo punto di ingresso verrà utilizzato dai file di configurazione [webpack](https://webpack.js.org/configuration/) come punto di ingresso per l&#39;intero modulo `ui.frontend`.

1.  Inspect i file sotto `src/main/webpack/site/styles`:

   ![File di stile](assets/client-side-libraries/style-files.png)

   Questi stili di file per gli elementi globali nel modello, come l’intestazione, il piè di pagina e il contenitore del contenuto principale. Le regole CSS in questi file hanno come target diversi elementi HTML `header`, `main` e `footer`. Questi elementi HTML sono stati definiti dai criteri nel capitolo precedente [Pagine e Modelli](./pages-templates.md).

1. Espandete la cartella `components` in `src/main/webpack` ed esaminate i file.

   ![File Sass componenti](assets/client-side-libraries/component-sass-files.png)

   Ogni file viene mappato su un componente core come il [componente Accordion](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Ogni componente di base è creato con la notazione [Modificatore elemento blocco](https://getbem.com/) o BEM per facilitare il targeting di classi CSS specifiche con regole di stile. I file sotto `/components` sono stati sovrapposti dal AEM Project Archetype con le diverse regole BEM per ciascun componente.

1. Scaricate il file di stili di base WKND **[wknd-base-Styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** e **decomprimete**.

   ![Stili base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Per accelerare l&#39;esercitazione, abbiamo fornito i diversi file Sass che implementano il marchio WKND basato su Componenti di base e la struttura del Modello pagina articolo.

1. Sovrascrivete il contenuto di `ui.frontend/src` con i file del passaggio precedente. Il contenuto del file ZIP deve sovrascrivere le cartelle seguenti:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![File modificati](assets/client-side-libraries/changed-files-uifrontend.png)

    Inspect i file modificati per visualizzare i dettagli dell&#39;implementazione dello stile WKND.

##  l&#39;integrazione ui.frontend {#ui-frontend-integration} di Inspect

Un elemento di integrazione chiave integrato nel modulo **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prende gli artifact CSS e JS compilati da un progetto webpack/npm e li trasforma in AEM librerie lato client.

![Integrazione con l&#39;architettura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Archetype imposta automaticamente questa integrazione. Quindi, esplora come funziona.


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
   ```

   >[!CAUTION]
   >
   > Potreste ricevere un errore come &quot;ERROR in ./src/main/webpack/site/main.scss&quot;.
   > In genere questo accade perché l&#39;ambiente è cambiato dall&#39;esecuzione di `npm install`.
   > Eseguire `npm rebuild node-sass` per risolvere il problema. Ciò si verificherà se la versione di `npm` installata nel computer locale deve essere diversa dalla versione utilizzata da Maven `frontend-maven-plugin` nel file `aem-guides-wknd/pom.xml`. È possibile risolvere definitivamente questo problema modificando la versione nel file pom in modo che corrisponda alla versione locale o viceversa.

1. Il comando `npm run dev` deve creare e compilare il codice sorgente per il progetto Webpack e compilare infine il **clientlib-site** e **clientlib-dependencies** nel modulo **ui.apps**.

   >[!NOTE]
   >
   >È inoltre disponibile un profilo `npm run prod` che minificherà JS e CSS. Questa è la compilazione standard ogni volta che la build webpack viene attivata tramite Maven. Ulteriori dettagli sul modulo [ui.frontend sono disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1.  Inspect il file `site.css` sotto `ui.frontend/dist/clientlib-site/css/site.css`. Si tratta del CSS compilato in base ai file sorgente Sass.

   ![Cs sito distribuito](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1.  Inspect il file `ui.frontend/clientlib.config.js`. Si tratta del file di configurazione per un plugin npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) che trasforma il contenuto di `/dist` in una libreria client e lo sposta nel modulo `ui.apps`.

1.  Inspect il file `site.css` nel modulo **ui.apps** all&#39;indirizzo `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Deve trattarsi di una copia identica del file `site.css` dal modulo **ui.frontend**. Ora che si trova nel modulo **ui.apps** è possibile distribuirlo per AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Poiché **clientlib-site** viene compilata durante il tempo di creazione, utilizzando **npm** o **maven**, può essere ignorata dal controllo del codice sorgente nel modulo **ui.apps**.  Inspect il file `.gitignore` sotto **ui.apps**.

1. Sincronizzate la libreria `clientlib-site` con un&#39;istanza locale di AEM utilizzando gli strumenti di sviluppo o le competenze Maven.

   ![Sincronizza sito Clientlib](assets/client-side-libraries/sync-clientlib-site.png)

1. Aprite l&#39;articolo LA Skatepark in AEM all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Stili di base aggiornati per l&#39;articolo](assets/client-side-libraries/updated-base-styles.png)

   A questo punto dovrebbero essere visualizzati gli stili aggiornati per l&#39;articolo. Per cancellare tutti i file CSS memorizzati nella cache dal browser potrebbe essere necessario eseguire un aggiornamento rigido.

   Sta iniziando a guardare molto più vicino ai modelli!

   >[!NOTE]
   >
   > I passaggi eseguiti sopra per creare e distribuire il codice ui.frontend da AEM verranno eseguiti automaticamente quando una build Maven viene attivata dalla radice del progetto `mvn clean install -PautoInstallSinglePackage`.

>[!CAUTION]
>
> L&#39;utilizzo del modulo **ui.frontend** potrebbe non essere necessario per tutti i progetti. Il modulo **ui.frontend** aggiunge ulteriore complessità e se non è necessario/si desidera utilizzare alcuni di questi strumenti front-end avanzati (Sass, webpack, npm...) potrebbe non essere necessario.

## Inclusione di pagine e modelli {#page-inclusion}

Quindi, vediamo in che modo i clientlibs vengono citati nella pagina AEM. Una procedura ottimale comune nello sviluppo Web consiste nell&#39;includere CSS nell&#39;intestazione HTML `<head>` e in JavaScript immediatamente prima della chiusura del tag `</body>`.

1. Nel modulo **ui.apps** andate a `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Componente pagina struttura](assets/client-side-libraries/customheaderlibs-html.png)

   Si tratta del componente `page` utilizzato per eseguire il rendering di tutte le pagine nell&#39;implementazione WKND.

1. Aprire il file `customheaderlibs.html`. Osservate le righe `${clientlib.css @ categories='wknd.base'}`. Questo indica che il CSS per clientlib con una categoria di `wknd.base` sarà incluso tramite questo file, includendo in modo efficace **clientlib-base** nell&#39;intestazione di tutte le nostre pagine.

1. Aggiornate `customheaderlibs.html` per includere un riferimento agli stili di font Google precedentemente specificati nel modulo **ui.frontend**.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1.  Inspect il file `customfooterlibs.html`. Questo file, come `customheaderlibs.html`, deve essere sovrascritto mediante l&#39;implementazione di progetti. Qui la riga `${clientlib.js @ categories='wknd.base'}` indica che il codice JavaScript di **clientlib-base** sarà incluso nella parte inferiore di tutte le nostre pagine.

1. Esportate il componente `page` nel server AEM utilizzando gli strumenti di sviluppo o le competenze Maven.

1. Andate al modello Pagina articolo all&#39;indirizzo [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Fare clic sull&#39;icona **Informazioni pagina** e, nel menu, selezionare **Criteri pagina** per aprire la finestra di dialogo **Criteri pagina**.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy.png)

   *Informazioni pagina > Criteri pagina*

1. Tenere presente che le categorie per `wknd.dependencies` e `wknd.site` sono elencate qui. Per impostazione predefinita, i clientlibs configurati tramite il criterio pagina sono suddivisi per includere il CSS nell&#39;intestazione della pagina e il codice JavaScript all&#39;estremità del corpo. Se desiderato, potete elencare esplicitamente che clientlib JavaScript deve essere caricato nell&#39;intestazione Pagina. Questo è il caso di `wknd.dependencies`.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > È inoltre possibile fare riferimento direttamente allo script `wknd.site` o `wknd.dependencies` dal componente della pagina, utilizzando lo script `customheaderlibs.html` o `customfooterlibs.html`, come abbiamo visto in precedenza per la clientlib `wknd.base`. L’utilizzo del modello offre una certa flessibilità in quanto consente di scegliere e scegliere quali clientlibs vengono utilizzati per ogni modello. Ad esempio, se disponete di una libreria JavaScript molto pesante che verrà utilizzata solo su un modello selezionato.

1. Andate alla pagina **LA Skateparks** creata utilizzando il **Modello pagina articolo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Dovrebbe essere visibile una differenza nei font.

1. Fate clic sull&#39;icona **Informazioni pagina** e, nel menu, selezionate **Visualizza come pubblicato** per aprire la pagina dell&#39;articolo all&#39;esterno dell&#39;editor AEM.

   ![Visualizza come pubblicato](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizzare l&#39;origine pagina di [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e dovrebbe essere possibile visualizzare i seguenti riferimenti clientlib in `<head>`:

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

   Tenere presente che i clientlibs utilizzano l&#39;endpoint proxy `/etc.clientlibs`. Dovresti anche vedere le seguenti clientlib incluse nella parte inferiore della pagina:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Se si segue il 6.5/6.4, le librerie lato client non verranno ridotte automaticamente. Per abilitare la funzionalità di gestione (consigliato), consultare la documentazione in [HTML Library Manager (Gestione libreria HTML)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Dal lato della pubblicazione è fondamentale che le librerie client siano **non** servite da **/apps** in quanto il percorso deve essere limitato per motivi di sicurezza utilizzando la sezione [Dispatcher filter](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [proprietà allowProxy](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) della libreria client garantisce che i file CSS e JS vengano serviti da **/etc.clientlibs**.

## Webpack DevServer - Marcatura statica {#webpack-dev-static}

Nel paio di esercizi precedenti siamo stati in grado di aggiornare diversi file Sass nel modulo **ui.frontend** e attraverso un processo di compilazione, alla fine vedere queste modifiche riflesse in AEM. A questo punto, verranno esaminate le tecniche che sfruttano un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) per sviluppare rapidamente i nostri stili front-end rispetto a **static** HTML.

Questa tecnica è utile se la maggior parte degli stili e del codice front-end sarà eseguita da uno sviluppatore Front End dedicato che potrebbe non avere un accesso facile a un ambiente AEM. Questa tecnica consente inoltre alla FED di apportare modifiche direttamente all’HTML, che può quindi essere distribuito a uno sviluppatore AEM da implementare come componenti.

1. Copiate l&#39;origine della pagina dell&#39;articolo dello skatepark della LA all&#39;indirizzo [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Apri di nuovo l’IDE. Incollare la marcatura copiata da AEM nel modulo **ui.frontend** sotto `src/main/webpack/static`.`index.html`
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

1. Avviare il server di sviluppo webpack da un nuovo terminale eseguendo il comando seguente dall&#39;interno del modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Questa opzione consente di aprire una nuova finestra del browser in [http://localhost:8080/](Http://localhost:8080/) con tag statici.

1. Modificate il file `src/main/webpack/site/_variables.scss`. Sostituite la regola `$text-color` con quanto segue:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salva le modifiche.

1. Le modifiche dovrebbero essere visualizzate automaticamente nel browser su [http://localhost:8080](Http://localhost:8080).

   ![Modifiche al server di dev del webpack locale](assets/client-side-libraries/local-webpack-dev-server.png)

1. Rivedete il file `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contiene la configurazione del webpack utilizzata per avviare il webpack-dev-server. Si noti che esegue il proxy dei percorsi `/content` e `/etc.clientlibs` da un&#39;istanza di AEM eseguita localmente. In questo modo vengono rese disponibili le immagini e altri clientlibs (non gestiti dal codice **ui.frontend**).

   >[!CAUTION]
   >
   > L’immagine src della marcatura statica punta a un componente immagine live in un’istanza AEM locale. Le immagini risulteranno interrotte se il percorso dell&#39;immagine cambia, se AEM non viene avviato o se il browser non ha eseguito l&#39;accesso all&#39;istanza AEM locale. Se si passa a una risorsa esterna, è anche possibile sostituire le immagini con riferimenti statici.

1. È possibile **arrestare** il server webpack dalla riga di comando digitando `CTRL+C`.

## Webpack DevServer - Watch e aemsync {#webpack-dev-watch}

Un&#39;altra tecnica consiste nel fare in modo che Node.js controlli le eventuali modifiche apportate ai file src nel modulo `ui.frontend`. Ogni volta che un file cambia, compilerà rapidamente la libreria client e utilizzerà il modulo [aemsync](https://www.npmjs.com/package/aemsync) npm per sincronizzare le modifiche a un server AEM in esecuzione.

1. Avviare il server di sviluppo webpack in modalità **watch** da un nuovo terminale eseguendo il comando seguente dall&#39;interno del modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. In questo modo, i file `src` vengono compilati e sincronizzati con AEM in [http://localhost:4502](Http://localhost:4502)

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

1. Andate all&#39;AEM e all&#39;articolo LO Skateparks: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Modifiche distribuite a AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Le modifiche devono essere distribuite in AEM. Si verifica un leggero ritardo e sarà necessario aggiornare il browser manualmente per vedere gli aggiornamenti. Tuttavia, la visualizzazione delle modifiche direttamente in AEM è utile se lavorate con nuovi componenti e con l’authoring tramite finestra di dialogo.

1. Ripristinare la modifica in `_variables.scss` e salvare le modifiche. Le modifiche devono essere nuovamente sincronizzate con l’istanza locale di AEM dopo un leggero ritardo.

1. Arrestate il server di sviluppo webpack ed eseguite una build Maven completa dalla radice del progetto:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Anche in questo caso il modulo `ui.frontend` viene compilato, trasformato in clientlibraries e distribuito per AEM tramite il modulo `ui.apps`. Ma questa volta Maven esegue tutto per noi.

## Congratulazioni! {#congratulations}

Congratulazioni, la pagina dell&#39;articolo ora ha alcuni stili coerenti che corrispondono al marchio WKND e si è familiarizzati con il modulo **ui.frontend**!

### Passaggi successivi {#next-steps}

Scopri come implementare singoli stili e riutilizzare i componenti core con  Experience Manager  Style System. [Lo sviluppo con Style ](style-system.md) Systemcover utilizzando Style System per estendere i componenti core con CSS specifici del marchio e configurazioni avanzate di criteri dell&#39;Editor modelli.

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in corrispondenza del blocco Git `tutorial/client-side-libraries-solution`.

1. Duplicare il repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `tutorial/client-side-libraries-solution`.

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
