---
title: Guida introduttiva ad  AEM Sites - Impostazione progetto
seo-title: Guida introduttiva ad  AEM Sites - Impostazione progetto
description: Copre la creazione di un progetto Maven Multi Module per gestire il codice e le configurazioni di un sito AEM.
sub-product: sites
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 4%

---


# Configurazione del progetto {#project-setup}

Questa esercitazione illustra la creazione di un progetto Maven Multi Module per gestire il codice e le configurazioni di un sito Adobe Experience Manager.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale. Accertatevi di disporre di una nuova istanza di Adobe Experience Manager disponibile localmente e che non siano stati installati pacchetti di esempio/dimostrazione aggiuntivi (diversi dai Service Pack richiesti).

## Obiettivo {#objective}

1. Scopri come generare un nuovo progetto AEM utilizzando un archetipo di Maven.
1. Comprendere i diversi moduli generati dall&#39;archivio AEM progetti e come funzionano insieme.
1. Scopri AEM componenti core sono inclusi in un progetto AEM.

## Cosa verrà creato {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In questo capitolo, verrà generato un nuovo progetto Adobe Experience Manager utilizzando il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Il progetto AEM contiene tutto il codice, il contenuto e le configurazioni utilizzati per l&#39;implementazione di Sites. Il progetto generato in questo capitolo servirà da base per l&#39;implementazione del sito WKND e sarà basato su capitoli futuri.

## Sfondo {#background}

**Cos&#39;è un progetto Maven?** - [Apache Maven](https://maven.apache.org/) è uno strumento di gestione software per costruire progetti. *Tutte le implementazioni Adobe Experience Manager* utilizzano i progetti Maven per creare, gestire e distribuire codice personalizzato sopra AEM.

**Cos&#39;è un archetipo Maven?** - Un archetipo [](https://maven.apache.org/archetype/index.html) Paradiso è un modello o un modello per la generazione di nuovi progetti. L&#39;archetipo AEM progetto ci consente di generare un nuovo progetto con uno spazio dei nomi personalizzato e di includere una struttura di progetto che segue le best practice, accelerando notevolmente il nostro progetto.

## Creazione del progetto {#create}

