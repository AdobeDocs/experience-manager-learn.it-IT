---
title: Creare un’app React con AEM OpenAPI | Tutorial sulla parte 4
description: Introduzione a Adobe Experience Manager (AEM) e OpenAPI. Crea un’app React che recupera contenuti/dati dalle API di distribuzione di frammenti di contenuto basate su OpenAPI di AEM.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 900
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 1%

---


# Creare un’app React con le API OpenAPI di distribuzione dei frammenti di contenuto di AEM

In questo capitolo, scopri come la distribuzione di frammenti di contenuto di AEM con API OpenAPI può stimolare l’esperienza in applicazioni esterne.

Viene utilizzata una semplice app React per richiedere e visualizzare il contenuto di **Team** e **Persona** esposto da AEM Content Fragment Delivery con API OpenAPI. L’utilizzo di React è in gran parte irrilevante e l’applicazione esterna che ne usufruisce può essere scritta in qualsiasi framework per qualsiasi piattaforma, purché possa effettuare richieste HTTP ad AEM as a Cloud Service.

## Prerequisiti

Si presume che i passaggi descritti nelle parti precedenti di questo tutorial in più parti siano stati completati.

È necessario installare il seguente software:

* [Node.js v22+](https://nodejs.org/en)
* [Codice di Visual Studio](https://code.visualstudio.com/)

## Obiettivi

Scopri come:

* Scarica e avvia l’app React di esempio.
* Richiama AEM Content Fragment Delivery con API OpenAPI per un elenco di team e dei relativi membri di riferimento.
* Richiama Consegna di frammenti di contenuto di AEM con API OpenAPI per recuperare i dettagli di un membro del team.

## Configurare CORS su AEM as a Cloud Service

Questo esempio di app React viene eseguita localmente (su `http://localhost:3000`) e si connette alla distribuzione di frammenti di contenuto AEM del servizio di pubblicazione di AEM con API OpenAPI. Per consentire questa connessione, è necessario configurare CORS (Cross-Origin Resource Sharing) nel servizio AEM Publish (o Anteprima).

Segui le [istruzioni sull&#39;impostazione di un&#39;applicazione a pagina singola in esecuzione su `http://localhost:3000` per consentire le richieste CORS al servizio di pubblicazione AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#different-domains).

### Proxy CORS locale

In alternativa, per lo sviluppo, esegui un [proxy CORS locale](https://www.npmjs.com/package/local-cors-proxy) che consente una connessione compatibile con CORS ad AEM.

```bash
$ npm install --global lcp
$ lcp --proxyUrl https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com
```

Aggiorna il valore `--proxyUrl` nell&#39;URL di pubblicazione (o anteprima) di AEM.

Con il proxy CORS locale in esecuzione, accedi alle API di distribuzione dei frammenti di contenuto di AEM all&#39;indirizzo `http://localhost:8010/proxy` per evitare problemi CORS.

## Clonare l’app React di esempio

Un esempio di app React sottoposta a stubbed-out viene implementata con il codice necessario per interagire con AEM Content Fragment Delivery (Distribuzione di frammenti di contenuto) con le API OpenAPI e visualizzare i dati di team e persone ottenuti da queste.

Il codice sorgente dell&#39;app React di esempio è [disponibile su Github.com](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic).

Per ottenere l’app React:

1. Clona l&#39;app WKND OpenAPI React di esempio da [Github.com](https://github.com/adobe/aem-tutorials) dal tag [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-tutorials.git
   $ cd aem-tutorials  
   $ git fetch --tags
   $ git tag
   $ git checkout tags/headless_open-api_basic
   ```

1. Passare alla cartella `headless/open-api/basic` e aprirla nell&#39;IDE.

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ code .
   ```

1. Aggiorna `.env` per connetterti al servizio AEM as a Cloud Service Publish, dove vengono pubblicati i frammenti di contenuto. Questo può indicare il servizio di anteprima di AEM se desideri testare l’app con il servizio di anteprima di AEM (e i frammenti di contenuto vengono pubblicati lì).

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   ```

   Quando si utilizza [Local CORS Proxy](#local-cors-proxy), impostare `REACT_APP_HOST_URI` su `http://localhost:8010/proxy`.

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=http://localhost:8010/proxy
   ```

1. Avviare l’app React

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ npm install
   $ npm start
   ```

1. L&#39;app React viene avviata in modalità di sviluppo il [http://localhost:3000/](http://localhost:3000/). Le modifiche apportate all’app React durante l’esercitazione vengono immediatamente riportate nel browser web.

>[!IMPORTANT]
>
>   Questa app React è parzialmente implementata. Segui i passaggi descritti in questo tutorial per completare l’implementazione. I file JavaScript che richiedono un lavoro di implementazione presentano il seguente commento. Accertati di aggiungere/aggiornare il codice in tali file con il codice specificato in questa esercitazione.
>
>
>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>  &#x200B;>  // TODO: implementalo seguendo i passaggi dell’esercitazione AEM Headless
>  &#x200B;>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>

## Anatomia dell’app React

L’app React di esempio ha tre parti principali che richiedono l’aggiornamento.

1. Il file `.env` contiene l&#39;URL del servizio di pubblicazione (o anteprima) di AEM.
1. `src/components/Teams.js` visualizza un elenco di team e dei relativi membri.
1. `src/components/Person.js` visualizza i dettagli di un singolo membro del team.

## Implementare le funzionalità dei team

Crea la funzionalità per visualizzare i team e i relativi membri nella visualizzazione principale dell’app React. Questa funzionalità richiede:

* Nuovo [hook useEffect personalizzato di React](https://react.dev/reference/react/useEffect#useeffect) che richiama l&#39;**API List all Content Fragments** tramite una richiesta di recupero e quindi ottiene il valore `fullName` per ogni `teamMember` per la visualizzazione.

Una volta completata, la vista principale dell’app si popola con i dati dei team provenienti da AEM.

![Visualizzazione team](./assets/4/teams.png)

1. Apri `src/components/Teams.js`.

1. Implementa il componente **Team** per recuperare l&#39;elenco dei team da [Elenca tutte le API dei frammenti di contenuto](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragments) ed eseguire il rendering del contenuto dei team. Questa procedura è suddivisa nei seguenti passaggi:

1. Crea un hook `useEffect` che richiama l&#39;API **List all Content Fragments** di AEM e memorizza i dati nello stato del componente React.
1. Per ogni frammento di contenuto **Team** restituito, richiama l&#39;API **Get a Content Fragment** per recuperare i dettagli completi del team, inclusi i membri e i relativi `fullNames`.
1. Eseguire il rendering dei dati dei team utilizzando la funzione `Team`.

   ```javascript
   import { useEffect, useState } from "react";
   import { Link } from "react-router-dom";
   import "./Teams.scss";
   
   function Teams() {
   
     // The teams folder is the only folder-tree that is allowed to contain Team Content Fragments.
     const TEAMS_FOLDER = '/content/dam/my-project/en/teams';
   
     // State to store the teams data
     const [teams, setTeams] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches all teams and their associated member details
       * This is a two-step process:
       * 1. First, get all team content fragments from the specified folder
       * 2. Then, for each team, fetch the full details including hydrated references to get the team member names
       */
       const fetchData = async () => {
         try {
           // Step 1: Fetch all teams from the teams folder
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments?path=${TEAMS_FOLDER}`
           );
           const allTeams = (await response.json()).items || [];
   
           // Step 2: Fetch detailed information for each team with hydrated references
           const hydratedTeams = [];
           for (const team of allTeams) {
             const hydratedTeamResponse = await fetch(
               `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${team.id}?references=direct-hydrated`
             );
             hydratedTeams.push(await hydratedTeamResponse.json());
           }
   
           setTeams(hydratedTeams);
         } catch (error) {
           console.error("Error fetching content fragments:", error);
         }
       };
   
       fetchData();
     }, [TEAMS_FOLDER]);
   
     // Show loading state while teams data is being fetched
     if (!teams) {
       return <div>Loading teams...</div>;
     }
   
     // Render the teams
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return (
             <Team 
               key={index} 
               {..team}
             />
           );
         })}
       </div>
     );
   }
   
   /**
   * Team component - renders a single team with its details and members
   * @param {string} fields - The authorable fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   */
   function Team({ fields, references, path }) {
     if (!fields.title || !fields.teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{fields.title}</h2>
         {/* Render description as HTML using dangerouslySetInnerHTML */}
         <p 
           className="team__description" 
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
         />
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render each team member as a link to their detail page */}
             {fields.teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                     {/* Display the full name from the hydrated reference */}
                     {references[teamMember].value.fields.fullName}
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

## Implementare la funzionalità della persona

Quando la funzionalità [Team](#implement-teams-functionality) è stata completata, implementare la funzionalità per gestire la visualizzazione dei dettagli di un membro del team o di una persona.

![Visualizzazione persona](./assets/4/person.png)

Per effettuare questo collegamento:

1. Apri `src/components/Person.js`
1. Nel componente React `Person`, analizzare il parametro route `id`. Si noti che le route dell&#39;app React sono state precedentemente impostate per accettare il parametro URL `id` (vedere `/src/App.js`).
1. Recupera i dati della persona da AEM con l&#39;API [Get Content Fragment](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragment).

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
     // Get the person ID from the URL parameter
     const { id } = useParams();
   
     // State to store the person data
     const [person, setPerson] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches person data from AEM Content Fragment Delivery API
       * Uses the ID from URL parameters to get the specific person's details
       */
       const fetchData = async () => {
         try {
           /* Hydrate references for access to profilePicture asset path */
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${id}?references=direct-hydrated`
           );
           const json = await response.json();
           setPerson(json || null);
         } catch (error) {
           console.error("Error fetching person data:", error);
         }
       };
       fetchData();
     }, [id]); // Re-fetch when ID changes
   
     // Show loading state while person data is being fetched
     if (!person) {
       return <div>Loading person...</div>;
     }
   
     return (
       <div className="person">
         {/* Person profile image - Look up the profilePicture reference in the references object */}
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
           alt={person.fields.fullName}
         />
         {/* Display person's occupations */}
         <div className="person__occupations">
           {person.fields.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
   
         {/* Person's main content: name and biography */}
         <div className="person__content">
           <h1 className="person__full-name">{person.fields.fullName}</h1>
           {/* Render biography as HTML content */}
           <div
             className="person__biography"
             dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
           />
         </div>
       </div>
     );  
   }
   
   export default Person;
   ```

### Ottieni il codice completato

Il codice sorgente completo per questo capitolo è [disponibile su Github.com](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end).

```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_4-end
```

## Prova l’app

Rivedi l&#39;app [http://localhost:3000/](http://localhost:3000/) e fai clic sui collegamenti _Membro del team_. Inoltre, puoi aggiungere più team e/o membri al Team Alpha aggiungendo Frammenti di contenuto nel servizio AEM Author e pubblicandoli.

## Dietro le quinte

Apri la console **Strumenti per sviluppatori > Rete** del browser e **Filtra** per `/adobe/contentFragments` richieste di recupero mentre interagisci con l&#39;app React.

## Congratulazioni.

Congratulazioni Hai creato correttamente un’app React per utilizzare e visualizzare frammenti di contenuto da Distribuzione di frammenti di contenuto di AEM con API OpenAPI.
