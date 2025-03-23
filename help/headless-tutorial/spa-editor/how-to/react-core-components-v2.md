---
title: Come utilizzare AEM React Editable Components v2
description: Scopri come utilizzare AEM React Editable Components v2 per alimentare un’app React.
version: Experience Manager as a Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 190
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 1%

---

# Come utilizzare AEM React Editable Components v2

{{edge-delivery-services}}

AEM fornisce [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), un SDK basato su Node.js che consente la creazione di componenti React che supportano la modifica di componenti nel contesto tramite AEM SPA Editor.

+ [modulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Progetto Github](https://github.com/adobe/aem-react-editable-components)
+ [Documentazione di Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Per ulteriori dettagli ed esempi di codice per AEM React Editable Components v2, consulta la documentazione tecnica:

+ [Integrazione con la documentazione di AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentazione del componente modificabile](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentazione Helpers](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## Pagine AEM

I componenti AEM React Editable funzionano sia con l’editor di applicazioni a pagina singola che con le app Remote SPA React. Il contenuto dei componenti React modificabili deve essere esposto tramite pagine AEM che estendono il [componente Pagina SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). I componenti AEM mappati su componenti React modificabili devono implementare il framework di esportazione di componenti [AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html), ad esempio [Componenti WCM core di AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it).


## Dipendenze

Assicurati che l’app React sia in esecuzione su Node.js 14+.

Il set minimo di dipendenze per l’app React per utilizzare AEM React Editable Components v2 è: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping` e `@adobe/aem-spa-page-model-manager`.


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [La base dei componenti WCM core di AEM React](https://github.com/adobe/aem-react-core-wcm-components-base) e [la SPA dei componenti WCM core di AEM React](https://github.com/adobe/aem-react-core-wcm-components-spa) non sono compatibili con AEM React Editable Components v2.

## Editor SPA

Quando si utilizza AEM React Editable Components con un’app React basata su un editor di applicazioni a pagina singola, AEM `ModelManager` SDK, come SDK:

1. Recupera il contenuto da AEM
1. Popola i componenti React Edible con il contenuto di AEM

Racchiudi l’app React con un ModelManager inizializzato ed esegui il rendering dell’app React. L&#39;app React deve contenere un&#39;istanza del componente `<Page>` esportato da `@adobe/aem-react-editable-components`. Il componente `<Page>` ha una logica per la creazione dinamica dei componenti React in base a `.model.json` forniti da AEM.

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

`<Page>` viene passato come rappresentazione della pagina AEM come JSON tramite `pageModel` fornito da `ModelManager`. Il componente `<Page>` crea in modo dinamico i componenti React per gli oggetti in `pageModel` confrontando `resourceType` con un componente React che si registra nel tipo di risorsa tramite `MapTo(..)`.

## Componenti modificabili

A `<Page>` viene passata la rappresentazione della pagina AEM come JSON tramite `ModelManager`. Il componente `<Page>` crea quindi in modo dinamico i componenti React per ogni oggetto nel JSON facendo corrispondere il valore `resourceType` dell&#39;oggetto JS con un componente React che si registra nel tipo di risorsa tramite la chiamata `MapTo(..)` del componente. Ad esempio, per creare un’istanza viene utilizzato quanto segue

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

Il JSON fornito da AEM può essere utilizzato per creare un’istanza e popolare dinamicamente un componente React modificabile.

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## Incorporazione di componenti

I componenti modificabili possono essere riutilizzati e incorporati l’uno nell’altro. Quando si incorpora un componente modificabile in un altro, è necessario tenere presenti due considerazioni chiave:

1. Il contenuto JSON di AEM per il componente di incorporamento deve contenere il contenuto per soddisfare i componenti incorporati. Questa operazione viene eseguita creando una finestra di dialogo per il componente AEM che raccoglie i dati richiesti.
1. L&#39;istanza &quot;non modificabile&quot; del componente React deve essere incorporata, anziché l&#39;istanza &quot;modificabile&quot; con `<EditableComponent>`. Il motivo è che, se il componente incorporato ha il wrapper `<EditableComponent>`, l&#39;editor di applicazioni a pagina singola tenta di vestire il componente interno con il cromo di modifica (casella blu al passaggio del mouse), anziché il componente esterno di incorporamento.

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

Il JSON fornito da AEM può essere utilizzato per creare in modo dinamico un’istanza e popolare un componente React modificabile che incorpora un altro componente React.


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```
