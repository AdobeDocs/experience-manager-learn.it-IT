---
title: Elaborazione di eventi AEM tramite Azione Adobe I/O Runtime
description: Scopri come elaborare gli eventi AEM ricevuti utilizzando Azione Adobe I/O Runtime.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---


# Elaborazione di eventi AEM tramite Azione Adobe I/O Runtime

Scopri come elaborare gli eventi AEM ricevuti utilizzando [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Azione. Questo esempio migliora quello precedente [Azione Adobe I/O Runtime ed eventi AEM](runtime-action.md), assicurati di averlo completato prima di procedere con questo.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

In questo esempio, l’elaborazione dell’evento memorizza i dati dell’evento originale e l’evento ricevuto come messaggio di attività nell’archiviazione di Adobe I/O Runtime. Tuttavia, se l’evento è di _Frammento di contenuto modificato_ digita, quindi chiama anche il servizio di authoring AEM per trovare i dettagli della modifica. Infine, visualizza i dettagli dell’evento in un’applicazione a pagina singola (SPA).

## Prerequisiti

Per completare questa esercitazione, è necessario:

- Ambiente as a Cloud Service AEM con [Evento AEM abilitato](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Inoltre, il campione [Siti WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) il progetto deve essere distribuito su di esso.

- Accesso a [Console Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installato nel computer locale.

- Progetto inizializzato localmente dall’esempio precedente [Azione Adobe I/O Runtime ed eventi AEM](./runtime-action.md#initialize-project-for-local-development).


## Azione processore eventi AEM

In questo esempio, il processore eventi [azione](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) esegue le seguenti attività:

- Analizza l&#39;evento ricevuto in un messaggio di attività.
- Se l’evento ricevuto è di _Frammento di contenuto modificato_ digita, richiama il servizio di authoring AEM per trovare i dettagli della modifica.
- Persiste i dati dell’evento originale, il messaggio dell’attività e gli eventuali dettagli di modifica nell’archiviazione di Adobe I/O Runtime.

Per eseguire le attività di cui sopra, iniziamo aggiungendo un’azione al progetto, sviluppando moduli JavaScript per eseguire le attività di cui sopra e infine aggiornando il codice dell’azione per utilizzare i moduli sviluppati.

Fai riferimento alla allegata [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) file per il codice completo, e la sezione seguente evidenzia i file chiave.

### Aggiungi azione

- Per aggiungere un&#39;azione, eseguire il comando seguente:

  ```bash
  aio app add action
  ```

- Seleziona `@adobe/generator-add-action-generic` come modello di azione, assegna all’azione il nome `aem-event-processor`.

  ![Aggiungi azione](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### Sviluppare moduli JavaScript

Per eseguire le attività sopra indicate, sviluppiamo i seguenti moduli JavaScript.

- Il `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` Il modulo determina se l’evento ricevuto è di _Frammento di contenuto modificato_ tipo.

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- Il `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` Il modulo chiama il servizio di authoring AEM per trovare i dettagli della modifica.

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  Fai riferimento a [Tutorial sulle credenziali del servizio AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) per saperne di più. Inoltre, il [File di configurazione di App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) per la gestione di segreti e parametri di azione.

- Il `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` Il modulo memorizza i dati dell’evento originale, il messaggio di attività e gli eventuali dettagli di modifica nell’archiviazione di Adobe I/O Runtime.

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### Aggiorna codice azione

Infine, aggiorna il codice dell’azione in `src/dx-excshell-1/actions/aem-event-processor/index.js` per utilizzare i moduli sviluppati.

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## Risorse aggiuntive

- Il `src/dx-excshell-1/actions/model` la cartella contiene `aemEvent.js` e `errors.js` file, utilizzati dall’azione per analizzare l’evento ricevuto e gestire rispettivamente gli errori.
- Il `src/dx-excshell-1/actions/load-processed-aem-events` La cartella contiene il codice dell’azione, che viene utilizzato dall’SPA per caricare gli eventi AEM elaborati dall’archivio Adobe I/O Runtime.
- Il `src/dx-excshell-1/web-src` contiene il codice SPA, che visualizza gli Eventi AEM elaborati.
- Il `src/dx-excshell-1/ext.config.yaml` Il file contiene la configurazione dell&#39;azione e i parametri.

## Concetto e soluzioni chiave

I requisiti di elaborazione degli eventi differiscono da progetto a progetto, tuttavia i passaggi chiave da questo esempio sono:

- L’elaborazione dell’evento può essere eseguita utilizzando Azione di Adobe I/O Runtime.
- L’azione Runtime può comunicare con sistemi quali applicazioni interne, soluzioni di terze parti e soluzioni di Adobe.
- L’azione runtime funge da punto di ingresso a un processo aziendale progettato per una modifica del contenuto.





