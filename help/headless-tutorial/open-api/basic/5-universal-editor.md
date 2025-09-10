---
title: Modificare l’app React con l’editor universale | Tutorial di headless - Parte 5
description: Scopri come rendere modificabile l’app React in AEM Universal Editor aggiungendo la strumentazione e la configurazione necessarie.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 800
source-git-commit: da3bfa25a424e3176fb7d53189169515db225228
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 2%

---


# Modificare l’app React con Universal Editor

In questo capitolo imparerai a rendere modificabile l&#39;app React integrata nel [capitolo precedente](./4-react-app.md) tramite AEM Universal Editor. Universal Editor consente agli autori di contenuti di modificare i contenuti direttamente nel contesto dell’esperienza dell’app React, mantenendo al contempo l’esperienza fluida di un’applicazione headless.

![Editor universale](./assets/5/main.png)

Universal Editor offre un modo efficace per abilitare la modifica contestuale per qualsiasi applicazione Web, consentendo agli autori di modificare il contenuto senza passare da un’interfaccia di authoring all’altra.

## Prerequisiti

* I passaggi precedenti di questa esercitazione sono stati completati, in particolare [Creare un&#39;app React che utilizza le OpenAPI di distribuzione dei frammenti di contenuto di AEM](./4-react-app.md)
* Conoscenza operativa di [come utilizzare e implementare Universal Editor](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/introduction).

## Obiettivi

Scopri come:

* Aggiungi la strumentazione Universal Editor all’app React.
* Configura l’app React per l’editor universale.
* Consente di modificare i contenuti direttamente nell’interfaccia dell’app React utilizzando l’editor universale.

## Strumentazione editor universale

Universal Editor richiede [attributi HTML e metatag](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types) per identificare il contenuto modificabile e stabilire la connessione tra l&#39;interfaccia utente e il contenuto AEM.

### Aggiungere tag dell’editor universale

Innanzitutto, aggiungi i metatag necessari per identificare l’app React come compatibile con Universal Editor.

1. Apri `public/index.html` nella tua app React.
1. Aggiungi i metatag [Universal Editor e lo script CORS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/getting-started) nella sezione `<head>` dell&#39;app React:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="utf-8" />
       <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
       <meta name="viewport" content="width=device-width, initial-scale=1" />
       <meta name="theme-color" content="#000000" />
       <meta name="description" content="WKND Teams React App" />
   
       <!-- Universal Editor meta tags and CORS script -->
       <meta name="urn:adobe:aue:system:aemconnection" content="aem:%REACT_APP_AEM_AUTHOR_HOST_URI%" />
       <script src="https://universal-editor-service.adobe.io/cors.js"></script>
   
       <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
       <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
       <title>WKND Teams</title>
   </head>
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="root"></div>
   </body>
   </html>
   ```

1. Aggiornare il file `.env` dell&#39;app React in modo da includere l&#39;host del servizio AEM Author per supportare write-back in Universal Editor (utilizzato nel valore del tag metat `urn:adobe:aue:system:aemconnection`).

   ```bash
   # The AEM Publish (or Preview) service
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   # The AEM Author service
   REACT_APP_AEM_AUTHOR_HOST_URI=https://author-p123-e456.adobeaemcloud.com
   ```

### Strumentazione del componente team

Ora aggiungi gli attributi dell’Editor universale per rendere modificabile il componente Team.

1. Apri `src/components/Teams.js`.
1. Aggiornare il componente `Team` per includere [gli attributi dei dati dell&#39;editor universale](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types):

   Quando imposti l’attributo `data-aue-resource`, accertati che il percorso AEM del frammento di contenuto restituito da AEM Content Fragment Delivery con API OpenAPI sia postfissato al percorso secondario della variante del frammento di contenuto; in questo caso `/jcr:content/data/master`.

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
               {...team}
           />
           );
       })}
       </div>
   );
   }
   
   /**
   * Team - renders a single team with its details and members
   * @param {Object} fields - The authored Content Fragment fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   * @param {string} path - Path of the team content fragment
   */
   function Team({ fields, references, path }) {
   if (!fields.title || !fields.teamMembers) {
       return null;
   }
   
   return (
       <>
       {/* Specify the correct Content Fragment variation path suffix in the data-aue-resource attribute */}
       <div className="team"
           data-aue-resource={`urn:aemconnection:${path}/jcr:content/data/master`}
           data-aue-type="component"
           data-aue-label={fields.title}>
   
           <h2 className="team__title"
           data-aue-prop="title"
           data-aue-type="text"
           data-aue-label="Team Title">{fields.title}</h2>
           <p className="team__description"
           data-aue-prop="description"
           data-aue-type="richtext"
           data-aue-label="Team Description"
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
           />
           <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {fields.teamMembers.map((teamMember, index) => {
               return (
                   <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                       {references[teamMember].value.fields.fullName}
                   </Link>
                   </li>
               );
               })}
           </ul>
           </div>
       </div>
       </>
   );
   }
   
   export default Teams;
   ```

### Strumento del componente persona

Allo stesso modo, aggiungi gli attributi dell’Editor universale al componente Persona.

1. Apri `src/components/Person.js`.
1. Aggiorna il componente per includere [attributi dati dell&#39;editor universale](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types):

   Quando imposti l’attributo `data-aue-resource`, accertati che il percorso AEM del frammento di contenuto restituito da AEM Content Fragment Delivery con API OpenAPI sia postfissato al percorso secondario della variante del frammento di contenuto; in questo caso `/jcr:content/data/master`.

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
       const { id } = useParams();
       const [person, setPerson] = useState(null);
   
       useEffect(() => {
           const fetchData = async () => {
           try {
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
       }, [id]);
   
       if (!person) {
           return <div>Loading person...</div>;
       }
   
       /* Add the Universal Editor data-aue-* attirbutes to the rendered HTML */
       return (
           <div className="person"
               data-aue-resource={`urn:aemconnection:${person.path}/jcr:content/data/master`}
               data-aue-type="component"
               data-aue-label={person.fields.fullName}>
               <img className="person__image"
                   src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
                   alt={person.fields.fullName}
                   data-aue-prop="profilePicture"
                   data-aue-type="media"
                   data-aue-label="Profile Picture"
               />
               <div className="person__occupations">
                   {person.fields.occupation.map((occupation, index) => {
                   return (
                       <span key={index} className="person__occupation">
                           {occupation}
                       </span>
                   );
                   })}
               </div>
   
               <div className="person__content">
                   <h1 className="person__full-name"
                       data-aue-prop="fullName"
                       data-aue-type="text"
                       data-aue-label="Full Name">
                       {person.fields.fullName}
                   </h1>
                   <div className="person__biography"
                       data-aue-prop="biographyText"
                       data-aue-type="richtext"
                       data-aue-label="Biography"
                       dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
                   />
               </div>
           </div>
       );
   }
   ```

### Ottieni il codice completato

Il codice sorgente completo per questo capitolo è [disponibile su Github.com](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end).


```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_5-end
```

## Test dell’integrazione con Universal Editor

Ora testa gli aggiornamenti della compatibilità con Universal Editor aprendo l’app React in Universal Editor.

### Avviare l’app React

1. Assicurati che l’app React sia in esecuzione:

   ```bash
   $ cd ~/Code/aem-guides-wknd-openapi/basic-tutorial
   $ npm install
   $ npm start
   ```

1. Verificare che l&#39;app si carichi in `http://localhost:3000` e visualizzi il contenuto dei team e delle persone.

### Esegui proxy SSL locale

Universal Editor richiede che l&#39;applicazione modificabile venga caricata su HTTPS.

1. Per eseguire l&#39;app React locale su HTTPS, utilizzare il modulo [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy) npm dalla riga di comando.

   ```bash
   $ npm install -g local-ssl-proxy
   $ local-ssl-proxy --source 3001 --target 3000
   ```

1. Apri `https://localhost:3001` nel browser Web
1. Accettare il certificato autofirmato.
1. Verifica il caricamento dell’app React.

### Apri in Editor universale

![Apri app in Universal Editor](./assets/5/open-app-in-universal-editor.png)

1. Passa a [Editor universale](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/).
1. Nel campo **URL sito**, immettere l&#39;URL dell&#39;app React HTTPS: `https://localhost:3001`.
1. Seleziona Fai Clic Su **Apri**.

L’editor universale deve caricare l’app React con le funzionalità di modifica abilitate.

### Verifica funzionalità di modifica

![Modifica in Universal Editor](./assets/5/edit-in-universal-editor.png)

1. Nell’editor universale, passa il cursore del mouse sugli elementi modificabili nell’app React.

1. Per spostarti all&#39;interno dell&#39;app React, attiva e disattiva di nuovo la modalità **Anteprima** per modificarla. Ricorda che **Anteprima** non ha nulla a che fare con il servizio Anteprima di AEM, ma attiva e disattiva la funzione di modifica di Chrome in Universal Editor.

1. Dovresti vedere come modificare gli indicatori ed essere in grado di fare clic sui vari elementi modificabili dell’app React.

1. Prova a modificare il titolo di un team:
   * Fai clic sul titolo di un team
   * Modificare il testo nel pannello delle proprietà
   * Salva le modifiche

1. Prova a modificare l’immagine del profilo di una persona:
   * Fai clic sull’immagine del profilo di una persona
   * Seleziona una nuova immagine dal selettore risorse
   * Salva le modifiche

1. Premi **Pubblica** in alto a destra nell&#39;Editor universale per pubblicare le modifiche sul servizio di pubblicazione (o anteprima) di AEM, in modo che vengano applicate nell&#39;app React nell&#39;Editor universale.

## Attributi dei dati dell’Editor universale

Per la documentazione completa sulla strumentazione di un&#39;applicazione per Universal Editor, fare riferimento alla [documentazione di Universal Editor](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/).

## Congratulazioni.

Congratulazioni Hai integrato correttamente Universal Editor con la tua app React. Gli autori dei contenuti ora possono modificare i frammenti di contenuto direttamente nell’interfaccia dell’app React, fornendo un’esperienza di authoring fluida mantenendo al contempo i vantaggi di un’architettura headless.

Ricorda che è sempre possibile ottenere il codice sorgente finale per questa esercitazione dal ramo `main` dell&#39;archivio [GitHub.com](https://github.com/adobe/aem-tutorials/tree/main).
