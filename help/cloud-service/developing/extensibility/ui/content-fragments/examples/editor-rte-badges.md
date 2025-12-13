---
title: Aggiungere badge all’editor Rich Text
description: Scopri come aggiungere badge all’Editor Rich Text (RTE) nell’Editor frammenti di contenuto di AEM
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 1%

---

# Aggiungere badge all’editor Rich Text

Scopri come aggiungere badge all’editor Rich Text nell’editor di frammenti di contenuto di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[Il badge Editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) è un&#39;estensione che rende il testo nell&#39;Editor Rich Text non modificabile. Ciò significa che un badge dichiarato come tale può essere rimosso solo completamente e non può essere modificato parzialmente. Questi badge supportano anche colorazioni speciali all’interno dell’editor Rich Text, indicando chiaramente agli autori di contenuto che il testo è un badge e quindi non modificabile. Inoltre, forniscono indicazioni visive sul significato del testo del badge.

Il caso d&#39;uso più comune per i badge Editor Rich Text consiste nell&#39;utilizzarli insieme a [widget Editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). In questo modo il contenuto inserito nell’editor Rich Text dal widget dell’editor Rich Text non è modificabile.

In genere, i badge in associazione ai widget vengono utilizzati per aggiungere il contenuto dinamico con una dipendenza di sistema esterna, ma _gli autori di contenuto non possono modificare_ il contenuto dinamico inserito per mantenerne l&#39;integrità. Possono essere rimossi solo come un intero elemento.

I **badge** vengono aggiunti all&#39;editor **RTE** nell&#39;editor frammenti di contenuto utilizzando il punto di estensione `rte`. Utilizzando il metodo `rte` del punto di estensione `getBadges()`, vengono aggiunti uno o più badge.

In questo esempio viene illustrato come aggiungere un widget denominato _Servizio clienti per prenotazioni di gruppi di grandi dimensioni_ per trovare, selezionare e aggiungere i dettagli del servizio clienti specifici dell&#39;avventura WKND, ad esempio **Nome rappresentante** e **Numero di telefono** all&#39;interno di un contenuto dell&#39;editor Rich Text. Utilizzando la funzionalità dei badge, **Il numero di telefono** è reso **non modificabile**, ma gli autori di contenuto WKND possono modificare il nome del rappresentante.

Inoltre, il **numero di telefono** ha uno stile diverso (blu), il che rappresenta un caso d&#39;uso aggiuntivo della funzionalità dei badge.

Per semplificare, in questo esempio viene utilizzato il framework [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) per sviluppare l&#39;interfaccia utente del widget o della finestra di dialogo e i numeri di telefono del Servizio clienti WKND hardcoded. Per controllare la non modifica e il diverso aspetto di stile del contenuto, viene utilizzato il carattere `#` nell&#39;attributo `prefix` e `suffix` della definizione dei badge.

## Punti dell’estensione

Questo esempio si estende fino al punto di estensione `rte` per aggiungere un badge all’editor Rich Text nell’Editor frammenti di contenuto.

| Interfaccia utente di AEM estesa | Punti dell’estensione |
| ------------------------ | --------------------- |
| [Editor frammento di contenuto](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Badge editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) e [Widget editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Estensione di esempio

Nell&#39;esempio seguente viene creato un widget del servizio clienti _Large Group Booking_. Premendo il tasto `{` nell&#39;editor Rich Text, viene aperto il menu di scelta rapida dei widget Rich Text. Selezionando l&#39;opzione _Large Group Bookings Customer Service_ dal menu di scelta rapida, viene aperta la finestra modale personalizzata.

Una volta aggiunto il numero del servizio clienti desiderato dal modale, i badge rendono _il numero di telefono non modificabile_ e gli assegnano uno stile di colore blu.

### Registrazione dell’estensione

`ExtensionRegistration.js`, mappato alla route `index.html`, è il punto di ingresso per l&#39;estensione AEM e definisce:

+ La definizione del badge è definita in `getBadges()` utilizzando gli attributi di configurazione `id`, `prefix`, `suffix`, `backgroundColor` e `textColor`.
+ In questo esempio, il carattere `#` viene utilizzato per definire i limiti di questo badge, il che significa che qualsiasi stringa nell&#39;editor Rich Text circondata da `#` viene considerata un&#39;istanza di questo badge.

Vedi anche i dettagli chiave del widget dell’editor Rich Text:

+ Definizione del widget nella funzione `getWidgets()` con attributi `id`, `label` e `url`.
+ Il valore dell&#39;attributo `url`, un percorso URL relativo (`/index.html#/largeBookingsCustomerService`) per caricare il modale.


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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Aggiungi route `largeBookingsCustomerService` in `App.js`{#add-widgets-route}

Nel componente React principale `App.js`, aggiungi la route `largeBookingsCustomerService` per eseguire il rendering dell&#39;interfaccia utente per il percorso URL relativo sopra indicato.

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

### Crea componente React `LargeBookingsCustomerService`{#create-widget-react-component}

L&#39;interfaccia utente del widget o della finestra di dialogo viene creata utilizzando il framework [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html).

Il codice del componente React quando si aggiungono i dettagli del servizio clienti, racchiude la variabile del numero di telefono con il carattere dei badge registrati `#` per convertirla in badge, come `#${phoneNumber}#`, rendendola quindi non modificabile.

Di seguito sono riportati gli elementi di rilievo del codice `LargeBookingsCustomerService`:

+ Il rendering dell&#39;interfaccia utente viene eseguito utilizzando i componenti Spectrum di React, ad esempio [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ L&#39;array `largeGroupCustomerServiceList` ha una mappatura hardcoded del nome e del numero di telefono del rappresentante. In uno scenario reale, questi dati possono essere recuperati dall’azione Adobe AppBuilder o da sistemi esterni o da gateway API basati su provider cloud o home-end.
+ `guestConnection` è inizializzato con l&#39;hook `useEffect` [React](https://react.dev/reference/react/useEffect) ed è gestito come stato del componente. Viene utilizzato per comunicare con l’host AEM.
+ La funzione `handleCustomerServiceChange` ottiene il nome e il numero di telefono del rappresentante e aggiorna le variabili dello stato del componente.
+ La funzione `addCustomerServiceDetails` che utilizza l&#39;oggetto `guestConnection` fornisce istruzioni RTE da eseguire. In questo caso, istruzione `insertContent` e snippet di codice HTML.
+ Per rendere non modificabile il numero di telefono **** utilizzando i badge, viene aggiunto il carattere speciale `#` prima e dopo la variabile `phoneNumber`, ad esempio `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

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
