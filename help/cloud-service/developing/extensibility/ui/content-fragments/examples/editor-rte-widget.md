---
title: Aggiungere widget all’editor Rich Text
description: Scopri come aggiungere widget all’Editor Rich Text nell’Editor frammenti di contenuto di AEM
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 476
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Aggiungere widget all’editor Rich Text

Scopri come aggiungere widget all’Editor Rich Text nell’Editor frammenti di contenuto di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

Per aggiungere contenuto dinamico nell&#39;editor Rich Text, è possibile utilizzare la funzionalità **widget**. I widget consentono di integrare l’interfaccia utente semplice o complessa nell’editor Rich Text e l’interfaccia utente tramite il framework JS scelto. Possono essere considerate finestre di dialogo aperte premendo il tasto speciale `{` nell&#39;editor Rich Text.

In genere, i widget vengono utilizzati per inserire il contenuto dinamico che ha una dipendenza di sistema esterna o che potrebbe cambiare in base al contesto corrente.

I **widget** vengono aggiunti all&#39;editor **RTE** nell&#39;editor frammenti di contenuto utilizzando il punto di estensione `rte`. Utilizzando il metodo `getWidgets()` del punto di estensione `rte`, vengono aggiunti uno o più widget. Vengono attivati premendo il tasto speciale `{` per aprire l&#39;opzione del menu di scelta rapida, quindi selezionare il widget desiderato per caricare l&#39;interfaccia utente della finestra di dialogo personalizzata.

In questo esempio viene illustrato come aggiungere un widget denominato _Elenco codici sconto_ per trovare, selezionare e aggiungere il codice sconto WKND specifico dell&#39;avventura all&#39;interno di un contenuto RTE. Questi codici di sconto possono essere gestiti in sistemi esterni come Order Management System (OMS), Product Information Management (PIM), applicazioni sviluppate in casa o un&#39;azione Adobe AppBuilder.

Per semplificare, in questo esempio viene utilizzato il framework [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) per sviluppare l&#39;interfaccia utente del widget o della finestra di dialogo e il nome dell&#39;avventura WKND hardcoded e i dati del codice sconto.

## Punto di estensione

Questo esempio si estende fino al punto di estensione `rte` per aggiungere un widget all&#39;editor Rich Text nell&#39;Editor frammenti di contenuto.

| Interfaccia utente di AEM estesa | Punto di estensione |
| ------------------------ | --------------------- | 
| [Editor frammento di contenuto](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Widget editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Estensione di esempio

Nell&#39;esempio seguente viene creato un widget _Elenco codici sconto_. Premendo il tasto speciale `{` nell&#39;editor Rich Text, viene aperto il menu di scelta rapida, quindi selezionando l&#39;opzione _Elenco codici sconto_ dal menu di scelta rapida viene aperta la finestra di dialogo.

Gli autori di contenuti WKND possono trovare, selezionare e aggiungere il codice di sconto corrente specifico per Adventure, se disponibile.

### Registrazione dell’estensione

`ExtensionRegistration.js`, mappato alla route index.html, è il punto di ingresso per l&#39;estensione AEM e definisce:

+ Definizione del widget nella funzione `getWidgets()` con attributi `id, label and url`.
+ Il valore dell&#39;attributo `url`, un percorso URL relativo (`/index.html#/discountCodes`) per caricare l&#39;interfaccia utente della finestra di dialogo.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {
          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget", // Provide a unique ID for the widget
              label: "Discount Code List", // Provide a label for the widget
              url: "/index.html#/discountCodes", // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
        }, // Add a comma here
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Aggiungi route `discountCodes` in `App.js`{#add-widgets-route}

Nel componente React principale `App.js`, aggiungi la route `discountCodes` per eseguire il rendering dell&#39;interfaccia utente per il percorso URL relativo sopra indicato.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### Crea componente React `DiscountCodes`{#create-widget-react-component}

L&#39;interfaccia utente del widget o della finestra di dialogo viene creata utilizzando il framework [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html). Di seguito è riportato il codice del componente `DiscountCodes`, con le evidenziazioni chiave:

+ Il rendering dell&#39;interfaccia utente viene eseguito utilizzando i componenti Spectrum di React, ad esempio [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ L&#39;array `adventureDiscountCodes` dispone della mappatura hardcoded del nome dell&#39;avventura e del codice sconto. In uno scenario reale, questi dati possono essere recuperati dall’azione Adobe AppBuilder o da sistemi esterni come PIM, OMS o gateway API basato su provider cloud o casalinghi.
+ `guestConnection` è inizializzato con l&#39;hook `useEffect` [React](https://react.dev/reference/react/useEffect) ed è gestito come stato del componente. Viene utilizzato per comunicare con l’host AEM.
+ La funzione `handleDiscountCodeChange` ottiene il codice sconto per il nome dell&#39;avventura selezionato e aggiorna la variabile di stato.
+ La funzione `addDiscountCode` che utilizza l&#39;oggetto `guestConnection` fornisce istruzioni RTE da eseguire. In questo caso, l&#39;istruzione `insertContent` e lo snippet di codice HTML del codice sconto effettivo da inserire nell&#39;editor Rich Text.

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
