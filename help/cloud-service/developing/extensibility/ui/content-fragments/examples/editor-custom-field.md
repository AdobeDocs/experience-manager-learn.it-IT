---
title: 'Console Frammenti di contenuto: campi personalizzati'
description: Scopri come creare un campo personalizzato nell’Editor frammento di contenuto AEM.
feature: Developer Tools, Content Fragments
version: Cloud Service
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 315
last-substantial-update: 2024-02-27T00:00:00Z
jira: KT-14903
thumbnail: KT-14903.jpeg
exl-id: 563bab0e-21e3-487c-9bf3-de15c3a81aba
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 1%

---

# Campi personalizzati

Scopri come creare campi personalizzati nell’Editor frammento di contenuto AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3427585?learn=on)

Le estensioni dell&#39;interfaccia utente dell&#39;AEM devono essere sviluppate utilizzando il framework [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html), in quanto mantiene un aspetto coerente con il resto dell&#39;AEM e dispone inoltre di un&#39;ampia libreria di funzionalità predefinite, riducendo i tempi di sviluppo.

## Punto di estensione

Questo esempio sostituisce un campo esistente nell’Editor frammento di contenuto con un’implementazione personalizzata.

| Interfaccia utente AEM estesa | Punto di estensione |
| ------------------------ | --------------------- | 
| [Editor frammento di contenuto](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rendering elemento modulo personalizzato](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/) |

## Estensione di esempio

Questo esempio illustra come limitare i valori dei campi nell’Editor frammento di contenuto a un set predeterminato sostituendo il campo standard con un elenco a discesa personalizzato di SKU predefinite. Gli autori possono selezionare da questo elenco SKU specifico. Anche se gli SKU provengono in genere da un sistema di gestione delle informazioni sui prodotti (PIM, Product Information Management), questo esempio semplifica la creazione di un elenco statico degli SKU.

Il codice sorgente per questo esempio è [disponibile per il download](./assets/editor-custom-field/content-fragment-editor-custom-field-src.zip).

### Definizione del modello per frammenti di contenuto

