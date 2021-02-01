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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1887'
ht-degree: 4%

---


# Configurazione del progetto {#project-setup}

Questa esercitazione illustra la creazione di un progetto Maven Multi Module per gestire il codice e le configurazioni di un sito Adobe Experience Manager.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment). Accertatevi di disporre di una nuova istanza di Adobe Experience Manager disponibile localmente e che non siano stati installati pacchetti di esempio/dimostrazione aggiuntivi (diversi dai Service Pack richiesti).

## Obiettivo {#objective}

1. Scopri come generare un nuovo progetto AEM utilizzando un archetipo di Maven.
1. Comprendere i diversi moduli generati dall&#39;archivio AEM progetti e come funzionano insieme.
1. Scopri AEM componenti core sono inclusi in un progetto AEM.

## Cosa verrà creato {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In questo capitolo, verrà generato un nuovo progetto Adobe Experience Manager utilizzando il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Il progetto AEM contiene tutto il codice, il contenuto e le configurazioni utilizzati per l&#39;implementazione di Sites. Il progetto generato in questo capitolo servirà da base per l&#39;implementazione del sito WKND e sarà basato su capitoli futuri.

**Cos&#39;è un progetto Maven?** -  [Apache ](https://maven.apache.org/) Mavenis uno strumento di gestione software per la creazione di progetti. *Tutte le implementazioni di Adobe Experience* Manager utilizzano i progetti Maven per creare, gestire e distribuire codice personalizzato sopra AEM.

**Cos&#39;è un archetipo Maven?** - Un  [Maven ](https://maven.apache.org/archetype/index.html) archetypeis un modello o un modello per la generazione di nuovi progetti. L&#39;archetipo AEM progetto ci consente di generare un nuovo progetto con uno spazio dei nomi personalizzato e di includere una struttura di progetto che segue le best practice, accelerando notevolmente il nostro progetto.

## Creare il progetto {#create}

