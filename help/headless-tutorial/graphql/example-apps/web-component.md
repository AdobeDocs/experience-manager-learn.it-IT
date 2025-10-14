---
title: Componente web/JS - Esempio di AEM Headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Web Component/JS illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 129
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 1%

---

# Componente Web

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione per componenti web illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti ed eseguire il rendering di una parte dell’interfaccia utente, utilizzando esclusivamente codice JavaScript.

![Componente Web con AEM Headless](./assets/web-component/web-component.png)

Visualizza il [codice sorgente in GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Node.js v18](https://nodejs.org/it/)
+ [Git](https://git-scm.com/)

## Requisiti di AEM

Il componente Web funziona con le seguenti opzioni di distribuzione di AEM.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=it)
+ Configurazione locale con [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)
   + Richiede [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (per la connessione a AEM 6.5 o AEM SDK locale)

L&#39;app di esempio si basa sull&#39;installazione di [basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip) e sull&#39;esistenza delle [configurazioni di distribuzione](../deployment/web-component.md) richieste.


>[!IMPORTANT]
>
>Il componente Web è progettato per connettersi a un ambiente __AEM Publish__, tuttavia può creare contenuto da AEM Author se viene fornita l&#39;autenticazione nel file [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) del componente Web.

## Come usare

1. Clona l&#39;archivio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Passare alla sottodirectory `web-component`.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Modifica il file `.../src/person.js` per includere i dettagli di connessione AEM:

   Nell&#39;oggetto `aemHeadlessService`, aggiorna `aemHost` in modo che punti al servizio di pubblicazione AEM.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Se ti connetti a un servizio di authoring di AEM, nell&#39;oggetto `aemCredentials`, fornisci le credenziali utente locali di AEM.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Aprire un terminale ed eseguire i comandi da `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Una nuova finestra del browser apre la pagina statica di HTML che incorpora il componente Web in [http://localhost:8080](http://localhost:8080).
1. Il componente Web _Informazioni persona_ viene visualizzato nella pagina Web.

## Il codice

Di seguito è riportato un riepilogo della modalità di creazione del componente Web, della modalità di connessione a AEM Headless per il recupero di contenuti tramite query persistenti GraphQL e della modalità di presentazione dei dati. Il codice completo si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Tag HTML del componente Web

Un componente Web riutilizzabile (o elemento personalizzato) `<person-info>` è stato aggiunto alla pagina HTML `../src/assets/aem-headless.html`. Supporta gli attributi `host` e `query-param-value` per determinare il comportamento del componente. Il valore dell&#39;attributo `host` sostituisce il valore `aemHost` dell&#39;oggetto `aemHeadlessService` in `person.js` e `query-param-value` viene utilizzato per selezionare la persona di cui eseguire il rendering.

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementazione del componente web

`person.js` definisce la funzionalità del componente Web e di seguito sono riportati gli elementi di rilievo di tale funzionalità.

#### Implementazione dell’elemento PersonInfo

L&#39;oggetto classe dell&#39;elemento personalizzato `<person-info>` definisce la funzionalità utilizzando i metodi del ciclo di vita `connectedCallback()`, allegando una radice dell&#39;ombreggiatura, recuperando la query persistente di GraphQL e la manipolazione DOM per creare la struttura DOM dell&#39;elemento personalizzato.

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
        personImgElement.setAttribute('src',
            host + (person.profilePicture._dynamicUrl || person.profilePicture._path));
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Registra elemento `<person-info>`

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Condivisione delle risorse tra le origini (CORS)

Questo componente web si basa su una configurazione CORS basata su AEM in esecuzione nell’ambiente AEM di destinazione e presuppone che la pagina host venga eseguita su `http://localhost:8080` in modalità di sviluppo e di seguito sia presente una configurazione OSGi CORS di esempio per il servizio AEM Author locale.

Rivedi [configurazioni di distribuzione](../deployment/web-component.md) per il rispettivo servizio AEM.
