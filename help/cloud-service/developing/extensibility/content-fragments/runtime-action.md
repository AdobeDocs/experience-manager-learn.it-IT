---
title: Estensione della console Frammenti di contenuto AEM - Azioni Adobe I/O Runtime
description: Scopri come creare un’estensione modale della console Frammenti di contenuto AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 3062900a-0461-4c6f-81e6-c76a7f613804
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 2%

---

# Azione Adobe I/O Runtime

![Azioni runtime di estensione dei frammenti di contenuto AEM](./assets/runtime-action/action-runtime-flow.png){align="center"}

Le estensioni dei frammenti di contenuto AEM possono facoltativamente includere un numero qualsiasi di [Azioni Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).

Le azioni Adobe I/O Runtime sono funzioni senza server che possono essere richiamate dall’estensione. Le azioni sono utili per eseguire lavori che richiedono l’interazione con l’AEM o altri servizi web Adobi.
Le azioni in genere sono più utili per eseguire attività di lunga durata (qualcosa di più di qualche secondo) o per effettuare richieste HTTP all’AEM o ad altri servizi web.

L&#39;utilizzo delle azioni di Adobe I/O Runtime per eseguire il lavoro offre i seguenti vantaggi:

+ Le azioni sono funzioni senza server eseguite al di fuori del contesto di un browser, eliminando la necessità di preoccuparsi di CORS
+ Le azioni non possono essere interrotte dall’utente (ad esempio, aggiornamento del browser)
+ Le azioni sono asincrone, quindi possono essere eseguite per tutto il tempo necessario senza bloccare l’utente

Nel contesto delle estensioni dei frammenti di contenuto dell’AEM, le azioni vengono utilizzate il più delle volte per comunicare direttamente con gli as a Cloud Service dell’AEM, spesso:

+ Raccolta di dati correlati dall’AEM sui frammenti di contenuto
+ Esecuzione di operazioni personalizzate sui frammenti di contenuto
+ Creazione personalizzata di frammenti di contenuto

Mentre l’estensione Frammento di contenuto AEM viene visualizzata nella console Frammento di contenuto, le estensioni e le relative azioni di supporto possono richiamare qualsiasi API HTTP AEM disponibile, inclusi gli endpoint API AEM personalizzati.

## Richiama un&#39;azione

Le azioni Adobe I/O Runtime vengono richiamate principalmente da due posizioni in un’estensione Frammento di contenuto AEM:

1. Il [di registrazione dell&#39;estensione](./extension-registration.md) `onClick(..)` handler
1. Entro un [modale](./modal.md)

### Dalla registrazione dell&#39;estensione

Le azioni Adobe I/O Runtime possono essere richiamate direttamente dal codice di registrazione dell’estensione. Il caso d’uso più comune consiste nell’associare un’azione a un [menu intestazione](./header-menu.md#no-modal)pulsante di che non utilizza [modali](./modal.md).

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
// allActions is an object containing all the actions defined in the extension's manifest
import allActions from '../config.json'

// actionWebInvoke is a helper that invokes an action
import actionWebInvoke from '../utils'
...
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your header menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension',  // Unique ID for the button
              'label': 'My header menu extension',       // Button label 
              'icon': 'Edit'                             // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          onClick() {
            // Set the HTTP headers required to access the Adobe I/O runtime action
            const headers = {
              'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
              'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
            };

            // Set the parameters to pass to the Adobe I/O Runtime action
            const params = {
              aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`, // Pass in the AEM host if the action interacts with AEM
              aemAccessToken: guestConnection.sharedContext.get('auth').imsToken
            };

            try {
              // Invoke Adobe I/O Runtime action named `generic`, with the configured headers and parameters.
              const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);
            } catch (e) {
              // Log and store any errors
              console.error(e)
            }           
          }
        }
      }
    })
  }
  init().catch(console.error);
}
```

### Da modale

Le azioni Adobe I/O Runtime possono essere richiamate direttamente dai modali per eseguire lavori più coinvolti, in particolare lavori che si basano sulla comunicazione con AEM as a Cloud Service, servizio web Adobe o anche servizi di terze parti.

Le azioni Adobe I/O Runtime sono applicazioni JavaScript basate su Node.js che vengono eseguite nell’ambiente Adobe I/O Runtime senza server. L’estensione SPA può indirizzare queste azioni tramite HTTP.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()
  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (!actionResponse) {
    // Else if the modal is ready to render and has not called the Adobe I/O Runtime action yet
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="cta" onPress={doWork}>Do work</Button>
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  } else {
    // Else the modal has called the Adobe I/O Runtime action and is ready to render the response
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        Done! The response from the action is: { actionResponse }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }

  /**
   * Invoke the Adobe I/O Runtime action and store the response in the React component's state.
   */
  async function doWork() {
    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,
      contentFragmentPaths: contentFragmentPaths
    };

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      // Invoke the Adobe I/O Runtime action named `generic`.
      const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);
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

+ `src/aem-cf-console-admin-1/actions/generic/index.js`

```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

async function main (params) {
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    logger.debug(stringParameters(params))

    // Check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'contentFragmentPaths' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    // Example HTTP request to AEM payload; This updates all 'title' properties of the Content Fragments to 'Hello World'
    const body = {
      "properties": {
        "elements": {
          "title": {
            "value": "Hello World"
          }
        }
      }
    };

    let results = await Promise.all(params.contentFragmentPaths.map(async (contentFragmentPath) => {
      // Invoke the AEM HTTP Assets Content Fragment API to update each Content Fragment
      // The AEM host is passed in as a parameter to the Adobe I/O Runtime action
      const res = await fetch(`${params.aemHost}${contentFragmentPath.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          // Pass in the accessToken as AEM Author service requires authentication/authorization
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    // Return a response to the AEM Content Fragment extension React application
    const response = {
      statusCode: 200,
      body: results
    };
    return response;

  } catch (error) {
    logger.error(error)
    return errorResponse(500, 'server error', logger)
  }
}
```

## API HTTP AEM

Le seguenti API HTTP per AEM sono comunemente utilizzate per interagire con AEM da estensioni:

+ [API GraphQL dell’AEM](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=it)
+ [API HTTP di AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html)
   + [Supporto dei frammenti di contenuto nell’API HTTP di AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/assets-api-content-fragments.html)
+ [API QueryBuilder AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api.html)
+ [Riferimento API AEM as a Cloud Service completo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/reference-materials.html)


## Moduli npm di Adobe

Di seguito sono riportati alcuni utili moduli npm per lo sviluppo di azioni Adobe I/O Runtime:

+ [@adobe/aio-sdk](https://www.npmjs.com/package/@adobe/aio-sdk)
   + [SDK core](https://github.com/adobe/aio-sdk-core)
   + [Libreria di stati](https://github.com/adobe/aio-lib-state)
   + [Libreria file](https://github.com/adobe/aio-lib-files)
   + [Libreria Adobe Target](https://github.com/adobe/aio-lib-target)
   + [Libreria Adobe Analytics](https://github.com/adobe/aio-lib-analytics)
   + [Libreria Adobe Campaign Standard](https://github.com/adobe/aio-lib-campaign-standard)
   + [Libreria dei profili cliente di Adobe](https://github.com/adobe/aio-lib-customer-profile)
   + [Libreria dati cliente Adobe Audience Manager](https://github.com/adobe/aio-lib-audience-manager-cd)
   + [Eventi Adobe I/O](https://github.com/adobe/aio-lib-events)
+ [@adobe/aio-lib-core-networking](https://github.com/adobe/aio-lib-core-networking)
+ [@adobe/node-httptransfer](https://github.com/adobe/node-httptransfer)
