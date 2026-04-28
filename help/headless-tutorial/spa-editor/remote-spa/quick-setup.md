---
title: Configurazione rapida dell’Editor delle applicazioni a pagina singola e dell’applicazione a pagina singola remota
description: Scopri come iniziare a utilizzare un’applicazione a pagina singola remota e l’editor di applicazioni a pagina singola di AEM in 15 minuti.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 13%

---

# Impostazione rapida

{{spa-editor-deprecation}}

La configurazione rapida è una procedura dettagliata che illustra come installare ed eseguire l’app WKND come applicazione a pagina singola remota e come eseguirne l’authoring con l’editor di applicazioni a pagina singola di AEM.

La configurazione rapida porta direttamente allo stato finale di questa esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Video introduttivo della configurazione rapida_

## Prerequisiti

Questo tutorial richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime)
+ [Node.js v18](https://nodejs.org/it/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Solo prerequisiti di macOS
   + [Xcode](https://developer.apple.com/xcode/) o [strumenti della riga di comando Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql (ramo: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Questo tutorial presuppone:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/it) come IDE
+ Una directory di lavoro di `~/Code/wknd-app`
+ Esecuzione di AEM SDK come servizio di authoring su `http://localhost:4502`
+ Esecuzione di AEM SDK con l’account `admin` locale con password `admin`
+ Esecuzione dell’applicazione a pagina singola su `http://localhost:3000`

## Avvia Quickstart di AEM SDK

Scarica e installa AEM SDK Quickstart sulla porta 4502, con le credenziali predefinite di `admin/admin`.

1. [Scarica la versione più recente di AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. Decomprimi AEM SDK in `~/aem-sdk`
1. Eseguire AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK viene avviato e avviato automaticamente il [http://localhost:4502](Http://localhost:4502). Log in using the following credentials:

+ Username: `admin`
+ Password: `admin`

## Download and install WKND Site package

This tutorial has a dependency on __WKND 2.1.0+&#39;s__ project (for content).

1. [Download the latest version of `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Log in to AEM SDK&#39;s Package Manager at [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) with the `admin` credentials.
1. __Upload__ the `aem-guides-wknd.all.x.x.x.zip` downloaded in step 1
1. Tap the __Install__ button for the entry `aem-guides-wknd.all-x.x.x.zip`

## Download and install WKND App SPA packages

To perform a quick setup, AEM packages are provided here that contain the tutorial&#39;s final  AEM configuration and content.

1. [Download `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Log in to AEM SDK&#39;s Package Manager at [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) with the `admin` credentials.
1. __Upload__ the `wknd-app.all.x.x.x.zip` downloaded in step 1
1. Tap the __Install__ button for the entry `wknd-app.all.x.x.x.zip`
1. __Upload__ the `wknd-app.ui.content.sample.x.x.x.zip` downloaded in step 2
1. Tap the __Install__ button for the entry `wknd-app.ui.content.sample.x.x.x.zip`

## Download the WKND App source

Download the WKND App&#39;s source code by from Github.com, and switch the branch containing the changes to the SPA performed in this tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Start the SPA application

From the project&#39;s root, install the SPA projects npm dependencies and run the application.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

If there are errors when running `npm install` try the following steps:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verify that the SPA is running at [http://localhost:3000](http://localhost:3000).

## Author content in AEM SPA Editor

Prima di creare il contenuto, disporre le finestre del browser in modo che AEM Author (`http://localhost:4502`) sia a sinistra e l&#39;applicazione a pagina singola remota (`http://localhost:3000`) sia a destra. Questa disposizione consente di vedere come le modifiche ai contenuti originati da AEM vengono immediatamente riportate nell’applicazione a pagina singola.

1. Accedi al servizio [AEM SDK Author](Http://localhost:4502) come `admin`
1. Passa a __Sites > App WKND > us > en__
1. Modifica __Home page app WKND__
1. Passa alla modalità __Modifica__

### Creare il componente fisso della visualizzazione Home

1. Tocca il testo __Avventure WKND__ per attivare il componente Titolo fisso (codificato nella visualizzazione Home dell&#39;applicazione a pagina singola)
1. Tocca l&#39;icona __chiave inglese__ nella barra delle azioni del componente Titolo
1. Modifica il contenuto del componente Titolo e salva
1. Aggiorna l&#39;applicazione a pagina singola in esecuzione su `http://localhost:3000` e osserva che le modifiche si riflettono

### Creare il componente contenitore della vista Home

1. Durante la modifica della __home page dell&#39;app WKND__...
1. Espandi la barra laterale dell&#39;editor di applicazioni a pagina singola ____ (a sinistra)
1. Tocca le icone __Componenti__
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore che si trova sotto il logo WKND e sopra il componente Titolo fisso
1. Aggiorna l&#39;applicazione a pagina singola in esecuzione su `http://localhost:3000` e osserva che le modifiche si riflettono

### Creare un componente contenitore su una route dinamica

1. Passa alla modalità __Anteprima__ nell&#39;editor SPA
1. Tocca la scheda __Bali Surf Camp__ e passa alla relativa route dinamica
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore che si trova sopra l&#39;intestazione __Itinerario__
1. Aggiorna l&#39;applicazione a pagina singola in esecuzione su `http://localhost:3000` e osserva che le modifiche si riflettono

Le nuove pagine di AEM nella __home page dell&#39;app WKND > Avventura__ _devono_ avere un nome di pagina di AEM che corrisponda al nome del frammento di contenuto dell&#39;avventura corrispondente. Questo perché la route SPA alla pagina AEM è basata sull’ultimo segmento della route, che è il nome del frammento di contenuto.

## Congratulazioni.

Hai appena avuto un rapido assaggio di come AEM SPA Editor può migliorare la tua SPA con aree controllate e modificabili! Se sei interessato, vedi il resto dell’esercitazione, ma assicurati di ricominciare da capo, poiché in questa configurazione rapida l’ambiente di sviluppo locale si trova ora allo stato finale dell’esercitazione.
