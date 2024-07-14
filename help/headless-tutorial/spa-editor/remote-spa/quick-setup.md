---
title: Configurazione rapida SPA Editor e SPA remoto
description: Scopri come iniziare a lavorare con un editor SPA per SPA e AEM in 15 minuti.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 2%

---

# Impostazione rapida

La configurazione rapida è una procedura dettagliata che illustra come installare ed eseguire l’app WKND come SPA remoto e come eseguirne l’authoring con l’Editor SPA dell’AEM.

La configurazione rapida porta direttamente allo stato finale di questa esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Video introduttivo della configurazione rapida_

## Prerequisiti

Questo tutorial richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/it/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Solo prerequisiti di macOS
   + [Xcode](https://developer.apple.com/xcode/) o [strumenti della riga di comando Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql (ramo: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Questo tutorial presuppone:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) come IDE
+ Una directory di lavoro di `~/Code/wknd-app`
+ Esecuzione dell&#39;SDK AEM come servizio Author in `http://localhost:4502`
+ Esecuzione dell&#39;SDK AEM con l&#39;account `admin` locale con password `admin`
+ Esecuzione dell&#39;SPA su `http://localhost:3000`

## Avviare Quickstart dell’SDK dell’AEM

Scarica e installa Quickstart dell&#39;SDK AEM sulla porta 4502, con le credenziali predefinite di `admin/admin`.

1. [Scarica l&#39;ultimo SDK per AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Decomprimi l&#39;SDK per AEM in `~/aem-sdk`
1. Eseguire Quickstart Jar per l’SDK dell’AEM

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

L&#39;SDK per AEM viene avviato e avviato automaticamente il [http://localhost:4502](Http://localhost:4502). Accedi utilizzando le seguenti credenziali:

+ Nome utente: `admin`
+ Password: `admin`

## Scaricare e installare il pacchetto del sito WKND

Questa esercitazione ha una dipendenza dal progetto __di__ WKND 2.1.0+ (per il contenuto).

1. [Scarica la versione più recente di `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Accedi a Gestione pacchetti dell&#39;SDK AEM all&#39;indirizzo [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con le credenziali `admin`.
1. __Carica__ `aem-guides-wknd.all.x.x.x.zip` scaricato nel passaggio 1
1. Tocca il pulsante __Installa__ per la voce `aem-guides-wknd.all-x.x.x.zip`

## Scaricare e installare i pacchetti WKND App SPA

Per eseguire una configurazione rapida, qui vengono forniti i pacchetti AEM che contengono la configurazione e il contenuto finale dell&#39;AEM del tutorial.

1. [Scarica ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Scarica ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Accedi a Gestione pacchetti dell&#39;SDK AEM all&#39;indirizzo [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con le credenziali `admin`.
1. __Carica__ `wknd-app.all.x.x.x.zip` scaricato nel passaggio 1
1. Tocca il pulsante __Installa__ per la voce `wknd-app.all.x.x.x.zip`
1. __Carica__ `wknd-app.ui.content.sample.x.x.x.zip` scaricato nel passaggio 2
1. Tocca il pulsante __Installa__ per la voce `wknd-app.ui.content.sample.x.x.x.zip`

## Scarica l’origine dell’app WKND

Scarica il codice sorgente dell’app WKND da Github.com e cambia il ramo contenente le modifiche all’SPA eseguite in questa esercitazione.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Avviare l’applicazione SPA

Dalla directory principale del progetto, installa i progetti SPA npm dependencies ed esegui l’applicazione.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

In caso di errori durante l&#39;esecuzione di `npm install`, effettuare le seguenti operazioni:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verificare che l&#39;SPA sia in esecuzione in [http://localhost:3000](http://localhost:3000).

## Creare contenuti nell’editor SPA dell’AEM

Prima dell&#39;authoring, disporre le finestre del browser in modo che l&#39;istanza Autore AEM (`http://localhost:4502`) sia a sinistra e che l&#39;SPA remoto (`http://localhost:3000`) sia a destra. Questa disposizione consente di vedere come le modifiche ai contenuti originati dall’AEM vengono immediatamente riportate nell’SPA.

1. Accedi a [servizio di authoring SDK per AEM](Http://localhost:4502) come `admin`
1. Passa a __Sites > App WKND > us > en__
1. Modifica __Home page app WKND__
1. Passa alla modalità __Modifica__

### Creare il componente fisso della visualizzazione Home

1. Tocca il testo __Avventure WKND__ per attivare il componente Titolo fisso (codificato nella visualizzazione Home dell&#39;SPA)
1. Tocca l&#39;icona __chiave inglese__ nella barra delle azioni del componente Titolo
1. Modifica il contenuto del componente Titolo e salva
1. Aggiorna l&#39;SPA in esecuzione su `http://localhost:3000` e osserva che le modifiche si riflettono

### Creare il componente contenitore della vista Home

1. Durante la modifica della __home page dell&#39;app WKND__...
1. Espandi la barra laterale dell&#39;editor __SPA__ (a sinistra)
1. Tocca le icone __Componenti__
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore che si trova sotto il logo WKND e sopra il componente Titolo fisso
1. Aggiorna l&#39;SPA in esecuzione su `http://localhost:3000` e osserva che le modifiche si riflettono

### Creare un componente contenitore su una route dinamica

1. Passa alla modalità __Anteprima__ nell&#39;editor SPA
1. Tocca la scheda __Bali Surf Camp__ e passa alla relativa route dinamica
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore che si trova sopra l&#39;intestazione __Itinerario__
1. Aggiorna l&#39;SPA in esecuzione su `http://localhost:3000` e osserva che le modifiche si riflettono

Le nuove pagine AEM nella __home page dell&#39;app WKND > Adventure__ _must_ hanno un nome di pagina AEM che corrisponde al nome del frammento di contenuto dell&#39;avventura corrispondente. Questo perché il percorso dell’SPA per la mappatura della pagina dell’AEM è basato sull’ultimo segmento del percorso, che è il nome del frammento di contenuto.

## Congratulazioni.

Hai appena avuto un rapido assaggio di come AEM SPA Editor può migliorare il vostro SPA con aree controllate e modificabili! Se sei interessato, vedi il resto dell’esercitazione, ma assicurati di ricominciare da capo, poiché in questa configurazione rapida l’ambiente di sviluppo locale si trova ora allo stato finale dell’esercitazione.
