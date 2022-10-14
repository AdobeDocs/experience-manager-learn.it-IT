---
title: Creare un’app React che esegue query AEM utilizzando l’API GraphQL - Guida introduttiva a AEM Headless - GraphQL
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Crea app React che recupera contenuti/dati dall’API GraphQL AEM, inoltre scopri come viene utilizzato AEM SDK JS headless.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 2%

---


# Creare un’app React che utilizza AEM API GraphQL

In questo capitolo viene illustrato come AEM le API GraphQL possono guidare l&#39;esperienza in un&#39;applicazione esterna.

Viene utilizzata una semplice app React per eseguire query e visualizzare **Team** e **Persona** contenuto esposto dalle API GraphQL AEM. L&#39;utilizzo di React è in gran parte poco importante e l&#39;applicazione esterna consueta potrebbe essere scritta in qualsiasi struttura per qualsiasi piattaforma.

## Prerequisiti

Si presume che i passaggi descritti nelle parti precedenti di questa esercitazione in più parti siano stati completati, oppure [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) è installato sui servizi Author e Publish AEM as a Cloud Service.

_Le schermate IDE in questo capitolo provengono da [Codice di Visual Studio](https://code.visualstudio.com/)_

È necessario installare il software seguente:

- [Node.js v14+](https://nodejs.org/it/)
- [npm 6.4+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- [Codice di Visual Studio](https://code.visualstudio.com/)

## Obiettivi

Scopri come:

- Scarica e avvia l’app React di esempio
- Query AEM endpoint GraphQL utilizzando [SDK JS AEM Headless](https://github.com/adobe/aem-headless-client-js)
- Query AEM per un elenco di team e relativi membri di riferimento
- Query AEM per i dettagli di un membro del team

## Ottieni l’app React di esempio

In questo capitolo, viene implementata un’app React di esempio sovrapposta con il codice necessario per interagire con AEM’API GraphQL e vengono visualizzati i dati relativi a team e persone ottenuti da tali app.

Il codice sorgente dell’app React di esempio è disponibile su Github.com all’indirizzo <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Per ottenere l’app React:

1. Clona l’app React WKND GraphQL di esempio da [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Passa a `basic-tutorial` e aprilo nell’IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![React App in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Aggiorna `.env.development` per connettersi a AEM servizio Pubblicazione as a Cloud Service.

   - Imposta `REACT_APP_HOST_URI`È il valore per l’URL di pubblicazione dell’AEM as a Cloud Service (ad es. `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) e `REACT_APP_AUTH_METHOD`Valore di `none`
   >[!NOTE]
   >
   > Assicurati di aver pubblicato la configurazione del progetto, i modelli di frammenti di contenuto, i frammenti di contenuto creati, gli endpoint GraphQL e le query persistenti dai passaggi precedenti.
   >
   > Se esegui i passaggi precedenti sull’SDK locale di AEM Author, puoi puntare a `http://localhost:4502` e `REACT_APP_AUTH_METHOD`Valore di `basic`.


1. Dalla riga di comando, passa alla `aem-guides-wknd-graphql/basic-tutorial` cartella

1. Avvia l&#39;app React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. L’app React viene avviata in modalità di sviluppo in [http://localhost:3000/](Http://localhost:3000/). Le modifiche apportate all’app React durante l’esercitazione vengono riportate immediatamente.

![App React parzialmente implementata](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Si tratta di un’app React parzialmente implementata, che verrà completata nei seguenti passaggi. I file JavaScript che richiedono un lavoro di implementazione hanno il seguente commento, assicurati di aggiungere/aggiornare il codice in tali file con il codice specificato in questa esercitazione.
>
>
> /***********************************************
>
>  // TODO :: Implementa questo seguendo i passaggi di AEM Tutorial headless
>
>  /***********************************************



## Anatomia dell’app React

L’app React di esempio ha tre parti principali:

1. La `src/api` La cartella contiene i file utilizzati per creare query GraphQL da AEM.
   - `src/api/aemHeadlessClient.js` inizializza ed esporta il client headless AEM utilizzato per comunicare con AEM
   - `src/api/usePersistedQueries.js` implementazioni [hook di React personalizzati](https://reactjs.org/docs/hooks-custom.html) restituisce i dati da AEM GraphQL a `Teams.js` e `Person.js` visualizza componenti.

1. La `src/components/Teams.js` visualizza un elenco dei team e dei relativi membri utilizzando una query a elenco.
1. La `src/components/Person.js` in file vengono visualizzati i dettagli di una singola persona utilizzando una query con singoli risultati con parametri.

## Rivedi l&#39;oggetto AEMHeadless

Consulta la sezione `aemHeadlessClient.js` come creare `AEMHeadless` oggetto utilizzato per comunicare con AEM.

1. Apri `src/api/aemHeadlessClient.js`.

1. Rivedi le linee 1-40:

   - Importazione `AEMHeadless` dichiarazione del [Client AEM headless per JavaScript](https://github.com/adobe/aem-headless-client-js), linea 11.

   - La configurazione dell’autorizzazione in base alle variabili definite in `.env.development`, riga 14-22 e, l&#39;espressione della funzione freccia `setAuthorization`, linea 31-40.

   - La `serviceUrl` configurazione per l&#39;incluso [proxy di sviluppo](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configurazione, linea 27.

1. Le linee 42-49, sono più importanti, in quanto creano l&#39;istanza `AEMHeadless` e esportalo per l’utilizzo in tutta l’app React.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementare per eseguire AEM query persistenti GraphQL

Apri `usePersistedQueries.js` file per implementare il generico `fetchPersistedQuery(..)` per eseguire le query persistenti AEM GraphQL. La `fetchPersistedQuery(..)` utilizza la funzione `aemHeadlessClient` dell&#39;oggetto `runPersistedQuery()` per eseguire la query in modo asincrono e basato su promesse.

Successivamente, React personalizzato `useEffect` gancia chiama questa funzione per recuperare dati specifici da AEM.

1. In `src/api/usePersistedQueries.js` **update** `fetchPersistedQuery(..)`, riga 35, con il codice seguente.

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

## Implementare la funzionalità Team

Quindi, crea la funzionalità per visualizzare i team e i loro membri nella vista principale dell’app React. Questa funzionalità richiede:

- Nuovo [gancio React useEffect personalizzato](https://reactjs.org/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` che richiama `my-project/all-teams` query persistente, restituendo un elenco di frammenti di contenuto del team in AEM.
- Componente React in `src/components/Teams.js` che richiama il nuovo React personalizzato `useEffect` collega ed esegue il rendering dei dati dei team.

Una volta completata, la visualizzazione principale dell’app si popola con i dati dei team da AEM.

![Visualizzazione Team](./assets/graphql-and-external-app/react-app__teams-view.png)

### Passaggi

1. Apri `src/api/usePersistedQueries.js`.

1. Individuare la funzione `useAllTeams()`

1. Per creare una `useEffect` gancio che richiama la query persistente `my-project/all-teams` tramite `fetchPersistedQuery(..)`, aggiungi il seguente codice. Il gancio restituisce anche solo i dati pertinenti dalla risposta GraphQL AEM a `data?.teamList?.items`, che consente ai componenti della vista React di essere agnostici delle strutture JSON principali.

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

1. In `Teams` React component, recupera l’elenco dei team da AEM utilizzando `useAllTeams()` aggancio.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Esegui la convalida dei dati basata sulla visualizzazione, visualizzando un messaggio di errore o un indicatore di caricamento in base ai dati restituiti.

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

1. Infine, esegui il rendering dei dati dei team. Viene eseguito il rendering di ogni team restituito dalla query GraphQL utilizzando il `Team` Reagisce a un sottocomponente.

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


## Implementare la funzionalità Persona

Con la [Funzionalità Team](#implement-teams-functionality) completa, implementiamo la funzionalità per gestire la visualizzazione dei dettagli di un membro del team o di una persona.

Questa funzionalità richiede:

- Nuovo [gancio React useEffect personalizzato](https://reactjs.org/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` che richiama il parametro `my-project/person-by-name` query persistente e restituisce un record a persona singola.

- Componente React in `src/components/Person.js` che utilizza il nome completo di una persona come parametro di query, richiama il nuovo React personalizzato `useEffect` collega ed esegue il rendering dei dati personali.

Una volta completato, selezionando il nome di una persona nella vista Team, esegue il rendering della visualizzazione persona.

![Utente](./assets/graphql-and-external-app/react-app__person-view.png)

1. Apri `src/api/usePersistedQueries.js`.

1. Individuare la funzione `usePersonByName(fullName)`

1. Per creare una `useEffect` gancio che richiama la query persistente `my-project/all-teams` tramite `fetchPersistedQuery(..)`, aggiungi il seguente codice. Il gancio restituisce anche solo i dati pertinenti dalla risposta GraphQL AEM a `data?.teamList?.items`, che consente ai componenti della vista React di essere agnostici delle strutture JSON principali.

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
1. In `Person` Componente React, analizzare il `fullName` il parametro di route e recuperare i dati personali da AEM utilizzando il `usePersonByName(fullName)` aggancio.

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

1. Esegui la convalida dei dati basata sulla visualizzazione, visualizzando un messaggio di errore o un indicatore di caricamento in base ai dati restituiti.

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

Esamina l’app [http://localhost:3000/](Http://localhost:3000/) e fai clic su _Membri_ link. Puoi anche aggiungere più team e/o membri al Team Alpha aggiungendo frammenti di contenuto in AEM.

## Sotto Il Cappuccio

Apri il browser **Strumenti per gli sviluppatori** > **Rete** e _Filtro_ per `all-teams` richiesta, nota la richiesta API GraphQL `/graphql/execute.json/my-project/all-teams` contro `http://localhost:3000` e **NOT** rispetto al valore di `REACT_APP_HOST_URI`(ad esempio, <https://publish-p123-e456.adobeaemcloud.com>), perché [configurazione proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) utilizzo `http-proxy-middleware` modulo .


![Richiesta API GraphQL tramite proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Rivedi il principale `../setupProxy.js` e all&#39;interno `../proxy/setupProxy.auth.**.js` i file notano come `/content` e `/graphql` i percorsi sono collegati tramite proxy e indicano che non si tratta di una risorsa statica.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

Tuttavia, questa non è un&#39;opzione adatta per l&#39;implementazione di produzione e per ulteriori dettagli visita _Distribuzione di produzione_ sezione .

## Congratulazioni!{#congratulations}

Congratulazioni! Hai creato correttamente l’app React per utilizzare e visualizzare dati dalle API GraphQL AEM come parte delle esercitazioni di base!
