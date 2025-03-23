---
title: 'API AEM headless e React: prima esercitazione di AEM Headless'
description: Scopri come coprire il recupero dei dati dei frammenti di contenuto dalle API GraphQL di AEM e come visualizzarli nell’app React.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
duration: 225
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# API AEM headless e React

Benvenuti in questo capitolo di esercitazione in cui esploreremo la configurazione di un’app React per la connessione con le API headless di Adobe Experience Manager (AEM) tramite AEM Headless SDK. Scopriremo come recuperare i dati dei frammenti di contenuto dalle API GraphQL di AEM e come visualizzarli nell’app React.

Le API AEM Headless consentono di accedere ai contenuti di AEM da qualsiasi app client. Ti guideremo attraverso la configurazione della tua app React per connettersi alle API AEM Headless tramite AEM Headless SDK. Questa configurazione stabilisce un canale di comunicazione riutilizzabile tra l’app React e AEM.

Ora utilizzeremo AEM Headless SDK per recuperare i dati dei frammenti di contenuto dalle API GraphQL di AEM. I frammenti di contenuto in AEM forniscono una gestione strutturata dei contenuti. Utilizzando AEM Headless SDK, puoi facilmente eseguire query e recuperare dati dei frammenti di contenuto utilizzando GraphQL.

Una volta ottenuti i dati dei frammenti di contenuto, li integreremo nella tua app React. Scoprirai come formattare e visualizzare i dati in modo interessante. Vengono descritte le best practice per la gestione e il rendering dei dati dei frammenti di contenuto nei componenti React, garantendo un’integrazione perfetta con l’interfaccia utente dell’app.

Nel corso dell’esercitazione forniremo spiegazioni, esempi di codice e suggerimenti pratici. Entro la fine, potrai configurare l’app React per connettersi alle API AEM Headless, recuperare i dati dei frammenti di contenuto utilizzando AEM Headless SDK e visualizzarli direttamente nell’app React. Iniziamo!


## Clonare l’app React

1. Clona l&#39;app da [Github](https://github.com/lamontacrook/headless-first/tree/main) eseguendo il comando seguente sulla riga di comando.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Passare alla directory `headless-first` e installare le dipendenze.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurare l’app React

1. Creare un file denominato `.env` nella directory principale del progetto. In `.env` impostare i seguenti valori:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Puoi recuperare un token sviluppatore in Cloud Manager. Accedi a [Adobe Cloud Manager](https://experience.adobe.com/). Fare clic su __Experience Manager > Cloud Manager__. Scegli il programma appropriato, quindi fai clic sui puntini di sospensione accanto all’ambiente.

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. Fai clic sulla scheda __Integrazioni__
   1. Fai clic sulla scheda Token locale e sul pulsante Ottieni token di sviluppo locale ____
   1. Copia il token di accesso che inizia dopo la virgoletta aperta fino a prima della virgoletta di chiusura.
   1. Incolla il token copiato come valore per `REACT_APP_TOKEN` nel file `.env`.
   1. Ora creiamo l&#39;app eseguendo `npm ci` sulla riga di comando.
   1. Avviare l&#39;app React ed eseguire `npm run start` sulla riga di comando.
   1. Tra [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) un file denominato `context.js` include il codice per impostare i valori nel file `.env` nel contesto dell&#39;app.

## Eseguire l’app React

1. Avviare l&#39;app React eseguendo `npm run start` sulla riga di comando.

   ```
   $ npm run start
   ```

   L&#39;app React verrà avviata e aprirà una finestra del browser per `http://localhost:3000`. Le modifiche all’app React verranno ricaricate automaticamente nel browser.

## Connettersi alle API AEM Headless

1. Per collegare l&#39;app React ad AEM as a Cloud Service, aggiungiamo alcuni elementi a `App.js`. Nell&#39;importazione `React`, aggiungi `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importa `AppContext` dal file `context.js`.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Ora all’interno del codice dell’app, definisci una variabile di contesto.

   ```javascript
   const context = useContext(AppContext);
   ```

   Infine, racchiudere il codice restituito in `<AppContext.Provider> ... </AppContext.Provider>`.

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

1. Importa il SDK `AEMHeadless`. Questa SDK è una libreria di supporto utilizzata dall’app per interagire con le API headless di AEM.

   Aggiungi questa istruzione di importazione a `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Aggiungi `{ useContext, useEffect, useState }` seguente all&#39;istruzione di importazione ` React`.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importa `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   All&#39;interno del componente `Home`, ottenere la variabile `context` da `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Inizializza AEM Headless SDK in un `useEffect()`, poiché AEM Headless SDK deve cambiare quando cambia la variabile `context`.

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
   > È presente un file `context.js` in `/utils` che sta leggendo elementi dal file `.env`. Per riferimento, `context.url` è l&#39;URL dell&#39;ambiente AEM as a Cloud Service. `context.endpoint` è il percorso completo dell&#39;endpoint creato nella lezione precedente. Infine, `context.token` è il token sviluppatore.


1. Creare uno stato di React che espone il contenuto proveniente da AEM Headless SDK.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Connetti l’app ad AEM. Utilizza la query persistente creata nella lezione precedente. Aggiungiamo il seguente codice in `useEffect` dopo l&#39;inizializzazione di AEM Headless SDK. Rendi `useEffect` dipendente dalla variabile `context`, come illustrato di seguito.


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

   AEM Headless SDK codifica la richiesta per GraphQL e aggiunge i parametri forniti. Puoi aprire la richiesta nel browser.

   >[!NOTE]
   >
   > Poiché la richiesta viene indirizzata all’ambiente di authoring, devi aver effettuato l’accesso a tale ambiente in un’altra scheda dello stesso browser.


## Rendering contenuto frammento di contenuto

1. Visualizza i Frammenti di contenuto nell’app. Restituisce `<div>` con il titolo del teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Sullo schermo dovrebbe essere visualizzato il campo del titolo del teaser.

1. L’ultimo passaggio consiste nell’aggiungere il teaser alla pagina. Il pacchetto include un componente teaser di React. Innanzitutto, includiamo l’importazione. Nella parte superiore del file `home.js`, aggiungi la riga:

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

Congratulazioni Hai aggiornato correttamente l’app React per l’integrazione con le API AEM Headless tramite AEM Headless SDK.

Quindi, creiamo un componente Elenco immagini più complesso che esegue il rendering dinamico dei frammenti di contenuto di riferimento da AEM.

[Capitolo successivo: Creare un componente Elenco immagini](./3-complex-components.md)
