---
title: Librerie client e workflow front-end
description: Scopri come utilizzare le librerie client per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites. Scopri come il modulo ui.frontend, un progetto webpack, può essere integrato nel processo di build end-to-end.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2432'
ht-degree: 1%

---

# Librerie client e workflow front-end {#client-side-libraries}

Scopri come le librerie o le clientlibs lato client vengono utilizzate per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites. Questo tutorial illustra anche come integrare il modulo [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html), un progetto [webpack](https://webpack.js.org/) separato, nel processo di build end-to-end.

## Prerequisiti {#prerequisites}

Esaminare gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

Si consiglia inoltre di rivedere l&#39;esercitazione sulle nozioni di base dei [componenti](component-basics.md#client-side-libraries) per comprendere le nozioni di base delle librerie lato client e di AEM.

### Progetto iniziale

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per estrarre il progetto iniziale.

Consulta il codice della riga di base su cui si basa l’esercitazione:

1. Controlla il ramo `tutorial/client-side-libraries-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implementa la base di codice in un’istanza AEM locale utilizzando le abilità Maven:

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

## Obiettivi

1. Scopri in che modo le librerie lato client vengono incluse in una pagina tramite un modello modificabile.
1. Scopri come utilizzare il modulo `ui.frontend` e un server di sviluppo Webpack per lo sviluppo front-end dedicato.
1. Comprendi il flusso di lavoro end-to-end per distribuire CSS e JavaScript compilati a un’implementazione di Sites.

## Cosa intendi creare {#what-build}

In questo capitolo, aggiungi alcuni stili di base per il sito WKND e il modello della pagina dell&#39;articolo per avvicinare l&#39;implementazione ai [modelli di progettazione dell&#39;interfaccia utente](assets/pages-templates/wknd-article-design.xd). Puoi utilizzare un flusso di lavoro front-end avanzato per integrare un progetto Webpack in una libreria client di AEM.

![Stili completati](assets/client-side-libraries/finished-styles.png)

*Pagina articolo con stili linea di base applicati*

## Informazioni di base {#background}

Le librerie lato client forniscono un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un’implementazione AEM Sites. Gli obiettivi di base per le librerie lato client o clientlibs sono:

1. Memorizza CSS/JS in piccoli file discreti per facilitarne lo sviluppo e la manutenzione
1. Gestire le dipendenze da framework di terze parti in modo organizzato
1. Riduci al minimo il numero di richieste lato client concatenando CSS/JS in una o due richieste.

Ulteriori informazioni sull&#39;utilizzo di [Librerie lato client sono disponibili qui.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

Le librerie lato client presentano alcune limitazioni. In particolare, è un supporto limitato per i linguaggi front-end più popolari come Sass, LESS e TypeScript. Nell&#39;esercitazione, vediamo in che modo il modulo **ui.frontend** può contribuire a risolvere questo problema.

Distribuisci la base di codice iniziale in un&#39;istanza AEM locale e passa a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Questa pagina non è formattata. Implementiamo le librerie lato client per il brand WKND per aggiungere CSS e JavaScript alla pagina.

## Organizzazione delle librerie lato client {#organization}

Esaminiamo ora l&#39;organizzazione delle clientlibs generate da [Archetipo progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it).

![Organizzazione libreria client di alto livello](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramma di alto livello: organizzazione della libreria lato client e inclusione delle pagine*

>[!NOTE]
>
> La seguente organizzazione di librerie lato client è generata da Archetipo progetto AEM ma rappresenta solo un punto di partenza. Il modo in cui un progetto gestisce e distribuisce CSS e JavaScript a un’implementazione di Sites può variare notevolmente in base a risorse, set di competenze e requisiti.

1. Utilizzando VSCode o un altro IDE, apri il modulo **ui.apps**.
1. Espandere il percorso `/apps/wknd/clientlibs` per visualizzare le clientlibs generate dall&#39;archetipo.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Nella sezione seguente, queste clientlibs sono esaminate in maggiore dettaglio.

1. Nella tabella seguente sono riepilogate le librerie client. Ulteriori dettagli su [incluse le librerie client sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrizione | Note |
   |-------------------| ------------| ------|
   | `clientlib-base` | Livello base di CSS e JavaScript necessario per il funzionamento del sito WKND | incorpora le librerie client dei Componenti core |
   | `clientlib-grid` | Genera il file CSS necessario per il funzionamento della modalità [Layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html). | I punti di interruzione per dispositivi mobili/tablet possono essere configurati qui |
   | `clientlib-site` | Contiene il tema specifico per il sito WKND | Generato dal modulo `ui.frontend` |
   | `clientlib-dependencies` | Incorpora qualsiasi dipendenza di terze parti | Generato dal modulo `ui.frontend` |

1. Si noti che `clientlib-site` e `clientlib-dependencies` vengono ignorati dal controllo del codice sorgente. Questo è per progettazione, poiché questi sono generati in fase di build dal modulo `ui.frontend`.

## Aggiorna stili di base {#base-styles}

Aggiornare quindi gli stili di base definiti nel modulo **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. I file nel modulo `ui.frontend` generano le librerie `clientlib-site` e `clientlib-dependecies` che contengono il tema del sito ed eventuali dipendenze di terze parti.

Le librerie lato client non supportano linguaggi più avanzati come [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Esistono diversi strumenti open source come [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) che accelerano e ottimizzano lo sviluppo front-end. L&#39;obiettivo del modulo **ui.frontend** è quello di poter utilizzare questi strumenti per gestire la maggior parte dei file di origine front-end.

1. Apri il modulo **ui.frontend** e passa a `src/main/webpack/site`.
1. Apri il file `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` è il punto di ingresso ai file Sass nel modulo `ui.frontend`. Include il file `_variables.scss`, che contiene una serie di variabili del brand da utilizzare nei diversi file Sass del progetto. Il file `_base.scss` è incluso e definisce alcuni stili di base per gli elementi HTML. Un&#39;espressione regolare include gli stili dei singoli componenti in `src/main/webpack/components`. Un&#39;altra espressione regolare include i file in `src/main/webpack/site/styles`.

1. Controllare il file `main.ts`. Include `main.scss` e un&#39;espressione regolare per raccogliere qualsiasi file `.js` o `.ts` nel progetto. Questo punto di ingresso viene utilizzato dai [file di configurazione Webpack](https://webpack.js.org/configuration/) come punto di ingresso per l&#39;intero modulo `ui.frontend`.

1. Esaminare i file sotto `src/main/webpack/site/styles`:

   ![File di stile](assets/client-side-libraries/style-files.png)

   Questi file contengono stili per gli elementi globali del modello, come il contenitore Intestazione, Piè di pagina e Contenuto principale. Le regole CSS in questi file hanno come destinazione diversi elementi HTML `header`, `main` e `footer`. Questi elementi di HTML sono stati definiti dai criteri del precedente capitolo [Pagine e modelli](./pages-templates.md).

1. Espandere la cartella `components` in `src/main/webpack` ed esaminare i file.

   ![File Sass componenti](assets/client-side-libraries/component-sass-files.png)

   Ogni file è associato a un componente core come [Componente Pannello a soffietto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). Ogni Componente core è generato con [Block Element Modifier](https://getbem.com/) o notazione BEM per semplificare il targeting di classi CSS specifiche con regole di stile. I file sotto `/components` sono stati sottoposti a stubb da Archetipo progetto AEM con diverse regole BEM per ogni componente.

1. Scarica il file WKND Base Styles **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** e **unzip**.

   ![Stili base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Per accelerare l’esercitazione, vengono forniti diversi file Sass che implementano il brand WKND in base ai Componenti core e alla struttura del modello per pagina dell’articolo.

1. Sovrascrivere il contenuto di `ui.frontend/src` con i file del passaggio precedente. Il contenuto del file ZIP deve sovrascrivere le seguenti cartelle:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![File modificati](assets/client-side-libraries/changed-files-uifrontend.png)

   Ispeziona i file modificati per visualizzare i dettagli dell’implementazione dello stile WKND.

## Controllare l’integrazione ui.frontend {#ui-frontend-integration}

Elemento di integrazione chiave incorporato nel modulo **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prende gli artefatti CSS e JS compilati da un progetto webpack/npm e li trasforma in librerie lato client di AEM.

![integrazione dell&#39;architettura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

L’Archetipo progetto AEM imposta automaticamente questa integrazione. Quindi, esplora come funziona.


1. Aprire un terminale della riga di comando e installare il modulo **ui.frontend** utilizzando il comando `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >L&#39;esecuzione di `npm install` è necessaria una sola volta, ad esempio dopo un nuovo clone o una nuova generazione del progetto.

1. Apri `ui.frontend/package.json` e nel **script** **start** aggiungi il comando `--env writeToDisk=true`.

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. Avvia il server di sviluppo Webpack in modalità **watch** eseguendo il comando seguente:

   ```shell
   $ npm run watch
   ```

1. In questo modo vengono compilati i file di origine dal modulo `ui.frontend` e le modifiche vengono sincronizzate con AEM in [http://localhost:4502](Http://localhost:4502)

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

1. Il comando `npm run watch` popola infine il **clientlib-site** e le **clientlib-dependencies** nel modulo **ui.apps**, che viene quindi sincronizzato automaticamente con AEM.

   >[!NOTE]
   >
   >Esiste anche un profilo `npm run prod` che minimizza JS e CSS. Questa è la compilazione standard ogni volta che la build del webpack viene attivata tramite Maven. Ulteriori dettagli sul modulo [ui.frontend sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Controllare il file `site.css` sotto `ui.frontend/dist/clientlib-site/site.css`. Si tratta del CSS compilato basato sui file di origine Sass.

   ![CSS sito distribuito](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Controllare il file `ui.frontend/clientlib.config.js`. Questo è il file di configurazione per un plug-in npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), che trasforma il contenuto di `/dist` in una libreria client e lo sposta nel modulo `ui.apps`.

1. Controllare il file `site.css` nel modulo **ui.apps** in `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Deve essere una copia identica del file `site.css` dal modulo **ui.frontend**. Ora che si trova nel modulo **ui.apps**, può essere distribuito ad AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Poiché **clientlib-site** viene compilato durante il tempo di compilazione, utilizzando **npm** o **maven**, può essere ignorato dal controllo del codice sorgente nel modulo **ui.apps**. Controllare il file `.gitignore` sotto **ui.apps**.

1. Apri l&#39;articolo LA Skatepark in AEM all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Stili base aggiornati per l&#39;articolo](assets/client-side-libraries/updated-base-styles.png)

   Ora dovresti vedere gli stili aggiornati per l’articolo. Per cancellare i file CSS memorizzati nella cache dal browser potrebbe essere necessario eseguire un aggiornamento rapido.

   Sta iniziando a sembrare molto più vicino ai modelli!

   >[!NOTE]
   >
   > I passaggi eseguiti in precedenza per generare e distribuire il codice ui.frontend in AEM vengono eseguiti automaticamente quando viene attivata una build Maven dalla radice del progetto `mvn clean install -PautoInstallSinglePackage`.

## Modifica dello stile

Apportare quindi una piccola modifica al modulo `ui.frontend` per visualizzare `npm run watch` e distribuire automaticamente gli stili all&#39;istanza AEM locale.

1. Dal modulo `ui.frontend` aprire il file: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Aggiorna la variabile di colore `$brand-primary`:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Salva le modifiche.

1. Torna al browser e aggiorna la pagina AEM per visualizzare gli aggiornamenti:

   ![Librerie lato client](assets/client-side-libraries/style-update-brand-primary.png)

1. Ripristinare la modifica al colore `$brand-primary` e arrestare la compilazione del webpack utilizzando il comando `CTRL+C`.

>[!CAUTION]
>
> L&#39;utilizzo del modulo **ui.frontend** potrebbe non essere necessario per tutti i progetti. Il modulo **ui.frontend** aggiunge ulteriore complessità e, se non è necessario o non si desidera utilizzare alcuni di questi strumenti front-end avanzati (Sass, webpack, npm...), potrebbe non essere necessario.

## Inclusione di pagine e modelli {#page-inclusion}

Quindi, esaminiamo come si fa riferimento alle clientlibs nella pagina di AEM. Una best practice comune nello sviluppo web consiste nell&#39;includere CSS nell&#39;intestazione HTML `<head>` e nel JavaScript immediatamente prima di chiudere il tag `</body>`.

1. Passa al modello per pagina articolo in [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Fai clic sull&#39;icona **Informazioni pagina** e nel menu seleziona **Criterio pagina** per aprire la finestra di dialogo **Criterio pagina**.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy.png)

   *Informazioni pagina > Criterio pagina*

1. Le categorie per `wknd.dependencies` e `wknd.site` sono elencate qui. Per impostazione predefinita, le clientlibs configurate tramite Criterio pagina vengono suddivise in modo da includere il CSS nell’intestazione della pagina e il JavaScript alla fine del corpo. Puoi elencare esplicitamente la libreria client JavaScript da caricare nell’intestazione della pagina. Questo è il caso di `wknd.dependencies`.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > È inoltre possibile fare riferimento direttamente a `wknd.site` o `wknd.dependencies` dal componente page, utilizzando lo script `customheaderlibs.html` o `customfooterlibs.html`. L’utilizzo del modello offre flessibilità in quanto consente di scegliere quali clientlibs utilizzare per il modello. Ad esempio, se disponi di una libreria JavaScript pesante che verrà utilizzata solo su un modello selezionato.

1. Passa alla pagina **La Skateparks** creata utilizzando il **modello pagina articolo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Fai clic sull&#39;icona **Informazioni pagina** e nel menu seleziona **Visualizza come pubblicato** per aprire la pagina dell&#39;articolo all&#39;esterno dell&#39;editor di AEM.

   ![Visualizza come pubblicato](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizza l&#39;origine Pagina di [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e dovresti essere in grado di visualizzare i seguenti riferimenti clientlib in `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Notare che clientlibs sta utilizzando l&#39;endpoint proxy `/etc.clientlibs`. Dovresti inoltre notare che la seguente libreria client include nella parte inferiore della pagina:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > In AEM 6.5/6.4 le librerie lato client non vengono minimizzate automaticamente. Consulta la documentazione su [Gestione librerie HTML per abilitare la minimizzazione (consigliata)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Per quanto riguarda la pubblicazione, è fondamentale che le librerie client siano **non** servite da **/app**, in quanto questo percorso deve essere limitato per motivi di sicurezza utilizzando la [sezione filtro Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La proprietà [allowProxy](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) della libreria client assicura che CSS e JS siano serviti da **/etc.clientlibs**.

### Passaggi successivi {#next-steps}

Scopri come implementare singoli stili e riutilizzare i Componenti core utilizzando il sistema di stili di Experience Manager. [Sviluppare con il sistema di stili](style-system.md) include l&#39;utilizzo del sistema di stili per estendere i componenti core con CSS specifici per il brand e configurazioni di criteri avanzate dell&#39;Editor modelli.

Visualizza il codice finito in [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente in nel ramo Git `tutorial/client-side-libraries-solution`.

1. Clona l&#39;archivio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Controllare il ramo `tutorial/client-side-libraries-solution`.

## Strumenti e risorse aggiuntivi {#additional-resources}

### Webpack DevServer - Markup statico {#webpack-dev-static}

Nella coppia di esercizi precedente, diversi file Sass nel modulo **ui.frontend** sono stati aggiornati e attraverso un processo di compilazione, alla fine vedi che queste modifiche si riflettono in AEM. Esaminiamo ora una tecnica che utilizza un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) per sviluppare rapidamente gli stili front-end rispetto a **static** HTML.

Questa tecnica è utile se la maggior parte degli stili e del codice front-end viene eseguita da uno sviluppatore front-end dedicato che potrebbe non avere un facile accesso a un ambiente AEM. Questa tecnica consente inoltre alla FED di apportare modifiche direttamente a HTML, che possono quindi essere consegnate a uno sviluppatore AEM per implementarle come componenti.

1. Copia l&#39;origine della pagina dell&#39;articolo di skatepark LA all&#39;indirizzo [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Riapri l’IDE. Incolla il markup copiato da AEM in `index.html` nel modulo **ui.frontend** sotto `src/main/webpack/static`.
1. Modifica il markup copiato e rimuovi eventuali riferimenti a **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Rimuovi questi riferimenti perché il server di sviluppo Webpack genera automaticamente questi artefatti.

1. Avvia il server di sviluppo Webpack da un nuovo terminale eseguendo il comando seguente dal modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Verrà aperta una nuova finestra del browser in [http://localhost:8080/](http://localhost:8080/) con markup statico.

1. Modificare il file `src/main/webpack/site/_variables.scss`. Sostituire la regola `$text-color` con quanto segue:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salva le modifiche.

1. Dovresti vedere automaticamente le modifiche riflesse nel browser su [http://localhost:8080](http://localhost:8080).

   ![Modifiche al server di sviluppo Webpack locale](assets/client-side-libraries/local-webpack-dev-server.png)

1. Esaminare il file `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contiene la configurazione del webpack utilizzata per avviare il webpack-dev-server. Esegue il proxy dei percorsi `/content` e `/etc.clientlibs` da un&#39;istanza di AEM in esecuzione locale. In questo modo vengono rese disponibili le immagini e altre clientlibs (non gestite dal codice **ui.frontend**).

   >[!CAUTION]
   >
   > L’SRC immagine del markup statico punta a un componente immagine live in un’istanza AEM locale. Le immagini appaiono interrotte se il percorso cambia, se AEM non viene avviato o se il browser non ha effettuato l’accesso all’istanza AEM locale. Se si consegna a una risorsa esterna, è anche possibile sostituire le immagini con riferimenti statici.

1. È possibile **arrestare** il server Webpack dalla riga di comando digitando `CTRL+C`.

### Debug delle librerie lato client {#debugging-clientlibs}

Se si utilizzano metodi diversi di **categorie** e **incorpora** per includere più librerie client, la risoluzione dei problemi può risultare laboriosa. AEM espone diversi strumenti per facilitare questa fase. Uno degli strumenti più importanti è **Ricostruisci librerie client** che forza AEM a ricompilare eventuali file LESS e generare il CSS.

* [**Dump librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Elenca le librerie client registrate nell&#39;istanza AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Output di test**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - consente a un utente di visualizzare l&#39;output HTML previsto di clientlib include in base alla categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Convalida dipendenze librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - evidenzia le dipendenze o le categorie incorporate che non è possibile trovare. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rigenera librerie client**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - consente a un utente di forzare AEM a rigenerare le librerie client o a invalidare la cache delle librerie client. Questo strumento è efficace per lo sviluppo con LESS, in quanto può costringere AEM a ricompilare i CSS generati. In generale, è più efficace annullare la validità delle cache ed eseguire quindi l’aggiornamento di una pagina anziché ricostruire le librerie. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![rigenera libreria client](assets/client-side-libraries/rebuild-clientlibs.png)
