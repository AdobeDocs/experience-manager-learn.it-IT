---
title: Creare un’app React che interroga l’AEM utilizzando l’API GraphQL - Guida introduttiva di AEM Headless - GraphQL
description: Introduzione a Adobe Experience Manager (AEM) e GraphQL. Crea un’app React che recupera contenuti/dati dall’API GraphQL dell’AEM. Scopri anche come viene utilizzato l’SDK JS dell’AEM headless.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 611
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1181'
ht-degree: 0%

---


# Creare un’app React che utilizza le API GraphQL dell’AEM

In questo capitolo, scopri come le API GraphQL dell’AEM possono guidare l’esperienza in un’applicazione esterna.

Una semplice app React viene utilizzata per eseguire query e visualizzare **Team** e **Persona** contenuti esposti dalle API GraphQL dell’AEM. L’utilizzo di React è in gran parte poco importante e l’applicazione esterna che lo utilizza potrebbe essere scritta in qualsiasi framework per qualsiasi piattaforma.

## Prerequisiti

Si presume che i passaggi descritti nelle parti precedenti di questo tutorial in più parti siano stati completati, oppure [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip) è installato nei servizi di authoring e pubblicazione as a Cloud Service dell’AEM.

_Le schermate IDE di questo capitolo provengono da [Codice di Visual Studio](https://code.visualstudio.com/)_

È necessario installare il seguente software:

- [Node.js v18](https://nodejs.org/en)
- [Codice di Visual Studio](https://code.visualstudio.com/)

## Obiettivi

Scopri come:

- Scarica e avvia l’app React di esempio
- Eseguire query sugli endpoint di AEM GraphQL utilizzando [SDK JS per AEM headless](https://github.com/adobe/aem-headless-client-js)
- Query AEM per un elenco di team e dei relativi membri di riferimento
- Eseguire una query AEM per i dettagli di un membro del team

## Scarica l’app React di esempio

In questo capitolo, viene implementata un’app React di esempio sottoposta a stubbed-out con il codice necessario per interagire con l’API GraphQL dell’AEM e visualizzare i dati di persone e team ottenuti da tali API.

Il codice sorgente dell’app React di esempio è disponibile all’indirizzo Github.com <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Per ottenere l’app React:

1. Clona l’app WKND GraphQL React di esempio da [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Accedi a `basic-tutorial` e aprirla nell’IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![App React in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Aggiorna `.env.development` per connettersi al servizio di pubblicazione as a Cloud Service dell’AEM.

   - Imposta `REACT_APP_HOST_URI`il valore di deve essere l’URL di pubblicazione dell’as a Cloud Service AEM (ad es. `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) e `REACT_APP_AUTH_METHOD`valore di a `none`

   >[!NOTE]
   >
   > Assicurati di aver pubblicato la configurazione del progetto, i modelli per frammenti di contenuto, i frammenti di contenuto creati, gli endpoint di GraphQL e le query persistenti dei passaggi precedenti.
   >
   > Se hai eseguito i passaggi precedenti sull’SDK dell’AEM Author locale, puoi puntare a `http://localhost:4502` e `REACT_APP_AUTH_METHOD`valore di a `basic`.


1. Dalla riga di comando, vai al `aem-guides-wknd-graphql/basic-tutorial` cartella

1. Avviare l’app React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. L’app React viene avviata in modalità di sviluppo su [http://localhost:3000/](http://localhost:3000/). Le modifiche apportate all’app React durante l’esercitazione vengono riportate immediatamente.

![App React parzialmente implementata](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Questa app React è parzialmente implementata. Segui i passaggi descritti in questo tutorial per completare l’implementazione. I file JavaScript che richiedono un lavoro di implementazione presentano il seguente commento: accertati di aggiungere/aggiornare il codice in tali file con il codice specificato in questa esercitazione.
>
>
> //*********************************
>
>  // TODO Per implementarlo, segui i passaggi descritti nell’esercitazione su AEM headless
>
>  //*********************************
>

## Anatomia dell’app React

L’app React di esempio è composta da tre parti principali:

1. Il `src/api` La cartella contiene i file utilizzati per eseguire query GraphQL all’AEM.
   - `src/api/aemHeadlessClient.js` inizializza ed esporta il client headless AEM utilizzato per comunicare con l’AEM
   - `src/api/usePersistedQueries.js` implementa [hook React personalizzati](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) restituire i dati dal GraphQL dell’AEM alla `Teams.js` e `Person.js` visualizzare i componenti.

1. Il `src/components/Teams.js` file visualizza un elenco di team e dei relativi membri, utilizzando una query elenco.
1. Il `src/components/Person.js` file visualizza i dettagli di una singola persona utilizzando una query con parametri a risultato singolo.

## Esaminare l&#39;oggetto AEMHeadless

Rivedi `aemHeadlessClient.js` file per la creazione di `AEMHeadless` oggetto utilizzato per comunicare con l’AEM.

1. Apri `src/api/aemHeadlessClient.js`.

1. Rivedi le righe da 1 a 40:

   - L&#39;importazione `AEMHeadless` dichiarazione del [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js), riga 11.

   - La configurazione dell’autorizzazione in base alle variabili definite in `.env.development`, riga 14-22 e, l&#39;espressione della funzione freccia `setAuthorization`, riga 31-40.

   - Il `serviceUrl` configurazione per il [proxy di sviluppo](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configurazione, riga 27.

1. Le righe da 42 a 49, sono le più importanti, in quanto creano l’istanza `AEMHeadless` ed esportarlo per l’utilizzo in tutta l’app React.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementazione per eseguire query persistenti AEM GraphQL

Per implementare il modello generico `fetchPersistedQuery(..)` funzione per eseguire le query persistenti GraphQL dell’AEM `usePersistedQueries.js` file. Il `fetchPersistedQuery(..)` utilizza la funzione `aemHeadlessClient` dell&#39;oggetto `runPersistedQuery()` funzione per eseguire query in modo asincrono, comportamento basato su promesse.

In seguito, React personalizzato `useEffect` hook richiama questa funzione per recuperare dati specifici dall’AEM.

1. In entrata `src/api/usePersistedQueries.js` **aggiorna** `fetchPersistedQuery(..)`, riga 35, con il codice seguente.

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## Implementare le funzionalità dei team

Quindi, crea la funzionalità per visualizzare i team e i relativi membri nella visualizzazione principale dell’app React. Questa funzionalità richiede:

- Una nuova [hook React useEffect personalizzato](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` che richiama il `my-project/all-teams` query persistente, che restituisce un elenco di frammenti di contenuto team in AEM.
- Un componente React in `src/components/Teams.js` che richiama il nuovo React personalizzato `useEffect` aggancia ed esegue il rendering dei dati dei team.

Una volta completata, la vista principale dell’app si popola con i dati dei team provenienti dall’AEM.

![Visualizzazione team](./assets/graphql-and-external-app/react-app__teams-view.png)

### Passaggi

1. Apri `src/api/usePersistedQueries.js`.

1. Individuare la funzione `useAllTeams()`

1. Per creare un `useEffect` hook che richiama la query persistente `my-project/all-teams` tramite `fetchPersistedQuery(..)`, aggiungi il seguente codice. L’hook restituisce inoltre solo i dati pertinenti della risposta GraphQL dell’AEM al `data?.teamList?.items`, che consente ai componenti della vista React di essere agnostici delle strutture JSON principali.

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. Apri `src/components/Teams.js`

1. In `Teams` componente React, recupera l’elenco dei team dall’AEM utilizzando `useAllTeams()` gancio.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Eseguire la convalida dei dati basati sulla visualizzazione, visualizzando un messaggio di errore o caricando un indicatore basato sui dati restituiti.

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. Infine, esegui il rendering dei dati dei team. Ogni team restituito dalla query GraphQL viene sottoposto a rendering utilizzando il `Team` Componente secondario React.

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## Implementare le funzionalità della persona

Con il [Funzionalità dei team](#implement-teams-functionality) Al termine, implementiamo la funzionalità per gestire la visualizzazione dei dettagli di un membro del team o di una persona.

Questa funzionalità richiede:

- Una nuova [hook React useEffect personalizzato](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` che richiama il parametro `my-project/person-by-name` persistente e restituisce un singolo record persona.

- Un componente React in `src/components/Person.js` che utilizza il nome completo di una persona come parametro di query, richiama il nuovo React personalizzato `useEffect` aggancia ed esegue il rendering dei dati della persona.

Una volta completato, selezionando il nome di una persona nella vista Team, viene riprodotta la vista della persona.

![Persona](./assets/graphql-and-external-app/react-app__person-view.png)

1. Apri `src/api/usePersistedQueries.js`.

1. Individuare la funzione `usePersonByName(fullName)`

1. Per creare un `useEffect` hook che richiama la query persistente `my-project/all-teams` tramite `fetchPersistedQuery(..)`, aggiungi il seguente codice. L’hook restituisce inoltre solo i dati pertinenti della risposta GraphQL dell’AEM al `data?.teamList?.items`, che consente ai componenti della vista React di essere agnostici delle strutture JSON principali.

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. Apri `src/components/Person.js`
1. In `Person` Componente React, analizzare `fullName` e recuperare i dati della persona dall’AEM utilizzando il `usePersonByName(fullName)` gancio.

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. Eseguire la convalida dei dati basata sulla visualizzazione, visualizzando un messaggio di errore o caricando un indicatore basato sui dati restituiti.

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. Infine, esegui il rendering dei dati della persona.

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## Prova l’app

Rivedere l’app [http://localhost:3000/](http://localhost:3000/) e fai clic su _Membri_ collegamenti. Inoltre, puoi aggiungere più team e/o membri all’Alpha Team aggiungendo Frammenti di contenuto all’AEM.

>[!IMPORTANT]
>
>Per verificare le modifiche apportate all’implementazione o se non riesci a far funzionare l’app dopo le modifiche di cui sopra, fai riferimento a [esercitazione di base](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) ramo della soluzione.

## Sotto il cappuccio

Apri il file del browser **Strumenti per sviluppatori** > **Rete** e _Filtro_ per `all-teams` richiesta. Osserva la richiesta API di GraphQL `/graphql/execute.json/my-project/all-teams` è effettuato contro `http://localhost:3000` e **NOT** rispetto al valore di `REACT_APP_HOST_URI`, ad esempio `<https://publish-pxxx-exxx.adobeaemcloud.com`. Le richieste vengono effettuate rispetto al dominio dell’app React perché [configurazione proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) è abilitato tramite `http-proxy-middleware` modulo.


![Richiesta API GraphQL tramite Proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Rivedi l’elemento principale `../setupProxy.js` file e in `../proxy/setupProxy.auth.**.js` file nota come `/content` e `/graphql` I percorsi sono proxy e indicano che non si tratta di una risorsa statica.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

L’utilizzo del proxy locale non è adatto per la distribuzione nell’ambiente di produzione e ulteriori dettagli sono disponibili all’indirizzo _Distribuzione di produzione_ sezione.

## Congratulazioni.{#congratulations}

Congratulazioni. Hai creato correttamente l’app React per utilizzare e visualizzare dati dalle API GraphQL dell’AEM come parte di un’esercitazione di base.
