---
title: Impostare gli strumenti di sviluppo per AEM come sviluppo Cloud Service
description: Configurare una macchina di sviluppo locale con tutti gli strumenti di base necessari per lo sviluppo rispetto AEM locale.
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1428'
ht-degree: 2%

---


# Impostare strumenti di sviluppo

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Strumenti di sviluppo della configurazione"
>abstract="Lo sviluppo di Adobe Experience Manager (AEM) richiede l’installazione e la configurazione di un set minimo di strumenti di sviluppo nel computer sviluppatore. Questi strumenti includono Java, Maven, Adobe I/O CLI, Development IDE e altro ancora."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Nozioni di base sullo sviluppo"

Lo sviluppo di Adobe Experience Manager (AEM) richiede l’installazione e la configurazione di un set minimo di strumenti di sviluppo nel computer sviluppatore. Questi strumenti supportano lo sviluppo e la creazione di progetti AEM.

Tenere presente che `~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows, è l’equivalente di `%HOMEPATH%`.

## Installa Java

Experience Manager è un’applicazione Java e quindi richiede l’SDK Java per supportare lo sviluppo e il AEM come SDK di Cloud Service.

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
      + Homebrew su macOS richiede [Xcode](https://apps.apple.com/us/app/xcode/id497799835) o [Strumenti della riga di comando](https://developer.apple.com/download/more/), installabile tramite il comando:
         + `xcode-select --install`
   + [Installa Homebrew su Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Installa Homebrew su Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Verifica che Homebrew sia installato eseguendo il comando: `brew --version`

![Ebraico](./assets/development-tools/homebrew.png)

Se utilizzi Homebrew, segui le istruzioni __Installa utilizzando Homebrew__ nelle sezioni seguenti. Se stai __non__ utilizzando Homebrew, installa gli strumenti utilizzando i collegamenti specifici del sistema operativo.

## Installa Git

[](https://git-scm.com/) Attiva il sistema di gestione del controllo del codice sorgente utilizzato da  [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html) ed è quindi necessario per lo sviluppo.

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

[Node.](https://nodejs.org) jsis un ambiente di runtime JavaScript utilizzato per lavorare con le risorse front-end di un progetto  __ui.__ frontendsub di un progetto AEM. Node.js è distribuito con [npm](https://www.npmjs.com/), è il gestore di pacchetti Node.js di fatto, utilizzato per gestire le dipendenze JavaScript.

+ Installa Node.js utilizzando Homebrew
   1. Apri il prompt dei comandi/terminale
   1. Esegui il comando: `brew install node`
   1. Verifica che Node.js sia installato, utilizzando il comando: `node -v`
   1. Verifica che npm sia installato utilizzando il comando: `npm -v`
+ Oppure, scarica e installa Node.js (macOS, Linux o Windows)
   1. [Scaricare e installare Node.js](https://nodejs.org/en/download/)
   1. Apri il prompt dei comandi/terminale
   1. Verifica che Node.js sia installato, utilizzando il comando: `node -v`
   1. Verifica che npm sia installato utilizzando il comando: `npm -v`

![Node.js e npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM progetti AEM basati su Project Archetype](https://github.com/adobe/aem-project-archetype) installano una versione isolata di Node.js al momento della creazione. È bene mantenere sincronizzata (o vicina) la versione del sistema di sviluppo locale delle versioni Node.js e npm specificate nel reactor pom.xml del progetto Maven AEM.
>
>Vedi questo esempio [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) per sapere dove trovare le versioni di Node.js e npm build.

## Installa Maven

Apache Maven è lo strumento a riga di comando Java open-source utilizzato per creare AEM progetti generati dall’Archetipo Maven AEM progetto. Tutti i principali IDE ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Codice di Visual Studio](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), ecc.) hanno integrato il supporto Maven.

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

L’ [Adobe I/O CLI](https://github.com/adobe/aio-cli), o `aio`, fornisce l’accesso alla riga di comando a diversi servizi Adobe, tra cui [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) e [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/O CLI svolge un ruolo integrale nello sviluppo su AEM come Cloud Service, in quanto fornisce agli sviluppatori la possibilità di:

+ Registri code da AEM as a Cloud Services services
+ Gestire le pipeline di Cloud Manager da CLI

### Installare Adobe I/O CLI

1. Assicurati che [Node.js sia installato](#node-js) in quanto Adobe I/O CLI è un modulo npm
   + Esegui `node --version` per confermare
1. Esegui `npm install -g @adobe/aio-cli` per installare globalmente il modulo npm `aio`

### Configurare il plug-in Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

Il plug-in Adobe I/O Cloud Manager consente a aio CLI di interagire con Adobe Cloud Manager tramite il comando `aio cloudmanager` .

1. Esegui `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` per installare il [plug-in aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

### Impostare il plug-in Adobe I/O CLI Asset compute{#aio-asset-compute}

Il plug-in Adobe I/O Cloud Manager consente a aio CLI di generare ed eseguire i processi di lavoro Asset compute tramite il comando `aio asset-compute` .

1. Esegui `aio plugins:install @adobe/aio-cli-plugin-asset-compute` per installare il plug-in di Asset compute [aio](https://github.com/adobe/aio-cli-plugin-asset-compute).

### Configurare l’autenticazione CLI di Adobe I/O

Affinché Adobe I/O CLI possa comunicare con Cloud Manager, è necessario creare un’integrazione [Cloud Manager in Adobe I/O Console](https://github.com/adobe/aio-cli-plugin-cloudmanager) e ottenere le credenziali per la corretta autenticazione.

1. Accedi a [console.adobe.io](https://console.adobe.io)
1. Assicurati che la tua organizzazione che include il prodotto Cloud Manager a cui connetterti sia attiva nel commutatore dell’organizzazione Adobe
1. Crea un nuovo o apri un programma di Adobe I/O [esistente](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + I programmi della console di Adobe I/O sono semplicemente raggruppamenti organizzativi di integrazioni, creazione o utilizzo e programmi esistenti in base a come desideri gestire le integrazioni
   + Se viene richiesto di creare un nuovo progetto, seleziona &quot;Progetto vuoto&quot; (vs. &quot;Crea da modello&quot;)
   + I programmi della console di Adobe I/O sono concetti diversi rispetto ai programmi di Cloud Manager
1. Creare una nuova integrazione API di Cloud Manager con il profilo &quot;Sviluppatore - Cloud Service&quot;
1. Per ottenere le credenziali dell’account di servizio (JWT) è necessario compilare il file [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) di Adobe I/O CLI
1. Carica il file `config.json` nell’Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Carica il file `private.key` nell’Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Inizia [l’esecuzione di comandi](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) per Cloud Manager tramite Adobe I/O CLI.

## Configurare l&#39;IDE di sviluppo

Lo sviluppo AEM consiste principalmente nello sviluppo Java e Front-end (JavaScript, CSS, ecc.) e nella gestione XML. Di seguito sono riportati gli IDE più popolari per lo sviluppo AEM.

### IDEA IntelliJ

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEA è un potente IDE per lo sviluppo Java. IntelliJ IDEA è dotato di due sapori, un&#39;edizione comunitaria gratuita e una versione commerciale (a pagamento) Ultimate. La versione Community gratuita è sufficiente per lo sviluppo AEM, tuttavia Ultimate [espande la sua funzionalità impostata](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [Scarica IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Scarica lo strumento Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Codice di Microsoft Visual Studio

__[Visual Studio Code](https://code.visualstudio.com/)__  (VS Code) è uno strumento gratuito open-source per sviluppatori front-end. È possibile impostare Visual Studio Code per integrare la sincronizzazione dei contenuti con AEM con l&#39;aiuto di uno strumento di Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code è la scelta ideale per gli sviluppatori front-end che creano principalmente codice front-end; JavaScript, CSS e HTML. Mentre VS Code dispone di supporto Java tramite [estensioni](https://code.visualstudio.com/docs/java/java-tutorial), potrebbe mancare alcune delle funzioni avanzate fornite da più specifiche Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Download del codice di Visual Studio](https://code.visualstudio.com/Download)
+ [Scarica lo strumento Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Scarica l&#39;estensione del codice VS aemfeed](https://aemfed.io/)
+ [Scarica AEM Sincronizza l&#39;estensione del codice VS](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse ](https://www.eclipse.org/ide/)__ IDEs è un popolare IDE per lo sviluppo Java e supporta il   __[AEM Developer ](https://eclipse.adobe.com/aem/dev-tools/)__ Toolsplug-in fornito da Adobe, fornendo un&#39;interfaccia grafica interna all&#39;IDE per l&#39;authoring e per sincronizzare il contenuto JCR con un&#39;istanza AEM locale.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Scarica Eclipse](https://www.eclipse.org/ide/)
+ [Download degli strumenti di sviluppo Eclipse](https://eclipse.adobe.com/aem/dev-tools/)
