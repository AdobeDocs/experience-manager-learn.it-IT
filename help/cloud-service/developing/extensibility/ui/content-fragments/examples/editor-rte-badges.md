---
title: Aggiungere badge all’editor Rich Text
description: Scopri come aggiungere badge all’Editor Rich Text (RTE) nell’Editor frammenti di contenuto AEM
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: 8e99c660fed409d44d34cf4edf6bf1b59fa29e34
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 0%

---


# Aggiungere badge all’editor Rich Text

![Esempio di badge dell’Editor frammento di contenuto](./assets/rte/rte-badge-home.png){align="center"}

[Badge editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)  sono estensioni che rendono non modificabile il testo nell’Editor Rich Text (RTE). Ciò significa che un badge dichiarato come tale può essere rimosso solo completamente e non può essere modificato parzialmente. Questi badge supportano anche colorazioni speciali all’interno dell’editor Rich Text, indicando chiaramente agli autori di contenuto che il testo è un badge e quindi non modificabile. Inoltre, forniscono indicazioni visive sul significato del testo del badge.

Il caso d’uso più comune per i badge Editor Rich Text è utilizzarli insieme a [Widget editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). In questo modo il contenuto inserito nell’editor Rich Text dal widget dell’editor Rich Text non è modificabile.

In genere, i badge in associazione ai widget vengono utilizzati per aggiungere il contenuto dinamico che ha una dipendenza di sistema esterna, ma _gli autori di contenuti non possono modificare_ il contenuto dinamico inserito per mantenerne l’integrità. Possono essere rimossi solo come un intero elemento.

Il **badge** sono aggiunti al **RTE** nell’Editor frammento di contenuto utilizzando `rte` punto di estensione. Utilizzo di `rte` del punto di estensione `getBadges()` metodo vengono aggiunti uno o più badge.

Questo esempio mostra come aggiungere un widget denominato _Servizio clienti per prenotazioni per gruppi di grandi dimensioni_ per trovare, selezionare e aggiungere i dettagli del servizio clienti WKND specifico dell’avventura, come **Nome rappresentante** e **Numero di telefono** all’interno di un contenuto RTE. Utilizzo della funzionalità badge **Numero di telefono** è stato creato **non modificabile** Tuttavia, gli autori di contenuti WKND possono modificare il Nome rappresentante.

Inoltre, il **Numero di telefono** ha uno stile diverso (blu), che è un caso d’uso aggiuntivo della funzionalità dei badge.

Per semplificare, in questo esempio viene utilizzato [Spettro di reazione Adobe](https://react-spectrum.adobe.com/react-spectrum/index.html) per sviluppare l’interfaccia utente del widget o della finestra di dialogo e i numeri di telefono del servizio clienti WKND hardcoded. Per controllare la mancata modifica e il diverso aspetto di stile del contenuto, `#` viene utilizzato nel `prefix` e `suffix` attributo della definizione dei badge.

## Punti di estensione

Questo esempio si estende al punto di estensione `rte` per aggiungere un badge all’editor Rich Text nell’Editor frammenti di contenuto.

| Interfaccia utente AEM estesa | Punti di estensione |
| ------------------------ | --------------------- | 
| [Editor frammento di contenuto](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Badge editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) e [Widget editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Estensione di esempio

Nell&#39;esempio seguente viene creata una _Servizio clienti per prenotazioni per gruppi di grandi dimensioni_ widget. Premendo il tasto `{` all’interno dell’editor Rich Text, viene aperto il menu di scelta rapida dei widget dell’editor Rich Text. Selezionando _Servizio clienti per prenotazioni per gruppi di grandi dimensioni_ dal menu di scelta rapida viene aperto il modale personalizzato.

Una volta aggiunto il numero di servizio cliente desiderato dal modale, i badge rendono _Numero di telefono non modificabile_ e lo incolla in blu.

### Registrazione dell’estensione

`ExtensionRegistration.js`, mappato a `index.html` è il punto di ingresso per l&#39;estensione AEM e definisce:

+ La definizione del badge è definita in `getBadges()` utilizzo degli attributi di configurazione `id`, `prefix`, `suffix`, `backgroundColor` e `textColor`.
+ In questo esempio, la proprietà `#` Questo carattere viene utilizzato per definire i limiti del badge, ovvero qualsiasi stringa nell’editor Rich Text circondata da `#` viene trattato come un’istanza di questo badge.

Vedi anche i dettagli chiave del widget dell’editor Rich Text:

+ La definizione del widget in `getWidgets()` funzione con `id`, `label` e `url` attributi.
+ Il `url` valore attributo, un percorso URL relativo (`/index.html#/largeBookingsCustomerService`) per caricare il modale.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
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

          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",                    // Provide a unique ID for the badge
              prefix: "#",                          // Provide a Badge starting character
              suffix: "#",                          // Provide a Badge ending character
              backgroundColor: "",                  // Provide HEX or text CSS color code for the background
              textColor: "#071DF8"                  // Provide HEX or text CSS color code for the text
            }
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",       // Provide a unique ID for the widget
              label: "Large Group Bookings Customer Service",          // Provide a label for the widget
              url: "/index.html#/largeBookingsCustomerService",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/largeBookingsCustomerService`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Aggiungi `largeBookingsCustomerService` instradamento `App.js`{#add-widgets-route}

Nel componente React principale `App.js`, aggiungi `largeBookingsCustomerService` route per eseguire il rendering dell&#39;interfaccia utente per il percorso URL relativo indicato sopra.

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### Crea `LargeBookingsCustomerService` Componente React{#create-widget-react-component}

Il widget o l&#39;interfaccia utente della finestra di dialogo viene creata utilizzando [Spettro di reazione Adobe](https://react-spectrum.adobe.com/react-spectrum/index.html) infrastruttura.

Il codice del componente React quando si aggiungono i dettagli del servizio clienti, racchiude la variabile del numero di telefono con il `#` carattere dei badge registrati per convertirlo in badge, come `#${phoneNumber}#`, quindi renderlo non modificabile.

Ecco gli elementi di rilievo di `LargeBookingsCustomerService` codice:

+ Il rendering dell’interfaccia utente viene eseguito utilizzando i componenti Spectrum di React, come [CasellaCombinata](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [GruppoPulsanti](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Pulsante](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ Il `largeGroupCustomerServiceList` l’array dispone della mappatura hardcoded del nome rappresentativo e del numero di telefono. In uno scenario reale, questi dati possono essere recuperati da un’azione Adobe AppBuilder o da sistemi esterni o da un gateway API basato su provider cloud o sviluppato in casa.
+ Il `guestConnection` viene inizializzato utilizzando `useEffect` [Gancio di reazione](https://react.dev/reference/react/useEffect) e gestito come stato componente. Viene utilizzato per comunicare con l’ospite dell’AEM.
+ Il `handleCustomerServiceChange` La funzione ottiene il nome e il numero di telefono del rappresentante e aggiorna le variabili dello stato del componente.
+ Il `addCustomerServiceDetails` funzione che utilizza `guestConnection` L&#39;oggetto fornisce istruzioni RTE da eseguire. In questo caso `insertContent` frammento di codice di istruzioni e HTML.
+ Per rendere **numero di telefono non modificabile** utilizzando i badge, il `#` un carattere speciale viene aggiunto prima e dopo il `phoneNumber` variabile, come `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
