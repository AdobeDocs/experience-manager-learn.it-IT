---
title: Componente Web/JS - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Web Component/JS illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 5%

---


# Componente Web

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione componente web illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti ed eseguire il rendering di una parte dell’interfaccia utente, eseguita utilizzando un puro codice JavaScript.

![Componente web con AEM headless](./assets/web-component/web-component.png)

Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (se ci si connette a AEM locale 6.5 o AEM SDK)
+ [Node.js v10+](https://nodejs.org/it/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## Requisiti AEM

Il componente Web funziona con le seguenti opzioni di distribuzione AEM.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configurazione locale tramite [l’SDK di AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it?lang=en#install-local-aem-instances)

Tutte le implementazioni richiedono la `tutorial-solution-content.zip` dal [File della soluzione](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) da installare e da installare [Configurazioni di distribuzione](../deployment/web-component.md) sono eseguite.


>[!IMPORTANT]
>
>Il componente Web è progettato per la connessione a un __Pubblicazione AEM__ ambiente, tuttavia può generare contenuto da AEM Author se l’autenticazione è fornita nel componente Web [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) file.

## Come utilizzare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Passa a `web-component` sottodirectory.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Modifica le `.../src/person.js` per includere i dettagli di connessione AEM:

   In `aemHeadlessService` oggetto, aggiornare `aemHost` per puntare al servizio AEM Publish.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Se ti connetti a un servizio AEM Author, nella `aemCredentials` fornire le credenziali utente AEM locali.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Apri un terminale ed esegui i comandi da `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Viene visualizzata una nuova finestra del browser in cui viene aperta la pagina HTML statica che incorpora il componente Web in [http://localhost:8080](Http://localhost:8080).
1. La _Informazioni sulla persona_ Il componente Web viene visualizzato nella pagina Web.

## Il codice

Di seguito è riportato un riepilogo di come viene generato il componente Web, di come si connette a AEM Headless per recuperare il contenuto utilizzando le query persistenti GraphQL e di come tali dati vengono presentati. Il codice completo è disponibile in [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Tag HTML componente Web

Un componente Web riutilizzabile (aka elemento personalizzato) `<person-info>` viene aggiunto al `../src/assets/aem-headless.html` pagina HTML. Supporta `host` e `query-param-value` per determinare il comportamento del componente. La `host` sostituzioni dei valori dell&#39;attributo `aemHost` valore da `aemHeadlessService` oggetto in `person.js`e `query-param-value` viene utilizzato per selezionare la persona di cui eseguire il rendering.

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementazione di un componente web

La `person.js` definisce la funzionalità del componente Web e di seguito sono riportati i principali elementi di rilievo.

#### Implementazione dell’elemento PersonInfo

La `<person-info>` l’oggetto classe dell’elemento personalizzato definisce la funzionalità utilizzando `connectedCallback()` metodi del ciclo di vita, associazione di una radice shadow, recupero di query persistenti GraphQL e manipolazione DOM per creare la struttura DOM interna dell&#39;elemento personalizzato.

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Registrare `<person-info>` elemento

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Condivisione delle risorse tra le origini (CORS)

Questo componente Web si basa su una configurazione CORS basata su AEM in esecuzione nell’ambiente AEM di destinazione e presuppone che la pagina host sia in esecuzione su `http://localhost:8080` in modalità di sviluppo e di seguito è riportato un esempio di configurazione CORS OSGi per il servizio AEM Author locale.

Consulta [configurazioni di distribuzione](../deployment/web-component.md) per il rispettivo servizio AEM.

![Configurazione CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)