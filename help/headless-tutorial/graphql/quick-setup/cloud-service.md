---
title: Configurazione rapida AEM Headless per AEM as a Cloud Service
description: La configurazione rapida AEM Headless ti permette di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un’app React che consuma il contenuto rispetto AEM API GraphQL headless.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1075'
ht-degree: 2%

---


# Configurazione rapida AEM Headless per AEM as a Cloud Service

La configurazione rapida AEM Headless ti permette di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un esempio di app React (a SPA) che consuma il contenuto sopra AEM API GraphQL headless.

## Prerequisiti

Per eseguire questa configurazione rapida, è necessario quanto segue:

+ AEM ambiente Sandbox as a Cloud Service (preferibilmente Sviluppo)
+ Accesso a AEM as a Cloud Service e Cloud Manager
   + `AEM Administrator` accesso a AEM as a Cloud Service
   + `Cloud Manager - Deployment Manager` accesso a Cloud Manager
+ È necessario installare localmente i seguenti strumenti:
   + [Node.js v10+](https://nodejs.org/it/)
   + [npm 6+](https://www.npmjs.com/)
   + [Git](https://git-scm.com/)
   + Un IDE (ad esempio, [Codice Microsoft® Visual Studio](https://code.visualstudio.com/)

## 1. Creare un archivio Git di Cloud Manager

Innanzitutto, crea un archivio Git di Cloud Manager utilizzato per distribuire il sito WKND. Il sito WKND è un esempio AEM progetto web che contiene contenuto (frammenti di contenuto) e un endpoint GraphQL AEM utilizzato dall&#39;app React dell&#39;impostazione rapida.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339073/?quality=12&learn=on)

1. Passa a [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Seleziona Cloud Manager __Programma__ che contiene l&#39;ambiente as a Cloud Service AEM da utilizzare per questa configurazione rapida
1. Creare un archivio Git per il progetto WKND Site
   1. Seleziona __Repository__ nella navigazione superiore
   1. Seleziona __Aggiungi archivio__ nella barra delle azioni superiore
   1. Assegna un nome al nuovo archivio Git: `aem-headless-quick-setup`
   1. Seleziona __Salva__ e attendere l’inizializzazione dell’archivio Git

## 2. Invia un esempio di progetto del sito WKND all’archivio Git di Cloud Manager

Con l’archivio Git di Cloud Manager creato, clona il codice sorgente del progetto WKND Site da GitHub e invialo all’archivio Git di Cloud Manager. Ora Cloud Manager consente di accedere al progetto WKND Site e di distribuirlo nell’ambiente as a Cloud Service AEM.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339074/?quality=12&learn=on)

1. Dalla riga di comando, duplica il codice sorgente del progetto WKND Site di esempio da GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Aggiungere l’archivio Git di Cloud Manager come remoto
   1. Seleziona __Repository__ nella navigazione superiore
   1. Seleziona __Accesso alle informazioni sul repository__ dalla barra delle azioni superiore
   1. Esegui comando trovato in __Aggiungere un remoto all’archivio Git__ dalla riga di comando

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup/
      ```

1. Invia il codice sorgente del progetto di esempio all’archivio Git di Cloud Manager

   1. Invia il codice dal tuo archivio Git locale all’archivio Git di Cloud Manager

      ```shell
      $ git push adobe master:main
      ```

      Quando viene richiesto di specificare le credenziali, fornisci __Nome utente__ e __Password__ da Cloud Manager __Informazioni archivio__ modale.

## 3. Distribuisci il sito WKND per AEM as a Cloud Service

Con il progetto WKND Site inviato all’archivio Git di Cloud Manager, non può essere distribuito AEM as a Cloud Service utilizzando le pipeline di Cloud Manager.

Tieni presente che il progetto Sito WKND fornisce contenuti di esempio che l’app React consuma rispetto AEM API GraphQL headless.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339075/?quality=12&learn=on)

1. Allega un __pipeline di distribuzione non di produzione__ al nuovo archivio Git
   1. Seleziona __Tubi__ nella navigazione superiore
   1. Seleziona __Aggiungi pipeline__ dalla barra delle azioni superiore
   1. Sulla __Configurazione__ scheda
      1. Seleziona __Pipeline di distribuzione__ opzione
      1. Imposta la __Nome della pipeline non di produzione__ a `Dev Deployment pipeline`
      1. Seleziona __Trigger distribuzione > In caso di modifiche Git__
      1. Seleziona __Comportamento degli errori di metrica importante > Continua immediatamente__
      1. Seleziona __Continua__
   1. Sulla __Codice sorgente__ scheda
      1. Seleziona __Codice Stack Completo__ opzione
      1. Seleziona la __AEM ambiente di sviluppo as a Cloud Service__ dal __Ambienti di distribuzione idonei__ casella di selezione
      1. Seleziona `aem-headless-quick-setup` in __Archivio__ casella di selezione
      1. Seleziona `main` dal __Ramo Git__ casella di selezione
      1. Seleziona __Salva__
1. Esegui il __Pipeline di distribuzione di sviluppo__
   1. Seleziona __Panoramica__ nella navigazione superiore
   1. Individua la nuova creazione __pipeline di distribuzione di sviluppo__ in __Tubi__ sezione
   1. Seleziona la __...__ a destra dell’entrata della pipeline
   1. Seleziona __Esegui__ e conferma nel modale
   1. Seleziona la __...__ a destra della pipeline attualmente in esecuzione
   1. Seleziona __Visualizza dettagli__
1. Dai dettagli dell’esecuzione della pipeline, controlla l’avanzamento fino al completamento corretto. L’esecuzione della pipeline deve richiedere tra 45-60 minuti.

## 4. Scaricare ed eseguire l&#39;app WKND React

Con AEM as a Cloud Service avviato con il contenuto del progetto WKND Site , scarica e avvia l’app di reazione WKND di esempio che consuma il contenuto del sito WKND rispetto AEM API GraphQL headless.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339076/?quality=12&learn=on)

1. Dalla riga di comando, duplica il codice sorgente dell’app React da GitHub.

   ```shell
   $ cd ~/Code
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri la cartella `~/Code/aem-guides-wknd-graphql` nell’IDE.
1. Nell’IDE, apri il file . `react-app/.env.development`.
1. Punta alla AEM as a Cloud Service __Pubblica__ URI host del servizio dal  `REACT_APP_HOST_URI` proprietà.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com/
   ...
   ```

   Per trovare l&#39;URI host del servizio Publish AEM as a Cloud Service:

   1. In Cloud Manager, seleziona __Ambienti__ nella navigazione superiore
   1. Seleziona __Sviluppo__ ambiente
   1. Individua il __Servizio Publish (AEM e Dispatcher)__ collegamento __Segmenti di ambiente__ tabella
   1. Copia l&#39;indirizzo del collegamento e utilizzalo come URI del servizio di pubblicazione as a Cloud Service AEM

1. Nell’IDE, salva le modifiche apportate a `.env.development`
1. Dalla riga di comando, esegui l’app React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. L&#39;app React, in esecuzione localmente, inizia il [http://localhost:3000](Http://localhost:3000) e visualizza un elenco delle avventure, provenienti da AEM as a Cloud Service utilizzando AEM API GraphQL di Headless.

## 5. Modifica contenuto in AEM

Con l’app React WKND di esempio che si collega e consuma contenuti dalle API GraphQL AEM headless, crea contenuti nel servizio Author di AEM e osserva come l’esperienza dell’app React si aggiorna di concerto.

_Schermata dei passaggi_
>[!VIDEO](https://video.tv.adobe.com/v/339077/?quality=12&learn=on)

1. Accedi a AEM servizio Author as a Cloud Service
1. Passa a __Risorse > File > WKND > Inglese > Avventure__
1. Apri __Ciclismo nello Utah meridionale__ Cartella
1. Seleziona la __Ciclismo nello Utah meridionale__ Frammento di contenuto e seleziona __Modifica__ dalla barra delle azioni superiore
1. Aggiorna alcuni campi del frammento di contenuto, ad esempio:
   + Titolo: `Cycling Utah's National Parks`
   + Lunghezza del viaggio: `6 Days`
   + Difficoltà: `Intermediate`
   + Prezzo: `$3500`
   + Immagine principale: `/content/dam/wknd/en/activities/cycling/mountain-biking.jpg`
1. Seleziona __Salva__ nella barra delle azioni superiore
1. Seleziona __Pubblicazione rapida__ dalla barra delle azioni superiore __...__
1. Aggiorna l&#39;app React in esecuzione su [http://localhost:3000](Http://localhost:3000).
1. Nell’app React, seleziona l’ora aggiornata e verifica le modifiche apportate al contenuto del frammento di contenuto.

1. Utilizzando lo stesso approccio, nel servizio AEM Author:
   1. Annullare la pubblicazione di un frammento di contenuto avventura esistente e verificare che sia stato rimosso dall’esperienza React App
   1. Crea e pubblica un nuovo frammento di contenuto Avventura e verifica che venga visualizzato nell’esperienza App React

   >[!TIP]
   >
   > Se non hai dimestichezza con la creazione e la pubblicazione di frammenti di contenuto nuovi o con l’annullamento della pubblicazione di frammenti di contenuto esistenti, guarda la schermata precedente.

## Congratulazioni!

Congratulazioni! Hai utilizzato con successo AEM Headless per alimentare un&#39;app React!

Per capire in dettaglio come l’app React consuma contenuti da AEM as a Cloud Service, effettua il check-out [tutorial AEM Headless](../multi-step/overview.md). L’esercitazione esplora il modo in cui i frammenti di contenuto in AEM sono stati creati e come l’app React utilizza i loro contenuti come JSON.

### Passaggi successivi

+ [Avvia l&#39;esercitazione AEM Headless](../multi-step/overview.md)