Ci sono un paio di opzioni per creare un progetto Maven Multi-module per AEM. Questa esercitazione utilizzerà il [Maven AEM Project Archetype **22**](https://github.com/adobe/aem-project-archetype). Cloud Manager [fornisce anche una procedura guidata](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) per l&#39;interfaccia utente per avviare la creazione di un progetto di applicazione AEM. Il progetto sottostante generato dall&#39;interfaccia utente di Cloud Manager si traduce nella stessa struttura in cui viene utilizzato direttamente l&#39;archetipo.

>[!NOTE]
>
>Per seguire questa esercitazione, utilizzate la versione **22** dell&#39;archetipo. Tuttavia, è sempre consigliabile utilizzare la versione **più** recente dell&#39;archetipo per generare un nuovo progetto.

La serie successiva di passaggi verrà eseguita utilizzando un terminale della riga di comando basato su UNIX, ma dovrebbe essere simile se si utilizza un terminale Windows.

1. Aprite un terminale della riga di comando e verificate che Maven sia stato installato e aggiunto al percorso della riga di comando:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Verifica che il profilo **adobe-public** sia attivo eseguendo il comando seguente:

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Se **non** visualizzi il **adobe-public** , significa che il repo del Adobe  non ha un riferimento corretto nel `~/.m2/settings.xml` file. Per installare e configurare Apache Maven in [un ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven)di sviluppo locale, è necessario rivedere i passaggi.

1. Andate a una directory in cui desiderate generare il progetto AEM. Può trattarsi di qualsiasi directory in cui si desidera mantenere il codice sorgente del progetto. Ad esempio, una directory denominata `code` sotto la directory principale dell&#39;utente:

   ```shell
   $ cd ~/code
   ```

1. Incollate quanto segue nella riga di comando per [generare il progetto in modalità](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)batch:

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >Per impostazione predefinita, la generazione di un progetto dall&#39;archetipo di Paradiso utilizza la modalità interattiva. Per evitare che i valori ottenuti con l&#39;indicazione del grasso vengano rilevati, è necessario utilizzare la modalità batch. È anche possibile creare il progetto Maven AEM utilizzando il plug-in [AEM Developer Tools per Eclipse](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/aem-eclipse.html).

   >[!CAUTION]
   >
   >Se ricevi un errore simile al seguente: *Impossibile eseguire l&#39;obiettivo org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate (default-cli) nel progetto standalone-pom: L&#39;archetipo desiderato non esiste*. È un&#39;indicazione che il repo del Adobe  non è correttamente riferito nel `~/.m2/settings.xml` file. Rivedete i passaggi precedenti e verificate che il file settings.xml faccia riferimento al repo del Adobe .

   Nella tabella seguente sono elencati i valori utilizzati per questa esercitazione:

   | Nome | Valori | Descrizione |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | Base Maven groupId |
   | artifactId | aem-guide-wknd | Base Maven ArtifactId |
   | version | 0.0.1-SNAPSHOT | Versione |
   | package | com.adobe.aem.guides.wknd | Pacchetto origine Java |
   | appsFolderName | wknd | /apps name |
   | artifactName | Progetto Siti WKND | Nome progetto Maven |
   | componentGroupName | WKND | Nome gruppo di componenti AEM |
   | contentFolderName | wknd | /content, nome cartella |
   | confFolderName | wknd | /conf nome cartella |
   | cssId | wknd | prefisso utilizzato in css generato |
   | packageGroup | wknd | Nome gruppo di pacchetti di contenuti |
   | siteName | Sito WKND | Nome AEM sito |
   | optionAemVersion | 6.5.0 | Versione AEM di destinazione |
   | language_country | en_us | lingua/codice del paese per creare la struttura del contenuto da (ad esempio en_us) |
   | optionIncludeExamples | y | Includi un sito di esempio nella libreria dei componenti |
   | optionIncludeErrorHandler | n | Includi una pagina di risposta personalizzata 404 |
   | optionIncludeFrontendModule | y | Includere un modulo frontale dedicato |
   | isSingleCountryWebsite | n | Creare una struttura gerarchica nel contenuto di esempio |
   | optionDispatcherConfig | nessuno | Generazione di un modulo di configurazione dispatcher |

1. La seguente struttura di cartelle e file verrà generata dal Maven archetype sul file system locale:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Creazione del progetto {#build}

Ora che abbiamo generato un nuovo progetto, possiamo distribuire il codice del progetto a un&#39;istanza locale di AEM.

1. Verificare che un&#39;istanza di AEM sia in esecuzione localmente sulla porta **4502**.
1. Dalla riga di comando andate alla directory del `aem-guides-wknd` progetto.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Eseguite il comando seguente per creare e distribuire l&#39;intero progetto a AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Il profilo Maven `autoInstallSinglePackage` compila i singoli moduli del progetto e distribuisce un singolo pacchetto all&#39;istanza AEM. Per impostazione predefinita, questo pacchetto viene distribuito in un&#39;istanza AEM in esecuzione localmente sulla porta **4502** e con le credenziali di `admin:admin`.

1. Passa a Gestione pacchetti nell’istanza di AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Dovresti vedere tre pacchetti per `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.content`, e `aem-guides-wknd.all`.

   Dovresti anche vedere più pacchetti per [AEM componenti](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html) core inclusi nel progetto da archetype. Questo verrà descritto più avanti nell&#39;esercitazione.

1. Passate alla console Siti: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Il sito WKND sarà uno dei siti. Includerà una struttura del sito con una gerarchia di master lingua e Stati Uniti. La gerarchia del sito è basata sui valori per `language_country` e `isSingleCountryWebsite` durante la generazione del progetto utilizzando il tipo di archetipo.

1. Per aprire la pagina **US** `>` English **, selezionatela e fate clic sul pulsante** Modifica **** nella barra dei menu:

   ![console del sito](assets/project-setup/aem-sites-console.png)

1. Alcuni contenuti sono già stati creati e diversi componenti possono essere aggiunti a una pagina. Sperimentate con questi componenti per avere un&#39;idea della funzionalità. La configurazione di questa pagina e dei componenti verrà esaminata più avanti nell’esercitazione.

##  Inspect il progetto {#project-structure}

L&#39;archetipo AEM è costituito da singoli moduli Maven:

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - Pacchetto Java contenente tutte le funzionalità di base come servizi OSGi, listener o pianificatori, nonché il codice Java relativo ai componenti come servlet o filtri di richiesta.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) - contiene le parti /apps del progetto, ad esempio clientlibs JS&amp;CSS, componenti e configurazioni OSGi
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) - contiene contenuti e configurazioni strutturali come modelli modificabili, schemi di metadati (/content, /conf)
* ui.test - Pacchetto Java contenente test JUnit eseguiti sul lato server. Questo bundle non deve essere distribuito in produzione.
* ui.launcher - contiene il codice colla che distribuisce il bundle ui.test (e i bundle dipendenti) al server e attiva l&#39;esecuzione JUnit remota
* [ui.frontend](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html) - (facoltativo) contiene gli artefatti necessari per utilizzare il modulo di build front-end basato su Webpack.
* all - si tratta di un modulo Maven vuoto che combina i moduli di cui sopra in un unico pacchetto che può essere implementato in un ambiente AEM.

