---
title: Impostare gli strumenti di sviluppo per lo sviluppo AEM as a Cloud Service
description: Imposta un computer di sviluppo locale con tutti gli strumenti di base necessari per lo sviluppo locale in AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3508
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '1302'
ht-degree: 6%

---

# Configurare gli strumenti di sviluppo {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configurare gli strumenti di sviluppo"
>abstract="Per sviluppare per Adobe Experience Manager (AEM) è necessario installare e configurare un set minimo di strumenti specifici nel computer dello sviluppatore, compresi Java, Maven, Adobe I/O CLI, IDE di sviluppo e altri."
>additional-url="https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk" text="Nozioni di base sullo sviluppo"

Per sviluppare per Adobe Experience Manager (AEM) è necessario installare e configurare un set minimo di strumenti specifici nel computer dello sviluppatore, Questi strumenti supportano lo sviluppo e la creazione di progetti AEM.

`~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows equivale a `%HOMEPATH%`.

## Installare Java

Experience Manager è un’applicazione Java e pertanto richiede Java SDK per supportare lo sviluppo e AEM as a Cloud Service SDK.

1. [Scarica e installa la versione più recente Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifica che Oracle Java 11 SDK sia installato eseguendo il comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## Installa Homebrew

_L&#39;utilizzo di Homebrew è facoltativo, ma consigliato._

Homebrew è un gestore di pacchetti open-source per macOS, Windows e Linux. Tutti gli strumenti di supporto possono essere installati separatamente, Homebrew fornisce un modo conveniente per installare e aggiornare una varietà di strumenti di sviluppo necessari per lo sviluppo Experience Manager.

1. Apri il terminale
1. Verificare se Homebrew è già installato eseguendo il comando: `brew --version`.
1. Se Homebrew non è installato, installarlo

>[!BEGINTABS]

>[!TAB macOS]

[Homebrew in macOS](https://brew.sh/) richiede [Xcode](https://apps.apple.com/us/app/xcode/id497799835) o [Strumenti riga di comando](https://developer.apple.com/download/more/), installabile tramite il comando:

```shell
$ xcode-select --install
```

>[!TAB Windows]

[Installa Homebrew in Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[Installa Homebrew su Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. Verificare che Homebrew sia installato eseguendo il comando: `brew --version`

![Ebraico](./assets/development-tools/homebrew.png)

Se utilizzi Homebrew, segui le istruzioni __Installa utilizzando Homebrew__ nelle sezioni seguenti. Se __non__ utilizzi Homebrew, installa gli strumenti utilizzando i collegamenti specifici del sistema operativo.

## Installare Git

[Git](https://git-scm.com/) è il sistema di gestione del controllo del codice sorgente utilizzato da [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html?lang=it) ed è pertanto necessario per lo sviluppo.

>[!BEGINTABS]

>[!TAB Installa Git utilizzando Homebrew]

1. Apri il terminale/prompt dei comandi
1. Eseguire il comando: `$ brew install git`
1. Verificare che Git sia installato utilizzando il comando: `$ git --version`

>[!TAB Scarica e installa Git]

1. [Scarica e installa Git](https://git-scm.com/downloads)
1. Apri il terminale/prompt dei comandi
1. Verificare che Git sia installato utilizzando il comando: `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## Installare Node.js (e npm){#node-js}

