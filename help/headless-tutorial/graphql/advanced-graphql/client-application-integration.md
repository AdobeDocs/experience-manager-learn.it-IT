---
title: Integrazione delle applicazioni client - Concetti avanzati di AEM Headless - GraphQL
description: Implementa le query persistenti e integrale nell’app WKND.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 241
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 1%

---

# Integrazione delle applicazioni client

Nel capitolo precedente, hai creato e aggiornato le query persistenti utilizzando GraphiQL Explorer.

Questo capitolo illustra i passaggi necessari per integrare le query persistenti con l&#39;applicazione client WKND (o app WKND) utilizzando le richieste HTTP GET all&#39;interno dei **componenti React** esistenti. Fornisce inoltre una sfida opzionale per applicare le tue conoscenze headless di AEM, competenze di codifica per migliorare l’applicazione client WKND.

## Prerequisiti {#prerequisites}

Questo documento fa parte di un&#39;esercitazione in più parti. Prima di procedere con questo capitolo, assicurati che i capitoli precedenti siano stati completati. L&#39;applicazione client WKND si connette al servizio di pubblicazione AEM, pertanto è importante che **abbiate pubblicato quanto segue nel servizio di pubblicazione AEM**.

* Configurazioni progetto
* Endpoint GraphQL
* Modelli per frammenti di contenuto
* Frammenti di contenuto creati
* Query persistenti GraphQL

Le schermate dell&#39;_IDE in questo capitolo provengono da [Visual Studio Code](https://code.visualstudio.com/)_

### Capitolo 1-4 Pacchetto di soluzioni (facoltativo) {#solution-package}

È disponibile un pacchetto di soluzione da installare che completa i passaggi descritti nell’interfaccia utente di AEM per i capitoli 1-4. Questo pacchetto è **non necessario** se i capitoli precedenti sono stati completati.