![Diagramma del progetto Maven](assets/project-setup/project-pom-structure.png)

Consulta la documentazione [](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/overview.html) AEM Project Archetype per ulteriori informazioni sui moduli Maven.

## Advanced Maven, comandi {#advanced-maven-commands}

Durante lo sviluppo si potrebbe lavorare con uno solo dei moduli e si desidera evitare di costruire l&#39;intero progetto per risparmiare tempo. È inoltre possibile distribuire direttamente a un&#39;istanza di AEM Publish o a un&#39;istanza di AEM non in esecuzione sulla porta 4502.

In seguito verranno esaminati alcuni ulteriori profili e comandi Maven utilizzabili per una maggiore flessibilità durante lo sviluppo.

### Modulo di base {#core-module}

Il modulo **[di base](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** contiene tutto il codice Java associato al progetto. Una volta creato, implementa un bundle OSGi in AEM. Per creare solo questo modulo:

1. Individuate la `core` cartella (sotto `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Eseguite il comando seguente:

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Andate a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Questa è la console Web OSGi e contiene informazioni su tutti i bundle installati nell&#39;istanza AEM.

1. Attiva la colonna di ordinamento **ID** e dovresti vedere il bundle WKND installato e attivo.

   ![Pacchetto principale](assets/project-setup/wknd-osgi-console.png)

1. È possibile vedere la posizione &#39;fisica&#39; del barattolo in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar):

   ![Posizione CRXDE del Jar](assets/project-setup/jcr-bundle-location.png)

### Moduli Ui.apps e Ui.content {#apps-content-module}

Il modulo **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven contiene tutto il codice di rendering necessario per il sito sottostante `/apps`. Ciò include CSS/JS che verrà memorizzato in un formato AEM denominato [clientlibs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html). Sono inclusi anche gli script [HTL](https://docs.adobe.com/docs/en/htl/overview.html) per il rendering HTML dinamico. Il modulo **ui.apps** può essere considerato come una mappa della struttura nel JCR, ma in un formato che può essere memorizzato in un file system e impegnato nel controllo del codice sorgente. Il modulo **ui.apps** contiene solo codice.

Per creare solo questo modulo:

1. Dalla riga di comando. Individuate la `ui.apps` cartella (sotto `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Eseguite il comando seguente:

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Andate a [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Dovresti vedere il `ui.apps` pacchetto come primo pacchetto installato e dovrebbe avere una marca temporale più recente di qualsiasi altro pacchetto.

   ![Pacchetto Ui.apps installato](assets/project-setup/ui-apps-package.png)

1. Tornate alla riga di comando ed eseguite il comando seguente (all’interno della `ui.apps` cartella):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Il profilo `autoInstallPackagePublish` è destinato a distribuire il pacchetto in un ambiente di pubblicazione in esecuzione sulla porta **4503**. L&#39;errore riportato sopra è previsto se non è possibile trovare un&#39;istanza AEM in esecuzione su http://localhost:4503.

1. Eseguite infine il comando seguente per distribuire il `ui.apps` pacchetto sulla porta **4504**:

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   Si prevede nuovamente un errore di build se non è disponibile un&#39;istanza AEM in esecuzione sulla porta **4504** . Il parametro `aem.port` è definito nel file POM in `aem-guides-wknd/pom.xml`.

Il modulo **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** è strutturato nello stesso modo del modulo **ui.apps** . L&#39;unica differenza è che il modulo **ui.content** contiene ciò che è noto come contenuto **modificabile** . **Il contenuto eseguibile** si riferisce essenzialmente a configurazioni non codificate come Modelli, Criteri o strutture di cartelle memorizzate nel controllo del codice sorgente **ma che** possono essere modificate direttamente in un&#39;istanza AEM. Questo verrà esplorato in modo molto più dettagliato nel capitolo Pagine e Modelli. Per ora il passo importante è che gli stessi comandi Maven utilizzati per creare il modulo **ui.apps** possono essere utilizzati per creare il modulo **ui.content** . Potete ripetere i passaggi indicati nella cartella **ui.content** .

### Modulo Ui.frontend {#ui-frontend-module}

Il modulo **[ui.frontend](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/uifrontend.html)** è un modulo Maven che in realtà è un progetto [webpack](https://webpack.js.org/) . Questo modulo è impostato come un sistema di build front-end dedicato che produce file JavaScript e CSS, che vengono a loro volta distribuiti per AEM. Il modulo **ui.frontend** consente agli sviluppatori di codificare con linguaggi come [Sass](https://sass-lang.com/), [TypeScript](https://www.typescriptlang.org/), utilizzare moduli [npm](https://www.npmjs.com/) e integrare l&#39;output direttamente in AEM.

Il modulo **ui.frontend** verrà trattato in modo molto più dettagliato nel capitolo sulle librerie lato client e sullo sviluppo front-end. Per ora vediamo come è integrata nel progetto.

1. Dalla riga di comando. Individuate la `ui.frontend` cartella (sotto `aem-guides-wknd`):

   ```shell
   $ cd ../ui.frontend
   ```

1. Eseguite il comando seguente:

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   Notate le righe come `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`. Indica che i file CSS e JS compilati vengono copiati nella `ui.apps` cartella.

1. Visualizzare il timestamp modificato per il file `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`. Deve essere aggiornato più di recente degli altri file nel `ui.apps` modulo.

   A differenza degli altri moduli che abbiamo esaminato, il modulo **ui.frontend** non distribuisce direttamente a AEM. I file CSS e JS vengono invece copiati nel modulo **ui.apps** , quindi il modulo **ui.apps** viene distribuito a AEM. Se guardate l&#39;ordine di compilazione dal primo comando Maven, vedrete che **ui.frontend** è sempre costruito *prima* di **ui.apps**.

   Più avanti nell&#39;esercitazione vedremo le caratteristiche avanzate del modulo **ui.frontend** e del server di sviluppo webpack incorporato per un rapido sviluppo.

## Inclusione di componenti core {#core-components}

Il archetype incorpora automaticamente [AEM Componenti](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html) principali nel progetto. In precedenza, durante l&#39;analisi dei pacchetti distribuiti in AEM, erano inclusi più pacchetti correlati ai componenti core. I componenti core sono una serie di componenti di base progettati per accelerare lo sviluppo di un progetto AEM Sites . I componenti core sono open source e disponibili su [GitHub](https://github.com/adobe/aem-core-wcm-components). Ulteriori informazioni su come i componenti core sono [inclusi nel progetto sono disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components).

1. Viene aperto l’editor di testo preferito `aem-guides-wknd/pom.xml`.

1. Cerca `core.wcm.components.version`. Verrà visualizzata la versione di Componenti di base inclusa:

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > AEM Project Archetype includerà una versione di AEM Core Components, tuttavia questi progetti hanno cicli di rilascio diversi, e quindi la versione inclusa di Core Components potrebbe non essere la più recente. Come procedura ottimale, devi sempre cercare di sfruttare l&#39;ultima versione dei componenti core. Nuove funzioni e correzioni di bug vengono aggiornate frequentemente. Le informazioni più recenti sulla [versione sono disponibili su GitHub](https://github.com/adobe/aem-core-wcm-components/releases).

1. Se scorri verso il basso fino alla `dependencies` sezione, vengono visualizzate le singole dipendenze dei componenti core:

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## Gestione del controllo del codice sorgente {#source-control}

È sempre consigliabile utilizzare un controllo del codice sorgente per gestire il codice nell&#39;applicazione. Questa esercitazione utilizza git e GitHub. Ci sono diversi file che vengono generati da Maven e/o dall&#39;IDE di scelta che dovrebbero essere ignorati dalla SCM.

Maven creerà una cartella di destinazione ogni volta che create e installate il pacchetto di codice. La cartella e il contenuto di destinazione devono essere esclusi da SCM.

Sotto ui.apps noterete anche molti file .content.xml creati. Questi file XML mappano i tipi di nodo e le proprietà del contenuto installato nel JCR. Questi file sono critici e **non** devono essere ignorati.

AEM archetipo progetto genererà un file di esempio `.gitignore` che può essere utilizzato come punto di partenza per cui i file possono essere ignorati in modo sicuro. Il file viene generato in `<src>/aem-guides-wknd/.gitignore`.

## Recensione {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo progetto AEM!

### Passaggi successivi {#next-steps}

Scopri la tecnologia di base di un componente Adobe Experience Manager (AEM) Sites tramite un semplice `HelloWorld` esempio con l’esercitazione [Component Basics](component-basics.md) .