[Node.js](https://nodejs.org) è un ambiente runtime di JavaScript utilizzato per le risorse front-end del sottoprogetto __ui.frontend__ di un progetto AEM. Node.js è distribuito con [npm](https://www.npmjs.com/), è il gestore di pacchetti Node.js, utilizzato per gestire le dipendenze di JavaScript.

>[!BEGINTABS]

>[!TAB Installa Node.js utilizzando Homebrew]

1. Apri il terminale/prompt dei comandi
1. Eseguire il comando: `$ brew install node`
1. Verificare che Node.js sia installato utilizzando il comando: `$ node -v`
1. Verificare che npm sia installato utilizzando il comando: `$ npm -v`

>[!TAB Scarica e installa Node.js]

1. [Scarica e installa Node.js](https://nodejs.org/it/download/)
2. Apri il terminale/prompt dei comandi
3. Verificare che Node.js sia installato utilizzando il comando: `$ node -v`
4. Verificare che npm sia installato utilizzando il comando: `$ npm -v`

>[!ENDTABS]

![Node.js e npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>I progetti AEM basati su [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) installano una versione isolata di Node.js al momento della compilazione. È consigliabile mantenere sincronizzata (o vicina) la versione del sistema di sviluppo locale con le versioni di Node.js e npm specificate nel file pom.xml del progetto Reactor di AEM Maven.
>
>Consulta questo esempio [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) per sapere dove individuare le versioni di Node.js e npm.

## Installare Maven

Apache Maven è lo strumento da riga di comando Java open-source utilizzato per creare progetti AEM generati dall’archetipo Maven del progetto AEM. Tutti gli IDE principali ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/) e così via) dispongono del supporto Maven integrato.


>[!BEGINTABS]

>[!TAB Installa Maven utilizzando Homebrew]

1. Apri il terminale/prompt dei comandi
1. Eseguire il comando: `$ brew install maven`
1. Verificare che Maven sia installato utilizzando il comando: `$ mvn -v`

>[!TAB Scarica e installa Maven]

1. [Scarica Maven](https://maven.apache.org/download.cgi)
1. [Installa Maven](https://maven.apache.org/install.html)
1. Apri il terminale/prompt dei comandi
1. Verificare che Maven sia installato utilizzando il comando: `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## Configurare Adobe I/O CLI{#aio-cli}

[Adobe I/O CLI](https://github.com/adobe/aio-cli) o `aio` fornisce l&#39;accesso alla riga di comando a diversi servizi Adobe, tra cui [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) e [Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/O CLI svolge un ruolo fondamentale nello sviluppo su AEM as a Cloud Service in quanto consente agli sviluppatori di:

+ Registri di coda dai servizi di AEM as a Cloud Services
+ Gestire le pipeline Cloud Manager da CLI
+ Distribuisci in [ambienti di sviluppo rapido AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=it)

### Installare Adobe I/O CLI

1. Verificare che [Node.js sia installato](#node-js) poiché Adobe I/O CLI è un modulo npm
   + Esegui `node --version` per confermare
1. Eseguire `npm install -g @adobe/aio-cli` per installare il modulo npm `aio` a livello globale

### Configurare il plug-in Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

Il plug-in Cloud Manager di Adobe I/O consente all&#39;interfaccia della riga di comando aio di interagire con Adobe Cloud Manager tramite il comando `aio cloudmanager`.

1. Eseguire `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` per installare il plug-in di Cloud Manager [aio](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Configurare l’autenticazione CLI di Adobe I/O

Affinché Adobe I/O CLI possa comunicare con Cloud Manager, è necessario creare un&#39;integrazione di [Cloud Manager nella console di Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager) e ottenere le credenziali per l&#39;autenticazione.

1. Accedi a [console.adobe.io](https://console.adobe.io)
1. Assicurati che l’organizzazione che include il prodotto Cloud Manager a cui connettersi sia attiva nello switcher dell’organizzazione Adobe
1. Crea un nuovo programma [Adobe I/O](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md) o apri un programma esistente
   + I progetti della console Adobe I/O sono semplicemente raggruppamenti organizzativi di integrazioni, creano o utilizzano e progetti esistenti in base a come desideri gestire le integrazioni.
   + Se crei un nuovo progetto, seleziona &quot;Progetto vuoto&quot; se richiesto (anziché &quot;Crea da modello&quot;).
   + I programmi della console Adobe I/O sono concetti diversi rispetto ai programmi Cloud Manager
1. Creare una nuova integrazione API Cloud Manager
   + Selezionare il tipo di credenziali &quot;Oauth Server-to-server&quot;.
   + Seleziona il profilo di prodotto &quot;Deployment Manager - Cloud Service&quot;.
   + Salva API configurata
1. Per ottenere le credenziali necessarie per popolare il file [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) di Adobe I/O CLI, apri le credenziali &quot;OAuth Server-to-server&quot; appena create e seleziona &quot;Scarica JSON&quot; dalla barra delle azioni in alto a destra.
1. Apri il file JSON scaricato e rinominato tutte le chiavi in minuscolo. `CLIENT_ID`, ad esempio, diventa `client_id`.
1. Carica il file `config.json` in Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager /path/to/downloaded/json --file --json`

Inizia [l&#39;esecuzione dei comandi](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) per Cloud Manager tramite Adobe I/O CLI.

### Configurare il plug-in AEM Rapid Development Environment{#rde}

Il plug-in AEM Rapid Development Environment consente all&#39;interfaccia CLI dell&#39;aio di interagire con AEM as a Cloud Service [Rapid Development Environments](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=it) tramite il comando `aio aem:rde`.

1. Eseguire `aio plugins:install @adobe/aio-cli-plugin-aem-rde` per installare il plug-in [AEM Rapid Development Environments](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Configurare il plug-in Adobe I/O CLI Asset Compute{#aio-asset-compute}

Il plug-in Adobe I/O Cloud Manager consente all&#39;interfaccia della riga di comando aio di generare ed eseguire processi di lavoro Asset Compute tramite il comando `aio asset-compute`.

1. Eseguire `aio plugins:install @adobe/aio-cli-plugin-asset-compute` per installare il plug-in Asset Compute [aio](https://github.com/adobe/aio-cli-plugin-asset-compute).

## Configurare l’IDE di sviluppo

Lo sviluppo AEM consiste principalmente nello sviluppo Java e front-end (JavaScript, CSS, ecc.) e nella gestione XML. Di seguito sono riportati gli IDE più comuni per lo sviluppo AEM.

### IDEA IntelliJ

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ è un potente IDE per lo sviluppo Java. IntelliJ IDEA è disponibile in due versioni, una versione Community Edition gratuita e una versione Ultimate commerciale (a pagamento). La versione gratuita della community è sufficiente per lo sviluppo di AEM, tuttavia Ultimate [espande il set di funzionalità](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/327239?quality=12&learn=on&captions=ita)

+ [Scarica IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Scarica lo strumento repository](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Codice Microsoft Visual Studio

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code) è uno strumento gratuito open-source per sviluppatori front-end. È possibile configurare Visual Studio Code per integrare la sincronizzazione dei contenuti con AEM con l&#39;aiuto di uno strumento di Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code è la scelta ideale per gli sviluppatori front-end che creano principalmente codice front-end, ovvero JavaScript, CSS e HTML. Anche se il codice VS dispone del supporto Java tramite [estensioni](https://code.visualstudio.com/docs/java/java-tutorial), potrebbero mancare alcune delle funzioni avanzate fornite da più specifiche di Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Scarica codice Visual Studio](https://code.visualstudio.com/Download)
+ [Scarica lo strumento repository](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Scarica estensione codice VS di AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ è uno dei più diffusi IDE per lo sviluppo Java e supporta il plug-in __[Strumenti per sviluppatori AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=it)__ fornito da Adobe, che fornisce un&#39;interfaccia grafica interna all&#39;IDE per l&#39;authoring e la sincronizzazione dei contenuti JCR con un&#39;istanza AEM locale.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Scarica Eclipse](https://www.eclipse.org/ide/)
+ [Scarica gli strumenti di sviluppo Eclipse](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=it)
