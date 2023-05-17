---
title: AEM API headless e React - AEM prima esercitazione headless
description: Scopri come recuperare i dati dei frammenti di contenuto dalle API GraphQL AEM e visualizzarli nell’app React.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---


# AEM API headless e React

Benvenuto in questo capitolo di esercitazione per la configurazione di un’app React per la connessione con le API Adobe Experience Manager (AEM) Headless utilizzando l’SDK AEM Headless. Scopriremo come recuperare i dati dei frammenti di contenuto dalle API di GraphQL AEM e visualizzarli nell’app React.

AEM le API headless consentono l’accesso AEM contenuto da qualsiasi app client. Ti guideremo attraverso la configurazione della tua app React per connetterti a AEM API Headless utilizzando l’SDK di AEM Headless. Questa configurazione stabilisce un canale di comunicazione riutilizzabile tra l’app React e AEM.

Successivamente, utilizzeremo l’SDK AEM Headless per recuperare i dati dei frammenti di contenuto dalle API di AEM GraphQL. I frammenti di contenuto in AEM forniscono una gestione dei contenuti strutturata. Utilizzando l’SDK senza titolo AEM puoi eseguire facilmente query e recuperare i dati dei frammenti di contenuto utilizzando GraphQL.

Una volta disponibili i dati dei frammenti di contenuto, li integreremo nell’app React. Imparerai a formattare e visualizzare i dati in modo accattivante. Scopriremo le best practice per la gestione e il rendering dei dati dei frammenti di contenuto nei componenti React, garantendo un’integrazione perfetta con l’interfaccia utente dell’app.

Nel corso dell&#39;esercitazione, forniremo spiegazioni, esempi di codice e suggerimenti pratici. Entro la fine, potrai configurare la tua app React per connetterti a AEM API headless, recuperare i dati dei frammenti di contenuto tramite l’SDK di AEM Headless e visualizzarli direttamente nell’app React. Cominciamo!


## Clona l’app React

1. Clona l’app da [Github](https://github.com/lamontacrook/headless-first/tree/main) eseguendo il comando seguente sulla riga di comando.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Cambia in `headless-first` e installare le dipendenze.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurare l’app React

1. Crea un file denominato `.env` nella directory principale del progetto. In `.env` imposta i seguenti valori:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Puoi recuperare un token sviluppatore in Cloud Manager. Accedi a [Adobe Cloud Manager](https://experience.adobe.com/). Fai clic su __Experience Manager > Cloud Manager__. Scegli il Programma appropriato e fai clic sui puntini di sospensione accanto all&#39;Ambiente.

   ![Console per sviluppatori AEM](./assets/2/developer-console.png)

   1. Fai clic in __Integrazioni__ scheda
   1. Fai clic su __Scheda Token locale e ottieni token di sviluppo locale__ pulsante
   1. Copia il token di accesso che inizia dopo il preventivo aperto fino a prima del preventivo di chiusura.
   1. Incolla il token copiato come valore per `REACT_APP_TOKEN` in `.env` file.
   1. Ora creiamo l’app tramite l’esecuzione di `npm ci` sulla riga di comando.
   1. Ora avvia l&#39;app React ed esegue `npm run start` sulla riga di comando.
   1. In [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) un file denominato `context.js`  include il codice per impostare i valori nel `.env` nel contesto dell&#39;app.

## Eseguire l’app React

1. Avvia l&#39;app React eseguendo `npm run start` sulla riga di comando.

   ```
   $ npm run start
   ```

   L’app React verrà avviata e verrà aperta una finestra del browser per `http://localhost:3000`. Le modifiche all’app React verranno ricaricate automaticamente nel browser.

## Connettersi a AEM API headless

1. Per collegare l’app React a AEM as a Cloud Service, aggiungiamo alcuni elementi a `App.js`. In `React` importa, aggiungi `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importa `AppContext` dal `context.js` file.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Ora all&#39;interno del codice dell&#39;app, definisci una variabile di contesto.

   ```javascript
   const context = useContext(AppContext);
   ```

   Infine, inserisci il codice di ritorno in `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Per riferimento, la `App.js` Ora dovrebbe essere così.

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. Importa `AEMHeadless` SDK. Questa SDK è una libreria helper utilizzata dall&#39;app per interagire con AEM API Headless.

   Aggiungi questa istruzione di importazione al `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Aggiungi quanto segue `{ useContext, useEffect, useState }` al` React` dichiarazione import.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importa `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Dentro `Home` componente, ottieni il `context` dalla variabile `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Inizializzare l&#39;SDK AEM Headless all&#39;interno di un  `useEffect()`, poiché l&#39;SDK AEM Headless deve cambiare quando il  `context` cambia la variabile .

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > C&#39;è una `context.js` file in `/utils` che sta leggendo gli elementi dal `.env` file. Per riferimento, la `context.url` è l&#39;URL dell&#39;ambiente as a Cloud Service AEM. La `context.endpoint` è il percorso completo dell’endpoint creato nella lezione precedente. Infine, il `context.token` è il token sviluppatore.


1. Creare uno stato React che espone il contenuto proveniente dall’SDK AEM Headless.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Collega l’app a AEM. Utilizza la query persistente creata nella lezione precedente. Aggiungiamo il seguente codice all&#39;interno del `useEffect` dopo l&#39;inizializzazione dell&#39;SDK di AEM Headless. Crea `useEffect` a seconda del  `context` come illustrato di seguito.


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. Apri la visualizzazione Rete degli strumenti per sviluppatori per esaminare la richiesta GraphQL.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Strumenti di sviluppo Chrome](./assets/2/dev-tools.png)

   L’SDK AEM Headless codifica la richiesta per GraphQL e aggiunge i parametri forniti. Puoi aprire la richiesta nel browser.

   >[!NOTE]
   >
   > Poiché la richiesta viene indirizzata all’ambiente di authoring, devi aver effettuato l’accesso all’ambiente in un’altra scheda dello stesso browser.


## Contenuto del frammento di contenuto di rendering

1. Visualizza i frammenti di contenuto nell’app. Restituire un `<div>` con il titolo del teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Sullo schermo viene visualizzato il campo titolo del teaser.

1. L’ultimo passaggio consiste nell’aggiungere il teaser alla pagina. Un componente teaser React è incluso nel pacchetto. Innanzitutto, includiamo l’importazione. Nella parte superiore del `home.js` aggiungi la riga:

   `import Teaser from '../../components/teaser/teaser';`

   Aggiorna l&#39;istruzione return:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Ora dovresti visualizzare il teaser con il contenuto incluso nel frammento.


## Passaggi successivi

Congratulazioni. Hai aggiornato correttamente l’app React per l’integrazione con le API AEM Headless utilizzando l’SDK AEM Headless.

Quindi, creiamo un componente Elenco immagini più complesso che esegue il rendering dinamico dei frammenti di contenuto a cui si fa riferimento da AEM.

[Capitolo successivo: Creare un componente Elenco immagini](./3-complex-components.md)