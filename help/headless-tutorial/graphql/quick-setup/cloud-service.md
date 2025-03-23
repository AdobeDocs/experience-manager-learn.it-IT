---
title: Configurazione rapida AEM Headless per AEM as a Cloud Service
description: La configurazione rapida AEM Headless ti consente di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un’app React che utilizza il contenuto rispetto alle API AEM Headless GraphQL.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# Configurazione rapida AEM Headless per AEM as a Cloud Service

La configurazione rapida AEM Headless ti consente di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un esempio di app React (a SPA) che consuma il contenuto rispetto alle API AEM Headless GraphQL.

## Prerequisiti

Per eseguire questa configurazione rapida sono necessari i seguenti elementi:

+ Ambiente Sandbox AEM as a Cloud Service (preferibilmente Sviluppo)
+ Accesso ad AEM as a Cloud Service e Cloud Manager
   + Accesso __Amministratore AEM__ ad AEM as a Cloud Service
   + __Cloud Manager - Accesso a Cloud Manager da parte del Responsabile della distribuzione__
+ I seguenti strumenti devono essere installati localmente:
   + [Node.js v18](https://nodejs.org/it/)
   + [Git](https://git-scm.com/)
   + Un IDE (ad esempio, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Creare un archivio Git di Cloud Manager

Innanzitutto, crea un archivio Git di Cloud Manager utilizzato per distribuire il sito WKND. Il sito WKND è un progetto di esempio per un sito web AEM che contiene contenuti (frammenti di contenuto) e un endpoint GraphQL AEM utilizzato dall’app React della configurazione rapida.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Passa a [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Seleziona il __programma__ Cloud Manager contenente l&#39;ambiente AEM as a Cloud Service da utilizzare per questa installazione rapida
1. Creare un archivio Git per il progetto del sito WKND
   1. Seleziona __Archivi__ nella navigazione superiore
   1. Seleziona __Aggiungi archivio__ nella barra delle azioni superiore
   1. Denomina il nuovo archivio Git: `aem-headless-quick-setup-wknd`
      + I nomi degli archivi Git devono essere univoci per ciascuna organizzazione Adobe,
   1. Seleziona __Salva__ e attendi l&#39;inizializzazione dell&#39;archivio Git

## 2. Invia il progetto di esempio del sito WKND all’archivio Git di Cloud Manager

Una volta creato l’archivio Git di Cloud Manager, clona il codice sorgente del progetto del sito WKND da GitHub e invialo all’archivio Git di Cloud Manager. Ora puoi accedere a Cloud Manager e implementare il progetto del sito WKND nell’ambiente AEM as a Cloud Service.

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
   1. Comando Execute trovato in __Aggiungi un remoto all&#39;archivio Git__ dalla riga di comando

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Invia il codice sorgente del progetto di esempio dall’archivio Git locale all’archivio Git di Cloud Manager

   ```shell
   $ git push adobe main:main
   ```

   Quando vengono richieste le credenziali, fornisci il __Nome utente__ e la __Password__ dal modale __Informazioni archivio__ di Cloud Manager.

## 3. Distribuire il sito WKND in AEM as a Cloud Service

Quando il progetto del sito WKND viene inviato all’archivio Git di Cloud Manager, non può essere distribuito ad AEM as a Cloud Service utilizzando le pipeline di Cloud Manager.

Nota: il progetto del sito WKND fornisce contenuti di esempio che l’app React utilizza rispetto alle API GraphQL headless di AEM.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Allega una __pipeline di distribuzione non di produzione__ al nuovo archivio Git
   1. Seleziona __Pipeline__ nella navigazione superiore
   1. Seleziona __Aggiungi pipeline__ dalla barra delle azioni superiore
   1. Nella scheda __Configurazione__
      1. Seleziona opzione __Pipeline di distribuzione__
      1. Imposta __Nome pipeline non di produzione__ su `Dev Deployment pipeline`
      1. Seleziona __Trigger distribuzione > In caso di modifiche Git__
      1. Seleziona __Comportamento in caso di errori di metriche importanti > Continua immediatamente__
      1. Seleziona __Continua__
   1. Nella scheda __Codice Source__
      1. Seleziona opzione __Codice full stack__
      1. Seleziona l&#39;__ambiente di sviluppo AEM as a Cloud Service__ dalla casella di selezione __Ambienti di distribuzione idonei__
      1. Seleziona `aem-headless-quick-setup-wknd` nella casella di selezione __Archivio__
      1. Seleziona `main` dalla casella di selezione __Ramo Git__
      1. Seleziona __Salva__
1. Esegui la pipeline di distribuzione __Dev__
   1. Seleziona __Panoramica__ nella navigazione superiore
   1. Individua la pipeline di distribuzione __Dev__ appena creata nella sezione __Pipeline__
   1. Seleziona __...__ a destra della voce della pipeline
   1. Seleziona __Esegui__ e conferma nel modale
   1. Seleziona __...__ a destra della pipeline attualmente in esecuzione
   1. Seleziona __Visualizza dettagli__
1. Dai dettagli dell’esecuzione della pipeline, monitora l’avanzamento fino al suo completamento corretto. L’esecuzione della pipeline dovrebbe richiedere tra 30 e 40 minuti.

## 4. Scaricare ed eseguire l’app WKND React

Con AEM as a Cloud Service avviato con il contenuto del progetto del sito WKND, scarica e avvia l’app di reazione WKND di esempio che utilizza il contenuto del sito WKND sulle API GraphQL headless di AEM.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Dalla riga di comando, clona il codice sorgente dell’app React da GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Aprire la cartella `~/Code/aem-guides-wknd-graphql/react-app` nell&#39;IDE.
1. Nell&#39;IDE aprire il file `.env.development`.
1. Puntare all&#39;URI host del servizio __Pubblica__ di AEM as a Cloud Service dalla proprietà `REACT_APP_HOST_URI`.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Per trovare l’URI host del servizio di pubblicazione di AEM as a Cloud Service:

   1. In Cloud Manager, seleziona __Ambienti__ nella navigazione superiore
   1. Seleziona ambiente __Sviluppo__
   1. Individua la tabella __Servizio di pubblicazione (AEM e Dispatcher)__ collegamento __Segmenti di ambiente__
   1. Copia l’indirizzo del collegamento e utilizzalo come URI del servizio di pubblicazione di AEM as a Cloud Service

1. Nell&#39;IDE, salvare le modifiche apportate a `.env.development`
1. Dalla riga di comando, esegui React App

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. L&#39;app React, eseguita localmente, inizia il [http://localhost:3000](http://localhost:3000) e visualizza un elenco di avventure, provenienti da AEM as a Cloud Service con le API GraphQL di AEM Headless.

## 5. Modificare i contenuti in AEM

Con l’app di esempio WKND React che si connette e utilizza i contenuti delle API AEM Headless GraphQL, crea contenuti nel servizio AEM Author e scopri come l’esperienza dell’app React si aggiorna insieme.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Accedi al servizio AEM as a Cloud Service Author
1. Passa a __Assets > File > WKND condiviso > Inglese > Avventure__
1. Apri la cartella __Ciclismo nello Utah meridionale__
1. Seleziona il frammento di contenuto __Ciclismo nello Utah meridionale__ e seleziona __Modifica__ dalla barra delle azioni superiore
1. Aggiorna alcuni campi del frammento di contenuto, ad esempio:
   + Titolo: `Cycling Utah's National Parks`
   + Durata viaggio: `6 Days`
   + Difficoltà: `Intermediate`
   + Prezzo: `3500`
   + Immagine principale: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Seleziona __Salva__ nella barra delle azioni superiore
1. Seleziona __Pubblicazione rapida__ dalla barra delle azioni superiore __...__
1. Aggiorna l&#39;app React in esecuzione su [http://localhost:3000](http://localhost:3000).
1. Nell’app React, seleziona l’avventura Ciclismo ora aggiornata e verifica le modifiche al contenuto apportate al frammento di contenuto.

1. Con lo stesso approccio, nel servizio AEM Author:
   1. Annulla la pubblicazione di un frammento di contenuto Adventure esistente e verifica che sia stato rimosso dall’esperienza di app di React
   1. Crea e pubblica un nuovo frammento di contenuto Avventura e verificane la visualizzazione nell’esperienza di React App

   >[!TIP]
   >
   > Se non hai familiarità con la creazione e la pubblicazione di nuovi frammenti di contenuto o con l’annullamento della pubblicazione di frammenti di contenuto esistenti, guarda la schermata precedente.

## Congratulazioni.

Congratulazioni Hai utilizzato con successo AEM Headless per alimentare un’app React.

Per informazioni dettagliate su come l&#39;app React utilizza i contenuti da AEM as a Cloud Service, consulta [l&#39;esercitazione AEM Headless](../multi-step/overview.md). Il tutorial esplora come vengono creati i frammenti di contenuto in AEM e come questa app React utilizza i contenuti come JSON.

### Passaggi successivi

+ [Avvia l’esercitazione di AEM Headless](../multi-step/overview.md)
