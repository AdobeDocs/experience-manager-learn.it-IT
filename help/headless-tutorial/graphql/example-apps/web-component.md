---
title: Componente web/JS - Esempio di AEM headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Web Component/JS illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell'AEM utilizzando query persistenti.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 6%

---

# Componente Web

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione per componenti web illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti ed eseguire il rendering di una parte dell’interfaccia utente, utilizzando esclusivamente codice JavaScript.

![Componente web con AEM headless](./assets/web-component/web-component.png)

Visualizza [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (se ci si connette a AEM 6.5 locale o a AEM SDK)
+ [Node.js v18](https://nodejs.org/it/)
+ [Git](https://git-scm.com/)

## Requisiti AEM

Il componente Web funziona con le seguenti opzioni di distribuzione AEM.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=it)
+ Configurazione locale con [l’SDK di AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)
+ [QuickStart per AEM 6.5 SP13+](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it?lang=en#install-local-aem-instances)

Tutte le distribuzioni richiedono `tutorial-solution-content.zip` dal [File di soluzione](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) da installare e necessario [Configurazioni di implementazione](../deployment/web-component.md) vengono eseguite.


>[!IMPORTANT]
>
>Il componente Web è progettato per connettersi a un __Pubblicazione AEM__ Tuttavia, può originare il contenuto da AEM Author se l’autenticazione viene fornita nel file del componente Web [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) file.

## Come usare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Accedi a `web-component` sottodirectory.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Modifica il `.../src/person.js` file per includere i dettagli di connessione AEM:

   In `aemHeadlessService` oggetto, aggiorna `aemHost` per puntare al servizio di pubblicazione AEM.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Se ti connetti a un servizio AEM Author, nel `aemCredentials` , fornire le credenziali utente AEM locali.

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

1. Una nuova finestra del browser apre la pagina HTML statica che incorpora il componente Web in [http://localhost:8080](Http://localhost:8080).
1. Il _Informazioni persona_ Il componente Web viene visualizzato nella pagina Web.

## Il codice

Di seguito è riportato un riepilogo della modalità di creazione del componente Web, della modalità di connessione a headless AEM per il recupero di contenuti tramite query persistenti GraphQL e della modalità di presentazione dei dati. Il codice completo è disponibile su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Tag HTML del componente Web

Un componente web riutilizzabile (o elemento personalizzato) `<person-info>` viene aggiunto al `../src/assets/aem-headless.html` pagina HTML. Supporta `host` e `query-param-value` attributi per determinare il comportamento del componente. Il `host` sostituzioni di valore dell&#39;attributo `aemHost` valore da `aemHeadlessService` oggetto in `person.js`, e `query-param-value` viene utilizzato per selezionare la persona da riprodurre.

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementazione del componente web

Il `person.js` definisce la funzionalità del componente Web e di seguito sono riportate le principali caratteristiche salienti.

#### Implementazione dell’elemento PersonInfo

Il `<person-info>` l&#39;oggetto class dell&#39;elemento personalizzato definisce la funzionalità utilizzando `connectedCallback()` metodi del ciclo di vita, associazione di una radice dell&#39;ombra, recupero di query persistenti di GraphQL e manipolazione DOM per creare la struttura DOM dell&#39;ombra interna dell&#39;elemento personalizzato.

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

#### Registra il `<person-info>` elemento

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Condivisione delle risorse tra le origini (CORS)

Questo componente web si basa su una configurazione CORS basata su AEM in esecuzione nell’ambiente AEM di destinazione e presuppone che la pagina host venga eseguita su `http://localhost:8080` in modalità di sviluppo e di seguito è riportato un esempio di configurazione CORS OSGi per il servizio AEM Author locale.

Rivedi [configurazioni di implementazione](../deployment/web-component.md) per i rispettivi servizi AEM.

![Configurazione CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
