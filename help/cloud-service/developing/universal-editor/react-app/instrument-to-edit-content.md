---
title: App Instrument React per modificare i contenuti tramite Editor universale
description: Scopri come dotare l’app React per modificare il contenuto utilizzando l’editor universale.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 421
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 2a25cd44-cbd1-465e-ae3f-d3876e915114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 0%

---

# App Instrument React per modificare i contenuti tramite Editor universale

Scopri come dotare l’app React per modificare il contenuto utilizzando l’editor universale.

## Prerequisiti

Hai configurato l’ambiente di sviluppo locale come descritto nella precedente [Configurazione sviluppo locale](./local-development-setup.md) passaggio.

## Includi la libreria principale di Universal Editor

Iniziamo includendo la libreria principale di Universal Editor nell’app WKND Teams React. Si tratta di una libreria JavaScript che fornisce il livello di comunicazione tra l’app modificata e l’editor universale.

Esistono due modi per includere la libreria principale di Universal Editor nell’app React:

1. Dipendenza del modulo nodo dal registro npm, vedere [@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors).
1. Tag script (`<script>`) all&#39;interno del file HTML.

Per questa esercitazione, utilizziamo l’approccio tag Script.

1. Installare `react-helmet-async` pacchetto per gestire `<script>` nell’app React.

   ```bash
   $ npm install react-helmet-async
   ```

1. Aggiornare il `src/App.js` file dell’app WKND Teams React che include la libreria principale di Universal Editor.

   ```javascript
   ...
   import { Helmet, HelmetProvider } from "react-helmet-async";
   
   function App() {
   return (
       <HelmetProvider>
           <div className="App">
               <Helmet>
                   {/* AEM Universal Editor :: CORE Library
                     Loads the LATEST Universal Editor library
                   */}
                   <script
                       src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                       async
                   />
               </Helmet>
               <Router>
                   <header>
                       <Link to={"/"}>
                       <img src={logo} className="logo" alt="WKND Logo" />
                       </Link>
                       <hr />
                   </header>
                   <Routes>
                       <Route path="/" element={<Home />} />
                       <Route path="/person/:fullName" element={<Person />} />
                   </Routes>
               </Router>
           </div>
       </HelmetProvider>
   );
   }
   
   export default App;
   ```

## Aggiungi metadati - origine contenuto

Per collegare l’app WKND Teams React _con l’origine di contenuto_ per la modifica, devi fornire i metadati della connessione. Il servizio Universal Editor utilizza questi metadati per stabilire una connessione con l&#39;origine di contenuto.

I metadati della connessione vengono memorizzati come `<meta>` nel file HTML. La sintassi per i metadati della connessione è la seguente:

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

Aggiungiamo i metadati della connessione all’app WKND Teams React all’interno di `<Helmet>` componente. Aggiornare il `src/App.js` file con i seguenti elementi `<meta>` tag. In questo esempio, l’origine di contenuto è un’istanza AEM locale in esecuzione su `https://localhost:8443`.

```javascript
...
function App() {
return (
    <HelmetProvider>
        <div className="App">
            <Helmet>
                {/* AEM Universal Editor :: CORE Library
                    Loads the LATEST Universal Editor library
                */}
                <script
                    src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                    async
                />
                {/* AEM Universal Editor :: Connection metadata 
                    Connects to local AEM instance
                */}
                <meta
                    name="urn:adobe:aue:system:aemconnection"
                    content={`aem:https://localhost:8443`}
                />
            </Helmet>
            ...
    </HelmetProvider>
);
}

export default App;
```

Il `aemconnection` fornisce un nome breve dell&#39;origine di contenuto. La strumentazione successiva utilizza il nome breve per fare riferimento all’origine di contenuto.

## Aggiungi metadati - configurazione del servizio Universal Editor locale

Al posto del servizio Universal Editor ospitato dagli Adobi, viene utilizzata una copia locale del servizio Universal Editor per lo sviluppo locale. Il servizio locale associa l’editor universale e l’SDK dell’AEM; pertanto, aggiungiamo i metadati del servizio dell’editor universale locale all’app WKND Teams React.

Queste impostazioni di configurazione vengono memorizzate anche come `<meta>` nel file HTML. La sintassi per i metadati del servizio Universal Editor locale è la seguente:

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

Aggiungiamo i metadati della connessione all’app WKND Teams React all’interno di `<Helmet>` componente. Aggiornare il `src/App.js` file con i seguenti elementi `<meta>` tag. In questo esempio, il servizio Universal Editor locale è in esecuzione su `https://localhost:8001`.

