---
title: 'API e React di AEM headless: primo tutorial su AEM headless'
description: Scopri come coprire il recupero dei dati dei frammenti di contenuto dalle API GraphQL dell’AEM e come visualizzarli nell’app React.
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


# API e React di AEM headless

Benvenuti in questo capitolo di esercitazione in cui esploreremo la configurazione di un’app React per la connessione con le API headless di Adobe Experience Manager (AEM) tramite l’SDK AEM headless. Scopriremo come recuperare i dati dei frammenti di contenuto dalle API GraphQL dell’AEM e come visualizzarli nell’app React.

Le API headless dell’AEM consentono di accedere ai contenuti dell’AEM da qualsiasi app client. Ti guideremo attraverso la configurazione della tua app React per connettersi alle API AEM headless tramite l’SDK AEM headless. Questa configurazione stabilisce un canale di comunicazione riutilizzabile tra l’app React e l’AEM.

Ora utilizzeremo l’SDK headless dell’AEM per recuperare i dati dei frammenti di contenuto dalle API GraphQL dell’AEM. I frammenti di contenuto nell’AEM forniscono una gestione strutturata dei contenuti. Utilizzando l’SDK headless dell’AEM, puoi facilmente eseguire query e recuperare dati dei frammenti di contenuto utilizzando GraphQL.

Una volta ottenuti i dati dei frammenti di contenuto, li integreremo nella tua app React. Scoprirai come formattare e visualizzare i dati in modo interessante. Vengono descritte le best practice per la gestione e il rendering dei dati dei frammenti di contenuto nei componenti React, garantendo un’integrazione perfetta con l’interfaccia utente dell’app.

Nel corso dell’esercitazione forniremo spiegazioni, esempi di codice e suggerimenti pratici. Entro la fine, potrai configurare l’app React per connettersi alle API headless dell’AEM, recuperare i dati dei frammenti di contenuto utilizzando l’SDK headless dell’AEM e visualizzarli direttamente nell’app React. Iniziamo!


## Clonare l’app React

1. Clona l’app da [Github](https://github.com/lamontacrook/headless-first/tree/main) eseguendo il comando seguente sulla riga di comando.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Cambia in `headless-first` e installarle.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurare l’app React

1. Crea un file denominato `.env` nella directory principale del progetto. In entrata `.env` imposta i seguenti valori:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Puoi recuperare un token sviluppatore in Cloud Manager. Accedi a [Adobe Cloud Manager](https://experience.adobe.com/). Clic __Experience Manager > Cloud Manager__. Scegli il programma appropriato, quindi fai clic sui puntini di sospensione accanto all’ambiente.

   ![Console per sviluppatori AEM](./assets/2/developer-console.png)

   1. Fai clic su nella __Integrazioni__ scheda
   1. Clic __Scheda Token locale e ottieni token di sviluppo locale__ pulsante
   1. Copia il token di accesso che inizia dopo la virgoletta aperta fino a prima della virgoletta di chiusura.
   1. Incolla il token copiato come valore per `REACT_APP_TOKEN` nel `.env` file.
   1. Ora creiamo l’app eseguendo `npm ci` sulla riga di comando.
   1. Ora avvia l’app React ed eseguendo `npm run start` sulla riga di comando.
   1. In [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) un file denominato `context.js`  include il codice per impostare i valori nella `.env` nel contesto dell&#39;app.

## Eseguire l’app React

1. Avviare l’app React eseguendo `npm run start` sulla riga di comando.

   ```
   $ npm run start
   ```

   L’app React verrà avviata e aprirà una finestra del browser per `http://localhost:3000`. Le modifiche all’app React verranno ricaricate automaticamente nel browser.

## Connessione alle API headless dell’AEM

1. Per collegare l’app React a AEM as a Cloud Service, aggiungiamo alcuni elementi a `App.js`. In `React` importa, aggiungi `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importa `AppContext` dal `context.js` file.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Ora all’interno del codice dell’app, definisci una variabile di contesto.

   ```javascript
   const context = useContext(AppContext);
   ```

   Infine, inserisci il codice restituito in `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Per riferimento, `App.js` ora dovrebbe essere così.

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

1. Importa `AEMHeadless` SDK Questo SDK è una libreria helper utilizzata dall’app per interagire con le API headless dell’AEM.

   Aggiungi questa istruzione di importazione al `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Aggiungi quanto segue `{ useContext, useEffect, useState }` al` React` istruzione import.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importa `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   All&#39;interno del `Home` componente, ottieni il `context` variabile dalla `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Inizializzare l’SDK headless dell’AEM all’interno di una  `useEffect()`, poiché l’SDK headless dell’AEM deve cambiare quando  `context` modifiche alle variabili.

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
   > È presente un `context.js` file in `/utils` ovvero la lettura di elementi da `.env` file. Per riferimento, `context.url` è l’URL dell’ambiente as a Cloud Service dell’AEM. Il `context.endpoint` è il percorso completo dell’endpoint creato nella lezione precedente. Infine, la `context.token` è il token di sviluppo.


1. Creare uno stato di React che espone il contenuto proveniente dall’SDK headless dell’AEM.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Connetti l’app all’AEM. Utilizza la query persistente creata nella lezione precedente. Aggiungiamo il seguente codice all&#39;interno del `useEffect` dopo l’inizializzazione dell’SDK headless dell’AEM. Rendi `useEffect` a seconda del  `context` come mostrato di seguito.


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

1. Apri la vista Rete degli strumenti per sviluppatori per rivedere la richiesta GraphQL.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Strumenti di sviluppo Chrome](./assets/2/dev-tools.png)

   L’SDK headless dell’AEM codifica la richiesta per GraphQL e aggiunge i parametri forniti. Puoi aprire la richiesta nel browser.

   >[!NOTE]
   >
   > Poiché la richiesta viene indirizzata all’ambiente di authoring, devi aver effettuato l’accesso a tale ambiente in un’altra scheda dello stesso browser.


## Rendering contenuto frammento di contenuto

1. Visualizza i Frammenti di contenuto nell’app. Restituisce un `<div>` con il titolo del teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Sullo schermo dovrebbe essere visualizzato il campo del titolo del teaser.

1. L’ultimo passaggio consiste nell’aggiungere il teaser alla pagina. Il pacchetto include un componente teaser di React. Innanzitutto, includiamo l’importazione. Nella parte superiore della sezione `home.js` file, aggiungi la riga:

   `import Teaser from '../../components/teaser/teaser';`

   Aggiornare l&#39;istruzione return:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Ora dovresti visualizzare il teaser con il contenuto incluso all’interno del frammento.


## Passaggi successivi

Congratulazioni. Hai aggiornato correttamente l’app React per l’integrazione con le API AEM headless tramite l’SDK AEM headless.

Quindi, creiamo un componente Elenco immagini più complesso che esegue il rendering dinamico dei frammenti di contenuto di riferimento dall’AEM.

[Capitolo successivo: Creare un componente Elenco immagini](./3-complex-components.md)