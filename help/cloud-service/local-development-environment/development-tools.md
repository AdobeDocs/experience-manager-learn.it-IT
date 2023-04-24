---
title: Impostare gli strumenti di sviluppo per AEM sviluppo as a Cloud Service
description: Configurare una macchina di sviluppo locale con tutti gli strumenti di base necessari per lo sviluppo rispetto AEM locale.
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1502'
ht-degree: 7%

---

# Impostare strumenti di sviluppo {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configurare gli strumenti di sviluppo"
>abstract="Per sviluppare per Adobe Experience Manager (AEM) è necessario installare e configurare un set minimo di strumenti specifici nel computer dello sviluppatore, compresi Java, Maven, Adobe I/O CLI, IDE di sviluppo e altri."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=it" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=it" text="Nozioni di base sullo sviluppo"

Per sviluppare per Adobe Experience Manager (AEM) è necessario installare e configurare un set minimo di strumenti specifici nel computer dello sviluppatore, Questi strumenti supportano lo sviluppo e la creazione di progetti AEM.

Tieni presente che `~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows, è l&#39;equivalente di `%HOMEPATH%`.

## Installa Java

Experience Manager è un’applicazione Java e quindi richiede l’SDK Java per supportare lo sviluppo e l’SDK as a Cloud Service AEM.

1. [Scarica e installa l’SDK Java 11 della versione più recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifica che Java 11 SDK sia installato eseguendo il comando :
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Installa Homebrew

_L’utilizzo di Homebrew è facoltativo, ma consigliato._

Homebrew è un gestore di pacchetti open-source per macOS, Windows e Linux. Tutti gli strumenti di supporto possono essere installati separatamente, Homebrew fornisce un modo conveniente per installare e aggiornare una varietà di strumenti di sviluppo necessari per lo sviluppo Experience Manager.

1. Apri il terminale
1. Controlla se Homebrew è già installato eseguendo il comando: `brew --version`.
1. Se Homebrew non è installato, installare Homebrew
   + [Installa Homebrew su macOS](https://brew.sh/)
      + Homebreo su macOS richiede [Xcode](https://apps.apple.com/us/app/xcode/id497799835) o [Strumenti della riga di comando](https://developer.apple.com/download/more/), installabile tramite il comando:
         + `xcode-select --install`
   + [Installa Homebrew su Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Installa Homebrew su Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Verifica che Homebrew sia installato eseguendo il comando: `brew --version`

![Ebraico](./assets/development-tools/homebrew.png)

Se utilizzi l’ebraico, segui la __Installa con Homebrew__ le istruzioni riportate nelle sezioni seguenti. Se sei __not__ utilizzando Homebrew, installa gli strumenti utilizzando i collegamenti specifici del sistema operativo.

## Installa Git

[Git](https://git-scm.com/) è il sistema di gestione del controllo del codice sorgente utilizzato da [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html), e quindi è necessario per lo sviluppo.

+ Installare Git utilizzando l’inglese Homebrew
   1. Apri il prompt dei comandi/terminale
   1. Esegui il comando: `brew install git`
   1. Verifica che Git sia installato, utilizzando il comando: `git --version`
+ Oppure, scarica e installa Git (macOS, Linux o Windows)
   1. [Scarica e installa Git](https://git-scm.com/downloads)
   1. Apri il prompt dei comandi/terminale
   1. Verifica che Git sia installato, utilizzando il comando: `git --version`

![Git](./assets/development-tools/git.png)

## Installa Node.js (e npm){#node-js}

[Node.js](https://nodejs.org) è un ambiente di runtime JavaScript utilizzato per lavorare con le risorse front-end di un progetto AEM __ui.frontend__ sottoprogetto. Node.js è distribuito con [npm](https://www.npmjs.com/), è il gestore di pacchetti Node.js di fatto, utilizzato per gestire le dipendenze JavaScript.

+ Installa Node.js utilizzando Homebrew
   1. Apri il prompt dei comandi/terminale
   1. Esegui il comando: `brew install node`
   1. Verifica che Node.js sia installato, utilizzando il comando: `node -v`
   1. Verifica che npm sia installato utilizzando il comando: `npm -v`
+ Oppure, scarica e installa Node.js (macOS, Linux o Windows)
   1. [Scaricare e installare Node.js](https://nodejs.org/it/download/)
   1. Apri il prompt dei comandi/terminale
   1. Verifica che Node.js sia installato, utilizzando il comando: `node -v`
   1. Verifica che npm sia installato utilizzando il comando: `npm -v`

![Node.js e npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype)In AEM progetti basati su vengono installate una versione isolata di Node.js al momento della creazione. È bene mantenere sincronizzata (o vicina) la versione del sistema di sviluppo locale delle versioni Node.js e npm specificate nel reactor pom.xml del progetto Maven AEM.
>
>Vedi questo esempio [AEM reattore di progetto pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) per individuare le versioni di Node.js e npm build.

## Installa Maven

Apache Maven è lo strumento a riga di comando Java open-source utilizzato per creare AEM progetti generati dall’Archetipo Maven AEM progetto. Tutti gli IDE principali ([IDEA IntelliJ](https://www.jetbrains.com/idea/), [Codice di Visual Studio](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), ecc.) hanno integrato il supporto Maven.

+ Installare Maven utilizzando l’ebraico
   1. Apri il prompt dei comandi/terminale
   1. Esegui il comando: `brew install maven`
   1. Verifica che Maven sia installato, utilizzando il comando: `mvn -v`
+ Oppure, scarica e installa Maven (macOS, Linux o Windows)
   1. [Scarica Maven](https://maven.apache.org/download.cgi)
   1. [Installa Maven](https://maven.apache.org/install.html)
   1. Apri il prompt dei comandi/terminale
   1. Verifica che Maven sia installato, utilizzando il comando: `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Configurare Adobe I/O CLI{#aio-cli}

La [Adobe I/O CLI](https://github.com/adobe/aio-cli)oppure `aio`, fornisce l’accesso da riga di comando a diversi servizi Adobe, tra cui [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) e [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/O CLI svolge un ruolo integrale nello sviluppo su AEM as a Cloud Service in quanto fornisce agli sviluppatori la possibilità di:

+ Registri code da AEM as a Cloud Services services
+ Gestire le pipeline di Cloud Manager da CLI
+ Distribuisci su [AEM ambienti di sviluppo rapido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### Installare Adobe I/O CLI

1. Assicurati [Node.js è installato](#node-js) come Adobe I/O CLI è un modulo npm
   + Esegui `node --version` confermare
1. Esegui `npm install -g @adobe/aio-cli` per installare `aio` modulo npm globale

### Configurare il plug-in Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

Il plug-in Adobe I/O Cloud Manager consente a aio CLI di interagire con Adobe Cloud Manager tramite l’ `aio cloudmanager` comando.

1. Esegui `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` per installare [plug-in aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Configurare l’autenticazione CLI di Adobe I/O

Affinché Adobe I/O CLI comunichi con Cloud Manager, un [L’integrazione di Cloud Manager deve essere creata in Adobe I/O Console](https://github.com/adobe/aio-cli-plugin-cloudmanager), e le credenziali devono essere ottenute per l&#39;autenticazione.

1. Accedi a [console.adobe.io](https://console.adobe.io)
1. Assicurati che la tua organizzazione che include il prodotto Cloud Manager a cui connetterti sia attiva nel commutatore dell’organizzazione Adobe
1. Crea un nuovo o apri un esistente [programma Adobe I/O](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + I programmi della console di Adobe I/O sono semplicemente raggruppamenti organizzativi di integrazioni, creazione o utilizzo e programmi esistenti in base a come desideri gestire le integrazioni
   + Se viene richiesto di creare un nuovo progetto, seleziona &quot;Progetto vuoto&quot; (vs. &quot;Crea da modello&quot;)
   + I programmi della console di Adobe I/O sono concetti diversi rispetto ai programmi di Cloud Manager
1. Creare una nuova integrazione API di Cloud Manager con il profilo &quot;Sviluppatore - Cloud Service&quot;
1. Per ottenere le credenziali dell’account di servizio (JWT) è necessario popolare Adobe I/O CLI’s [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. Carica il `config.json` in Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Carica il `private.key` in Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Inizia [esecuzione di comandi](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) per Cloud Manager tramite Adobe I/O CLI.

### Configurare il plug-in AEM Rapid Development Environment{#rde}

Il plug-in AEM Rapid Development Environment consente a aio CLI di interagire con AEM as a Cloud Service [Ambienti di sviluppo rapidi](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) tramite `aio aem:rde` comando.

1. Esegui `aio plugins:install @adobe/aio-cli-plugin-aem-rde` per installare [Plug-in AEM Rapid Development Environments](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Impostare il plug-in Adobe I/O CLI Asset compute{#aio-asset-compute}

Il plug-in Adobe I/O Cloud Manager consente a aio CLI di generare ed eseguire i processi di lavoro Asset compute tramite il `aio asset-compute` comando.

1. Esegui `aio plugins:install @adobe/aio-cli-plugin-asset-compute` per installare [plug-in Asset compute aio](https://github.com/adobe/aio-cli-plugin-asset-compute).


## Configurare l&#39;IDE di sviluppo

Lo sviluppo AEM consiste principalmente nello sviluppo Java e Front-end (JavaScript, CSS, ecc.) e nella gestione XML. Di seguito sono riportati gli IDE più popolari per lo sviluppo AEM.

### IDEA IntelliJ

__[IDEA IntelliJ](https://www.jetbrains.com/idea/)__ è un potente IDE per lo sviluppo Java. IntelliJ IDEA è dotato di due sapori, un&#39;edizione comunitaria gratuita e una versione commerciale (a pagamento) Ultimate. La versione comunitaria gratuita è sufficiente per lo sviluppo AEM, tuttavia l&#39;Ultimate [espande il set di funzionalità](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [Scarica IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Scarica lo strumento Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Codice di Microsoft Visual Studio

__[Codice di Visual Studio](https://code.visualstudio.com/)__ (VS Code) è uno strumento gratuito open-source per sviluppatori front-end. È possibile configurare Visual Studio Code per integrare la sincronizzazione dei contenuti con AEM con l&#39;aiuto di uno strumento di Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code è la scelta ideale per gli sviluppatori front-end che creano principalmente codice front-end; JavaScript, CSS e HTML. Mentre il codice VS supporta Java tramite [estensioni](https://code.visualstudio.com/docs/java/java-tutorial), potrebbe mancare alcune delle funzioni avanzate fornite da Java più specifiche.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Download del codice di Visual Studio](https://code.visualstudio.com/Download)
+ [Scarica lo strumento Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Scarica l&#39;estensione del codice VS aemfeed](https://aemfed.io/)
+ [Scarica AEM Sincronizza l&#39;estensione del codice VS](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[IDE Eclipse](https://www.eclipse.org/ide/)__ è un IDE popolare per lo sviluppo Java e supporta l’  __[Strumenti per sviluppatori AEM](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ plug-in fornito dall&#39;Adobe, che fornisce un&#39;interfaccia grafica interna all&#39;IDE per l&#39;authoring e per sincronizzare il contenuto JCR con un&#39;istanza AEM locale.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Scarica Eclipse](https://www.eclipse.org/ide/)
+ [Download degli strumenti di sviluppo Eclipse](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