```javascript
...

function App() {
  return (
    <HelmetProvider>
      <div className="App">
        <Helmet>
          {/* AEM Universal Editor :: CORE Library
              Loads the LATEST Universal Editor library
          */}
          <script
            src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
            async
          />
          {/* AEM Universal Editor :: Connection metadata 
              Connects to local AEM instance
          */}
          <meta
            name="urn:adobe:aue:system:aemconnection"
            content={`aem:https://localhost:8443`}
          />
          {/* AEM Universal Editor :: Configuration for Service
              Using locally running Universal Editor service
          */}
          <meta
            name="urn:adobe:aue:config:service"
            content={`https://localhost:8001`}
          />
        </Helmet>
        ...
    </HelmetProvider>
);
}
export default App;
```

## Strumentazione dei componenti React

Per modificare i contenuti dell’app WKND Teams React, ad esempio _titolo del team e descrizione del team_, è necessario dotare di strumenti i componenti React. La strumentazione comporta l&#39;aggiunta di attributi di dati rilevanti (`data-aue-*`) agli elementi HTML che si desidera rendere modificabili mediante l&#39;Editor universale. Per ulteriori informazioni sugli attributi dei dati, consulta [Attributi e tipi](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types).

### Definire gli elementi modificabili

Iniziamo definendo gli elementi che desideri modificare utilizzando l’Editor universale. Nell’app WKND Teams React, il titolo e la descrizione del team sono memorizzati nel frammento di contenuto del team in AEM, che rappresenta quindi i candidati migliori per la modifica.

Diamo uno strumento al `Teams` Componente React per rendere modificabili il titolo e la descrizione del team.

1. Apri `src/components/Teams.js` file dell’app WKND Teams React.
1. Aggiungi il `data-aue-prop`, `data-aue-type` e `data-aue-label` attribuire agli elementi del titolo del team e della descrizione.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       return (
           <div className="team">
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <h2 className="team__title" data-aue-prop="title" data-aue-type="text" data-aue-label="title">{title}</h2>
               <p className="team__description" data-aue-prop="description" data-aue-type="richtext" data-aue-label="description">{description.plaintext}</p>
               ...
           </div>
       );
   }
   
   export default Teams;
   ```

