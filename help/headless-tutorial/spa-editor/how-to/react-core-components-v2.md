---
title: Come utilizzare AEM React Editable Components v2
description: Scopri come utilizzare AEM React Editable Components v2 per abilitare un’app React.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: f02d5e01388ee61228254951b05c37c336423348
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 1%

---


# Come utilizzare AEM React Editable Components v2

AEM fornisce [AEM React Modificabili Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), un SDK basato su Node.js che consente la creazione di componenti React che supportano la modifica di componenti nel contesto tramite AEM editor di SPA.

+ [modulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Progetto Github](https://github.com/adobe/aem-react-editable-components)
+ [Documentazione di Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Per ulteriori dettagli ed esempi di codice per AEM React Editable Components v2, consulta la documentazione tecnica:

+ [Integrazione con la documentazione AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentazione del componente modificabile](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentazione per i collaboratori](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## Pagine AEM

AEM React Modificable Components funziona sia con SPA Editor che con le app Remote SPA React. Il contenuto che popola i componenti React modificabili deve essere esposto tramite pagine AEM che estendono il [Componente Pagina SPA](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html). I componenti AEM, mappati su componenti React modificabili, devono implementare AEM [Struttura per l&#39;esportazione di componenti](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) - quali [Componenti core WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it).


## Dipendenze

Assicurati che l&#39;app React sia in esecuzione su Node.js 14+.

Il set minimo di dipendenze per l’app React da utilizzare AEM React Modificable Components v2 è: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`e  `@adobe/aem-spa-page-model-manager`.


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
> [Base dei componenti core WCM AEM React](https://github.com/adobe/aem-react-core-wcm-components-base) e [SPA dei componenti core WCM per AEM](https://github.com/adobe/aem-react-core-wcm-components-spa) non sono compatibili con AEM React Editable Components v2.

## Editor SPA

Quando si utilizzano i AEM componenti modificabili React con un’app React basata su editor SPA, la AEM `ModelManager` SDK, come SDK:

1. Recupera il contenuto da AEM
1. Popola i componenti React Edibile con contenuto AEM

Racchiudi l’app React con un ModelManager inizializzato ed esegui il rendering dell’app React. L&#39;app React deve contenere un&#39;istanza del `<Page>` componente esportato da `@adobe/aem-react-editable-components`. La `<Page>` Il componente ha la logica per creare in modo dinamico i componenti React in base alla `.model.json` fornito da AEM.

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

La `<Page>` viene passato come rappresentazione della pagina AEM come JSON, tramite `pageModel` di cui `ModelManager`. La `<Page>` crea in modo dinamico i componenti React per gli oggetti nel `pageModel` in base alla corrispondenza `resourceType` con un componente React che si registra al tipo di risorsa tramite `MapTo(..)`.

## Componenti modificabili

La `<Page>` viene passata la rappresentazione della pagina AEM come JSON, tramite `ModelManager`. La `<Page>` quindi crea in modo dinamico i componenti React per ogni oggetto nel JSON, confrontando l’oggetto JS `resourceType` con un componente React che si registra al tipo di risorsa tramite il componente `MapTo(..)` chiamata. Ad esempio, per creare un&#39;istanza, viene utilizzato quanto segue

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

Il JSON fornito da AEM di cui sopra può essere utilizzato per creare un’istanza dinamica e popolare un componente React modificabile.

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

I componenti modificabili possono essere riutilizzati e incorporati uno nell’altro. Esistono due considerazioni chiave per incorporare un componente modificabile in un altro:

1. Il contenuto JSON di AEM per il componente di incorporamento deve contenere il contenuto per soddisfare i componenti incorporati. Questa operazione viene eseguita creando una finestra di dialogo per il componente AEM che raccoglie i dati richiesti.
1. L’istanza &quot;non modificabile&quot; del componente React deve essere incorporata, anziché l’istanza &quot;modificabile&quot; che viene racchiusa con `<EditableComponent>`. Il motivo è che, se il componente incorporato ha `<EditableComponent>` wrapper, l’Editor SPA tenta di vestire il componente interno con il chrome di modifica (casella al passaggio del mouse blu), anziché con il componente di incorporamento esterno.

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

Il JSON fornito da AEM di cui sopra può essere utilizzato per creare un’istanza dinamica e popolare un componente React modificabile che incorpora un altro componente React.


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



