---
title: Creare un’app React che esegue query su AEM utilizzando l’API GraphQL - Guida introduttiva di AEM Headless - GraphQL
description: Introduzione a Adobe Experience Manager (AEM) e GraphQL. Crea un’app React che recupera contenuti/dati dall’API GraphQL di AEM e scopri come viene utilizzato AEM Headless JS SDK.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 410
source-git-commit: e7f556737cdf6a92c0503d3b4a52eef1f71c8330
workflow-type: tm+mt
source-wordcount: '1479'
ht-degree: 2%

---


# Creare un’app React che utilizza le API GraphQL di AEM

In questo capitolo, scopri come le API GraphQL di AEM possono guidare l’esperienza in un’applicazione esterna.

Viene utilizzata una semplice app React per eseguire query e visualizzare il contenuto di **Team** e **Persona** esposto dalle API GraphQL di AEM. L’utilizzo di React è in gran parte poco importante e l’applicazione esterna che lo utilizza potrebbe essere scritta in qualsiasi framework per qualsiasi piattaforma.

## Prerequisiti

Si presume che i passaggi descritti nelle parti precedenti di questo tutorial in più parti siano stati completati o che [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip) sia installato nei servizi Author e Publish di AEM.

_Le schermate IDE in questo capitolo provengono da [Visual Studio Code](https://code.visualstudio.com/)_

### Ambiente AEM

Per completare questa esercitazione, ti consigliamo di avere accesso come amministratore AEM a uno dei seguenti elementi:

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configurazione locale con [AEM Cloud Service SDK](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview)
+ [AEM 6.5 LTS](https://experienceleague.adobe.com/docs/experience-manager-65/content/release-notes/release-notes.html) con [Pacchetto indice GraphQL 1.0.5+](https://experienceleague.adobe.com/docs/experience-manager-65/content/headless/graphql-api/graphql-endpoint.html) installato

### Requisiti software

È necessario installare il seguente software:

+ [Node.js v18+](https://nodejs.org/en)
+ [Codice di Visual Studio](https://code.visualstudio.com/)
+ [Git](https://git-scm.com/)
+ [Java JDK](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/introduction/technical-requirements) (se ci si connette all&#39;istanza locale di AEM SDK o 6.5)

## Obiettivi

Scopri come:

+ Scarica e avvia l’app React di esempio
+ Esegui query sugli endpoint GraphQL di AEM utilizzando [AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)
+ Cercare in AEM un elenco di team e dei relativi membri di riferimento
+ Eseguire una query su AEM per i dettagli di un membro del team

## Scarica l’app React di esempio

In questo capitolo, viene implementata un’app React di esempio sottoposta a stubbed-out con il codice necessario per interagire con l’API GraphQL di AEM e visualizzare i dati di team e persone ottenuti da tali API.

Il codice sorgente dell&#39;app React di esempio è disponibile sul sito Github.com all&#39;indirizzo <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Per ottenere l’app React:

1. Clona l&#39;app WKND GraphQL React di esempio da [Github.com](https://github.com/adobe/aem-guides-wknd-graphql). Ricordare che questo archivio Git contiene diversi progetti, quindi accertarsi di spostarsi nella cartella `basic-tutorial` come descritto nel passaggio successivo.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Passare alla cartella `basic-tutorial` e aprirla nell&#39;IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![App React in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Aggiorna `.env.development` per connetterti al servizio di pubblicazione AEM.

   Per ulteriori dettagli sulle variabili di ambiente del progetto di esempio e su come impostarle, [rivedi il file README.md](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#update-environment-variables).

   **AEM as a Cloud Service**

   Quando si collega l&#39;app React al servizio di pubblicazione AEM as a Cloud Service, impostare il valore di `REACT_APP_HOST_URI` come URL di pubblicazione dell&#39;AEM as a Cloud Service (ad es. `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) e il valore di `REACT_APP_AUTH_METHOD` in `none`

   **AEM SDK per AEM as a Cloud Service (locale)**

   Quando si collega l&#39;app React a un ambiente AEM as a Cloud Service SDK locale, impostare il valore di `REACT_APP_HOST_URI` su localhost e porta di pubblicazione (ad esempio `REACT_APP_HOST_URI=http://localhost:4503`) e il valore di `REACT_APP_AUTH_METHOD` in `none`

   **Ambiente ospitato da AEM 6.5 LTS**

   Quando si collega l&#39;app React al servizio di pubblicazione AEM as a Cloud Service, impostare il valore di `REACT_APP_HOST_URI` come URL di pubblicazione AEM 6.5 (ad esempio `REACT_APP_HOST_URI=https://dev.mysite.com`) e il valore di `REACT_APP_AUTH_METHOD` in `none`

   **Avvio rapido AEM 6.5 LTS (locale)**

   Quando si collega l&#39;app React a un avvio rapido locale di AEM 6.5, impostare il valore di `REACT_APP_HOST_URI` su localhost e porta di pubblicazione (ad esempio `REACT_APP_HOST_URI=http://localhost:4503`) e il valore di `REACT_APP_AUTH_METHOD` in `none`

   >[!NOTE]
   >
   > Assicurati di aver pubblicato la configurazione del progetto, i modelli per frammenti di contenuto, i frammenti di contenuto creati, gli endpoint di GraphQL e le query persistenti dei passaggi precedenti.
   >
   > Se hai eseguito i passaggi precedenti su AEM Author SDK locale, puoi puntare a `http://localhost:4502` e al valore di `REACT_APP_AUTH_METHOD` a `basic`.


1. Dalla riga di comando, passare alla cartella `aem-guides-wknd-graphql/basic-tutorial`

1. Avviare l’app React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. L&#39;app React viene avviata in modalità di sviluppo il [http://localhost:3000/](http://localhost:3000/). Le modifiche apportate all’app React durante l’esercitazione vengono riportate immediatamente.

![App React parzialmente implementata](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Questa app React è parzialmente implementata. Segui i passaggi descritti in questo tutorial per completare l’implementazione. I file JavaScript che richiedono un lavoro di implementazione presentano il seguente commento. Accertati di aggiungere/aggiornare il codice in tali file con il codice specificato in questa esercitazione.
>
>
> //*********************************
>
>  // TODO Per implementarlo, segui i passaggi dell’esercitazione AEM Headless
>
>  //*********************************
>

## Anatomia dell’app React

L’app React di esempio è composta da tre parti principali:

1. La cartella `src/api` contiene i file utilizzati per eseguire query GraphQL su AEM.
   + `src/api/aemHeadlessClient.js` inizializza ed esporta il client AEM Headless utilizzato per comunicare con AEM
   + `src/api/usePersistedQueries.js` implementa [hook React personalizzati](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) per restituire dati da AEM GraphQL ai componenti di visualizzazione `Teams.js` e `Person.js`.

1. Nel file `src/components/Teams.js` viene visualizzato un elenco di team e dei relativi membri tramite una query elenco.
1. Nel file `src/components/Person.js` vengono visualizzati i dettagli di una singola persona mediante una query con parametri a risultato singolo.

## Esaminare l&#39;oggetto AEMHeadless

Esaminare il file `aemHeadlessClient.js` per informazioni su come creare l&#39;oggetto `AEMHeadless` utilizzato per comunicare con AEM.

1. Apri `src/api/aemHeadlessClient.js`.

1. Rivedi le righe da 1 a 40:

   + Dichiarazione di importazione di `AEMHeadless` dal client [AEM Headless per JavaScript](https://github.com/adobe/aem-headless-client-js), riga 11.

   + Configurazione dell&#39;autorizzazione basata sulle variabili definite in `.env.development`, riga 14-22, e sull&#39;espressione della funzione freccia `setAuthorization`, riga 31-40.

   + Configurazione di `serviceUrl` per la configurazione di [development proxy](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#proxy-api-requests) inclusa, riga 27.

1. Le righe da 42 a 49 sono le più importanti, in quanto creano un&#39;istanza del client `AEMHeadless` e lo esportano per l&#39;utilizzo in tutta l&#39;app React.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementazione per eseguire query persistenti di AEM GraphQL

Per implementare la funzione generica `fetchPersistedQuery(..)` per eseguire le query persistenti di AEM GraphQL, aprire il file `usePersistedQueries.js`. La funzione `fetchPersistedQuery(..)` utilizza la funzione `aemHeadlessClient` dell&#39;oggetto `runPersistedQuery()` per eseguire query in modo asincrono, in base a promesse.

Successivamente, l&#39;hook personalizzato di React `useEffect` chiama questa funzione per recuperare dati specifici da AEM.

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

## Implementare le funzionalità dei team

Quindi, crea la funzionalità per visualizzare i team e i relativi membri nella visualizzazione principale dell’app React. Questa funzionalità richiede:

+ Nuovo [hook useEffect personalizzato di React](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` che richiama la query persistente `my-project/all-teams`, restituendo un elenco di frammenti di contenuto team in AEM.
+ Un componente React in `src/components/Teams.js` che richiama il nuovo hook personalizzato di React `useEffect` ed esegue il rendering dei dati dei team.

Una volta completata, la vista principale dell’app si popola con i dati dei team provenienti da AEM.

![Visualizzazione team](./assets/graphql-and-external-app/react-app__teams-view.png)

### Passaggi

1. Apri `src/api/usePersistedQueries.js`.

1. Individua la funzione `useAllTeams()`

1. Per creare un hook `useEffect` che richiama la query persistente `my-project/all-teams` tramite `fetchPersistedQuery(..)`, aggiungere il codice seguente. L&#39;hook restituisce inoltre solo i dati rilevanti della risposta di AEM GraphQL in `data?.teamList?.items`, consentendo ai componenti della visualizzazione React di essere agnostici delle strutture JSON padre.

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

1. Nel componente React `Teams`, recupera l&#39;elenco dei team da AEM utilizzando l&#39;hook `useAllTeams()`.

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

1. Infine, esegui il rendering dei dati dei team. Viene eseguito il rendering di ogni team restituito dalla query GraphQL utilizzando il sottocomponente React `Team` fornito.

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

Con la funzionalità [Team](#implement-teams-functionality) completata, implementiamo la funzionalità per gestire la visualizzazione dei dettagli di un membro del team o di una persona.

Questa funzionalità richiede:

+ Un nuovo [hook useEffect personalizzato di React](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` che richiama la query persistente con parametri `my-project/person-by-name` e restituisce un singolo record persona.

+ Un componente React in `src/components/Person.js` che utilizza il nome completo di una persona come parametro di query, richiama il nuovo hook personalizzato React `useEffect` ed esegue il rendering dei dati della persona.

Una volta completato, selezionando il nome di una persona nella vista Team, viene riprodotta la vista della persona.

![Persona](./assets/graphql-and-external-app/react-app__person-view.png)

1. Apri `src/api/usePersistedQueries.js`.

1. Individua la funzione `usePersonByName(fullName)`

1. Per creare un hook `useEffect` che richiama la query persistente `my-project/all-teams` tramite `fetchPersistedQuery(..)`, aggiungere il codice seguente. L&#39;hook restituisce inoltre solo i dati rilevanti della risposta di AEM GraphQL in `data?.teamList?.items`, consentendo ai componenti della visualizzazione React di essere agnostici delle strutture JSON padre.

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
1. Nel componente React `Person`, analizzare il parametro di route `fullName` e recuperare i dati della persona da AEM utilizzando l&#39;hook `usePersonByName(fullName)`.

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


## Configurazione CORS

Questa app React di esempio non richiede la condivisione CORS (Cross-origin resource sharing) durante lo sviluppo, perché si connette ad AEM utilizzando un proxy locale. L’utilizzo di proxy locali è solo per facilitare lo sviluppo rapido e non è destinato a casi di utilizzo non di sviluppo.

Tuttavia, negli scenari di produzione, in genere è consigliabile che l’applicazione client comunichi direttamente con AEM dal browser dell’utente. Per abilitare questa funzione, potrebbe essere necessario configurare [CORS in AEM](../deployment/overview.md) per consentire le richieste di recupero dall&#39;app Web.

## Prova l’app

Rivedi l&#39;app [http://localhost:3000/](http://localhost:3000/) e fai clic sui collegamenti _Membri_. Inoltre, puoi aggiungere più team e/o membri al Team Alpha aggiungendo Frammenti di contenuto in AEM.

>[!IMPORTANT]
>
>Per verificare le modifiche apportate all&#39;implementazione o se non riesci a far funzionare l&#39;app dopo le modifiche di cui sopra, fai riferimento al ramo [basic-tutorial](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) `solution`.

## Sotto il cappuccio

Apri **Strumenti per sviluppatori** > **Rete** e _Filtro_ del browser per la richiesta `all-teams`. Si noti che la richiesta API di GraphQL `/graphql/execute.json/my-project/all-teams` viene effettuata per `http://localhost:3000` e **NOT** rispetto al valore di `REACT_APP_HOST_URI`, ad esempio `<https://publish-pxxx-exxx.adobeaemcloud.com`. Le richieste vengono effettuate sul dominio dell&#39;app React perché [la configurazione proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) è abilitata utilizzando il modulo `http-proxy-middleware`.


![Richiesta API GraphQL tramite Proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Rivedi il file `../setupProxy.js` principale e nei file `../proxy/setupProxy.auth.**.js` osserva come `/content` e `/graphql` percorsi sono proxy e indica che non si tratta di una risorsa statica.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

L&#39;utilizzo del proxy locale non è un&#39;opzione adatta per la distribuzione di produzione. Ulteriori dettagli sono disponibili nella sezione _Distribuzione di produzione_.

## Congratulazioni.{#congratulations}

Congratulazioni. Hai creato correttamente l’app React per utilizzare e visualizzare dati dalle API GraphQL di AEM come parte di un’esercitazione di base.
