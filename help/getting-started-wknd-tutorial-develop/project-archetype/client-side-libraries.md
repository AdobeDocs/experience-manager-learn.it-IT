---
title: Librerie client e workflow front-end
description: Scopri come utilizzare le librerie client per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites. Scopri come il modulo ui.frontend, un progetto webpack, può essere integrato nel processo di build end-to-end.
version: 6.4, 6.5, Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 680
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2546'
ht-degree: 0%

---

# Librerie client e workflow front-end {#client-side-libraries}

Scopri come le librerie lato client o clientlibs vengono utilizzate per distribuire e gestire CSS e JavaScript per un’implementazione di Adobe Experience Manager (AEM) Sites. Questo tutorial illustra anche come [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) modulo, un [webpack](https://webpack.js.org/) progetto, può essere integrato nel processo di sviluppo end-to-end.

## Prerequisiti {#prerequisites}

Esaminare gli strumenti e le istruzioni necessari per l&#39;impostazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

Si consiglia inoltre di rivedere [Nozioni di base sui componenti](component-basics.md#client-side-libraries) tutorial per comprendere le nozioni di base sulle librerie lato client e sull’AEM.

### Progetto iniziale

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per estrarre il progetto iniziale.

Consulta il codice della riga di base su cui si basa l’esercitazione:

1. Consulta la sezione `tutorial/client-side-libraries-start` ramo da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implementa la base di codice in un’istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se si utilizza AEM 6.5 o 6.4, aggiungere `classic` profilo a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) oppure estrarre il codice localmente passando al ramo `tutorial/client-side-libraries-solution`.

## Obiettivi

1. Scopri in che modo le librerie lato client vengono incluse in una pagina tramite un modello modificabile.
1. Scopri come utilizzare il `ui.frontend` e un server di sviluppo webpack per lo sviluppo front-end dedicato.
1. Comprendere il flusso di lavoro end-to-end per distribuire CSS e JavaScript compilati a un’implementazione di Sites.

## Cosa intendi creare {#what-build}

In questo capitolo, aggiungi alcuni stili di base per il sito WKND e il modello della pagina dell’articolo per avvicinare l’implementazione al [Modelli di progettazione interfaccia utente](assets/pages-templates/wknd-article-design.xd). Puoi utilizzare un flusso di lavoro front-end avanzato per integrare un progetto Webpack in una libreria client AEM.

![Stili finiti](assets/client-side-libraries/finished-styles.png)

*Pagina articolo con stili linea di base applicati*

## Informazioni di base {#background}

Le librerie lato client forniscono un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un’implementazione AEM Sites. Gli obiettivi di base per le librerie lato client o clientlibs sono:

1. Memorizza CSS/JS in piccoli file discreti per facilitarne lo sviluppo e la manutenzione
1. Gestire le dipendenze da framework di terze parti in modo organizzato
1. Riduci al minimo il numero di richieste lato client concatenando CSS/JS in una o due richieste.

Ulteriori informazioni sull’utilizzo di [Le librerie lato client sono disponibili qui.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

Le librerie lato client presentano alcune limitazioni. In particolare, è un supporto limitato per i linguaggi front-end più popolari come Sass, LESS e TypeScript. Nell’esercitazione, vediamo in che modo il **ui.frontend** Questo modulo può essere utile per risolvere il problema.

Distribuisci la base di codice iniziale in un’istanza AEM locale e passa a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Questa pagina non è formattata. Implementiamo le librerie lato client per il brand WKND per aggiungere CSS e JavaScript alla pagina.

## Organizzazione delle librerie lato client {#organization}

Ora esploriamo l’organizzazione delle clientlibs generate da [Archetipo progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it).

![Organizzazione di librerie client di alto livello](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramma di alto livello: organizzazione della libreria lato client e inclusione delle pagine*

>[!NOTE]
>
> La seguente organizzazione di librerie lato client è generata da Archetipo di progetto AEM ma rappresenta solo un punto di partenza. Il modo in cui un progetto alla fine gestisce e distribuisce CSS e JavaScript a un’implementazione di Sites può variare notevolmente in base a risorse, set di competenze e requisiti.

1. Utilizzando VSCode o un altro IDE aprire **ui.apps** modulo.
1. Espandi il percorso `/apps/wknd/clientlibs` per visualizzare le clientlibs generate dall’archetipo.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Nella sezione seguente, queste clientlibs sono esaminate in maggiore dettaglio.

1. Nella tabella seguente sono riepilogate le librerie client. Ulteriori dettagli su [librerie client incluse sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrizione | Note |
   |-------------------| ------------| ------|
   | `clientlib-base` | Livello base di CSS e JavaScript necessari per il funzionamento del sito WKND | incorpora le librerie client dei Componenti core |
   | `clientlib-grid` | Genera il CSS necessario per [Modalità Layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) al lavoro. | I punti di interruzione per dispositivi mobili/tablet possono essere configurati qui |
   | `clientlib-site` | Contiene il tema specifico per il sito WKND | Generato da `ui.frontend` modulo |
   | `clientlib-dependencies` | Incorpora qualsiasi dipendenza di terze parti | Generato da `ui.frontend` modulo |

1. Osserva che `clientlib-site` e `clientlib-dependencies` vengono ignorati dal controllo del codice sorgente. Questo avviene per progettazione, in quanto vengono generati in fase di build da `ui.frontend` modulo.

## Aggiorna stili di base {#base-styles}

Quindi, aggiorna gli stili di base definiti nella **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** modulo. I file in `ui.frontend` generare il modulo `clientlib-site` e `clientlib-dependecies` librerie che contengono il tema del sito ed eventuali dipendenze di terze parti.

Le librerie lato client non supportano linguaggi più avanzati come [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Esistono diversi strumenti open-source come [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) per accelerare e ottimizzare lo sviluppo front-end. L&#39;obiettivo della **ui.frontend** Il modulo deve essere in grado di utilizzare questi strumenti per gestire la maggior parte dei file di origine front-end.

1. Apri il **ui.frontend** e passare a `src/main/webpack/site`.
1. Apri il file `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` è il punto di ingresso ai file Sass in `ui.frontend` modulo. Include `_variables.scss` , che contiene una serie di variabili del brand da utilizzare nei diversi file Sass del progetto. Il `_base.scss` è incluso anche il file e definisce alcuni stili di base per gli elementi HTML. Un’espressione regolare include gli stili per i singoli stili dei componenti in `src/main/webpack/components`. Un’altra espressione regolare include i file in `src/main/webpack/site/styles`.

1. Inspect il file `main.ts`. Include `main.scss` e un’espressione regolare per raccogliere qualsiasi `.js` o `.ts` file nel progetto. Questo punto di ingresso viene utilizzato da [file di configurazione webpack](https://webpack.js.org/configuration/) come punto di ingresso per l&#39;intero `ui.frontend` modulo.

1. Inspect i file sotto `src/main/webpack/site/styles`:

   ![File di stile](assets/client-side-libraries/style-files.png)

   Questi file contengono stili per gli elementi globali del modello, come il contenitore Intestazione, Piè di pagina e Contenuto principale. Le regole CSS in questi file sono destinate a diversi elementi HTML `header`, `main`, e  `footer`. Questi elementi HTML sono stati definiti dai criteri del capitolo precedente [Pagine e modelli](./pages-templates.md).

1. Espandi `components` cartella in `src/main/webpack` ed esaminare i file.

   ![File Sass dei componenti](assets/client-side-libraries/component-sass-files.png)

   Ogni file viene mappato su un Componente core come [Componente Pannello a soffietto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). Ogni Componente core è creato con [Blocca modificatore elemento](https://getbem.com/) o la notazione BEM per agevolare il targeting di classi CSS specifiche con regole di stile. I file sotto `/components` sono stati inseriti dall’Archetipo di progetto AEM con diverse regole BEM per ogni componente.

1. Scaricare gli stili di base WKND **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** e **decomprimi** il file.

   ![Stili base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Per accelerare l’esercitazione, vengono forniti diversi file Sass che implementano il brand WKND in base ai Componenti core e alla struttura del modello per pagina dell’articolo.

1. Sovrascrivi il contenuto di `ui.frontend/src` con file del passaggio precedente. Il contenuto del file ZIP deve sovrascrivere le seguenti cartelle:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![File modificati](assets/client-side-libraries/changed-files-uifrontend.png)

   Per visualizzare i dettagli dell’implementazione dello stile WKND, Inspect ha modificato i file.

## Integrazione ui.frontend in Inspect {#ui-frontend-integration}

Un elemento di integrazione chiave integrato in **ui.frontend** modulo, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) prende gli artefatti CSS e JS compilati da un progetto webpack/npm e li trasforma in librerie AEM lato client.

![Integrazione dell’architettura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

L’Archetipo progetto AEM configura automaticamente questa integrazione. Quindi, esplora come funziona.


1. Aprire un terminale della riga di comando e installare **ui.frontend** modulo che utilizza `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` L’esecuzione è necessaria una sola volta, ad esempio dopo un nuovo clone o una nuova generazione del progetto.

1. Avviare il server di sviluppo Webpack in **guardare** mediante l&#39;esecuzione del comando seguente:

   ```shell
   $ npm run watch
   ```

1. In questo modo i file di origine vengono compilati dal `ui.frontend` e sincronizza le modifiche con AEM in corrispondenza di [http://localhost:4502](Http://localhost:4502)

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

1. Il comando `npm run watch` alla fine popola **clientlib-site** e **clientlib-dependencies** nel **ui.apps** che viene quindi sincronizzato automaticamente con l&#39;AEM.

   >[!NOTE]
   >
   >È inoltre disponibile un `npm run prod` profilo che minimizza JS e CSS. Questa è la compilazione standard ogni volta che la build del webpack viene attivata tramite Maven. Maggiori dettagli su [Il modulo ui.frontend si trova qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect il file `site.css` sotto `ui.frontend/dist/clientlib-site/site.css`. Si tratta del CSS compilato basato sui file di origine Sass.

   ![CSS sito distribuito](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect il file `ui.frontend/clientlib.config.js`. File di configurazione per un plug-in npm. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) che trasforma il contenuto di `/dist` in una libreria client e la sposta nella `ui.apps` modulo.

1. Inspect il file `site.css` nel **ui.apps** modulo in `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Deve essere una copia identica del `site.css` file da **ui.frontend** modulo. Ora che è in **ui.apps** può essere implementato in AEM.

   ![clientlib-site ui.apps](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Da **clientlib-site** viene compilato durante la generazione, utilizzando **npm**, o **maven**, può essere tranquillamente ignorato dal controllo del codice sorgente in **ui.apps** modulo. Inspect `.gitignore` file sotto **ui.apps**.

1. Apri l’articolo La Skatepark in AEM all’indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Stili base aggiornati per l&#39;articolo](assets/client-side-libraries/updated-base-styles.png)

   Ora dovresti vedere gli stili aggiornati per l’articolo. Per cancellare i file CSS memorizzati nella cache dal browser potrebbe essere necessario eseguire un aggiornamento rapido.

   Sta iniziando a sembrare molto più vicino ai modelli!

   >[!NOTE]
   >
   > I passaggi sopra descritti per generare e distribuire il codice ui.frontend in AEM vengono eseguiti automaticamente quando viene attivata una build Maven dalla radice del progetto `mvn clean install -PautoInstallSinglePackage`.

## Modifica dello stile

Quindi, apporta una piccola modifica al `ui.frontend` per visualizzare `npm run watch` distribuire automaticamente gli stili nell’istanza AEM locale.

1. Da, il `ui.frontend` modulo apri il file: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Aggiornare il `$brand-primary` variabile colore:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Salva le modifiche.

1. Torna al browser e aggiorna la pagina AEM per visualizzare gli aggiornamenti:

   ![Librerie lato client](assets/client-side-libraries/style-update-brand-primary.png)

1. Ripristina la modifica in `$brand-primary` colorare e arrestare la build del webpack utilizzando il comando `CTRL+C`.

>[!CAUTION]
>
> L&#39;uso del **ui.frontend** non è necessario per tutti i progetti. Il **ui.frontend** Il modulo aggiunge ulteriore complessità e, se non è necessario o non si desidera utilizzare alcuni di questi strumenti front-end avanzati (Sass, webpack, npm...), potrebbe non essere necessario.

## Inclusione di pagine e modelli {#page-inclusion}

Quindi, esaminiamo come si fa riferimento alle clientlibs nella pagina AEM. Una best practice comune nello sviluppo web consiste nell’includere CSS nell’intestazione HTML `<head>` e JavaScript immediatamente prima della chiusura `</body>` tag.

1. Passa al modello per pagina articolo all’indirizzo [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Fai clic su **Informazioni pagina** e nel menu seleziona **Criterio pagina** per aprire **Criterio pagina** .

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy.png)

   *Informazioni pagina > Criterio pagina*

1. Tieni presente che le categorie per `wknd.dependencies` e `wknd.site` sono elencati qui. Per impostazione predefinita, le clientlibs configurate tramite Criterio pagina vengono suddivise in modo da includere il CSS nell’intestazione della pagina e il JavaScript alla fine del corpo. Puoi elencare esplicitamente il JavaScript clientlib da caricare nell’intestazione della pagina. Questo è il caso per `wknd.dependencies`.

   ![Criterio pagina menu modello pagina articolo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > È inoltre possibile fare riferimento al `wknd.site` o `wknd.dependencies` direttamente dal componente Pagina, utilizzando `customheaderlibs.html` o `customfooterlibs.html` script. L’utilizzo del modello offre flessibilità in quanto consente di scegliere quali clientlibs utilizzare per il modello. Ad esempio, se disponi di una libreria JavaScript pesante che verrà utilizzata solo su un modello selezionato.

1. Accedi a **La Skatepark** pagina creata utilizzando **Modello pagina articolo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Fai clic su **Informazioni pagina** e nel menu seleziona **Visualizza come pubblicato** per aprire la pagina dell’articolo all’esterno dell’editor AEM.

   ![Visualizza come pubblicato](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizza l’origine Pagina di [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e dovresti essere in grado di visualizzare i seguenti riferimenti clientlib nel `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Nota che le clientlibs utilizzano il proxy `/etc.clientlibs` endpoint. Dovresti inoltre notare che la seguente libreria client include nella parte inferiore della pagina:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Per AEM 6.5/6.4 le librerie lato client non vengono minimizzate automaticamente. Consulta la documentazione su [Gestione librerie HTML per abilitare la minimizzazione (scelta consigliata)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Per quanto riguarda la pubblicazione, è fondamentale che le librerie client siano **non** servito da **/apps** poiché questo percorso deve essere limitato per motivi di sicurezza utilizzando [Sezione filtro di Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). Il [allowProxy, proprietà](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) della libreria client assicura che i file CSS e JS siano serviti da **/etc.clientlibs**.

### Passaggi successivi {#next-steps}

Scopri come implementare singoli stili e riutilizzare i Componenti core utilizzando il Sistema di stili di Experience Manager. [Sviluppo con il sistema di stili](style-system.md) In è stato descritto l’utilizzo del sistema di stili per estendere i componenti core con CSS specifici per il brand e configurazioni di policy avanzate dell’Editor modelli.

Visualizza il codice finito il [GitHub](https://github.com/adobe/aem-guides-wknd) oppure controlla e distribuisci il codice localmente in sul ramo Git `tutorial/client-side-libraries-solution`.

1. Clona il [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) archivio.
1. Consulta la sezione `tutorial/client-side-libraries-solution` filiale.

## Strumenti e risorse aggiuntivi {#additional-resources}

### Webpack DevServer - Markup statico {#webpack-dev-static}

Nella coppia precedente di esercizi diversi file Sass in **ui.frontend** sono stati aggiornati e attraverso un processo di build, alla fine vedono che questi cambiamenti si riflettono nell&#39;AEM. Esaminiamo ora una tecnica che utilizza [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) per sviluppare rapidamente gli stili front-end rispetto a **statico** HTML.

Questa tecnica è utile se la maggior parte degli stili e del codice front-end viene eseguita da uno sviluppatore front-end dedicato che potrebbe non avere un facile accesso a un ambiente AEM. Questa tecnica consente inoltre alla FED di apportare modifiche direttamente alle HTML, che possono poi essere consegnate a uno sviluppatore AEM per implementarle come componenti.

1. Copia la sorgente della pagina dell’articolo skatepark su LA all’indirizzo [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Riapri l’IDE. Incolla il markup copiato da AEM in `index.html` nel **ui.frontend** modulo sotto `src/main/webpack/static`.
1. Modificare il markup copiato e rimuovere i riferimenti a **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Rimuovi questi riferimenti perché il server di sviluppo Webpack genera automaticamente questi artefatti.

1. Avviare il server di sviluppo Webpack da un nuovo terminale eseguendo il comando seguente dall&#39;interno di **ui.frontend** modulo:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dovrebbe aprirsi una nuova finestra del browser in [http://localhost:8080/](http://localhost:8080/) con markup statico.

1. Modifica il file `src/main/webpack/site/_variables.scss` file. Sostituisci il `$text-color` regola con quanto segue:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salva le modifiche.

1. Dovresti vedere automaticamente le modifiche riflesse nel browser su [http://localhost:8080](http://localhost:8080).

   ![Modifiche al server di sviluppo Webpack locale](assets/client-side-libraries/local-webpack-dev-server.png)

1. Rivedi `/aem-guides-wknd.ui.frontend/webpack.dev.js` file. Contiene la configurazione del webpack utilizzata per avviare il webpack-dev-server. Proxy dei percorsi `/content` e `/etc.clientlibs` da un’istanza dell’AEM in esecuzione a livello locale. In questo modo le immagini e altre clientlibs (non gestite da **ui.frontend** codice) sono resi disponibili.

   >[!CAUTION]
   >
   > L’SRC immagine del markup statico punta a un componente immagine live su un’istanza AEM locale. Le immagini appaiono interrotte se il percorso cambia, se l’AEM non viene avviato o se il browser non ha effettuato l’accesso all’istanza AEM locale. Se si consegna a una risorsa esterna, è anche possibile sostituire le immagini con riferimenti statici.

1. È possibile **stop** il server webpack dalla riga di comando digitando `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** è uno strumento open-source della riga di comando che può essere utilizzato per accelerare lo sviluppo front-end. È alimentato da [aemsync](https://www.npmjs.com/package/aemsync), [Sincronizzazione browser](https://browsersync.io/), e [Tracciamento registro Sling](https://sling.apache.org/documentation/bundles/log-tracers.html).

Ad alto livello, la `aemfed`è progettato per ascoltare le modifiche apportate ai file all&#39;interno di **ui.apps** e sincronizzarli automaticamente direttamente con un&#39;istanza AEM in esecuzione. In base alle modifiche, un browser locale si aggiorna automaticamente, velocizzando così lo sviluppo front-end. È inoltre progettato per funzionare con Sling Log Tracer per visualizzare automaticamente eventuali errori lato server direttamente nel terminale.

Se si lavora molto con **ui.apps** , modificando script HTL e creando componenti personalizzati, **aemfed** può essere uno strumento potente da utilizzare. [La documentazione completa è disponibile qui](https://github.com/abmaonline/aemfed).

### Debug delle librerie lato client {#debugging-clientlibs}

Utilizzo di diversi metodi di **categorie** e **incorpora** per includere più librerie client può essere complicato risolvere i problemi. L&#39;AEM espone diversi strumenti per aiutarlo. Uno degli strumenti più importanti è **Rigenera librerie client** che costringe l’AEM a ricompilare eventuali file LESS e generare il CSS.

* [**Dump librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : elenca le librerie client registrate nell’istanza AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Output di prova**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : consente a un utente di visualizzare l’output HTML previsto di clientlib include in base alla categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Convalida dipendenze librerie**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : evidenzia le dipendenze o le categorie incorporate che non sono state trovate. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rigenera librerie client**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : consente a un utente di forzare l’AEM a rigenerare le librerie client o a invalidare la cache delle librerie client. Questo strumento è efficace quando si sviluppa con MENO in quanto può costringere l’AEM a ricompilare il CSS generato. In generale, è più efficace annullare la validità delle cache ed eseguire quindi l’aggiornamento di una pagina anziché ricostruire le librerie. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![rigenerare la libreria client](assets/client-side-libraries/rebuild-clientlibs.png)
