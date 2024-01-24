---
title: Come utilizzare i componenti modificabili di React dell’AEM v2
description: Scopri come utilizzare AEM React Editable Components v2 per alimentare un’app React.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 244
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 0%

---

# Come utilizzare i componenti modificabili di React dell’AEM v2

{{edge-delivery-services}}

L&#39;AEM fornisce [Componenti modificabili di React AEM v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components): SDK basato su Node.js che consente la creazione di componenti React che supportano la modifica di componenti nel contesto tramite l’Editor SPA dell’AEM.

+ [modulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Progetto Github](https://github.com/adobe/aem-react-editable-components)
+ [Documentazione di Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Per ulteriori dettagli ed esempi di codice per AEM React Editable Components v2, consulta la documentazione tecnica:

+ [Integrazione con la documentazione AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentazione del componente modificabile](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentazione degli helper](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## Pagine AEM

I componenti modificabili di AEM React funzionano sia con l’editor SPA che con le app Remote SPA React. I contenuti che popolano i componenti React modificabili, devono essere esposti tramite pagine AEM che estendono il [Componente pagina SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). I componenti AEM, mappati su componenti React modificabili, devono implementare l’AEM [Framework di esportazione dei componenti](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) - quali [Componenti WCM core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it).


## Dipendenze

Assicurati che l’app React sia in esecuzione su Node.js 14+.

L’insieme minimo di dipendenze che l’app React deve utilizzare per AEM React Editable Components v2 è: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`, e  `@adobe/aem-spa-page-model-manager`.


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
> [Base componenti WCM core React AEM](https://github.com/adobe/aem-react-core-wcm-components-base) e [SPA dei componenti WCM core React dell’AEM](https://github.com/adobe/aem-react-core-wcm-components-spa) non sono compatibili con React Editable Components v2 dell’AEM.

## Editor SPA

Quando si utilizza l’app React Editable Components di AEM con un’app React basata sull’editor di SPA, l’AEM `ModelManager` SDK, come SDK:

1. Recupera i contenuti dall’AEM
1. Popola i componenti React Edible con contenuto AEM

Racchiudi l’app React con un ModelManager inizializzato ed esegui il rendering dell’app React. L’app React deve contenere un’istanza del `<Page>` componente esportato da `@adobe/aem-react-editable-components`. Il `<Page>` ha la logica per creare in modo dinamico componenti React in base al `.model.json` fornite dall&#39;AEM.

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

Il `<Page>` viene passato come rappresentazione della pagina AEM come JSON, tramite `pageModel` fornite da `ModelManager`. Il `<Page>` crea in modo dinamico componenti React per gli oggetti nel `pageModel` facendo corrispondere `resourceType` con un componente React che si registra nel tipo di risorsa tramite `MapTo(..)`.

## Componenti modificabili

Il `<Page>` ha superato la rappresentazione della pagina AEM come JSON, tramite il `ModelManager`. Il `<Page>` componente crea quindi in modo dinamico componenti React per ogni oggetto nel JSON, confrontando i `resourceType` valore con un componente React che si registra nel tipo di risorsa tramite il componente `MapTo(..)` chiamata. Ad esempio, per creare un’istanza viene utilizzato quanto segue

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

Il JSON fornito dall’AEM di cui sopra può essere utilizzato per creare in modo dinamico un’istanza e popolare un componente React modificabile.

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

1. Il contenuto JSON dell’AEM per il componente che incorpora deve contenere il contenuto per soddisfare i componenti incorporati. A tal fine, crea una finestra di dialogo per il componente AEM che raccoglie i dati necessari.
1. L’istanza &quot;non modificabile&quot; del componente React deve essere incorporata, anziché l’istanza &quot;modificabile&quot; racchiusa in `<EditableComponent>`. Il motivo è che, se il componente incorporato ha `<EditableComponent>` wrapper, l’editor SPA tenta di vestire il componente interno con il chrome di modifica (casella blu al passaggio del mouse), anziché il componente esterno di incorporamento.

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

Il JSON fornito dall’AEM di cui sopra può essere utilizzato per creare in modo dinamico un’istanza e popolare un componente React modificabile che incorpora un altro componente React.


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
