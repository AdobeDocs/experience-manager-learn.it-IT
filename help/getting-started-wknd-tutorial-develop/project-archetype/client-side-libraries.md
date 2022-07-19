---
title: Librerie lato client e flusso di lavoro front-end
description: Scopri come utilizzare le librerie lato client o le librerie client per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites . Scopri come il modulo ui.frontend, un progetto webpack, può essere integrato nel processo di compilazione end-to-end.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '2878'
ht-degree: 2%

---

# Librerie lato client e flusso di lavoro front-end {#client-side-libraries}

Scopri come le librerie lato client o le librerie client vengono utilizzate per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites. Questa esercitazione descriverà anche come [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) un modulo disaccoppiato [webpack](https://webpack.js.org/) può essere integrato nel processo di generazione end-to-end.

## Prerequisiti {#prerequisites}

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

Si consiglia inoltre di rivedere il [Nozioni di base sui componenti](component-basics.md#client-side-libraries) esercitazione per comprendere i principi di base delle librerie e dei AEM lato client.

### Progetto iniziale

>[!NOTE]
>
> Se hai completato con successo il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Consulta la sezione `tutorial/client-side-libraries-start` ramo [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi la variabile `classic` su qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o controlla il codice localmente passando al ramo `tutorial/client-side-libraries-solution`.

## Obiettivi

1. Scopri come le librerie lato client vengono incluse in una pagina tramite un modello modificabile.
1. Scopri come utilizzare il modulo UI.Frontend e un server di sviluppo webpack per lo sviluppo front-end dedicato.
1. Comprendi il flusso di lavoro end-to-end della distribuzione di CSS e JavaScript compilati a un’implementazione di Sites.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo verranno aggiunti alcuni stili di base per il sito WKND e il modello di pagina dell’articolo, al fine di avvicinare l’implementazione a [Moduli di progettazione dell’interfaccia utente](assets/pages-templates/wknd-article-design.xd). Utilizza un flusso di lavoro front-end avanzato per integrare un progetto webpack in una libreria client AEM.

![Stili completati](assets/client-side-libraries/finished-styles.png)

*Pagina articolo con stili di linea di base applicati*

## Informazioni di base {#background}

Le librerie lato client forniscono un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un’implementazione AEM Sites. Gli obiettivi di base per le librerie lato client o le librerie client sono:

1. Archiviare CSS/JS in piccoli file discreti per facilitarne lo sviluppo e la manutenzione
1. Gestire le dipendenze da framework di terze parti in modo organizzato
1. Riduci al minimo il numero di richieste lato client concatenando CSS/JS in una o due richieste.

Ulteriori informazioni sull&#39;utilizzo [Le librerie lato client si trovano qui.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

Le librerie lato client presentano alcune limitazioni. In particolare, il supporto limitato per i linguaggi front-end popolari come Sass, LESS e TypeScript. Nell’esercitazione verrà illustrato come **ui.frontend** modulo può essere utile per risolvere questo problema.

Distribuisci la base di codice iniziale in un&#39;istanza AEM locale e passa a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Questa pagina non è attualmente formattata. Implementeremo quindi le librerie lato client per il marchio WKND per aggiungere CSS e Javascript alla pagina.

## Organizzazione delle librerie lato client {#organization}

Ora esploreremo l&#39;organizzazione di clientlibs generati da [Archetipo di progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![Organizzazione di librerie client di alto livello](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramma di alto livello Organizzazione della libreria lato client e inclusione della pagina*

>[!NOTE]
>
> La seguente organizzazione di librerie lato client viene generata da AEM Project Archetype, ma rappresenta solo un punto di partenza. Il modo in cui un progetto gestisce e distribuisce CSS e JavaScript a un’implementazione di Sites può variare notevolmente in base a risorse, competenze e requisiti.

1. Utilizzando VSCode o un altro IDE, apri il **ui.apps** modulo .
1. Espandi il percorso `/apps/wknd/clientlibs` per visualizzare le clientlibs generate dall’archetipo.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Esamineremo questi clientlibs in modo più dettagliato di seguito.

1. Nella tabella seguente sono riepilogate le librerie client. Maggiori dettagli [qui puoi trovare anche le librerie client](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrizione | Note |
   |-------------------| ------------| ------|
   | `clientlib-base` | Livello di base di CSS e JavaScript necessari per il funzionamento del sito WKND | incorpora le librerie client dei componenti core |
   | `clientlib-grid` | Genera il CSS necessario per [Modalità Layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) per lavorare. | I punti di interruzione Mobile/Tablet possono essere configurati qui |
   | `clientlib-site` | Contiene un tema specifico per il sito WKND | Generato dal `ui.frontend` modulo |
   | `clientlib-dependencies` | Incorpora tutte le dipendenze di terze parti | Generato dal `ui.frontend` modulo |

1. Osserva che `clientlib-site` e `clientlib-dependencies` vengono ignorati dal controllo del codice sorgente. Questo è di progettazione, in quanto verranno generate in fase di creazione dal `ui.frontend` modulo .

## Aggiorna stili di base {#base-styles}

Quindi, aggiorna gli stili di base definiti in **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** modulo . I file nel `ui.frontend` il modulo genera `clientlib-site` e `clientlib-dependecies` librerie che contengono il tema del sito ed eventuali dipendenze di terze parti.

Le librerie lato client hanno alcune limitazioni quando si tratta di supportare lingue come [Squalo](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Ci sono diversi strumenti open-source come [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) che accelera e ottimizza lo sviluppo front-end. L&#39;obiettivo del **ui.frontend** Il modulo deve essere in grado di utilizzare questi strumenti per gestire la maggior parte dei file sorgente front-end.

1. Apri **ui.frontend** modulo e passa a `src/main/webpack/site`.
1. Aprire il file `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)
librerie lato client/main-scss

   `main.scss` è il punto di ingresso di tutti i file Sass nel `ui.frontend` modulo . Includerà `_variables.scss` , che contiene una serie di variabili di marchio da utilizzare in diversi file Sass del progetto. La `_base.scss` Il file è incluso e definisce alcuni stili di base per gli elementi di HTML. Un&#39;espressione regolare include tutti gli stili per i singoli stili dei componenti in `src/main/webpack/components`. Un&#39;altra espressione regolare include tutti i file in `src/main/webpack/site/styles`.

1. Inspect il file `main.ts`. Include `main.scss` e un&#39;espressione regolare per raccogliere qualsiasi `.js` o `.ts` nel progetto. Questo punto di ingresso verrà utilizzato dal [file di configurazione del webpack](https://webpack.js.org/configuration/) come punto di ingresso per l&#39;intero `ui.frontend` modulo .

1. Inspect i file sottostanti `src/main/webpack/site/styles`:

   ![File di stile](assets/client-side-libraries/style-files.png)

   Questi file sono stili per elementi globali nel modello, come Intestazione, Piè di pagina e contenitore di contenuto principale. Le regole CSS in questi file hanno come target diversi elementi di HTML `header`, `main`e  `footer`. Questi elementi HTML sono stati definiti dai criteri del capitolo precedente [Pagine e modelli](./pages-templates.md).

1. Espandi la `components` cartella sotto `src/main/webpack` e ispezionare i file.

   ![File Sass dei componenti](assets/client-side-libraries/component-sass-files.png)

   Ogni file viene mappato su un componente core come [Componente a soffietto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Ogni componente core è generato con [Modificatore elemento blocco](https://getbem.com/) o notazione BEM per facilitare il targeting di classi CSS specifiche con regole di stile. I file sottostanti `/components` sono stati inseriti dall’Archetipo di progetto AEM con le diverse regole BEM per ciascun componente.

1. Download degli stili di base WKND **[wknd-base-Styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** e **decomprimere** il file.

   ![Stili di base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Per accelerare l’esercitazione, abbiamo fornito i diversi file Sass che implementano il brand WKND basato su componenti core e sulla struttura del modello di pagina dell’articolo.

1. Sovrascrivi il contenuto di `ui.frontend/src` con file del passaggio precedente. Il contenuto dello zip deve sovrascrivere le seguenti cartelle:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![File modificati](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect ha modificato i file per visualizzare i dettagli dell’implementazione dello stile WKND.

## Integrazione di Inspect con ui.frontend {#ui-frontend-integration}

Un elemento di integrazione chiave integrato nel **ui.frontend** modulo, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prende gli artefatti CSS e JS compilati da un progetto webpack/npm e li trasforma in librerie lato client AEM.

![Integrazione dell&#39;architettura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

Il AEM Project Archetype imposta automaticamente questa integrazione. Poi, esplora come funziona.


1. Aprire un terminale della riga di comando e installare **ui.frontend** modulo che utilizza `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` deve essere eseguito una sola volta, dopo un nuovo clone o generazione del progetto.

1. Avvia il server di sviluppo webpack in **guardare** eseguendo il comando seguente:

   ```shell
   $ npm run watch
   ```

1. In questo modo verrà compilato il `src` nei file `ui.frontend` modulo e sincronizza le modifiche con AEM in [http://localhost:4502](Http://localhost:4502)

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

1. Comando `npm run watch` in ultima analisi compila **clientlib-site** e **clientlib-dipendenze** in **ui.apps** che viene quindi sincronizzato automaticamente con AEM.

   >[!NOTE]
   >
   >C&#39;è anche un `npm run prod` che minimizza JS e CSS. Questa è la compilazione standard ogni volta che la build del webpack viene attivata tramite Maven. Maggiori dettagli sulla [modulo ui.frontend disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect il file `site.css` sotto `ui.frontend/dist/clientlib-site/site.css`. Questo è il CSS compilato in base ai file di origine Sass.

   ![Css sito distribuito](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect il file `ui.frontend/clientlib.config.js`. Questo è il file di configurazione per un plugin npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) che trasforma il contenuto di `/dist` in una libreria client e la sposta in `ui.apps` modulo .

1. Inspect il file `site.css` in **ui.apps** modulo a `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Questa dovrebbe essere una copia identica del `site.css` dal **ui.frontend** modulo . Ora che è **ui.apps** modulo che può essere distribuito a AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Da **clientlib-site** viene compilato durante la generazione, utilizzando **npm** o **Maven**, può essere ignorato dal controllo del codice sorgente nel **ui.apps** modulo . Inspect `.gitignore` file sottostante **ui.apps**.

1. Apri l&#39;articolo di LA Skatepark in AEM a: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Stili di base aggiornati per l’articolo](assets/client-side-libraries/updated-base-styles.png)

   Ora dovresti visualizzare gli stili aggiornati per l’articolo. Potrebbe essere necessario eseguire un aggiornamento per cancellare tutti i file CSS memorizzati nella cache dal browser.

   Sta iniziando a guardare molto più vicino ai mascalzoni!

   >[!NOTE]
   >
   > I passaggi eseguiti in precedenza per generare e distribuire il codice ui.frontend da AEM verranno eseguiti automaticamente quando una build Maven viene attivata dalla directory principale del progetto `mvn clean install -PautoInstallSinglePackage`.

## Apportare una modifica allo stile

Quindi, apporta una piccola modifica al `ui.frontend` per visualizzare il modulo `npm run watch` distribuisci automaticamente gli stili nell&#39;istanza AEM locale.

1. In `ui.frontend` il modulo apre il file: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Aggiorna `$brand-primary` variabile di colore:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Salva le modifiche.

1. Torna al browser e aggiorna la pagina AEM per visualizzare gli aggiornamenti:

   ![Librerie lato client](assets/client-side-libraries/style-update-brand-primary.png)

1. Ripristina la `$brand-primary` colorare e arrestare la build del webpack utilizzando il comando `CTRL+C`.

>[!CAUTION]
>
> L&#39;uso del **ui.frontend** Il modulo potrebbe non essere necessario per tutti i progetti. La **ui.frontend** Il modulo aggiunge una complessità aggiuntiva e se non c&#39;è bisogno/desiderio di utilizzare alcuni di questi strumenti front-end avanzati (Sass, webpack, npm...) potrebbe non essere necessario.

## Inclusione di pagine e modelli {#page-inclusion}

Quindi, esaminiamo come viene fatto riferimento alle clientlibs nella pagina di AEM. Una best practice comune nello sviluppo web è quella di includere i CSS nell’intestazione di HTML `<head>` e JavaScript immediatamente prima della chiusura `</body>` tag .

1. Passa al modello Pagina articolo in [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Fai clic sul pulsante **Informazioni pagina** e nel menu seleziona **Criterio pagina** per aprire **Criterio pagina** finestra di dialogo.

   ![Criterio pagina del menu del modello di pagina dell’articolo](assets/client-side-libraries/template-page-policy.png)

   *Informazioni pagina > Criterio pagina*

1. Si noti che le categorie di `wknd.dependencies` e `wknd.site` sono elencati qui. Per impostazione predefinita, le clientlibs configurate tramite i Criteri di pagina sono suddivise in modo da includere il CSS nell’intestazione della pagina e il JavaScript all’estremità del corpo. Se lo desideri, puoi elencare esplicitamente che il JavaScript clientlib deve essere caricato nell’intestazione della pagina. Ciò vale per `wknd.dependencies`.

   ![Criterio pagina del menu del modello di pagina dell’articolo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > È inoltre possibile fare riferimento al `wknd.site` o `wknd.dependencies` direttamente dal componente pagina, utilizzando `customheaderlibs.html` o `customfooterlibs.html` come abbiamo visto prima per `wknd.base` clientlib. L’utilizzo del modello offre una certa flessibilità in che consente di scegliere e scegliere quali clientlibs vengono utilizzati per modello. Ad esempio, se disponi di una libreria JavaScript molto pesante che verrà utilizzata solo su un modello selezionato.

1. Passa a **LA Skatepark** pagina creata utilizzando **Modello pagina articolo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Fai clic sul pulsante **Informazioni pagina** e nel menu seleziona **Visualizza come pubblicato** per aprire la pagina dell’articolo all’esterno dell’editor AEM.

   ![Visualizza come pubblicato](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizza l’origine della pagina di [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e dovresti essere in grado di visualizzare i seguenti riferimenti clientlib nel `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Tieni presente che le clientlibs utilizzano il proxy `/etc.clientlibs` punto finale. Dovresti anche vedere i seguenti inclusioni clientlib nella parte inferiore della pagina:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Se si segue il 6.5/6.4, le librerie lato client non verranno minimizzate automaticamente. Consulta la documentazione sul [Gestione libreria HTML per abilitare la minimizzazione (scelta consigliata)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >È fondamentale sul lato di pubblicazione che le librerie client siano **not** servito da **/apps** poiché questo percorso deve essere limitato per motivi di sicurezza utilizzando [Sezione filtro del dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [allowProxy, proprietà](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) della libreria client assicura che CSS e JS vengano serviti da **/etc.clientlibs**.

### Passaggi successivi {#next-steps}

Scopri come implementare singoli stili e riutilizzare i componenti core utilizzando il sistema di stili di Experience Manager. [Sviluppo con il sistema di stili](style-system.md) copre l’utilizzo del sistema di stili per estendere i componenti core con CSS specifici per il brand e configurazioni avanzate dei criteri dell’Editor modelli.

Visualizza il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) o rivedi e distribuisci il codice localmente in sul blocco Git `tutorial/client-side-libraries-solution`.

1. Clona il [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) archivio.
1. Consulta la sezione `tutorial/client-side-libraries-solution` ramo.

## Strumenti e risorse aggiuntivi {#additional-resources}

### Webpack DevServer - Markup statico {#webpack-dev-static}

Nel paio di esercizi precedenti siamo stati in grado di aggiornare diversi file Sass nel **ui.frontend** modulo e attraverso un processo di compilazione, vedere queste modifiche riflesse in AEM. Ora esamineremo le tecniche che sfruttano un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) per sviluppare rapidamente i nostri stili front-end **statico** HTML.

Questa tecnica è utile se la maggior parte degli stili e del codice front-end viene eseguita da uno sviluppatore Front End dedicato che potrebbe non avere un facile accesso a un ambiente AEM. Questa tecnica consente inoltre al FED di apportare modifiche direttamente al HTML, che può quindi essere consegnato a uno sviluppatore AEM da implementare come componenti.

1. Copia la pagina sorgente della pagina dell&#39;articolo dello skatepark LA all&#39;indirizzo [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Riapri l’IDE. Incolla il markup copiato da AEM nel `index.html` in **ui.frontend** modulo sottostante `src/main/webpack/static`.
1. Modificare il markup copiato e rimuovere eventuali riferimenti a **clientlib-site** e **clientlib-dipendenze**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Possiamo rimuovere questi riferimenti perché il server di sviluppo del webpack genererà questi artefatti automaticamente.

1. Avviare il server di sviluppo del webpack da un nuovo terminale eseguendo il seguente comando dall&#39;interno del **ui.frontend** modulo:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dovrebbe aprire una nuova finestra del browser in [http://localhost:8080/](Http://localhost:8080/) con marcatura statica.

1. Modificare il file `src/main/webpack/site/_variables.scss` file. Sostituisci il `$text-color` regola con quanto segue:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salva le modifiche.

1. Dovresti visualizzare automaticamente le modifiche riportate automaticamente nel browser in [http://localhost:8080](Http://localhost:8080).

   ![Modifiche al server di sviluppo del webpack locale](assets/client-side-libraries/local-webpack-dev-server.png)

1. Consulta la sezione `/aem-guides-wknd.ui.frontend/webpack.dev.js` file. Contiene la configurazione del webpack utilizzata per avviare il webpack-dev-server. Si noti che proxy i percorsi `/content` e `/etc.clientlibs` da un&#39;istanza di AEM in esecuzione locale. In questo modo le immagini e altre clientlib (non gestite dalla **ui.frontend** (code) sono resi disponibili.

   >[!CAUTION]
   >
   > L’immagine src del markup statico punta a un componente immagine live su un’istanza AEM locale. Le immagini appariranno interrotte se il percorso dell&#39;immagine cambia, se AEM non viene avviato o se il browser non ha effettuato l&#39;accesso all&#39;istanza AEM locale. Se si passa a una risorsa esterna, è anche possibile sostituire le immagini con riferimenti statici.

1. È possibile **stop** il server webpack dalla riga di comando digitando `CTRL+C`.

### aemed {#develop-aemfed}

[**aemed**](https://aemfed.io/) è uno strumento open-source a riga di comando che può essere utilizzato per accelerare lo sviluppo front-end. È alimentato da  [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) e [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

Ad alto livello **aemed** è progettato per ascoltare le modifiche apportate ai file all&#39;interno di **ui.apps** e li sincronizza automaticamente direttamente in un&#39;istanza AEM in esecuzione. In base alle modifiche, un browser locale si aggiorna automaticamente, accelerando così lo sviluppo front-end. È inoltre progettato per lavorare con Sling Log Tracer per visualizzare automaticamente eventuali errori lato server direttamente nel terminale.

Se stai facendo molto lavoro all&#39;interno del **ui.apps** modulo, modifica degli script HTL e creazione di componenti personalizzati, **aemed** può essere uno strumento molto potente da usare. [La documentazione completa è disponibile qui.](https://github.com/abmaonline/aemfed).

### Debug delle librerie lato client {#debugging-clientlibs}

Con diversi metodi di **categorie** e **incorporamenti** per includere più librerie client, può essere difficile risolvere i problemi. AEM espone diversi strumenti per aiutarti. Uno degli strumenti più importanti è **Rigenera librerie client** che costringerà AEM a ricompilare qualsiasi file LESS e a generare il CSS.

* [**Libri di scarico**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Elenca tutte le librerie client registrate nell&#39;istanza AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Output del test**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - consente a un utente di visualizzare l’output HTML previsto degli include clientlib in base alla categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Convalida delle dipendenze delle librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - evidenzia eventuali dipendenze o categorie incorporate che non è possibile trovare. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rigenera librerie client**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - consente a un utente di forzare AEM a ricostruire tutte le librerie client o di annullare la validità della cache delle librerie client. Questo strumento è particolarmente efficace quando si sviluppa con LESS in quanto può forzare AEM a ricompilare il CSS generato. In generale, è più efficace annullare la validità delle cache, quindi eseguire un aggiornamento della pagina anziché ricostruire tutte le librerie. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![rigenerare la libreria client](assets/client-side-libraries/rebuild-clientlibs.png)