1. Scarica [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. In AEM, passa a **Strumenti** > **Distribuzione** > **Pacchetti** per accedere a **Gestione pacchetti**.
1. Carica e installa il pacchetto (file zip) scaricato nel passaggio precedente.
1. Replica il pacchetto nel servizio di pubblicazione AEM

## Obiettivi {#objectives}

In questa esercitazione imparerai a integrare le richieste di query persistenti nell&#39;app di esempio WKND GraphQL React utilizzando [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clonare ed eseguire l&#39;applicazione client di esempio {#clone-client-app}

Per accelerare l’esercitazione, viene fornita un’app JS di React iniziale.

1. Clona l&#39;archivio [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifica il file `aem-guides-wknd-graphql/advanced-tutorial/.env.development` e imposta `REACT_APP_HOST_URI` in modo che punti al servizio AEM Publish di destinazione.

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
   > Le istruzioni precedenti indicano di connettere l&#39;app React al servizio di pubblicazione **AEM**. Tuttavia, per connettersi al servizio di authoring **AEM** è necessario ottenere un token di sviluppo locale per l&#39;ambiente AEM as a Cloud Service di destinazione.
   >
   > È inoltre possibile connettere l&#39;app a un&#39;istanza Autore [locale utilizzando AEMaaCS SDK](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) utilizzando l&#39;autenticazione di base.


1. Apri un terminale ed esegui i comandi:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Una nuova finestra del browser deve essere caricata su [http://localhost:3000](http://localhost:3000)


1. Tocca **Camping** > **Yosemite Backpacking** per visualizzare i dettagli dell&#39;avventura di Yosemite Backpacking.

   ![Schermata Backpacking Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Apri gli strumenti per sviluppatori del browser ed esamina la richiesta `XHR`

   ![PUBBLICA GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Dovrebbero essere presenti `GET` richieste all&#39;endpoint GraphQL con il nome di configurazione del progetto (`wknd-shared`), il nome di query persistente (`adventure-by-slug`), il nome della variabile (`slug`), il valore (`yosemite-backpacking`) e codifiche di caratteri speciali.

>[!IMPORTANT]
>
>    Se ti stai chiedendo perché la richiesta API di GraphQL viene effettuata contro `http://localhost:3000` e NON contro il dominio del servizio di pubblicazione di AEM, controlla [Sotto il cappuccio](../multi-step/graphql-and-react-app.md#under-the-hood) dall&#39;esercitazione di base.


## Rivedi il codice

Nel passaggio [Esercitazione di base - Creare un&#39;app React che utilizza le API GraphQL di AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) abbiamo rivisto e migliorato alcuni file chiave per acquisire esperienza pratica. Prima di migliorare l’app WKND, controlla i file chiave.

* [Rivedi l&#39;oggetto AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementare per eseguire query persistenti di AEM GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Rivedi `Adventures` componente React

La visualizzazione principale dell&#39;app WKND React è l&#39;elenco di tutte le avventure e puoi filtrarle in base al tipo di attività _Campeggio, Ciclismo_. Il rendering di questa visualizzazione viene eseguito dal componente `Adventures`. Di seguito sono riportati i principali dettagli di implementazione:

* L&#39;hook `src/components/Adventures.js` chiama `useAllAdventures(adventureActivity)` e qui l&#39;argomento `adventureActivity` è di tipo attività.

* L&#39;hook `useAllAdventures(adventureActivity)` è definito nel file `src/api/usePersistedQueries.js`. In base al valore `adventureActivity`, determina la query persistente da chiamare. Se il valore non è nullo, chiama `wknd-shared/adventures-by-activity`, altrimenti ottiene tutte le avventure disponibili `wknd-shared/adventures-all`.

* L&#39;hook utilizza la funzione principale `fetchPersistedQuery(..)` che delega l&#39;esecuzione della query a `AEMHeadless` tramite `aemHeadlessClient.js`.

* L&#39;hook restituisce inoltre solo i dati rilevanti della risposta di AEM GraphQL in `response.data?.adventureList?.items`, consentendo ai componenti della visualizzazione React di `Adventures` di essere agnostici delle strutture JSON padre.

* Dopo la corretta esecuzione della query, la funzione di rendering `AdventureListItem(..)` di `Adventures.js` aggiunge l&#39;elemento HTML per visualizzare le informazioni relative a _Immagine, Durata viaggio, Prezzo e Titolo_.

### Rivedi `AdventureDetail` componente React

Il componente React di `AdventureDetail` esegue il rendering dei dettagli dell&#39;avventura. Di seguito sono riportati i principali dettagli di implementazione:

* L&#39;hook `src/components/AdventureDetail.js` chiama `useAdventureBySlug(slug)` e qui l&#39;argomento `slug` è il parametro di query.

* Come sopra, l&#39;hook `useAdventureBySlug(slug)` è definito nel file `src/api/usePersistedQueries.js`. Chiama la query persistente `wknd-shared/adventure-by-slug` delegando a `AEMHeadless` tramite `aemHeadlessClient.js`.

* Dopo la corretta esecuzione della query, la funzione di rendering `AdventureDetailRender(..)` di `AdventureDetail.js` aggiunge l&#39;elemento HTML per visualizzare i dettagli Adventure.


## Migliorare il codice

### Usa query persistente `adventure-details-by-slug`

Nel capitolo precedente è stata creata la query persistente `adventure-details-by-slug`, che fornisce informazioni aggiuntive su Adventure come _location, INSTRUCTorTeam e administrator_. Sostituiamo `adventure-by-slug` con `adventure-details-by-slug` query persistente per eseguire il rendering di queste informazioni aggiuntive.

1. Apri `src/api/usePersistedQueries.js`.

1. Individuare la funzione `useAdventureBySlug()` e aggiornare la query come

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

1. Individuare la funzione `AdventureDetailRender(..)` e aggiornare la funzione restituita come

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

   **InformazioniPosizione**

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
>I file aggiornati sono disponibili nel progetto **AEM Guides WKND - GraphQL**. Vedere la [sezione Esercitazione avanzata](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial).


Dopo aver completato i miglioramenti di cui sopra, l&#39;app WKND si presenta come di seguito e gli strumenti di sviluppo del browser mostrano `adventure-details-by-slug` chiamata di query persistente.

![APP WKND avanzata](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Sfida di miglioramento (opzionale)

La visualizzazione principale dell&#39;app WKND React ti consente di filtrare queste avventure in base al tipo di attività _Campeggio, Ciclismo_. Tuttavia, il team aziendale WKND desidera avere una funzionalità di filtro aggiuntiva basata su _Posizione_. I requisiti sono:

* Nella visualizzazione principale dell&#39;app WKND, nell&#39;angolo in alto a sinistra o a destra, aggiungi l&#39;icona di filtro _Posizione_.
* Facendo clic sull&#39;icona di filtro _Posizione_ verrà visualizzato l&#39;elenco delle posizioni.
* Facendo clic su un’opzione di posizione desiderata dall’elenco, vengono visualizzate solo le avventure corrispondenti.
* Se esiste una sola avventura corrispondente, viene visualizzata la vista Dettagli avventura.

## Complimenti

Congratulazioni Ora hai completato l’integrazione e l’implementazione delle query persistenti nell’app WKND di esempio.
