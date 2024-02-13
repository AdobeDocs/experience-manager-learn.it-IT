---
title: Eventi AEM Assets per l’integrazione con la soluzione PIM
description: Scopri come integrare i sistemi AEM Assets e Product Information Management (PIM) per gli aggiornamenti dei metadati delle risorse.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
source-git-commit: 5d8ee3b9ab6fb974f7faebb1d0ce42d699e2063c
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 0%

---


# Eventi AEM Assets per l’integrazione con la soluzione PIM

Scopri come integrare AEM Assets e il sistema di gestione delle informazioni di prodotto (PIM) per aggiornare i metadati delle risorse **utilizzo di AEM Eventing**. Dopo aver ricevuto un evento AEM Assets, i metadati delle risorse possono essere aggiornati in AEM, PIM o in entrambi i sistemi, in base ai requisiti aziendali. Tuttavia, in questo esempio, aggiorniamo i metadati delle risorse in AEM.

Per eseguire l’aggiornamento dei metadati della risorsa **codice esterno all’AEM**, il [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) viene utilizzata una piattaforma senza server. Il flusso di elaborazione degli eventi è il seguente:

![Eventi AEM Assets per l’integrazione con la soluzione PIM](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. Il servizio di authoring AEM attiva un _Elaborazione risorsa completata_ al termine del caricamento di una risorsa.
1. L&#39;evento viene inviato al [Eventi Adobe I/O](https://developer.adobe.com/events/) servizio.
1. Il servizio Eventi di Adobe I/O trasmette l’evento al [Azione Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) per l’elaborazione.
1. L’azione Adobe I/O Runtime richiama l’API PIM oggetto di simulazione per recuperare metadati aggiuntivi come SKU e informazioni sul fornitore.
1. I metadati aggiuntivi PIM recuperati vengono quindi aggiornati in AEM Assets utilizzando [API di authoring di Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

## Prerequisiti

Per completare questa esercitazione, è necessario:

- Ambiente as a Cloud Service AEM con [Evento AEM abilitato](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Inoltre, il campione [Siti WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) il progetto deve essere distribuito su di esso.

- Accesso a [Console Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installato nel computer locale.

## Passaggi di sviluppo

Le fasi di sviluppo di alto livello sono:

1. [Crea progetto nella console di Adobe Developer (ADC)](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [Inizializza progetto per sviluppo locale](./runtime-action.md#initialize-project-for-local-development)
1. Configurare il progetto in ADC
1. Configura il servizio di authoring AEM per abilitare la comunicazione dei progetti ADC
1. Sviluppa azione di runtime che orchestra il recupero e l’aggiornamento dei metadati
1. Caricare una risorsa nel servizio Author dell’AEM e verificare l’aggiornamento dei metadati

Per i passaggi dettagliati sul 1-2 consulta [Azione Adobe I/O Runtime ed eventi AEM](./runtime-action.md#) esempio, e per 3-6 fare riferimento alle sezioni seguenti.

### Configurare il progetto nella console di Adobe Developer (ADC)

Per ricevere gli eventi AEM Assets ed eseguire l’azione Adobe I/O Runtime creata nel passaggio precedente, configura il progetto in ADC.

- In ADC, passa a [progetto](https://developer.adobe.com/console/projects). Seleziona la `Stage` workspace, è qui che è stata implementata l’azione di runtime.

- Clic **Aggiungi servizio** e seleziona **Evento** opzione. In **Aggiungi eventi** finestra di dialogo, seleziona **Experience Cloud** > **AEM Assets** e fai clic su **Successivo**. Segui i passaggi di configurazione aggiuntivi, seleziona l’istanza AEMCS, _Elaborazione risorsa completata_ evento, tipo di autenticazione server-to-server OAuth e altri dettagli.

  ![Evento AEM Assets - Aggiungi evento](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- Infine, nella sezione **Come ricevere gli eventi** step, expand **Azione runtime** e selezionare il _generico_ azione creata nel passaggio precedente. Clic **Salva eventi configurati**.

  ![Evento AEM Assets - Ricevi evento](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- Allo stesso modo, fai clic su **Aggiungi servizio** e seleziona **API** opzione. In **Aggiungere un’API** modale, seleziona **Experience Cloud** > **API AEM AS A CLOUD SERVICE** e fai clic su **Successivo**.

  ![Aggiungi API AEM as a Cloud Service - Configura progetto](../assets/examples/assets-pim-integration/add-aem-api.png)

- Quindi seleziona **OAuth Server-to-Server** per il tipo di autenticazione e fai clic su **Successivo**.

- Quindi seleziona la **Amministratori AEM-XXX** profilo di prodotto e fai clic su **Salva API configurata**. Per accedere alle funzioni e alle autorizzazioni granulari, il profilo di prodotto selezionato deve essere associato all’evento AEM Assets che produce l’ambiente AEMCS.

  ![Aggiungi API AEM as a Cloud Service - Configura progetto](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

### Configura il servizio di authoring AEM per abilitare la comunicazione dei progetti ADC

Per aggiornare i metadati delle risorse in AEM dal progetto ADC di cui sopra, configura il servizio Author AEM con l’ID client del progetto ADC. Il _id client_ viene aggiunta come variabile di ambiente utilizzando [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html#add-variables) UI.

- Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/), seleziona **Programma** > **Ambiente** > **Puntini di sospensione** > **Visualizza dettagli** > **Configurazione** scheda.

  ![Adobe Cloud Manager - Configurazione dell’ambiente](../assets/examples/assets-pim-integration/cloud-manager-environment-configuration.png)

- Then **Aggiungi configurazione** e immettere i dettagli della variabile come

  | Nome | Valore | Servizio AEM | Tipo |
  | ----------- | ----------- | ----------- | ----------- |
  | ADOBE_SUPPLIED_CLIENT_ID | &lt;COPY_FROM_ADC_PROJECT_CREDENTIALS> | Autore | Variabile |

  ![Adobe Cloud Manager - Configurazione dell’ambiente](../assets/examples/assets-pim-integration/add-environment-variable.png)

- Clic **Aggiungi** e **Salva** la configurazione.

### Azione di sviluppo runtime

Per eseguire il recupero e l’aggiornamento dei metadati, aggiorna la sezione creata automaticamente _generico_ codice azione in `src/dx-excshell-1/actions/generic` cartella.

Per il codice completo, fai riferimento al file WKND-Assets-PIM-Integration.zip allegato, mentre la sezione seguente evidenzia i file chiave.

- Il `src/dx-excshell-1/actions/generic/mockPIMCommunicator.js` Il file ironizza sulla chiamata API PIM per recuperare metadati aggiuntivi come SKU e nome del fornitore.

  ```javascript
  /**
   * Mock PIM API to get the product data such as SKU, Supplier, etc.
   *
   * In a real-world scenario, this function would call the PIM API to get the product data.
   * For this example, we are returning mock data.
   *
   * @param {string} assetId - The assetId to get the product data.
   */
  module.exports = {
      async getPIMData(assetId) {
          if (!assetId) {
          throw new Error('Invalid assetId');
          }
          // Mock response data for demo purposes
          const data = {
          SKUID: 'MockSKU 123',
          SupplierName: 'mock-supplier',
          // ... other product data
          };
          return data;
      },
  };
  ```

- Il `src/dx-excshell-1/actions/generic/aemCommunicator.js` aggiorna i metadati delle risorse in AEM utilizzando [API di authoring di Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

  ```javascript
  const fetch = require('node-fetch');
  
  ...
  
  /**
  *  Get IMS Access Token using Client Credentials Flow
  *
  * @param {*} clientId - IMS Client ID from ADC project's OAuth Server-to-Server Integration
  * @param {*} clientSecret - IMS Client Secret from ADC project's OAuth Server-to-Server Integration
  * @param {*} scopes - IMS Meta Scopes from ADC project's OAuth Server-to-Server Integration as comma separated strings
  * @returns {string} - Returns the IMS Access Token
  */
  async function getIMSAccessToken(clientId, clientSecret, scopes) {
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';
  
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
    };
  
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();
  
    return responseJSON.access_token;
  }    
  
  async function updateAEMAssetMetadata(metadataDetails, aemAssetEvent, params) {
    ...
    // Transform the metadata details to JSON Patch format,
    // see https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/patchAssetMetadata
    const transformedMetadata = Object.keys(metadataDetails).map((key) => ({
      op: 'add',
      path: `wknd-${key.toLowerCase()}`,
      value: metadataDetails[key],
    }));
  
    ...
  
    // Get ADC project's OAuth Server-to-Server Integration credentials
    const clientId = params.ADC_CECREDENTIALS_CLIENTID;
    const clientSecret = params.ADC_CECREDENTIALS_CLIENTSECRET;
    const scopes = params.ADC_CECREDENTIALS_METASCOPES;
  
    // Get IMS Access Token using Client Credentials Flow
    const access_token = await getIMSAccessToken(clientId, clientSecret, scopes);
  
    // Call AEM Author service to update the metadata using Assets Author API
    // See https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/
    const res = await fetch(`${aemAuthorHost}/adobe/assets/${assetId}/metadata`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json-patch+json',
        'If-Match': '*',
        'X-Adobe-Accept-Experimental': '1',
        'X-Api-Key': 'aem-assets-management-api', // temporary value
        Authorization: `Bearer ${access_token}`,
      },
      body: JSON.stringify(transformedMetadata),
    });
  
    ...
  }
  
  module.exports = { updateAEMAssetMetadata };
  ```

  Il `.env` file memorizza i dettagli delle credenziali server-to-server OAuth del progetto ADC e vengono passati come parametri all&#39;azione utilizzando `ext.config.yaml` file. Consulta la sezione [File di configurazione di App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) per la gestione di segreti e parametri di azione.

- Il `src/dx-excshell-1/actions/model` la cartella contiene `aemAssetEvent.js` e `errors.js` file, utilizzati dall’azione per analizzare l’evento ricevuto e gestire rispettivamente gli errori.

- Il `src/dx-excshell-1/actions/generic/index.js` utilizza i moduli sopra indicati per orchestrare il recupero e l’aggiornamento dei metadati.

  ```javascript
  ...
  
  let responseMsg;
  // handle the challenge probe request, they are sent by I/O to verify the action is valid
  if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
  } else {
    logger.info('AEM Asset Event request received');
  
    // create AEM Asset Event object from request parameters
    const aemAssetEvent = new AEMAssetEvent(params);
  
    // Call mock PIM API to get the product data such as SKU, Supplier, etc.
    const mockPIMData = await mockPIMAPI.getPIMData(
      aemAssetEvent.getAssetName(),
    );
    logger.info('Mock PIM API response', mockPIMData);
  
    // Update PIM received data in AEM as Asset metadata
    const aemUpdateStatus = await updateAEMAssetMetadata(
      mockPIMData,
      aemAssetEvent,
      params,
    );
    logger.info('AEM Asset metadata update status', aemUpdateStatus);
  
    if (aemUpdateStatus) {
      // create response message
      responseMsg = JSON.stringify({
        message:
          'AEM Asset Event processed successfully, updated the asset metadata with PIM data.',
        assetdata: {
          assetName: aemAssetEvent.getAssetName(),
          assetPath: aemAssetEvent.getAssetPath(),
          assetId: aemAssetEvent.getAssetId(),
          aemHost: aemAssetEvent.getAEMHost(),
          pimdata: mockPIMData,
        },
      });
    } 
  
    // response object
    const response = {
      statusCode: 200,
      body: responseMsg,
    };
  
    // Return the response to the caller
    return response;
  
    ...
  }
  ```

Distribuisci l’azione aggiornata in Adobe I/O Runtime utilizzando il seguente comando:

```bash
$ aio app deploy
```

### Caricamento delle risorse e verifica dei metadati

Per verificare l’integrazione di AEM Assets e PIM, effettua le seguenti operazioni:

- Per visualizzare i metadati fittizi forniti da PIM come SKU e Nome fornitore, crea uno schema di metadati in AEM Assets consulta [Schemi di metadati](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/metadata-schemas.html) che visualizza le proprietà dei metadati SKU e nome fornitore.

- Carica una risorsa nel servizio di authoring AEM e verifica l’aggiornamento dei metadati.

  ![Aggiornamento metadati AEM Assets](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## Concetto e soluzioni chiave

La sincronizzazione dei metadati delle risorse tra AEM e altri sistemi come PIM è spesso necessaria all’interno dell’azienda. Utilizzo dell’AEM In caso di necessità, tali requisiti possono essere soddisfatti.

- Il codice dei metadati delle risorse viene eseguito al di fuori dell’AEM, evitando il carico sul servizio di authoring dell’AEM e quindi su un’architettura basata su eventi che viene scalata in modo indipendente.
- La nuova API Assets Author viene utilizzata per aggiornare i metadati delle risorse in AEM.
- L&#39;autenticazione API utilizza OAuth server-to-server (ossia il flusso delle credenziali del client); vedi [Guida all&#39;implementazione delle credenziali server-to-server di OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/).
- Al posto di Adobe I/O Runtime Actions, è possibile utilizzare altri webhook o Amazon EventBridge per ricevere l&#39;evento AEM Assets ed elaborare l&#39;aggiornamento dei metadati.
- Eventi asset tramite AEM Eventing consente alle aziende di automatizzare e semplificare i processi critici, promuovendo l&#39;efficienza e la coerenza tra gli ecosistemi di contenuti.

