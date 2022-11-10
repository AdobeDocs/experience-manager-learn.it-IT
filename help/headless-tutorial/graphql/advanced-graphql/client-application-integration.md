---
title: Integrazione delle applicazioni client - Concetti avanzati di AEM headless - GraphQL
description: Implementa query persistenti e integrale nell’app WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# Integrazione di applicazioni client

Nel capitolo precedente, hai creato e aggiornato query persistenti utilizzando Esplora risorse.

Questo capitolo illustra i passaggi necessari per integrare le query persistenti con l’applicazione client WKND (alias App WKND) utilizzando le richieste HTTP GET all’interno delle richieste esistenti **Reagire ai componenti**. Offre inoltre una sfida opzionale per applicare i vostri apprendisti senza testa AEM, competenze di codifica per migliorare l&#39;applicazione client WKND.

## Prerequisiti {#prerequisites}

Questo documento fa parte di un tutorial in più parti. Assicurarsi che i capitoli precedenti siano stati completati prima di procedere con questo capitolo. L’applicazione client WKND si connette al servizio di pubblicazione AEM, quindi è importante che tu **pubblicato quanto segue nel servizio di pubblicazione AEM**.

* Configurazioni del progetto
* Endpoint GraphQL
* Modelli per frammenti di contenuto
* Frammenti di contenuto creati
* Query persistenti GraphQL

