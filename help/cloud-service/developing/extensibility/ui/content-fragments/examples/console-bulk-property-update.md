---
title: 'Aggiornamento di proprietà in blocco, esempio: estensione della console Frammenti di contenuto di AEM'
description: Un esempio dell’estensione della console Frammenti di contenuto di AEM che aggiorna in blocco una proprietà di Frammenti di contenuto.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2024-01-26T00:00:00Z
exl-id: fbfb5c10-95f8-4875-88dd-9a941d7a16fd
duration: 1362
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---

# Estensione di esempio per aggiornamento proprietà in blocco

>[!VIDEO](https://video.tv.adobe.com/v/3412296?quality=12&learn=on)

Questo esempio di estensione della Console Frammenti di contenuto di AEM è un&#39;estensione della [barra delle azioni](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) che aggiorna in blocco una proprietà Frammento di contenuto a un valore comune.

Il flusso funzionale dell’estensione di esempio è il seguente:

![Flusso azioni di Adobe I/O Runtime](./assets/bulk-property-update/flow.png){align="center"}

1. Seleziona Frammenti di contenuto e fai clic sul pulsante dell&#39;estensione nella [barra delle azioni](#extension-registration) per aprire [modale](#modal).
2. [modale](#modal) visualizza un modulo di input personalizzato creato con [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/).
3. L&#39;invio del modulo invia l&#39;elenco dei frammenti di contenuto selezionati e l&#39;host AEM all&#39;[azione Adobe I/O Runtime personalizzata](#adobe-io-runtime-action).
4. L&#39;[azione Adobe I/O Runtime](#adobe-io-runtime-action) convalida gli input e invia richieste HTTP PUT ad AEM per aggiornare i frammenti di contenuto selezionati.
5. Una serie di HTTP PUT per ogni frammento di contenuto per aggiornare la proprietà specificata.
6. AEM as a Cloud Service salva in modo permanente gli aggiornamenti delle proprietà nel frammento di contenuto e restituisce risposte di esito positivo o negativo all’azione Adobe I/O Runtime.
7. Il modale ha ricevuto la risposta dall’azione Adobe I/O Runtime e visualizza un elenco degli aggiornamenti in blocco riusciti.

## Punto di estensione

Questo esempio si estende al punto di estensione `actionBar` per aggiungere un pulsante personalizzato alla console Frammenti di contenuto.

| Interfaccia utente di AEM estesa | Punto di estensione |
| ------------------------ | --------------------- |
| [Console Frammenti di contenuto](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [Barra delle azioni](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |


## Estensione di esempio

Nell&#39;esempio viene utilizzato un progetto Adobe Developer Console esistente e le opzioni seguenti vengono utilizzate durante l&#39;inizializzazione dell&#39;app App Builder tramite `aio app init`.

+ Scegliere i modelli da cercare: `All Extension Points`
+ Scegli i modelli da installare:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Specificare il nome dell&#39;estensione: `Bulk property update`
+ Fornire una breve descrizione dell&#39;estensione: `An example action bar extension that bulk updates a single property one or more content fragments.`
+ Quale versione iniziare?: `0.0.1`
+ Come procedere?
   + `Add a custom button to Action Bar`
      + Specificare il nome dell&#39;etichetta per il pulsante: `Bulk property update`
      + È necessario mostrare un modale per il pulsante? `y`
   + `Add server-side handler`
      + Adobe I/O Runtime consente di richiamare il codice senza server su richiesta. Specificare il nome dell&#39;azione: `generic`

L&#39;app di estensione App Builder generata viene aggiornata come descritto di seguito.

### Route delle app{#app-routes}

`src/aem-cf-console-admin-1/web-src/src/components/App.js` contiene il [router React](https://reactrouter.com/en/main).

Esistono due insiemi logici di route:

1. La prima route mappa le richieste a `index.html`, che richiama il componente React responsabile della [registrazione estensione](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. Il secondo set di route associa gli URL ai componenti React che eseguono il rendering del contenuto della finestra modale dell’estensione. Il parametro `:selection` rappresenta un percorso di frammento di contenuto elenco delimitato.

   Se l&#39;estensione dispone di più pulsanti per richiamare azioni discrete, ogni [registrazione estensione](#extension-registration) viene mappata a una route definita qui.

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

### Registrazione dell’estensione

`ExtensionRegistration.js`, mappato alla route `index.html`, è il punto di ingresso per l&#39;estensione AEM e definisce:

1. La posizione del pulsante di estensione viene visualizzata nell&#39;esperienza di authoring di AEM (`actionBar` o `headerMenu`)
1. Definizione del pulsante di estensione nella funzione `getButtons()`
1. Gestore di clic per il pulsante, nella funzione `onClick()`

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Some unique ID for the extension used to facilitate communication between the extension and Content Fragment Console
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButtons() {
            return [
              {
                id: "examples.action-bar.bulk-property-update", // Unique ID for the button
                label: "Bulk property update", // Button label
                icon: "OpenIn", // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
                // Click handler for the extension button
                onClick(selections) {
                  // Collect the selected content fragment paths
                  const selectionIds = selections.map(
                    (selection) => selection.id
                  );

                  // Create a URL that maps to the
                  const modalURL =
                    "/index.html#" +
                    generatePath(
                      "/content-fragment/:selection/bulk-property-update",
                      {
                        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                        selection: encodeURIComponent(selectionIds.join("|")),
                      }
                    );

                  // Open the route in the extension modal using the constructed URL
                  guestConnection.host.modal.showUrl({
                    // The modal title
                    title: "Bulk property update",
                    url: modalURL,
                  });
                },
              },
            ];
          },
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Finestra modale

Ogni route dell&#39;estensione, definita in [`App.js`](#app-routes), viene mappata a un componente React che esegue il rendering nella finestra modale dell&#39;estensione.

Nell&#39;app di esempio è presente un componente React modale (`BulkPropertyUpdateModal.js`) con tre stati:

1. Caricamento, che indica che l’utente deve attendere
1. Maschera Aggiornamento proprietà in blocco che consente all&#39;utente di specificare il nome e il valore della proprietà da aggiornare
1. Risposta dell’operazione di aggiornamento in blocco delle proprietà, in cui sono elencati i frammenti di contenuto aggiornati e quelli che non è stato possibile aggiornare

È importante sottolineare che qualsiasi interazione con AEM da parte dell&#39;estensione deve essere delegata a un&#39;azione [AppBuilder Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/), che è un processo separato senza server in esecuzione in [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
L’utilizzo delle azioni di Adobe I/O Runtime per comunicare con AEM consiste nell’evitare problemi di connettività CORS (Cross-Origin Resource Sharing).

Quando il modulo Bulk Property Update viene inviato, un `onSubmitHandler()` personalizzato richiama l&#39;azione Adobe I/O Runtime, passando l&#39;host AEM (dominio) corrente e il token di accesso AEM dell&#39;utente, che a sua volta chiama l&#39;[API AEM Content Fragment](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) per aggiornare i frammenti di contenuto.

Quando viene ricevuta la risposta dall’azione Adobe I/O Runtime, il modale viene aggiornato per visualizzare i risultati dell’operazione di aggiornamento in blocco delle proprietà.

+ `src/aem-cf-console-admin-1/web-src/src/components/BulkPropertyUpdateModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Form,
  Provider,
  Content,
  defaultTheme,
  ContextualHelp,
  Text,
  TextField,
  ButtonGroup,
  Button,
  Heading,
  ListView,
  Item,
} from '@adobe/react-spectrum'

import Spinner from "./Spinner"

import { useParams } from "react-router-dom"

import allActions from '../config.json'
import actionWebInvoke from '../utils'

import { extensionId } from "./Constants"

export default function BulkPropertyUpdateModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState()
  
  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  const [propertyName, setPropertyName] = useState(null);
  const [propertyValue, setPropertyValue] = useState(null);
  const [validationState, setValidationState] = useState({});

  // Get the selected content fragment paths from the route parameter `:selection`
  let { selection } = useParams();
  let fragmentIds = selection?.split('|') || [];
  
  console.log('Content Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error("Unable to locate a list of Content Fragments to update.")
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
       // extensionId is the unique id of this extension (you can make this up as long as its unique) .. in this case its `bulk-property-update` pulled out into Constants.js as it is also referenced in ExtensionRegistration.js
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])


  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (actionInvokeInProgress) {
    // If the bulk property action has been invoked but not completed, display a loading spinner
    return <Spinner />
  } else if (actionResponse) {
    // If the bulk property action has completed, display the response
    return renderResponse();
  } else {
    // Else display the bulk property update form
    return renderForm();
  }

  /**
   * Renders the Bulk Property Update form. 
   * This form has two fields:
   * - Property Name: The name of the Content Fragment property name to update
   * - Property Value: the value the Content Fragment property, specified by the Property Name, will be updated to
   * 
   * @returns the Bulk Property Update form
   */
  function renderForm() {
    return (
      // Use React Spectrum components to render the form
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">
          <Flex width="100%">
            <Form 
              width="100%">
              <TextField label="Property name"
                          isRequired={true}
                          validationState={validationState?.propertyName}
                onChange={setPropertyName}
                contextualHelp={
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>The <strong>Property name</strong> must be a valid for the Content Fragment model(s) the selected Content Fragments implement.</Text>
                    </Content>
                  </ContextualHelp>
                } />

              <TextField
                label="Property value"
                validationState={validationState?.propertyValue}
                onChange={setPropertyValue} />

              <ButtonGroup align="start" marginTop="size-200">
                <Button variant="cta" onPress={onSubmitHandler}>Update {fragmentIds.length} Content Fragments</Button>
              </ButtonGroup>
            </Form>
          </Flex>

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>
    )
  }
  /**
   * Display the response from the Adobe I/O Runtime action in the modal.
   * This includes:
   * - A list of content fragments that were updated successfully
   * - A list a content fragments that failed to update
   * 
   * @returns the response view
   */
  function renderResponse() {
    // Separate the successful and failed content fragments updates
    const successes = actionResponse.filter(item => item.status === 200);
    const failures = actionResponse.filter(item => item.status !== 200);

    return (
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">

          <Text>Bulk updated property <strong>{propertyName}</strong> with value <strong>{propertyValue}</strong></Text>

          {/* Render the list of content fragments that were updated successfully */}
          {successes.length > 0 &&
            <><Heading level="4">{successes.length} Content Fragments successfully updated</Heading>
              <ListView
                items={successes}
                selectionMode="none"
                aria-label="Successful updates"
              >
                {(item) => (
                  <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                    {item.fragmentId.split('/').pop()}
                  </Item>
                )}
              </ListView></>}

          {/* Render the list of content fragments that failed to update */}
          {failures.length > 0 &&
            <><Heading level="4">{failures.length} Content Fragments failed to update</Heading><ListView
              items={failures}
              selectionMode="none"
              aria-label="Failed updates"
            >
              {(item) => (
                <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                  {item.fragmentId.split('/').pop()}
                </Item>
              )}
            </ListView></>}

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>);
  }

  /**
   * Provide a close button for the modal, else it cannot be closed (without refreshing the browser window)
   * 
   * @returns a button that closes the modal.
   */
   function renderCloseButton() {
    return (
      <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
        <ButtonGroup align="end">
          <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
        </ButtonGroup>
      </Flex>
    );
  }

  /**
   * Handle the Bulk Property Update form submission.
   * This function calls the supporting Adobe I/O Runtime action to update the selected Content Fragments, and then returns the response for display in the modal
   * When invoking the Adobe I/O Runtime action, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The list of Content Fragment paths to update
   * - The Content Fragment property name to update
   * - The value to update the Content Fragment property to
   * 
   * @returns a list of content fragment update successes and failures
   */
  async function onSubmitHandler() {
    // Validate the form input fields
    if (propertyName?.length > 1) {
      setValidationState({propertyName: 'valid', propertyValue: 'valid'});
    } else {
      setValidationState({propertyName: 'invalid', propertyValue: 'valid'});
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    console.log('headers', headers);

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentIds: fragmentIds,
      propertyName: propertyName,
      propertyValue: propertyValue
    };

    // Invoke the Adobe I/O Runtime action named `generic`. This name defined in the `ext.config.yaml` file.
    const action = 'generic';

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      const actionResponse = await actionWebInvoke(allActions[action], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);

      console.log(`Response from ${action}:`, actionResponse)
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```


### Azione Adobe I/O Runtime

Un’app App Builder per estensione AEM può definire o utilizzare 0 o più azioni Adobe I/O Runtime.
Le azioni di Adobe Runtime devono essere lavoro responsabile che richiede l’interazione con AEM o altri servizi web di Adobe.

In questa app di esempio, l&#39;azione Adobe I/O Runtime, che utilizza il nome predefinito `generic`, è responsabile di:

1. Effettuare una serie di richieste HTTP all’API dei frammenti di contenuto di AEM per aggiornare i frammenti di contenuto.
1. Raccolta delle risposte di queste richieste HTTP, con raggruppamento in operazioni riuscite ed errori
1. Restituzione dell&#39;elenco delle operazioni riuscite e non riuscite per la visualizzazione da parte del modale (`BulkPropertyUpdateModal.js`)

+ `src/aem-cf-console-admin-1/actions/generic/index.js`


```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

// main function that will be executed by Adobe I/O Runtime
async function main (params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action')

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params))

    // check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'fragmentIds', 'propertyName', 'propertyValue' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    const body = {
      "properties": {
        "elements": {
          [params.propertyName]: {
            "value": params.propertyValue
          }
        }
      }
    };

    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    let results = await Promise.all(params.fragmentIds.map(async (fragmentId) => {

      logger.info(`Updating fragment ${fragmentId} with property ${params.propertyName} and value ${params.propertyValue}`);

      const res = await fetch(`${params.aemHost}${fragmentId.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    const response = {
      statusCode: 200,
      body: results
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the A
     return response;

  } catch (error) {
    // log any server errors
    logger.error(error)
    // return with 500
    return errorResponse(500, 'server error', logger)
  }
}

exports.main = main
```
