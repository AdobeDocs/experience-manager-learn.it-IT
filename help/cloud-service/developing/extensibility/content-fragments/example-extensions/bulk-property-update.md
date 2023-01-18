---
title: Esempio di aggiornamento delle proprietà in blocco AEM estensione della console Frammenti di contenuto
description: Esempio AEM’estensione della console Frammenti di contenuto che aggiorna in blocco una proprietà di Frammenti di contenuto.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
kt: 11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: a298dbd27dfda00c80d2098199eb418200af0233
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---


# Estensione esempio di aggiornamento proprietà bulk

![Aggiornamento delle proprietà in blocco](./assets/bulk-property-update/screenshot.png){align="center"}

Questo esempio AEM l’estensione della console Frammenti di contenuto è un [barra delle azioni](../action-bar.md) estensione che aggiorna in massa una proprietà Frammento di contenuto a un valore comune.

Il flusso funzionale dell&#39;estensione di esempio è il seguente:

![Flusso d&#39;azione Adobe I/O Runtime](./assets/bulk-property-update/flow.png){align="center"}

1. Seleziona Frammenti di contenuto e fai clic sul pulsante dell’estensione nella sezione [barra delle azioni](#extension-registration) apre [modale](#modal).
1. La [modale](#modal) visualizza un modulo di input personalizzato generato con [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/).
1. L’invio del modulo invia l’elenco dei frammenti di contenuto selezionati e l’host AEM al [azione Adobe I/O Runtime personalizzata](#adobe-io-runtime-action).
1. La [Azione Adobe I/O Runtime](#adobe-io-runtime-action) convalida gli input ed effettua richieste HTTP PUT per AEM per aggiornare i frammenti di contenuto selezionati.
1. Una serie di PUT HTTP per ciascun frammento di contenuto per aggiornare la proprietà specificata.
1. AEM as a Cloud Service persiste gli aggiornamenti delle proprietà al frammento di contenuto e restituisce risposte di successo o di errore all’azione Adobe I/O Runtime.
1. Il modale ha ricevuto la risposta dall&#39;azione Adobe I/O Runtime e visualizza un elenco di aggiornamenti in blocco riusciti.

Questo video esamina l’estensione dell’aggiornamento di massa delle proprietà di esempio, come funziona e come viene sviluppata.

>[!VIDEO](https://video.tv.adobe.com/v/3412296/?quality=12&learn=on)

## App Builder

Nell’esempio viene utilizzato un progetto esistente della console Adobe Developer e vengono utilizzate le seguenti opzioni quando si inizializza l’app Builder tramite `aio app init`.

+ Quali modelli desideri cercare?: `All Extension Points`
+ Scegli i modelli da installare:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Nome dell&#39;estensione: `Bulk property update`
+ Fornisci una breve descrizione dell&#39;estensione: `An example action bar extension that bulk updates a single property one or more content fragments.`
+ Con quale versione vuoi iniziare? `0.0.1`
+ Cosa vorreste fare dopo?
   + `Add a custom button to Action Bar`
      + Indicare il nome dell’etichetta del pulsante: `Bulk property update`
      + Devi mostrare un modale per il pulsante? `y`
   + `Add server-side handler`
      + Adobe I/O Runtime consente di richiamare il codice senza server su richiesta. Come assegnare un nome a questa azione?: `generic`

L’app di estensione generata per App Builder viene aggiornata come descritto di seguito.

## Indirizzi delle app{#app-routes}

La `src/aem-cf-console-admin-1/web-src/src/components/App.js` contiene [Reagisce router](https://reactrouter.com/en/main).

Esistono due set logici di percorsi:

1. Il primo percorso mappa le richieste al `index.html`, che richiama il componente React responsabile del [registrazione estensione](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. Il secondo set di percorsi mappa gli URL ai componenti React che eseguono il rendering del contenuto del modale dell’estensione. La `:selection` param rappresenta un percorso di frammento di contenuto elenco delimitato.

   Se l&#39;estensione dispone di più pulsanti per richiamare azioni discrete, ciascuno [registrazione estensione](#extension-registration) si abbina a un percorso qui definito.

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

## Registrazione delle estensioni

`ExtensionRegistration.js`, mappata su `index.html` route, è il punto di ingresso dell&#39;estensione AEM e definisce:

1. La posizione del pulsante di estensione viene visualizzata nell’esperienza di authoring AEM (`actionBar` o `headerMenu`)
1. Definizione del pulsante di estensione in `getButton()` Funzione
1. Il gestore di clic per il pulsante, nella `onClick()` Funzione

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'bulk-property-update',     // Unique ID for the button
              'label': 'Bulk property update',  // Button label 
              'icon': 'Edit'                    // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/bulk-property-update",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|'))
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Bulk property update",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

## Finestra modale

Ogni percorso dell&#39;estensione, come definito in [`App.js`](#app-routes), viene mappato su un componente React che esegue il rendering nel modale dell’estensione.

In questa app di esempio, esiste un componente React modale (`BulkPropertyUpdateModal.js`) con tre stati:

1. Caricamento, indica l&#39;attesa dell&#39;utente
1. Il modulo Aggiornamento proprietà bulk che consente all&#39;utente di specificare il nome e il valore della proprietà da aggiornare
1. Risposta dell’operazione di aggiornamento della proprietà in blocco, in cui sono elencati i frammenti di contenuto aggiornati e quelli che non è stato possibile aggiornare

È importante delegare a un [Azione Adobe I/O Runtime di AppBuilder](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/), che è un processo separato senza server in esecuzione in [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
L’utilizzo delle azioni di Adobe I/O Runtime per comunicare con AEM, consiste nell’evitare problemi di connettività CORS (Cross-Origin Resource Sharing).

Quando viene inviato il modulo di aggiornamento della proprietà bulk, viene visualizzata una `onSubmitHandler()` richiama l&#39;azione Adobe I/O Runtime, passando l&#39;host AEM corrente (dominio) e il token di accesso AEM dell&#39;utente, che a sua volta chiama il [API per frammenti di contenuto AEM](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) per aggiornare i frammenti di contenuto.

Quando viene ricevuta la risposta dall’azione Adobe I/O Runtime, il modale viene aggiornato per visualizzare i risultati dell’operazione di aggiornamento della proprietà in blocco.

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


## Azione Adobe I/O Runtime

Un&#39;app App Builder con estensione AEM può definire o utilizzare 0 o molte azioni Adobe I/O Runtime.
Le azioni di Adobe Runtime devono essere responsabili di un lavoro che richiede l’interazione con AEM o altri servizi Web di Adobe.

In questa app di esempio, l’azione Adobe I/O Runtime, che utilizza il nome predefinito `generic` - è responsabile:

1. Effettuare una serie di richieste HTTP all’API dei frammenti di contenuto AEM per aggiornare i frammenti di contenuto.
1. Raccolta delle risposte di queste richieste HTTP, combinandole in successi ed errori
1. Restituire l’elenco dei successi e degli errori di visualizzazione dal modale (`BulkPropertyUpdateModal.js`)

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
