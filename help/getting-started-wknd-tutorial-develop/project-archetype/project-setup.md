---
title: Guida introduttiva ad AEM Sites - Configurazione del progetto
seo-title: Guida introduttiva ad AEM Sites - Configurazione del progetto
description: Include la creazione di un progetto Maven Multi Module per gestire il codice e le configurazioni per un sito AEM.
sub-product: sites
feature: AEM Project Archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
topic: Gestione dei contenuti, sviluppo
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '1888'
ht-degree: 4%

---


# Configurazione del progetto {#project-setup}

Questa esercitazione descrive la creazione di un progetto Maven Multi Module per gestire il codice e le configurazioni per un sito Adobe Experience Manager.

## Prerequisiti {#prerequisites}

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](../overview.md#local-dev-environment). Assicurati di disporre di una nuova istanza di Adobe Experience Manager disponibile localmente e che non siano stati installati pacchetti di esempio/demo aggiuntivi (diversi dai Service Pack richiesti).

## Obiettivo {#objective}

1. Scopri come generare un nuovo progetto AEM utilizzando un archetipo Maven.
1. Scopri i diversi moduli generati dall’Archetipo di progetto AEM e come funzionano insieme.
1. Scopri come AEM componenti core sono inclusi in un progetto AEM.

## Cosa verrà creato {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In questo capitolo, verrà generato un nuovo progetto Adobe Experience Manager utilizzando [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Il progetto AEM contiene tutto il codice, il contenuto e le configurazioni utilizzati per l’implementazione di Sites. Il progetto generato in questo capitolo fungerà da base per l’implementazione del sito WKND e sarà basato sui prossimi capitoli.

**Cos&#39;è un progetto Maven?** -  [Apache ](https://maven.apache.org/) Mavenis uno strumento di gestione software per la creazione di progetti. *Tutte le implementazioni di Adobe Experience* Manager utilizzano progetti Maven per generare, gestire e distribuire codice personalizzato sopra AEM.

**Cos&#39;è un archetipo Maven?** - Un  [archetipo ](https://maven.apache.org/archetype/index.html) Maven è un modello o un pattern per la generazione di nuovi progetti. L’archetipo AEM progetto ci consente di generare un nuovo progetto con uno spazio dei nomi personalizzato e di includere una struttura di progetto che segue le best practice, accelerando notevolmente il nostro progetto.

## Crea il progetto {#create}

Sono disponibili due opzioni per la creazione di un progetto Maven Multi-Module per AEM. Questa esercitazione sfrutterà il [Maven AEM Project Archetype **26**](https://github.com/adobe/aem-project-archetype). Cloud Manager inoltre [fornisce una procedura guidata di interfaccia utente](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) per avviare la creazione di un progetto di applicazione AEM. Il progetto sottostante generato dall’interfaccia utente di Cloud Manager si traduce nella stessa struttura dell’utilizzo diretto dell’archetipo.

>[!NOTE]
>
>Questa esercitazione utilizza la versione **26** dell&#39;archetipo. È sempre consigliabile utilizzare la versione **più recente** dell’archetipo per generare un nuovo progetto.

La serie successiva di passaggi verrà eseguita utilizzando un terminale a riga di comando basato su UNIX, ma dovrebbe essere simile se si utilizza un terminale Windows.

1. Aprire un terminale a riga di comando. Verifica che Maven sia installato:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Verifica che il profilo **adobe-public** sia attivo eseguendo il seguente comando:

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

   Se fai **not** vedi il **adobe-public**, indica che nel file `~/.m2/settings.xml` non è presente un riferimento corretto all&#39;archivio Adobe. Rivedi i passaggi per installare e configurare Apache Maven in [un ambiente di sviluppo locale](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Passa a una directory in cui desideri generare il progetto AEM. Può trattarsi di qualsiasi directory in cui si desidera mantenere il codice sorgente del progetto. Ad esempio, una directory denominata `code` sotto la home directory dell&#39;utente:

   ```shell
   $ cd ~/code
   ```

1. Incolla quanto segue nella riga di comando per [generare il progetto in modalità batch](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=26 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Se il targeting AEM 6.5.5+ sostituisci `aemVersion="cloud"` con `aemVersion="6.5.5"`. Se il targeting è 6.4.8+, utilizza `aemVersion="6.4.8"`.

   Un elenco completo delle proprietà disponibili per la configurazione di un progetto [è disponibile qui](https://github.com/adobe/aem-project-archetype#available-properties).

1. La seguente struttura di file e cartelle verrà generata dall’archetipo Maven sul file system locale:

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

## Distribuisci e genera il progetto {#build}

Crea e distribuisci il codice del progetto in un&#39;istanza locale di AEM.

1. Assicurati di avere un&#39;istanza di authoring di AEM in esecuzione localmente sulla porta **4502**.
1. Dalla riga di comando accedi alla directory di progetto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Esegui il comando seguente per generare e distribuire l’intero progetto in AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La build richiede circa un minuto e termina con il seguente messaggio:

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

   Il profilo Maven `autoInstallSinglePackage` compila i singoli moduli del progetto e distribuisce un singolo pacchetto all’istanza AEM. Per impostazione predefinita, questo pacchetto verrà distribuito in un&#39;istanza AEM in esecuzione localmente sulla porta **4502** e con le credenziali di `admin:admin`.

1. Passa a Gestione pacchetti nell&#39;istanza AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Dovresti visualizzare i pacchetti per `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` e `aem-guides-wknd.all`.

1. Passa alla console Sites : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Il sito WKND sarà uno dei siti. Includerà una struttura del sito con una gerarchia di master lingua e Stati Uniti. Questa gerarchia del sito si basa sui valori per `language_country` e `isSingleCountryWebsite` durante la generazione del progetto utilizzando l’archetipo.

1. Apri la pagina **US** `>` **Inglese** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![console del sito](assets/project-setup/aem-sites-console.png)

1. Il contenuto iniziale è già stato creato e diversi componenti possono essere aggiunti a una pagina. Sperimenta con questi componenti per avere un&#39;idea della funzionalità. Scoprirai le nozioni di base di un componente nel capitolo successivo.

   ![Contenuto iniziale della pagina principale](assets/project-setup/start-home-page.png)

   *Contenuto di esempio generato dall’Archetype*

## Inspect il progetto {#project-structure}

Il progetto AEM generato è costituito da singoli moduli Maven, ciascuno con un ruolo diverso. Questa esercitazione e la maggior parte dello sviluppo si concentrano su questi moduli:

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)  - Java Code, principalmente sviluppatori back-end.
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)  - Contiene il codice sorgente per CSS, JavaScript, Sass, Type Script, principalmente per sviluppatori front-end.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)  - Contiene le definizioni dei componenti e delle finestre di dialogo, incorpora CSS e JavaScript compilati come librerie client.
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html)  - contiene contenuti strutturali e configurazioni come modelli modificabili, schemi di metadati (/content, /conf).