1. Aggiorna la pagina Editor universale nel browser che carica l’app WKND Teams React. Ora puoi vedere che gli elementi titolo e descrizione del team sono modificabili.

   ![Editor universale - Titolo e descrizione team WKND modificabili](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. Se tenti di modificare il titolo o la descrizione del team utilizzando la modifica in linea o la barra delle proprietà, viene visualizzato un pulsante di caricamento che non consente di modificare il contenuto. Perché Universal Editor non è a conoscenza dei dettagli delle risorse AEM per il caricamento e il salvataggio del contenuto.

   ![Editor universale - Caricamento titolo e descrizione team WKND](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

In sintesi, le modifiche di cui sopra contrassegnano gli elementi titolo e descrizione del team come modificabili nell’Editor universale. Tuttavia, **non è possibile modificare (tramite la barra in linea o proprietà) e salvare ancora le modifiche**, è necessario aggiungere i dettagli della risorsa AEM utilizzando `data-aue-resource` attributo. Facciamolo nel passaggio successivo.

### Definire i dettagli delle risorse AEM

Per salvare nuovamente in AEM il contenuto modificato e caricarlo nella barra delle proprietà, è necessario fornire i dettagli della risorsa AEM all’editor universale.

In questo caso, la risorsa AEM è il percorso del frammento di contenuto del team, quindi aggiungiamo i dettagli della risorsa al `Teams` Componente React al livello superiore `<div>` elemento.

1. Aggiornare il `src/components/Teams.js` file per aggiungere `data-aue-resource`, `data-aue-type` e `data-aue-label` attributi al livello superiore `<div>` elemento.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       // Render single Team
       function Team({ _path, title, shortName, description, teamMembers }) {
           // Must have title, shortName and at least 1 team member
           if (!_path || !title || !shortName || !teamMembers) {
               return null;
           }
   
         return (
           // AEM Universal Editor :: Instrumentation using data-aue-* attributes
           <div className="team" data-aue-resource={`urn:aemconnection:${_path}/jcr:content/data/master`} data-aue-type="reference" data-aue-label={title}>
           ...
           </div>
       );
       }
   }
   export default Teams;
   ```

   Il valore della proprietà `data-aue-resource` è il percorso della risorsa AEM del frammento di contenuto del team. Il `urn:aemconnection:` prefisso utilizza il nome breve dell&#39;origine di contenuto definita nei metadati della connessione.

1. Aggiorna la pagina Editor universale nel browser che carica l’app WKND Teams React. Ora puoi vedere che l’elemento Team di livello superiore è modificabile, ma la barra delle proprietà non carica ancora il contenuto. Nella scheda di rete del browser, è possibile visualizzare l’errore 401 Unauthorized per il `details` richiesta che carica il contenuto. Sta tentando di utilizzare il token IMS per l’autenticazione, ma l’SDK AEM locale non supporta l’autenticazione IMS.

   ![Editor universale - Team WKND modificabile](./assets/universal-editor-wknd-teams-team-editable.png)

1. Per correggere l’errore 401 Unauthorized, devi fornire i dettagli di autenticazione SDK AEM locale all’editor universale utilizzando **Intestazioni di autenticazione** nell&#39;editor universale. In qualità di SDK AEM locale, imposta il valore su `Basic YWRtaW46YWRtaW4=` per `admin:admin` credenziali.

   ![Editor universale - Aggiungere intestazioni di autenticazione](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. Aggiorna la pagina Editor universale nel browser che carica l’app WKND Teams React. Ora puoi vedere che la barra delle proprietà sta caricando il contenuto e modificare il titolo e la descrizione del team in linea o utilizzando la barra delle proprietà.

   ![Editor universale - Team WKND modificabile](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### Sotto il cofano

La barra delle proprietà carica il contenuto dalla risorsa AEM utilizzando il servizio Universal Editor locale. Utilizzando la scheda di rete del browser, è possibile visualizzare la richiesta POST al servizio Universal Editor locale (`https://localhost:8001/details`) per il caricamento del contenuto.

Quando si modifica il contenuto utilizzando la barra delle modifiche in linea o delle proprietà, le modifiche vengono salvate nuovamente nella risorsa AEM utilizzando il servizio Universal Editor locale. Utilizzando la scheda di rete del browser, è possibile visualizzare la richiesta POST al servizio Universal Editor locale (`https://localhost:8001/update` o `https://localhost:8001/patch`) per il salvataggio del contenuto.

![Editor universale - Team WKND modificabile](./assets/universal-editor-under-the-hood-request.png)

L’oggetto JSON del payload della richiesta contiene i dettagli necessari, come il server dei contenuti (`connections`), percorso risorsa (`target`) e il contenuto aggiornato (`patch`).

![Editor universale - Team WKND modificabile](./assets/universal-editor-under-the-hood-payload.png)

### Espandere il contenuto modificabile

Espandiamo il contenuto modificabile e applichiamo la strumentazione al **membri del gruppo** in modo da poter modificare i membri del gruppo utilizzando la barra delle proprietà.

Come sopra, aggiungiamo le relative `data-aue-*` attributi ai membri del team in `Teams` Componente React.

1. Aggiornare il `src/components/Teams.js` file per aggiungere attributi di dati al `<li key={index} className="team__member">` elemento.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {/* Render the referenced Person models associated with the team */}
               {teamMembers.map((teamMember, index) => {
                   return (
                       // AEM Universal Editor :: Instrumentation using data-aue-* attributes
                       <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                       <Link to={`/person/${teamMember.fullName}`}>
                           {teamMember.fullName}
                       </Link>
                       </li>
                   );
               })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

   Il valore della proprietà `data-aue-type` l&#39;attributo è `component` come i membri del gruppo sono memorizzati come `Person` Frammenti di contenuto nell’AEM e aiuta a indicare le parti mobili/eliminabili del contenuto.

1. Aggiorna la pagina Editor universale nel browser che carica l’app WKND Teams React. Ora puoi vedere che i membri del gruppo sono modificabili utilizzando la barra delle proprietà.

   ![Editor universale - Membri del team WKND modificabili](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### Sotto il cofano

Come sopra, il recupero e il salvataggio dei contenuti vengono eseguiti dal servizio Universal Editor locale. Il `/details`, `/update` o `/patch` Le richieste di caricamento e salvataggio dei contenuti vengono inviate al servizio Universal Editor locale.

### Definire l’aggiunta e l’eliminazione di contenuti

Finora hai reso modificabile il contenuto esistente, ma cosa succede se desideri aggiungere nuovo contenuto? Aggiungiamo la possibilità di aggiungere o eliminare membri del gruppo al team WKND utilizzando l’editor universale. Pertanto, gli autori dei contenuti non devono necessariamente rivolgersi all’AEM per aggiungere o eliminare membri del gruppo.

Tuttavia, per ricapitolare rapidamente, i membri del team WKND vengono memorizzati come `Person` Frammenti di contenuto nell’AEM e sono associati al Frammento di contenuto del team utilizzando `teamMembers` proprietà. Per rivedere la definizione del modello in AEM visita [my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project).

1. Creare innanzitutto il file di definizione del componente `/public/static/component-definition.json`. Questo file contiene la definizione del componente per `Person` Frammento di contenuto. Il `aem/cf` il plug-in consente di inserire frammenti di contenuto, in base a un modello e a un modello che forniscono i valori predefiniti da applicare.

   ```json
   {
       "groups": [
           {
           "title": "Content Fragments",
           "id": "content-fragments",
           "components": [
               {
               "title": "Person",
               "id": "person",
               "plugins": {
                   "aem": {
                       "cf": {
                           "name": "person",
                           "cfModel": "/conf/my-project/settings/dam/cfm/models/person",
                           "cfFolder": "/content/dam/my-project/en",
                           "title": "person",
                           "template": {
                               "fullName": "New Person",
                               "biographyText": "This is biography of new person"
                               }
                           }
                       }
                   }
               }
           ]
           }
       ]
   }
   ```

1. Quindi, fai riferimento al file di definizione del componente in `index.html` dell’app WKND Team React. Aggiornare il `public/index.html` del file `<head>` per includere il file di definizione del componente.

   ```html
   ...
   <script
       type="application/vnd.adobe.aue.component+json"
       src="/static/component-definition.json"
   ></script>
   <title>WKND App - Basic GraphQL Tutorial</title>
   </head>
   ...
   ```

1. Infine, aggiorna il `src/components/Teams.js` per aggiungere attributi di dati. Il **MEMBRI** per fungere da contenitore per i membri del team, aggiungiamo i `data-aue-prop`, `data-aue-type`, e `data-aue-label` attributi di `<div>` elemento.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       {/* AEM Universal Editor :: Team Members as container */}
       <div data-aue-prop="teamMembers" data-aue-type="container" data-aue-label="members">
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
           {/* Render the referenced Person models associated with the team */}
           {teamMembers.map((teamMember, index) => {
               return (
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                   <Link to={`/person/${teamMember.fullName}`}>
                   {teamMember.fullName}
                   </Link>
               </li>
               );
           })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

1. Aggiorna la pagina Editor universale nel browser che carica l’app WKND Teams React. È ora possibile vedere che **MEMBRI** funge da contenitore. È possibile inserire nuovi membri del gruppo utilizzando la barra delle proprietà e **+** icona.

   ![Editor universale - Inserimento membri team WKND](./assets/universal-editor-wknd-teams-add-team-members.png)

1. Per eliminare un membro del team, selezionarlo e fare clic sul pulsante **Elimina** icona.

   ![Editor universale - Eliminazione membri team WKND](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### Sotto il cofano

Le operazioni di aggiunta ed eliminazione dei contenuti vengono eseguite dal servizio Universal Editor locale. La richiesta POST a `/add` o `/remove` con un payload dettagliato, vengono aggiunti al servizio Universal Editor locale per aggiungere o eliminare contenuti all’AEM.

## File di soluzione

Per verificare le modifiche apportate all’implementazione o se non riesci a utilizzare l’app WKND Teams React con Universal Editor, consulta [basic-tutorial-instrumented-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) ramo della soluzione.

Confronto file per file con il **esercitazione di base** filiale disponibile [qui](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1).

## Complimenti

Hai instrumentato correttamente l’app WKND Teams React per aggiungere, modificare ed eliminare contenuti tramite l’editor universale. Hai imparato a includere la libreria principale, ad aggiungere la connessione e i metadati del servizio Universal Editor locale, e a utilizzare il componente React utilizzando vari dati (`data-aue-*`).
