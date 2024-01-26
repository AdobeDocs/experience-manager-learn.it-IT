---
title: Configurazione rapida AEM Headless per AEM as a Cloud Service
description: La configurazione rapida AEM Headless ti consente di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un’app React che consuma il contenuto rispetto alle API GraphQL AEM Headless.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 850
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# Configurazione rapida AEM Headless per AEM as a Cloud Service

La configurazione rapida AEM Headless ti consente di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un esempio di app React (SPA) che consuma il contenuto rispetto alle API AEM Headless GraphQL.

## Prerequisiti

Per eseguire questa configurazione rapida sono necessari i seguenti elementi:

+ Ambiente sandbox as a Cloud Service per AEM (preferibilmente sviluppo)
+ Accesso a AEM as a Cloud Service e Cloud Manager
   + __Amministratore AEM__ accesso a AEM as a Cloud Service
   + __Cloud Manager - Responsabile dell’implementazione__ accesso a Cloud Manager
+ I seguenti strumenti devono essere installati localmente:
   + [Node.js v18](https://nodejs.org/it/)
   + [Git](https://git-scm.com/)
   + Un IDE (ad esempio, [Codice Microsoft® Visual Studio](https://code.visualstudio.com/))

## 1. Creare un archivio Git di Cloud Manager

Innanzitutto, crea un archivio Git di Cloud Manager utilizzato per distribuire il sito WKND. Il sito WKND è un progetto di esempio per un sito web AEM che contiene contenuti (frammenti di contenuto) e un endpoint AEM GraphQL utilizzato dall’app React della configurazione rapida.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Accedi a [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Seleziona Cloud Manager __Programma__ che contiene l’ambiente as a Cloud Service per l’AEM da utilizzare per questa configurazione rapida
1. Creare un archivio Git per il progetto del sito WKND
   1. Seleziona __Archivi__ nella navigazione superiore
   1. Seleziona __Aggiungi archivio__ nella barra delle azioni superiore
   1. Denomina il nuovo archivio Git: `aem-headless-quick-setup-wknd`
      + I nomi degli archivi Git devono essere univoci per ogni organizzazione di Adobe,
   1. Seleziona __Salva__, e attendi l’inizializzazione dell’archivio Git

## 2. Invia il progetto di esempio del sito WKND all’archivio Git di Cloud Manager

Una volta creato l’archivio Git di Cloud Manager, clona il codice sorgente del progetto del sito WKND da GitHub e invialo all’archivio Git di Cloud Manager. Ora Cloud Manager per accedere e distribuire il progetto del sito WKND nell’ambiente as a Cloud Service dell’AEM.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. Dalla riga di comando, clona il codice sorgente del progetto WKND Site di esempio da GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Aggiungere l’archivio Git di Cloud Manager come archivio remoto
   1. Seleziona __Archivi__ nella navigazione superiore
   1. Seleziona __Accedi a dati archivio__ dalla barra delle azioni superiore
   1. Comando Execute trovato in __Aggiungere un remoto all’archivio Git__ dalla riga di comando

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Invia il codice sorgente del progetto di esempio dall’archivio Git locale all’archivio Git di Cloud Manager

   ```shell
   $ git push adobe main:main
   ```

   Quando vengono richieste le credenziali, fornisci __Nome utente__ e __Password__ da Cloud Manager __Informazioni archivio__ modale.

## 3. Distribuire il sito WKND in AEM as a Cloud Service

Quando il progetto del sito WKND viene inviato all’archivio Git di Cloud Manager, non può essere distribuito all’AEM as a Cloud Service tramite le pipeline di Cloud Manager.

Nota: il progetto del sito WKND fornisce contenuti di esempio che l’app React utilizza rispetto alle API GraphQL headless dell’AEM.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Allega un __Pipeline di implementazione non di produzione__ nel nuovo archivio Git
   1. Seleziona __Pipeline__ nella navigazione superiore
   1. Seleziona __Aggiungi pipeline__ dalla barra delle azioni superiore
   1. Il giorno __Configurazione__ scheda
      1. Seleziona __Pipeline di implementazione__ opzione
      1. Imposta il __Nome pipeline non di produzione__ a `Dev Deployment pipeline`
      1. Seleziona __Trigger distribuzione > In caso di modifiche Git__
      1. Seleziona __Comportamento in caso di errori di metriche importanti > Continua immediatamente__
      1. Seleziona __Continua__
   1. Il giorno __Codice sorgente__ scheda
      1. Seleziona __Codice full stack__ opzione
      1. Seleziona la __Ambiente di sviluppo as a Cloud Service AEM__ dal __Ambienti di implementazione idonei__ casella di selezione
      1. Seleziona `aem-headless-quick-setup-wknd` nel __Archivio__ casella di selezione
      1. Seleziona `main` dal __Ramo Git__ casella di selezione
      1. Seleziona __Salva__
1. Esegui il __Pipeline di implementazione per sviluppo__
   1. Seleziona __Panoramica__ nella navigazione superiore
   1. Individua la nuova __Pipeline di implementazione per sviluppo__ nel __Pipeline__ sezione
   1. Seleziona la __...__ a destra della voce pipeline
   1. Seleziona __Esegui__, e conferma nel modale
   1. Seleziona la __...__ a destra della pipeline attualmente in esecuzione
   1. Seleziona __Visualizza dettagli__
1. Dai dettagli dell’esecuzione della pipeline, monitora l’avanzamento fino al suo completamento corretto. L’esecuzione della pipeline dovrebbe richiedere tra 30 e 40 minuti.

## 4. Scaricare ed eseguire l’app WKND React

Con AEM as a Cloud Service avviato con il contenuto del progetto del sito WKND, scarica e avvia l’app di reazione WKND di esempio che utilizza il contenuto del sito WKND sulle API GraphQL headless dell’AEM.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Dalla riga di comando, clona il codice sorgente dell’app React da GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri la cartella `~/Code/aem-guides-wknd-graphql/react-app` nell’IDE.
1. Nell’IDE, apri il file `.env.development`.
1. Indicare l’AEM as a Cloud Service __Pubblica__ URI host del servizio da  `REACT_APP_HOST_URI` proprietà.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Per trovare l&#39;URI host del servizio di pubblicazione as a Cloud Service dell&#39;AEM:

   1. In Cloud Manager, seleziona __Ambienti__ nella navigazione superiore
   1. Seleziona __Sviluppo__ ambiente
   1. Individua il __Servizio di pubblicazione (AEM e Dispatcher)__ link __Segmenti di ambiente__ tabella
   1. Copia l’indirizzo del collegamento e utilizzalo come URI del servizio di pubblicazione as a Cloud Service dall’AEM

1. Nell’IDE, salva le modifiche apportate a `.env.development`
1. Dalla riga di comando, esegui React App

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. L’app React, in esecuzione localmente, inizia il [http://localhost:3000](http://localhost:3000) e visualizza un elenco di avventure, derivate dall’AEM as a Cloud Service utilizzando le API GraphQL dell’AEM Headless.

## 5. Modificare i contenuti in AEM

Con l’app di esempio WKND React che si connette e utilizza i contenuti delle API GraphQL headless dell’AEM, crea contenuti nel servizio di authoring dell’AEM e scopri come l’esperienza dell’app React si aggiorna insieme.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Accedi al servizio di authoring as a Cloud Service per AEM
1. Accedi a __Assets > File > WKND Condiviso > Inglese > Avventure__
1. Apri __Ciclismo nello Utah meridionale__ Cartella
1. Seleziona la __Ciclismo nello Utah meridionale__ Frammento di contenuto e seleziona __Modifica__ dalla barra delle azioni superiore
1. Aggiorna alcuni campi del frammento di contenuto, ad esempio:
   + Titolo: `Cycling Utah's National Parks`
   + Durata viaggio: `6 Days`
   + Difficoltà `Intermediate`
   + Prezzo: `3500`
   + Immagine principale: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Seleziona __Salva__ nella barra delle azioni superiore
1. Seleziona __Pubblicazione rapida__ dalla barra delle azioni superiore __...__
1. Aggiorna l’app React in esecuzione su [http://localhost:3000](http://localhost:3000).
1. Nell’app React, seleziona l’avventura Ciclismo ora aggiornata e verifica le modifiche al contenuto apportate al frammento di contenuto.

1. Con lo stesso approccio, nel servizio di authoring dell’AEM:
   1. Annulla la pubblicazione di un frammento di contenuto Adventure esistente e verifica che sia stato rimosso dall’esperienza di app di React
   1. Crea e pubblica un nuovo frammento di contenuto Avventura e verificane la visualizzazione nell’esperienza di React App

   >[!TIP]
   >
   > Se non hai familiarità con la creazione e la pubblicazione di nuovi frammenti di contenuto o con l’annullamento della pubblicazione di frammenti di contenuto esistenti, guarda la schermata precedente.

## Congratulazioni.

Congratulazioni. Hai utilizzato correttamente AEM Headless per alimentare un’app React.

Per comprendere in dettaglio come l’app React utilizza contenuti provenienti da AEM as a Cloud Service, vedi [tutorial AEM headless](../multi-step/overview.md). Il tutorial esplora come vengono creati i frammenti di contenuto in AEM e come questa app React utilizza i contenuti come JSON.

### Passaggi successivi

+ [Avvia l’esercitazione su AEM headless](../multi-step/overview.md)
