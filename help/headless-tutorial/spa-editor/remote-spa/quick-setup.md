---
title: Configurazione rapida SPA Editor e SPA remoto
description: Scopri come iniziare a usare un SPA remoto e AEM SPA Editor in 15 minuti!
topic: Senza testa, SPA, Sviluppo
feature: Editor SPA, componenti core, API, sviluppo
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '737'
ht-degree: 5%

---


# Configurazione rapida

La configurazione rapida è una procedura dettagliata che illustra come installare ed eseguire l’app WKND e come SPA remota e come crearla utilizzando AEM Editor SPA.

La configurazione rapida consente di passare direttamente allo stato finale dell&#39;esercitazione.

## Prerequisiti

Questa esercitazione richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/it/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql)

Questa esercitazione presuppone:

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Code come IDE
+ Una directory di lavoro di `~/Code/wknd-app`
+ Esecuzione dell&#39;SDK AEM come servizio Author su `http://localhost:4502`
+ Esecuzione dell&#39;SDK AEM con l&#39;account locale `admin` con password `admin`
+ Esecuzione del SPA su `http://localhost:3000`

## Avvia l&#39;AEM SDK Quickstart

Scarica e installa AEM SDK Quickstart sulla porta 4502, con le credenziali `admin/admin` predefinite.

1. [Scarica l&#39;SDK più recente AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Decomprimi l&#39;SDK AEM in `~/aem-sdk`
1. Esegui AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK verrà avviato e avviato automaticamente il [http://localhost:4502](Http://localhost:4502). Accedi utilizzando le seguenti credenziali:

+ Nome utente: `admin`
+ Password: `admin`

## Scarica e installa il pacchetto WKND Site

Questa esercitazione dipende dal progetto __WKND 0.3.0+__ (per contenuti).

1. [Scarica la versione più recente di  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Accedi a AEM Gestione pacchetti dell&#39;SDK all&#39;indirizzo [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con le credenziali `admin`.
1. ____ Carica il  `aem-guides-wknd.all.x.x.x.zip` download al passaggio 1
1. Toccare il pulsante __Installa__ per la voce `aem-guides-wknd.all-x.x.x.zip`

## Scaricare e installare pacchetti di SPA per app WKND

Per eseguire una configurazione rapida, vengono forniti pacchetti AEM che contengono la configurazione AEM finale e il contenuto dell’esercitazione.

1. Scarica `wknd-app.all.x.x.x.zip` dal pod DemoHub Assets
1. Scarica `wknd-app.ui.content.sample.x.x.x.zip` dal pod DemoHub Assets
1. Accedi a AEM Gestione pacchetti dell&#39;SDK all&#39;indirizzo [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con le credenziali `admin`.
1. ____ Carica il  `wknd-app.all.x.x.x.zip` download al passaggio 1
1. Toccare il pulsante __Installa__ per la voce `wknd-app.all.x.x.x.zip`
1. ____ Carica il  `wknd-app.ui.content.sample.x.x.x.zip` download al passaggio 2
1. Toccare il pulsante __Installa__ per la voce `wknd-app.ui.content.sample.x.x.x.zip`

## Scarica la sorgente dell’app WKND

Scarica il codice sorgente dell’app WKND da Github.com e cambia il ramo contenente le modifiche alle SPA eseguite in questa esercitazione.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
```

## Avvia l&#39;applicazione SPA

Dalla directory principale del progetto, installa le dipendenze npm dei progetti SPA ed esegui l’applicazione.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Se si verificano degli errori durante l&#39;esecuzione di `npm install`, prova i seguenti passaggi:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verifica che il SPA sia in esecuzione in [http://localhost:3000](Http://localhost:3000).

## Creare contenuti in AEM Editor SPA

Prima di creare contenuti, disponi le finestre del browser in modo che AEM Author (`http://localhost:4502`) sia a sinistra e il SPA remoto (`http://localhost:3000`) esegua a destra. Questa disposizione ti consente di vedere in che modo le modifiche ai contenuti di origine AEM si riflettono immediatamente nel SPA.

1. Accedi a [AEM servizio Author SDK](Http://localhost:4502) come `admin`
1. Passa a __Sites > App WKND > us > en__
1. Modifica __Pagina iniziale app WKND__
1. Passa alla modalità __Modifica__

### Creare il componente fisso della visualizzazione Home

1. Tocca il testo __Avventure WKND__ per attivare il componente Titolo fisso (codificato nella vista Home SPA)
1. Tocca l’icona __chiave inglese__ nella barra delle azioni del componente Titolo
1. Modifica il contenuto del componente Titolo e salva
1. Aggiorna il SPA in esecuzione su `http://localhost:3000` e osserva che le modifiche sono riflesse

### Creare il componente contenitore della vista Home

1. Durante la modifica di __WKND App Home Page__..
1. Espandi la barra laterale __SPA Editor__ (a sinistra)
1. Tocca le icone __Componenti__
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore situato sotto il logo WKND e sopra il componente Titolo fisso
1. Aggiorna il SPA in esecuzione su `http://localhost:3000` e osserva che le modifiche sono riflesse

### Creare un componente contenitore su un percorso dinamico

1. Passa alla modalità __Anteprima__ nell&#39;editor SPA
1. Tocca la scheda __Bali Surf Camp__ e naviga al suo percorso dinamico
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore che si trova sopra l’intestazione __Itinerario__
1. Aggiorna il SPA in esecuzione su `http://localhost:3000` e osserva che le modifiche sono riflesse

## Congratulazioni!

Hai appena avuto un rapido assaggio di come AEM editor SPA può migliorare il tuo SPA con aree controllate e modificabili! Se sei interessato, controlla il resto dell’esercitazione, ma assicurati di iniziare da zero, poiché in questa configurazione rapida l’ambiente di sviluppo locale è ora nello stato di fine dell’esercitazione!
