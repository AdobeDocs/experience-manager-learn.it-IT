---
title: Integrazione delle applicazioni client - Concetti avanzati di AEM headless - GraphQL
description: Implementa le query persistenti e integrale nell’app WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 345
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 1%

---

# Integrazione delle applicazioni client

Nel capitolo precedente, hai creato e aggiornato le query persistenti utilizzando GraphiQL Explorer.

Questo capitolo illustra i passaggi necessari per integrare le query persistenti con l’applicazione client WKND (o app WKND) utilizzando le richieste HTTP GET all’interno di **Componenti React**. Fornisce inoltre una sfida opzionale per applicare le tue conoscenze headless AEM, competenze di codifica per migliorare l’applicazione client WKND.

## Prerequisiti {#prerequisites}

Questo documento fa parte di un&#39;esercitazione in più parti. Prima di procedere con questo capitolo, assicurati che i capitoli precedenti siano stati completati. L’applicazione client WKND si connette al servizio di pubblicazione AEM, pertanto è importante che tu **ha pubblicato quanto segue nel servizio di pubblicazione dell&#39;AEM**.

* Configurazioni progetto
* Endpoint GraphQL
* Modelli per frammenti di contenuto
* Frammenti di contenuto creati
* Query persistenti GraphQL

Il _Le schermate IDE di questo capitolo provengono da [Codice di Visual Studio](https://code.visualstudio.com/)_

### Capitolo 1-4 Pacchetto di soluzioni (facoltativo) {#solution-package}

È disponibile un pacchetto di soluzione da installare che completa i passaggi descritti nell’interfaccia utente dell’AEM per i capitoli 1-4. Questo pacchetto è **non necessario** se i capitoli precedenti sono stati completati.

1. Scarica [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. In AEM, vai a **Strumenti** > **Distribuzione** > **Pacchetti** per accedere **Gestione pacchetti**.
1. Carica e installa il pacchetto (file zip) scaricato nel passaggio precedente.
1. Replica il pacchetto nel servizio di pubblicazione AEM

## Obiettivi {#objectives}

In questo tutorial, scopri come integrare le richieste di query persistenti nell’app di esempio WKND GraphQL React utilizzando [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clonare ed eseguire l&#39;applicazione client di esempio {#clone-client-app}

Per accelerare l’esercitazione, viene fornita un’app JS di React iniziale.

1. Clona il [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifica il `aem-guides-wknd-graphql/advanced-tutorial/.env.development` file e set `REACT_APP_HOST_URI` per puntare al servizio di pubblicazione AEM target.

   Se ti connetti a un’istanza Autore, aggiorna il metodo di autenticazione.

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

   ![Ambiente di sviluppo app React](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > Le istruzioni precedenti consistono nel collegare l’app React a **Servizio di pubblicazione AEM**, tuttavia per connettersi al **Servizio di authoring AEM** ottenere un token di sviluppo locale per l’ambiente di destinazione as a Cloud Service per AEM.
   >
   > È anche possibile collegare l’app a una [istanza Autore locale con SDK AEMaaCS](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) utilizzo dell’autenticazione di base.


1. Apri un terminale ed esegui i comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Viene caricata una nuova finestra del browser [http://localhost:3000](http://localhost:3000)


1. Tocca **Campeggio** > **Yosemite Backpacking** per visualizzare i dettagli dell’avventura Yosemite Backpacking.

   ![Schermata Backpacking di Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Apri gli strumenti di sviluppo del browser e controlla `XHR` richiesta

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Dovresti vedere `GET` richieste all’endpoint GraphQL con nome di configurazione del progetto (`wknd-shared`), nome query persistente (`adventure-by-slug`), nome variabile (`slug`), valore (`yosemite-backpacking`) e codifiche per caratteri speciali.

>[!IMPORTANT]
>
>    Se ti stai chiedendo perché la richiesta API di GraphQL viene effettuata contro `http://localhost:3000` e NON rispetto al dominio del servizio di pubblicazione AEM, revisione [Sotto il cappuccio](../multi-step/graphql-and-react-app.md#under-the-hood) dall&#39;esercitazione di base.


## Rivedi il codice

In [Tutorial di base: creare un’app React che utilizza le API GraphQL dell’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) fase avevamo rivisto e migliorato alcuni file chiave per ottenere competenze pratiche. Prima di migliorare l’app WKND, controlla i file chiave.

* [Esaminare l&#39;oggetto AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementazione per eseguire query persistenti AEM GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Revisione `Adventures` Componente React

La vista principale dell’app WKND React è l’elenco di tutte le avventure e puoi filtrarle in base al tipo di attività, ad esempio _Campeggio, Ciclismo_. Il rendering di questa visualizzazione viene eseguito da `Adventures` componente. Di seguito sono riportati i principali dettagli di implementazione:

* Il `src/components/Adventures.js` chiamate `useAllAdventures(adventureActivity)` hook e here `adventureActivity` l&#39;argomento è il tipo di attività.

* Il `useAllAdventures(adventureActivity)` hook definito in `src/api/usePersistedQueries.js` file. In base a `adventureActivity` determina quale query persistente chiamare. Se il valore non è nullo, chiama `wknd-shared/adventures-by-activity`, altrimenti ottiene tutte le avventure disponibili `wknd-shared/adventures-all`.

* L&#39;hook utilizza il `fetchPersistedQuery(..)` funzione che delega l’esecuzione della query a `AEMHeadless` tramite `aemHeadlessClient.js`.

* L’hook restituisce inoltre solo i dati pertinenti della risposta GraphQL dell’AEM al `response.data?.adventureList?.items`, consentendo `Adventures` I componenti della vista React devono essere agnostici delle strutture JSON principali.

* In caso di esecuzione corretta della query, il `AdventureListItem(..)` funzione di rendering da `Adventures.js` aggiunge un elemento HTML per visualizzare _Immagine, durata viaggio, prezzo e titolo_ informazioni.

### Revisione `AdventureDetail` Componente React

Il `AdventureDetail` Il componente React esegue il rendering dei dettagli dell’avventura. Di seguito sono riportati i principali dettagli di implementazione:

* Il `src/components/AdventureDetail.js` chiamate `useAdventureBySlug(slug)` hook e here `slug` è un parametro di query.

* Come sopra, il `useAdventureBySlug(slug)` hook definito in `src/api/usePersistedQueries.js` file. Chiama `wknd-shared/adventure-by-slug` query persistente tramite delega a `AEMHeadless` tramite `aemHeadlessClient.js`.

* In caso di esecuzione corretta della query, il `AdventureDetailRender(..)` funzione di rendering da `AdventureDetail.js` aggiunge l’elemento HTML per visualizzare i dettagli Adventure.


## Migliorare il codice

### Utilizzare `adventure-details-by-slug` query persistente

Nel capitolo precedente, abbiamo creato `adventure-details-by-slug` persistente, fornisce informazioni aggiuntive su Adventure come _posizione, team di istruttori e amministratore_. Sostituiamo `adventure-by-slug` con `adventure-details-by-slug` query persistente per il rendering di queste informazioni aggiuntive.

1. Apri `src/api/usePersistedQueries.js`.

1. Individuare la funzione `useAdventureBySlug()` e aggiorna query come

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

1. Individuare la funzione `AdventureDetailRender(..)` e aggiorna funzione di ritorno come

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

1. Definisci anche le funzioni di rendering corrispondenti:

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

   **Team istruttore**

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

   **Amministratore**

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

1. Apri `src/components/AdventureDetail.scss` e aggiungi le seguenti definizioni di classe

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
>I file aggiornati sono disponibili in **WKND sulle guide dell’AEM - GraphQL** progetto, vedi [Esercitazione avanzata](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) sezione.


Dopo aver completato i miglioramenti di cui sopra, l’app WKND si presenta come di seguito e gli strumenti per sviluppatori del browser mostrano `adventure-details-by-slug` chiamata di query persistente.

![APP WKND migliorata](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Sfida di miglioramento (opzionale)

La vista principale dell’app WKND React ti consente di filtrare queste avventure in base al tipo di attività, ad esempio _Campeggio, Ciclismo_. Tuttavia, il team aziendale WKND desidera avere un _Posizione_ funzionalità di filtro basata su. I requisiti sono:

* Nella visualizzazione principale dell’app WKND, nell’angolo in alto a sinistra o a destra, aggiungi _Posizione_ icona di filtro.
* Clic _Posizione_ l’icona del filtro deve visualizzare l’elenco delle posizioni.
* Facendo clic su un’opzione di posizione desiderata dall’elenco, vengono visualizzate solo le avventure corrispondenti.
* Se esiste una sola avventura corrispondente, viene visualizzata la vista Dettagli avventura.

## Complimenti

Congratulazioni. Ora hai completato l’integrazione e l’implementazione delle query persistenti nell’app WKND di esempio.