* **all**  - questo è un modulo Maven vuoto che combina i moduli di cui sopra in un singolo pacchetto che può essere distribuito in un ambiente AEM.

![Diagramma del progetto Maven](assets/project-setup/project-pom-structure.png)

Per ulteriori informazioni sui moduli Maven, consulta la [AEM documentazione di Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) .****

### Inclusione dei componenti core {#core-components}

[I ](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/introduction.html) componenti core di AEM sono un set di componenti WCM (Web Content Management) standardizzati per AEM. Questi componenti forniscono un set di riferimento di una funzionalità e sono progettati per essere formattati, personalizzati ed estesi per singoli progetti.

Gli ambienti AEM come Cloud Service includono la versione più recente di [AEM Componenti core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html). Pertanto, i progetti generati per AEM come Cloud Service **not** includono un incorporamento di AEM componenti core.

Per AEM progetti generati in 6.5/6.4, l’archetipo incorpora automaticamente [AEM componenti core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) nel progetto. È consigliabile incorporare AEM 6.5/6.4 AEM componenti core per garantire che la versione più recente venga distribuita con il progetto. Ulteriori informazioni su come i componenti core sono [inclusi nel progetto sono disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Gestione del controllo del codice sorgente {#source-control}

È sempre consigliabile utilizzare una qualche forma di controllo del codice sorgente per gestire il codice nell’applicazione. Questa esercitazione utilizza Git e GitHub. Ci sono diversi file che vengono generati da Maven e/o dall’IDE che sceglie e che devono essere ignorati da SCM.

Maven creerà una cartella di destinazione ogni volta che crei e installi il pacchetto di codice. La cartella e il contenuto di destinazione devono essere esclusi da SCM.

Sotto `ui.apps` osservate che vengono creati molti file `.content.xml`. Questi file XML mappano i tipi di nodo e le proprietà del contenuto installato nel JCR. Questi file sono critici e devono **non** essere ignorati.

L’archetipo del progetto AEM genera un file di esempio `.gitignore` che può essere utilizzato come punto di partenza per il quale i file possono essere tranquillamente ignorati. Il file viene generato in `<src>/aem-guides-wknd/.gitignore`.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo progetto AEM!

### Passaggi successivi {#next-steps}

Scopri la tecnologia di base di un componente Adobe Experience Manager (AEM) Sites tramite un semplice esempio `HelloWorld` con l’esercitazione [Nozioni di base sui componenti](component-basics.md) .

## Comandi Maven avanzati (Bonus) {#advanced-maven-commands}

Durante lo sviluppo potresti lavorare con uno solo dei moduli e vuoi evitare di costruire l&#39;intero progetto per risparmiare tempo. Puoi anche implementare direttamente in un’istanza di AEM Publish o forse in un’istanza di AEM non in esecuzione sulla porta 4502.

Ora esamineremo alcuni profili e comandi Maven aggiuntivi che puoi utilizzare per una maggiore flessibilità durante lo sviluppo.

### Modulo core {#core-module}

Il modulo **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** contiene tutto il codice Java associato al progetto. Quando viene generato, distribuisce un bundle OSGi a AEM. Per creare solo questo modulo:

1. Passa alla cartella `core` (sotto `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Esegui il comando seguente:

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

1. Passa a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Questa è la console Web OSGi e contiene informazioni su tutti i bundle installati nell&#39;istanza AEM.

1. Attiva la colonna di ordinamento **Id** e dovresti visualizzare il bundle WKND installato e attivo.

   ![Bundle core](assets/project-setup/wknd-osgi-console.png)

1. Puoi vedere la posizione &#39;fisica&#39; del barattolo in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Posizione CRXDE del Jar](assets/project-setup/jcr-bundle-location.png)

### Moduli Ui.apps e Ui.content {#apps-content-module}

Il modulo maven **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** contiene tutto il codice di rendering necessario per il sito sotto `/apps`. Ciò include CSS/JS che verranno memorizzati in un formato AEM denominato [clientlibs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/clientlibs.html). Sono inclusi anche gli script [HTL](https://docs.adobe.com/content/help/it-IT/experience-manager-htl/using/overview.html) per il rendering di HTML dinamico. È possibile considerare il modulo **ui.apps** come una mappa della struttura nel JCR, ma in un formato che può essere memorizzato in un file system e impegnato nel controllo del codice sorgente. Il modulo **ui.apps** contiene solo codice.

Per creare solo questo modulo:

1. Dalla riga di comando. Passa alla cartella `ui.apps` (sotto `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Esegui il comando seguente:

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

1. Passa a [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Dovresti visualizzare il pacchetto `ui.apps` come primo pacchetto installato e dovrebbe avere una marca temporale più recente rispetto a qualsiasi altro pacchetto.

   ![Pacchetto Ui.apps installato](assets/project-setup/ui-apps-package.png)

1. Torna alla riga di comando ed esegui il seguente comando (all&#39;interno della cartella `ui.apps` ):

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

   Il profilo `autoInstallPackagePublish` è destinato a distribuire il pacchetto in un ambiente Publish in esecuzione sulla porta **4503**. L&#39;errore precedente è previsto se non è possibile trovare un&#39;istanza AEM in esecuzione su http://localhost:4503.

1. Infine esegui il seguente comando per distribuire il pacchetto `ui.apps` sulla porta **4504**:

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

   Si prevede nuovamente un errore di compilazione se non è disponibile un&#39;istanza AEM in esecuzione sulla porta **4504**. Il parametro `aem.port` è definito nel file POM in `aem-guides-wknd/pom.xml`.

Il modulo **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** è strutturato nello stesso modo del modulo **ui.apps**. L&#39;unica differenza è che il modulo **ui.content** contiene il contenuto noto come **mutable**. ****   **** Il contenuto reciproco si riferisce essenzialmente a configurazioni non di codice come Modelli, Criteri o strutture di cartelle memorizzate nel controllo del codice sorgente e che possono essere modificate direttamente in un&#39;istanza AEM. Questo verrà descritto in modo molto più dettagliato nel capitolo Pagine e Modelli .

Gli stessi comandi Maven utilizzati per generare il modulo **ui.apps** possono essere utilizzati per creare il modulo **ui.content** . Puoi ripetere i passaggi precedenti dalla cartella **ui.content** .