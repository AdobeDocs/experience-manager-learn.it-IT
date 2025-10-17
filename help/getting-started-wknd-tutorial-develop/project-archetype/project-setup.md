---
title: Introduzione ad AEM Sites - Configurazione del progetto
description: Crea un progetto Maven con più moduli per gestire il codice e le configurazioni di un sito Experience Manager.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-3418
thumbnail: 30152.jpg
doc-type: Tutorial
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
duration: 502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1695'
ht-degree: 100%

---

# Configurazione del progetto {#project-setup}

Questo tutorial tratta la creazione di un progetto Maven con più moduli per gestire il codice e le configurazioni di un sito Adobe Experience Manager.

## Prerequisiti {#prerequisites}

Esamina gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](./overview.md#local-dev-environment). Assicurati di disporre di una nuova istanza di Adobe Experience Manager disponibile localmente e di non aver installato altri pacchetti di esempio/demo (diversi dai Service Pack richiesti).

## Obiettivo {#objective}

1. Scopri come generare un nuovo progetto AEM utilizzando un archetipo Maven.
1. Comprendi i diversi moduli generati dall&#39;archetipo del progetto AEM e come funzionano insieme.
1. Scopri in che modo i componenti core di AEM sono inclusi in un progetto AEM.

## Cosa stai per creare {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

In questo capitolo verrà generato un nuovo progetto Adobe Experience Manager utilizzando l&#39;[Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype). Il progetto AEM contiene codice, contenuto e configurazioni complete utilizzati per un’implementazione di Sites. Il progetto generato in questo capitolo funge da base per un&#39;implementazione del sito WKND e viene sviluppato nei capitoli futuri.

**Che cos’è un progetto Maven?** - [Apache Maven](https://maven.apache.org/) è uno strumento di gestione software per la generazione di progetti. Tutte le implementazioni di *Adobe Experience Manager* utilizzano i progetti Maven per generare, gestire e implementare codice personalizzato in AEM.

**Che cos’è un archetipo Maven?** - Un [archetipo Maven](https://maven.apache.org/archetype/index.html) è un modello o un pattern per la generazione di nuovi progetti. L&#39;archetipo del progetto AEM consente di generare un nuovo progetto con uno spazio dei nomi personalizzato e include una struttura di progetto che segue le best practice, accelerando notevolmente lo sviluppo del progetto.

## Creare il progetto {#create}

Sono disponibili due opzioni per la creazione di un progetto Maven con più moduli per AEM. Questa esercitazione utilizza l&#39;[Archetipo progetto AEM Maven **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager inoltre [fornisce una procedura guidata dell&#39;interfaccia utente](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html?lang=it) per avviare la creazione di un progetto di applicazione AEM. Il progetto sottostante generato dall&#39;interfaccia utente di Cloud Manager ha la stessa struttura di quello che si ottiene utilizzando direttamente l&#39;archetipo.

>[!NOTE]
>
>Questa esercitazione utilizza la versione **35** dell&#39;archetipo. Come best practice, utilizza sempre **l&#39;ultima versione** dell&#39;archetipo per generare un nuovo progetto.

La serie successiva di passaggi si svolgerà utilizzando un terminale della riga di comando basato su UNIX®, ma dovrebbe essere simile se utilizzi un terminale Windows.

1. Apri un terminale della riga di comando. Verifica che Maven sia installato:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Passa a una directory in cui desideri generare il progetto AEM. Può trattarsi di qualsiasi directory in cui desideri mantenere il codice origine del progetto. Ad esempio, una directory denominata `code` sotto la directory home dell&#39;utente:

   ```shell
   $ cd ~/code
   ```

1. Incolla quanto segue nella riga di comando per [generare il progetto in modalità batch](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Per eseguire il targeting di AEM 6.5.14+ sostituisci `aemVersion="cloud"` con `aemVersion="6.5.14"`.
   >
   > Inoltre, utilizza sempre l&#39;ultimo `archetypeVersion` facendo riferimento a [Archetipo progetto AEM > Utilizzo](https://github.com/adobe/aem-project-archetype#usage)

   Un elenco completo delle proprietà disponibili per la configurazione di un progetto [ è disponibile qui](https://github.com/adobe/aem-project-archetype#available-properties).

1. La seguente struttura di file e cartelle è generata dall&#39;archetipo Maven sul file system locale:

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
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Implementare i file e generare il progetto {#build}

Crea e implementa il codice del progetto in un&#39;istanza locale di AEM.

1. Verifica che un&#39;istanza di authoring di AEM sia in esecuzione localmente sulla porta **4502**.
1. Dalla riga di comando, passa alla directory del progetto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Esegui il seguente comando per generare e implementare l&#39;intero progetto in AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La generazione dura circa un minuto e deve terminare con il seguente messaggio:

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   Il profilo Maven `autoInstallSinglePackage` compila i singoli moduli del progetto e implementa un singolo pacchetto all&#39;istanza di AEM. Per impostazione predefinita, questo pacchetto viene distribuito a un&#39;istanza di AEM in esecuzione localmente sulla porta **4502** e con le credenziali di `admin:admin`.

1. Passa a Gestione pacchetti nell&#39;istanza AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Dovrebbero essere visualizzati pacchetti per `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` e `aem-guides-wknd.all`.

1. Passa alla console Sites: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Il sito WKND è uno dei siti. Include una struttura del sito con una gerarchia Masters per Stati Uniti e Lingua. Questa gerarchia del sito si basa sui valori per `language_country` e `isSingleCountryWebsite` durante la generazione del progetto utilizzando l&#39;archetipo.

1. Apri la pagina **Stati Uniti** `>` **Inglese** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![console sito](assets/project-setup/aem-sites-console.png)

1. Il contenuto iniziale è già stato creato e sono disponibili diversi componenti da aggiungere a una pagina. Prova questi componenti per un&#39;idea delle funzionalità. Scopri le nozioni di base di un componente nel prossimo capitolo.

   ![Contenuto iniziale della home](assets/project-setup/start-home-page.png)

   *Contenuto di esempio generato dall&#39;archetipo*

## Ispezionare il progetto {#project-structure}

Il progetto AEM generato è costituito da singoli moduli Maven, ciascuno con un ruolo diverso. Questa esercitazione e la maggior parte degli sviluppi si concentrano su questi moduli:

* [core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=it) - Codice Java, principalmente sviluppatori back-end.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=it&=it) - Contiene il codice origine per CSS, JavaScript, Sass, TypeScript, principalmente per sviluppatori front-end.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=it) - Contiene definizioni di componenti e finestre di dialogo, incorpora CSS e JavaScript compilati come librerie client.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=it) - Contiene contenuti strutturali e configurazioni come modelli modificabili, schemi di metadati (/content, /conf).

* **all** - Si tratta di un modulo Maven vuoto che combina i moduli di cui sopra in un singolo pacchetto che può essere implementato in un ambiente AEM.

![Diagramma del progetto Maven](assets/project-setup/project-pom-structure.png)

Per ulteriori informazioni su **tutti** i moduli Maven, consulta la [documentazione di Archetipo progetto AEM](https://experienceleague.adobe.com/it/docs/experience-manager-core-components/using/developing/archetype/overview).

### Inclusione dei Componenti core {#core-components}

I [Componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) sono un set di componenti WCM (Gestione contenuti web) standardizzati per AEM. Questi componenti forniscono un set di base di una funzionalità e sono formattati, personalizzati ed estesi per singoli progetti.

L&#39;ambiente AEM as a Cloud Service include la versione più recente dei [Componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it). Pertanto, i progetti generati per AEM as a Cloud Service **non** includono un incorporamento di Componenti core AEM.

Per i progetti generati da AEM 6.5/6.4, l&#39;archetipo incorpora automaticamente i [Componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) nel progetto. Per AEM 6.5/6.4 è consigliabile incorporare i Componenti core AEM per garantire che venga implementata con il progetto l&#39;ultima versione. Ulteriori informazioni sull&#39;inclusione dei Componenti core [nel progetto sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=it#core-components).

## Gestione controllo origine {#source-control}

È sempre consigliabile utilizzare una forma di controllo del codice origine per gestire il codice nell&#39;applicazione. Questa esercitazione utilizza git e GitHub. Esistono diversi file generati da Maven e/o dall&#39;IDE di scelta che devono essere ignorati da SCM.

Maven crea una cartella di destinazione ogni volta che generi e installi il pacchetto di codice. La cartella di destinazione e i contenuti devono essere esclusi da SCM.

Nel modulo `ui.apps` osserva che sono stati creati molti file `.content.xml`. Questi file XML mappano i tipi di nodo e le proprietà del contenuto installato nel JCR. Questi file sono critici e **non possono** essere ignorati.

L&#39;archetipo del progetto AEM genera un file `.gitignore` di esempio che può essere utilizzato come punto di partenza per il quale i file possono essere tranquillamente ignorati. Il file viene generato in `<src>/aem-guides-wknd/.gitignore`.

## Congratulazioni. {#congratulations}

Congratulazioni, hai creato il tuo primo progetto AEM!

### Passaggi successivi {#next-steps}

Comprendi la tecnologia sottostante di un componente Adobe Experience Manager (AEM) Sites tramite un semplice esempio di `HelloWorld` con l&#39;esercitazione sulle [Nozioni di base sui componenti](component-basics.md).

## Comandi Maven avanzati (bonus) {#advanced-maven-commands}

Durante lo sviluppo, potresti lavorare con uno solo dei moduli e voler evitare di generare l&#39;intero progetto per risparmiare tempo. Puoi anche implementare direttamente su un&#39;istanza di AEM Publish o su un&#39;istanza di AEM non in esecuzione sulla porta 4502.

Esaminiamo ora alcuni profili e comandi Maven aggiuntivi che puoi utilizzare per una maggiore flessibilità durante lo sviluppo.

### Modulo core {#core-module}

Il modulo **[core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=it)** contiene tutto il codice Java™ associato al progetto. La generazione del modulo **core** implementa un bundle OSGi in AEM. Per generare solo questo modulo:

1. Accedi alla cartella `core` (sotto `aem-guides-wknd`):

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

1. Passa a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Questa è la console web OSGi e contiene informazioni su tutti i bundle installati nell&#39;istanza di AEM.

1. Attiva/disattiva la colonna di ordinamento **Id** per visualizzare il bundle WKND installato e attivo.

   ![Bundle core](assets/project-setup/wknd-osgi-console.png)

1. Puoi visualizzare il percorso &quot;fisico&quot; del file jar in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Posizione CRXDE di Jar](assets/project-setup/jcr-bundle-location.png)

### Moduli Ui.apps e Ui.content {#apps-content-module}

Il modulo maven **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=it)** include tutto il codice di rendering necessario per il sito sotto `/apps`. Include i file CSS/JS che verranno memorizzati in un formato AEM denominato [clientlibs](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/developing/introduction/clientlibs). Sono inclusi anche gli script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=it) per il rendering del codice HTML dinamico. Puoi considerare il modulo **ui.apps** come una mappa della struttura contenuta nel JCR, ma in un formato che può essere memorizzato in un file system e impegnato nel controllo del codice origine. Il modulo **ui.apps** contiene solo codice.

Per generare solo questo modulo:

1. Dalla riga di comando. Passa alla cartella `ui.apps` (sotto `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Esegui il comando seguente:

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Passa a [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Dovresti visualizzare il pacchetto `ui.apps` come primo pacchetto installato e avere una marca temporale più recente di qualsiasi altro pacchetto.

   ![Pacchetto ui.apps installato](assets/project-setup/ui-apps-package.png)

1. Torna alla riga di comando ed esegui il comando seguente (nella cartella `ui.apps`):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   Il profilo `autoInstallPackagePublish` è destinato a implementare il pacchetto in un ambiente di pubblicazione in esecuzione sulla porta **4503**. L&#39;errore di cui sopra è previsto se non è possibile trovare un&#39;istanza di AEM in esecuzione su http://localhost:4503.

1. Esegui infine il comando seguente per implementare il pacchetto `ui.apps` sulla porta **4504**:

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

   Anche in questo caso, è previsto un errore di generazione se non è disponibile alcuna istanza di AEM in esecuzione sulla porta **4504**. Il parametro `aem.port` è definito nel file POM in `aem-guides-wknd/pom.xml`.

Il modulo **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=it)** è strutturato nello stesso modo del modulo **ui.apps**. L&#39;unica differenza consiste nel fatto che il modulo **ui.content** contiene il contenuto noto come **mutabile**. Il contenuto **mutabile** si riferisce essenzialmente a configurazioni non di codice come Modelli, Criteri o strutture di cartelle memorizzate nel controllo del codice origine **ma** possono essere modificate direttamente in un&#39;istanza di AEM. Questo argomento è trattato più dettagliatamente nel capitolo Pagine e modelli.

Gli stessi comandi Maven utilizzati per generare il modulo **ui.apps** possono essere utilizzati per generare il modulo **ui.content**. Puoi ripetere i passaggi precedenti dalla cartella **ui.content**.

## Risoluzione di problemi

In caso di problemi durante la generazione del progetto utilizzando l&#39;Archetipo progetto AEM, consulta l&#39;elenco di [problemi noti](https://github.com/adobe/aem-project-archetype#known-issues) e l&#39;elenco di [problemi](https://github.com/adobe/aem-project-archetype/issues) aperti.

## Congratulazioni di nuovo! {#congratulations-bonus}

Congratulazioni per aver consultato il materiale bonus.

### Passaggi successivi {#next-steps-bonus}

Comprendi la tecnologia sottostante di un componente Adobe Experience Manager (AEM) Sites tramite un semplice esempio di `HelloWorld` con l&#39;esercitazione sulle [Nozioni di base sui componenti](component-basics.md).