Ci sono un paio di opzioni per creare un progetto Maven Multi-module per AEM. Questa esercitazione utilizzerà il [Maven AEM Project Archetype **25**](https://github.com/adobe/aem-project-archetype). Cloud Manager offre anche [una procedura guidata di interfaccia utente](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) per avviare la creazione di un progetto di applicazione AEM. Il progetto sottostante generato dall&#39;interfaccia utente di Cloud Manager si traduce nella stessa struttura in cui viene utilizzato direttamente l&#39;archetipo.

>[!NOTE]
>
>Questa esercitazione utilizza la versione **25** dell&#39;archetipo. È sempre consigliabile utilizzare la versione **più recente** dell&#39;archetipo per generare un nuovo progetto.

La serie successiva di passaggi verrà eseguita utilizzando un terminale della riga di comando basato su UNIX, ma dovrebbe essere simile se si utilizza un terminale Windows.

1. Aprite un terminale della riga di comando. Verificate che Maven sia installato:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Verificare che il profilo **adobe-public** sia attivo eseguendo il comando seguente:

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

   Se si fa **not** vedere il **adobe-public** è un&#39;indicazione che il repo del Adobe  non è correttamente riferito nel file `~/.m2/settings.xml`. Per installare e configurare Apache Maven in [un ambiente di sviluppo locale](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven), è necessario rivedere i passaggi.

1. Andate a una directory in cui desiderate generare il progetto AEM. Può trattarsi di qualsiasi directory in cui si desidera mantenere il codice sorgente del progetto. Ad esempio, una directory denominata `code` sotto la directory principale dell&#39;utente:

   ```shell
   $ cd ~/code
   ```

1. Incollate quanto segue nella riga di comando per [generare il progetto in modalità batch](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=25 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Se utilizzate AEM 6.5.5.0+ o 6.4.8.1+, sostituite `aemVersion="cloud"` con la versione di destinazione di AEM, ad esempio `aemVersion="6.5.5"` o `aemVersion="6.4.8.1"`

   Un elenco completo delle proprietà disponibili per la configurazione di un progetto [è disponibile qui](https://github.com/adobe/aem-project-archetype#available-properties).

1. La seguente struttura di cartelle e file verrà generata dal Maven archetype sul file system locale:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- analyse/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Implementare e creare il progetto {#build}

Create e distribuite il codice del progetto in un&#39;istanza locale di AEM.

1. Verificare che un&#39;istanza di autore AEM eseguita localmente sulla porta **4502**.
1. Dalla riga di comando andate alla directory di progetto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Eseguite il comando seguente per creare e distribuire l&#39;intero progetto a AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La build richiederà circa un minuto e dovrebbe terminare con il seguente messaggio:

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Il profilo Maven `autoInstallSinglePackage` compila i singoli moduli del progetto e distribuisce un singolo pacchetto all&#39;istanza AEM. Per impostazione predefinita, il pacchetto viene distribuito in un&#39;istanza AEM eseguita localmente sulla porta **4502** e con le credenziali di `admin:admin`.

1. Passa a Gestione pacchetti nell’istanza di AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Dovresti vedere i pacchetti per `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` e `aem-guides-wknd.all`.

1. Passate alla console Siti: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Il sito WKND sarà uno dei siti. Includerà una struttura del sito con una gerarchia di master lingua e Stati Uniti. La gerarchia del sito si basa sui valori di `language_country` e `isSingleCountryWebsite` durante la generazione del progetto utilizzando archetype.

1. Aprite la pagina **US** `>` **English** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![console del sito](assets/project-setup/aem-sites-console.png)

1. Il contenuto iniziale è già stato creato e diversi componenti possono essere aggiunti a una pagina. Sperimentate con questi componenti per avere un&#39;idea della funzionalità. Scopri le nozioni di base di un componente nel capitolo successivo.

   ![Contenuto iniziale principale](assets/project-setup/start-home-page.png)

   *Contenuto di esempio generato da Archetype*

##  Inspect il progetto {#project-structure}

Il progetto AEM generato è composto da singoli moduli Maven, ciascuno con un ruolo diverso. Questa esercitazione e la maggior parte dello sviluppo si concentrano sui seguenti moduli:

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - Java Code, principalmente sviluppatori back-end.
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) - Contiene il codice sorgente per CSS, JavaScript, Sass, Type Script, principalmente per sviluppatori front-end.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) - Contiene definizioni di componenti e finestre di dialogo, incorpora CSS e JavaScript compilati come librerie client.
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) - contiene contenuti e configurazioni strutturali come modelli modificabili, schemi di metadati (/content, /conf).

* **all** - si tratta di un modulo Maven vuoto che unisce i moduli sopra in un unico pacchetto che può essere distribuito in un ambiente AEM.

![Diagramma del progetto Maven](assets/project-setup/project-pom-structure.png)

Per ulteriori informazioni sui moduli Maven, vedere la [AEM Project Archetype documentation](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) **all**.

### Inclusione di componenti core {#core-components}

[AEM ](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html) Componenti di base è un insieme di componenti Web Content Management (WCM) standardizzati per AEM. Questi componenti forniscono una serie di linee di base di una funzionalità e sono progettati per essere formattati, personalizzati ed estesi per singoli progetti.

AEM come ambienti di Cloud Service è inclusa la versione più recente di [AEM Componenti principali](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html). Pertanto, i progetti generati per AEM come Cloud Service **not** includono un&#39;incorporazione di AEM componenti core.

Per i progetti generati AEM 6.5/6.4, l&#39;archetype incorpora automaticamente [AEM componenti core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) nel progetto. È consigliabile incorporare AEM 6.5/6.4 AEM componenti core per garantire che la versione più recente venga distribuita con il progetto. Ulteriori informazioni sulle modalità di inclusione dei componenti core [nel progetto sono disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Gestione del controllo del codice sorgente {#source-control}

È sempre consigliabile utilizzare un controllo del codice sorgente per gestire il codice nell&#39;applicazione. Questa esercitazione utilizza git e GitHub. Ci sono diversi file che vengono generati da Maven e/o dall&#39;IDE di scelta che dovrebbero essere ignorati dalla SCM.

Maven creerà una cartella di destinazione ogni volta che create e installate il pacchetto di codice. La cartella e il contenuto di destinazione devono essere esclusi da SCM.

Sotto `ui.apps` si noti che molti `.content.xml` file vengono creati. Questi file XML mappano i tipi di nodo e le proprietà del contenuto installato nel JCR. Questi file sono critici e devono essere ignorati **non**.

AEM archetype progetto genera un file di esempio `.gitignore` che può essere utilizzato come punto di partenza per cui i file possono essere ignorati in modo sicuro. Il file viene generato in `<src>/aem-guides-wknd/.gitignore`.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo progetto AEM!

### Passaggi successivi {#next-steps}

Scopri la tecnologia di base di un componente Adobe Experience Manager (AEM) Sites tramite un semplice esempio `HelloWorld` con l&#39;esercitazione [Component Basics](component-basics.md).

## Comandi Advanced Maven (Bonus) {#advanced-maven-commands}

Durante lo sviluppo si potrebbe lavorare con uno solo dei moduli e si desidera evitare di costruire l&#39;intero progetto per risparmiare tempo. È inoltre possibile distribuire direttamente a un&#39;istanza di AEM Publish o a un&#39;istanza di AEM non in esecuzione sulla porta 4502.

In seguito verranno esaminati alcuni ulteriori profili e comandi Maven utilizzabili per una maggiore flessibilità durante lo sviluppo.

### Modulo core {#core-module}

Il modulo **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** contiene tutto il codice Java associato al progetto. Una volta creato, implementa un bundle OSGi in AEM. Per creare solo questo modulo:

1. Individuate la cartella `core` (sotto `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Eseguite il comando seguente:

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. Andate a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Questa è la console Web OSGi e contiene informazioni su tutti i bundle installati nell&#39;istanza AEM.

1. Attiva la colonna di ordinamento **Id** e dovresti vedere il bundle WKND installato e attivo.

   ![Pacchetto principale](assets/project-setup/wknd-osgi-console.png)

1. È possibile vedere la posizione &#39;fisica&#39; del barattolo in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Posizione CRXDE del Jar](assets/project-setup/jcr-bundle-location.png)

### Moduli Ui.apps e Ui.content {#apps-content-module}

Il modulo **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** contiene tutto il codice di rendering necessario per il sito al di sotto di `/apps`. Ciò include CSS/JS che verrà memorizzato in un formato AEM denominato [clientlibs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/clientlibs.html). Sono inclusi anche gli script [HTL](https://docs.adobe.com/content/help/it-IT/experience-manager-htl/using/overview.html) per il rendering HTML dinamico. È possibile considerare il modulo **ui.apps** come una mappa della struttura nel JCR, ma in un formato che può essere memorizzato in un file system e impegnato nel controllo del codice sorgente. Il modulo **ui.apps** contiene solo codice.

Per creare solo questo modulo:

1. Dalla riga di comando. Individuate la cartella `ui.apps` (sotto `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Eseguite il comando seguente:

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Andate a [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Il pacchetto `ui.apps` deve essere visualizzato come il primo pacchetto installato e deve avere una marca temporale più recente rispetto a qualsiasi altro pacchetto.

   ![Pacchetto Ui.apps installato](assets/project-setup/ui-apps-package.png)

1. Tornate alla riga di comando ed eseguite il comando seguente (all&#39;interno della cartella `ui.apps`):

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

   Il profilo `autoInstallPackagePublish` è destinato alla distribuzione del pacchetto in un ambiente di pubblicazione in esecuzione sulla porta **4503**. L&#39;errore riportato sopra è previsto se non è possibile trovare un&#39;istanza AEM in esecuzione su http://localhost:4503.

1. Eseguire infine il comando seguente per distribuire il pacchetto `ui.apps` sulla porta **4504**:

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

   Si prevede di nuovo un errore di compilazione se non è disponibile AEM&#39;istanza in esecuzione sulla porta **4504**. Il parametro `aem.port` è definito nel file POM in `aem-guides-wknd/pom.xml`.

Il modulo **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** è strutturato nello stesso modo del modulo **ui.apps**. L&#39;unica differenza è che il modulo **ui.content** contiene il contenuto **mutable**. **Il contenuto** reciproco si riferisce essenzialmente a configurazioni non di codice come Modelli, Criteri o strutture di cartelle memorizzate nel controllo del codice sorgente  **** ma che possono essere modificate direttamente in un&#39;istanza AEM. Questo verrà esplorato in modo molto più dettagliato nel capitolo Pagine e Modelli.

Gli stessi comandi Maven utilizzati per creare il modulo **ui.apps** possono essere utilizzati per creare il modulo **ui.content**. Non esitate a ripetere i passaggi descritti qui sopra dalla cartella **ui.content**.