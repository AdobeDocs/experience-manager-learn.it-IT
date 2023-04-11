---
title: Configurazione rapida SPA Editor e SPA remoto
description: Scopri come iniziare a usare un SPA remoto e AEM SPA Editor in 15 minuti!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 5%

---

# Configurazione rapida

La configurazione rapida è una procedura dettagliata che illustra come installare ed eseguire l’app WKND e come SPA remota e come crearla utilizzando AEM Editor SPA.

La configurazione rapida consente di passare direttamente allo stato finale dell&#39;esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Un video dettagliato sulla configurazione rapida_

## Prerequisiti

Questa esercitazione richiede quanto segue:

+ [SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/it/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Prerequisiti solo per macOS
   + [Xcode](https://developer.apple.com/xcode/) o [Strumenti della riga di comando Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip o versione successiva](https://github.com/adobe/aem-guides-wknd/releases)
+ [codice sorgente aem-guides-wknd-graphql (ramo: funzionalità/editor-spa)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Questa esercitazione presuppone:

+ [Codice Microsoft® Visual Studio](https://visualstudio.microsoft.com/) come IDE
+ Una directory di lavoro di `~/Code/wknd-app`
+ Esecuzione dell’SDK AEM come servizio Author in `http://localhost:4502`
+ Esecuzione dell&#39;SDK AEM con il locale `admin` account con password `admin`
+ Esecuzione del SPA su `http://localhost:3000`

## Avvia l&#39;AEM SDK Quickstart

Scarica e installa l&#39;SDK AEM Quickstart sulla porta 4502, con impostazione predefinita `admin/admin` credenziali.

1. [Scarica l&#39;SDK più recente AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Decomprimi l&#39;SDK AEM in `~/aem-sdk`
1. Esegui AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK viene avviato e avviato automaticamente al [http://localhost:4502](Http://localhost:4502). Accedi utilizzando le seguenti credenziali:

+ Nome utente: `admin`
+ Password: `admin`

## Scarica e installa il pacchetto WKND Site

Questa esercitazione ha una dipendenza da __WKND 2.1.0+__ progetto (per contenuto).

1. [Scarica la versione più recente di `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Accedi a AEM Gestione pacchetti dell&#39;SDK all&#39;indirizzo [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con `admin` credenziali.
1. __Carica__ la `aem-guides-wknd.all.x.x.x.zip` scaricato al passaggio 1
1. Tocca __Installa__ pulsante per la voce `aem-guides-wknd.all-x.x.x.zip`

## Scaricare e installare pacchetti di SPA per app WKND

Per eseguire una configurazione rapida, vengono forniti qui AEM pacchetti che contengono la configurazione e il contenuto finali AEM dell’esercitazione.

1. [Scarica ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Scarica ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Accedi a AEM Gestione pacchetti dell&#39;SDK all&#39;indirizzo [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con `admin` credenziali.
1. __Carica__ la `wknd-app.all.x.x.x.zip` scaricato al passaggio 1
1. Tocca __Installa__ pulsante per la voce `wknd-app.all.x.x.x.zip`
1. __Carica__ la `wknd-app.ui.content.sample.x.x.x.zip` scaricato al passaggio 2
1. Tocca __Installa__ pulsante per la voce `wknd-app.ui.content.sample.x.x.x.zip`

## Scarica la sorgente dell’app WKND

Scarica il codice sorgente dell’app WKND da Github.com e cambia il ramo contenente le modifiche alle SPA eseguite in questa esercitazione.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Avvia l&#39;applicazione SPA

Dalla directory principale del progetto, installa le dipendenze npm dei progetti SPA ed esegui l’applicazione.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

In caso di errori durante l&#39;esecuzione `npm install` prova i seguenti passaggi:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verifica che il SPA sia in esecuzione in [http://localhost:3000](Http://localhost:3000).

## Creare contenuti in AEM Editor SPA

Prima di creare contenuti, disponi le finestre del browser in modo che AEM Author (`http://localhost:4502`) è a sinistra e il SPA remoto (`http://localhost:3000`) viene eseguito a destra. Questa disposizione ti consente di vedere in che modo le modifiche ai contenuti di origine AEM si riflettono immediatamente nel SPA.

1. Accedi a [Servizio di authoring dell’SDK AEM](Http://localhost:4502) come `admin`
1. Passa a __Sites > WKND App > noi > it__
1. Modifica __Home page app WKND__
1. Passa a __Modifica__ modalità

### Creare il componente fisso della vista Home

1. Tocca il testo __Avventure WKND__ per attivare il componente Titolo fisso (codificato nella vista Home SPA)
1. Tocca __chiave__ sulla barra delle azioni del componente Titolo
1. Modifica il contenuto del componente Titolo e salva
1. Aggiorna il SPA in esecuzione su `http://localhost:3000` e vedere che le modifiche apportate si riflettono

### Creare il componente contenitore della vista Home

1. Durante la modifica __Home page app WKND__...
1. Espandi la __Barra laterale dell’editor di SPA__ (a sinistra)
1. Tocca __Componenti__ icone
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore situato sotto il logo WKND e sopra il componente Titolo fisso
1. Aggiorna il SPA in esecuzione su `http://localhost:3000` e vedere che le modifiche apportate si riflettono

### Creare un componente contenitore su un percorso dinamico

1. Passa a __Anteprima__ modalità in Editor SPA
1. Tocca __Bali Surf Camp__ scheda e naviga al percorso dinamico
1. Aggiungi, modifica o rimuovi componenti dal componente contenitore che si trova sopra il __Itinerario__ titolo
1. Aggiorna il SPA in esecuzione su `http://localhost:3000` e vedere che le modifiche apportate si riflettono

Nuove pagine AEM sotto __Home page dell&#39;app WKND > Avventura__ _deve_ hanno un nome di pagina AEM che corrisponde al nome del frammento di contenuto dell’avventura corrispondente. Questo perché il percorso SPA per AEM mappatura pagina è basato sull’ultimo segmento del percorso, che è il nome del frammento di contenuto.

## Congratulazioni. 

Hai appena avuto un rapido assaggio di come AEM editor SPA può migliorare il tuo SPA con aree controllate e modificabili! Se sei interessato, controlla il resto dell’esercitazione, ma assicurati di iniziare da zero, poiché in questa configurazione rapida l’ambiente di sviluppo locale è ora nello stato di fine dell’esercitazione!