La _Le schermate IDE in questo capitolo provengono da [Codice di Visual Studio](https://code.visualstudio.com/)_

### Capitolo 1-4 Pacchetto soluzione (facoltativo) {#solution-package}

È disponibile un pacchetto di soluzioni da installare che completa i passaggi nell’interfaccia utente AEM per i capitoli da 1 a 4. Questo pacchetto è **non necessario** se i capitoli precedenti sono stati completati.

1. Scarica [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. In AEM, passa a **Strumenti** > **Distribuzione** > **Pacchetti** accesso **Gestione pacchetti**.
1. Carica e installa il pacchetto (file zip) scaricato nel passaggio precedente.
1. Replicare il pacchetto al servizio AEM Publish

## Obiettivi {#objectives}

Questa esercitazione spiega come integrare le richieste di query persistenti nell’app di esempio WKND GraphQL React utilizzando l’ [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clona ed esegui l’applicazione client di esempio {#clone-client-app}

Per accelerare l&#39;esercitazione, viene fornita un&#39;app JS React iniziale.

1. Clona il [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifica le `aem-guides-wknd-graphql/advanced-tutorial/.env.development` file e set `REACT_APP_HOST_URI` per puntare al servizio di pubblicazione AEM target.

   Aggiorna il metodo di autenticazione se ti connetti a un&#39;istanza dell&#39;autore.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![Reazione dell’ambiente di sviluppo delle app](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > Le istruzioni di cui sopra sono per collegare l&#39;app React al **Servizio di pubblicazione AEM**, tuttavia per connettersi al **Servizio di authoring di AEM** ottenere un token di sviluppo locale per l’ambiente di destinazione AEM as a Cloud Service.
   >
   > È inoltre possibile collegare l’app a un [istanza di authoring locale tramite SDK AEMaaCS](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) utilizzo dell’autenticazione di base.


1. Apri un terminale ed esegui i comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Viene caricata una nuova finestra del browser [http://localhost:3000](Http://localhost:3000)


1. Tocca **Campeggio** > **Yosemite Backpack** per visualizzare i dettagli dell&#39;avventura del backpack Yosemite.

   ![Schermata di backpack Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Apri gli strumenti di sviluppo del browser ed esamina `XHR` richiesta

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Dovresti vedere `GET` richieste all&#39;endpoint GraphQL con nome di configurazione del progetto (`wknd-shared`), nome query persistente (`adventure-by-slug`), nome variabile (`slug`), value (`yosemite-backpacking`) e codifiche di caratteri speciali.

>[!IMPORTANT]
>
>    Se ti stai chiedendo perché la richiesta API GraphQL viene effettuata rispetto al `http://localhost:3000` e NON rispetto al dominio AEM Publish Service, controlla [Sotto Il Cappuccio](../multi-step/graphql-and-react-app.md#under-the-hood) Tutorial di base.


## Rivedi il Codice

In [Esercitazione di base - Creare un’app React che utilizza AEM API GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) abbiamo rivisto e migliorato alcuni file chiave per ottenere un&#39;esperienza pratica. Prima di ottimizzare l’app WKND, controlla i file chiave.

* [Rivedi l&#39;oggetto AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementare per eseguire AEM query persistenti GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Revisione `Adventures` Componente React

La visualizzazione principale dell&#39;app WKND React è l&#39;elenco di tutte le Avventure e puoi filtrare queste Avventure in base al tipo di attività come _Campeggio, Ciclismo_. Viene eseguito il rendering di questa visualizzazione da `Adventures` componente. Di seguito sono riportati i principali dettagli di implementazione:

* La `src/components/Adventures.js` chiama `useAllAdventures(adventureActivity)` gancio e qui `adventureActivity` argomento è il tipo di attività.

* La `useAllAdventures(adventureActivity)` il gancio è definito nella `src/api/usePersistedQueries.js` file. In base a `adventureActivity` determina la query persistente da chiamare. Se non un valore nullo, chiama `wknd-shared/adventures-by-activity`, altrimenti ottiene tutte le avventure disponibili `wknd-shared/adventures-all`.

* Il gancio utilizza il principale `fetchPersistedQuery(..)` funzione che delega l’esecuzione della query a `AEMHeadless` tramite `aemHeadlessClient.js`.

* Il gancio restituisce anche solo i dati pertinenti dalla risposta GraphQL AEM a `response.data?.adventureList?.items`, consentendo `Adventures` Reagisce affinché i componenti di visualizzazione siano agnostici delle strutture JSON principali.

* Al termine dell’esecuzione della query, il `AdventureListItem(..)` funzione di rendering da `Adventures.js` aggiunge un elemento HTML per visualizzare il _Immagine, lunghezza del viaggio, prezzo e titolo_ informazioni.

### Revisione `AdventureDetail` Componente React

La `AdventureDetail` Il componente React rende i dettagli dell’avventura. Di seguito sono riportati i principali dettagli di implementazione:

* La `src/components/AdventureDetail.js` chiama `useAdventureBySlug(slug)` gancio e qui `slug` argomento è il parametro query.

* Come sopra, il `useAdventureBySlug(slug)` il gancio è definito nella `src/api/usePersistedQueries.js` file. Chiama `wknd-shared/adventure-by-slug` query persistente delegando a `AEMHeadless` tramite `aemHeadlessClient.js`.

* Al termine dell’esecuzione della query, il `AdventureDetailRender(..)` funzione di rendering da `AdventureDetail.js` aggiunge un elemento HTML per visualizzare i dettagli dell’avventura.


## Migliorare il codice

### Utilizzo `adventure-details-by-slug` query persistente

Nel capitolo precedente, abbiamo creato il `adventure-details-by-slug` query persistente, fornisce informazioni aggiuntive su Avventura come _posizione, istruttoreTeam e amministratore_. Sostituiamo `adventure-by-slug` con `adventure-details-by-slug` query persistente per il rendering di queste informazioni aggiuntive.

1. Apri `src/api/usePersistedQueries.js`.

1. Individuare la funzione `useAdventureBySlug()` e aggiorna la query come

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### Visualizza informazioni aggiuntive

1. Per visualizzare ulteriori informazioni sull&#39;avventura, apri `src/components/AdventureDetail.js`

1. Individuare la funzione `AdventureDetailRender(..)` e aggiorna la funzione di ritorno come

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. Definisci anche le corrispondenti funzioni di rendering:

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **Dove si trova**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **IstruttoreTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **Administrator**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### Definire nuovi stili

1. Apri `src/components/AdventureDetail.scss` e aggiungere le seguenti definizioni di classe

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>I file aggiornati sono disponibili in **Guide AEM WKND - GraphQL** progetto, vedi [Tutorial avanzato](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) sezione .


Dopo aver completato i miglioramenti di cui sopra, l’app WKND si presenta come segue e gli strumenti per sviluppatori del browser mostrano `adventure-details-by-slug` chiamata di query persistente.

![APP WKND ottimizzata](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Sfida per il miglioramento (facoltativo)

La visualizzazione principale dell’app React WKND ti consente di filtrare queste Avventure in base al tipo di attività _Campeggio, Ciclismo_. Tuttavia il team di business WKND vuole avere un extra _Posizione_ funzionalità di filtro basato su . I requisiti sono

* Nella vista principale dell’app WKND, nell’angolo in alto a sinistra o a destra aggiungi _Posizione_ icona di filtro.
* Clic _Posizione_ l’icona di filtro deve visualizzare un elenco di posizioni.
* Facendo clic su un’opzione di posizione desiderata dall’elenco, verranno visualizzate solo le avventure corrispondenti.
* Se c&#39;è un solo Adventure corrispondente, viene visualizzata la visualizzazione Dettagli avventura.

## Congratulazioni

Congratulazioni. Ora hai completato l’integrazione e l’implementazione delle query persistenti nell’app WKND di esempio.