Questo esempio associa a qualsiasi campo Frammento di contenuto il cui nome è `sku` (tramite una [corrispondenza espressione regolare](#extension-registration) di `^sku$`) e lo sostituisce con un campo personalizzato. Nell’esempio viene utilizzato il modello WKND Adventure Content Fragment che è stato aggiornato e la definizione è la seguente:

![Definizione modello frammento di contenuto](./assets/editor-custom-field/content-fragment-editor.png)

Nonostante il campo SKU personalizzato venga visualizzato come elenco a discesa, il modello sottostante è configurato come campo di testo. L’implementazione del campo personalizzato deve solo essere allineata al nome e al tipo di proprietà appropriati, facilitando la sostituzione del campo standard con la versione a discesa personalizzata.


### Route delle app

Nel componente React principale `App.js`, includere la route `/sku-field` per eseguire il rendering del componente React `SkuField`.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
import React from "react";
import ErrorBoundary from "react-error-boundary";
import { HashRouter as Router, Routes, Route } from "react-router-dom";
import ExtensionRegistration from "./ExtensionRegistration";
import SkuField from "./SkuField"; // Custom component implemented below

function App() {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          <Route index element={<ExtensionRegistration />} />
          <Route
            exact path="index.html"
            element={<ExtensionRegistration />}
          />
          {/* This is the React route that maps to the custom field */}
          <Route
            exact path="/sku-field"
            element={<SkuField />}/>
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
...
```

Questa route personalizzata di `/sku-field` è mappata al componente `SkuField` ed è utilizzata di seguito nella [registrazione estensione](#extension-registration).

### Registrazione dell’estensione

`ExtensionRegistration.js`, mappato alla route index.html, è il punto di ingresso per l&#39;estensione AEM e definisce:

+ Definizione del widget nella funzione `getDefinitions()` con attributi `fieldNameExp` e `url`. L&#39;elenco completo degli attributi disponibili è disponibile nel [Riferimento API di rendering elemento modulo personalizzato](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference).
+ Il valore dell&#39;attributo `url`, un percorso URL relativo (`/index.html#/skuField`) per caricare l&#39;interfaccia utente del campo.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        field: {
          getDefinitions() {
            return [
              // Multiple fields can be registered here.
              {
                fieldNameExp: '^sku$',  // This is a regular expression that matches the field name in the Content Fragment Model to replace with React component specified in the `url` key.
                url: '/#/sku-field',    // The URL, which is mapped vai the Route in App.js, to the React component that will be used to render the field.
              },
              // Other bindings besides fieldNameExp, other bindings can be used as well as defined here:
              // https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference
            ];
          },
        },
      }
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Campo personalizzato

Il componente React di `SkuField` aggiorna l&#39;Editor frammento di contenuto con un&#39;interfaccia utente personalizzata, utilizzando Adobe React Spectrum per il modulo di selezione. Gli elementi di rilievo includono:

+ Utilizzo di `useEffect` per l&#39;inizializzazione e la connessione all&#39;Editor frammenti di contenuto dell&#39;AEM, con uno stato di caricamento visualizzato fino al completamento dell&#39;installazione.
+ Durante il rendering all&#39;interno di un iFrame, l&#39;altezza dell&#39;iFrame viene regolata dinamicamente tramite la funzione `onOpenChange` per adattarsi al menu a discesa del selettore Spectrum di Adobe React.
+ Comunica le selezioni dei campi all&#39;host utilizzando `connection.host.field.onChange(value)` nella funzione `onSelectionChange`, assicurandosi che il valore selezionato sia convalidato e salvato automaticamente in base alle linee guida del modello per frammenti di contenuto.

I campi personalizzati vengono riprodotti all’interno di un iFrame inserito nell’Editor frammento di contenuto. La comunicazione tra il codice di campo personalizzato e l&#39;Editor frammento di contenuto avviene esclusivamente tramite l&#39;oggetto `connection`, stabilito dalla funzione `attach` dal pacchetto `@adobe/uix-guest`.

`src/aem-cf-editor-1/web-src/src/components/SkuField.js`

```javascript
import React, { useEffect, useState } from "react";
import { extensionId } from "./Constants";
import { attach } from "@adobe/uix-guest";
import { Provider, View, lightTheme } from "@adobe/react-spectrum";
import { Picker, Item } from "@adobe/react-spectrum";

const SkuField = (props) => {
  const [connection, setConnection] = useState(null);
  const [validationState, setValidationState] = useState(null);
  const [value, setValue] = useState(null);
  const [model, setModel] = useState(null);
  const [items, setItems] = useState(null);

  /**
   * Mock function that gets a list of Adventure SKUs to display.
   * The data could come anywhere, AEM's HTTP APIs, a PIM, or other system.
   * @returns a list of items for the picker
   */
  async function getItems() {
    // Data to drive input field validation can come from anywhere.
    // Fo example this commented code shows how it could be fetched from an HTTP API.
    // fetch(MY_API_URL).then((response) => response.json()).then((data) => { return data; }

    // In this example, for simplicity, we generate a list of 25 SKUs.
    return Array.from({ length: 25 }, (_, i) => ({ 
        name: `WKND-${String(i + 1).padStart(3, '0')}`, 
        id: `WKND-${String(i + 1).padStart(3, '0')}` 
    }));
  }

  /**
   * When the fields changes, update the value in the Content Fragment Editor
   * @param {*} value the selected value in the picker
   */
  const onSelectionChange = async (value) => {
    // This sets the value in the React state of the custom field
    setValue(value);
    // This calls the setValue method on the host (AEM's Content Fragment Editor)
    connection.host.field.onChange(value);
  };

  /**
   * Some widgets, like the Picker, have a variable height.
   * In these cases adjust the Content Fragment Editor's iframe's height so the field doesn't get cut off.     *
   * @param {*} isOpen true if the picker is open, false if it's closed
   */
  const onOpenChange = async (isOpen) => {
    if (isOpen) {
      // Calculate the height of the picker box and its label, and surrounding padding.
      const pickerHeight = Number(document.body.clientHeight.toFixed(0));
      // Calculate the height of the picker options dropdown, and surrounding padding.
      // We do this  by multiplying the number of items by the height of each item (32px) and adding 12px for padding.
      const optionsHeight = items.length * 32 + 12;

      // Set the height of the iframe to the height of the picker + the height of the options, or 400px, whichever is smaller.
      // The options will scroll if they they cannot fit into 400px
      const height = Math.min(pickerHeight + optionsHeight, 400);

      // Set the height of the iframe in the Content Fragment Editor
      await connection.host.field.setHeight(height);
    } else {
      // Set the height of the iframe in the Content Fragment Editor to the height of the closed picker.
      await connection.host.field.setHeight(
        Number(document.body.clientHeight.toFixed(0))
      );
    }
  };

  useEffect(() => {
    const init = async () => {
      // Connect to the host (AEM's Content Fragment Editor)
      const conn = await attach({ id: extensionId });
      setConnection(conn);

      // get the Content Fragment Model
      setModel(await conn.host.field.getModel());

      // Share the validation state back to the client.
      // When conn.host.field.setValue(value) is called, the
      await conn.host.field.onValidationStateChange((state) => {
        // state can be `valid` or `invalid`.
        setValidationState(state);
      });
      // Get default value from the Content Fragment Editor
      // (either the default value set in the model, or a perviously set value)
      setValue(await conn.host.field.getDefaultValue());

      // Get the list of items for the picker; in this case its a list of adventure SKUs 
      // This could come from elsewhere in AEM or from an external system.
      setItems(await getItems());
    };

    init().catch(console.error);
  }, []);

  // If the component is not yet initialized, return a loading state.
  if (!connection || !model || !items) {
    // Put whatever loader you like here...
    return <Provider theme={lightTheme}>Loading custom field...</Provider>;
  }

  // Wrap the Spectrum UI component in a Provider theme, such that it is styled appropriately.
  // Render the picker, and bind to the data and custom event handlers.

  // Set as much of the model as we can, to allow maximum authoring flexibility without developer support.
  return (
    <Provider theme={lightTheme}>
      <View width="100%">
        <Picker
          label={model.fieldLabel}
          isRequired={model.required}
          placeholder={model.emptyText}
          errorMessage={model.customErrorMsg}
          selectedKey={value}
          necessityIndicator="icon"
          shouldFlip={false}
          width={"90%"}
          items={items}
          isInvalid={validationState === "invalid"}
          onSelectionChange={onSelectionChange}
          onOpenChange={onOpenChange}
        >
          {(item) => <Item key={item.value}>{item.name}</Item>}
        </Picker>
      </View>
    </Provider>
  );
};

export default SkuField;
```
