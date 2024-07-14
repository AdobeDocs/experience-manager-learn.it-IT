---
title: Eventi AEM Assets per l’integrazione con la soluzione PIM
description: Scopri come integrare i sistemi AEM Assets e Product Information Management (PIM) per gli aggiornamenti dei metadati delle risorse.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 761
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
exl-id: 070cbe54-2379-448b-bb7d-3756a60b65f0
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1116'
ht-degree: 0%

---

# Eventi AEM Assets per l’integrazione con la soluzione PIM

>[!IMPORTANT]
>
>Questa esercitazione utilizza API sperimentali di AEM as a Cloud Service. Per accedere a queste API, devi accettare un contratto software preliminare e far sì che queste API siano abilitate manualmente per il tuo ambiente da ingegneri Adobi. Per richiedere l’accesso, contatta il supporto Adobe.

Scopri come integrare AEM Assets con un sistema di terze parti, ad esempio un sistema di gestione delle informazioni sui prodotti (PIM) o di gestione delle linee di prodotto (PLM), per aggiornare i metadati delle risorse **utilizzando eventi I/O AEM nativi**. Dopo aver ricevuto un evento AEM Assets, i metadati della risorsa possono essere aggiornati nell’AEM, nel PIM o in entrambi i sistemi, in base ai requisiti aziendali. Tuttavia, questo esempio illustra come aggiornare i metadati delle risorse in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3427592?quality=12&learn=on)

Per eseguire l&#39;aggiornamento dei metadati della risorsa **code all&#39;esterno di AEM**, [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/), viene utilizzata una piattaforma senza server.

Il flusso di elaborazione degli eventi è il seguente:

![Eventi AEM Assets per integrazione PIM](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. Il servizio di authoring AEM attiva un evento _Elaborazione risorsa completata_ quando viene completato il caricamento di una risorsa e tutte le attività di elaborazione delle risorse sono state completate. In attesa del completamento dell’elaborazione, viene verificata la corretta esecuzione di tutte le elaborazioni predefinite, ad esempio l’estrazione dei metadati.
1. L&#39;evento viene inviato al servizio [Adobe I/O Events](https://developer.adobe.com/events/).
1. Il servizio Eventi di Adobe I/O passa l&#39;evento all&#39;[azione Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) per l&#39;elaborazione.
1. L’azione Adobe I/O Runtime richiama l’API del sistema PIM per recuperare metadati aggiuntivi come SKU, informazioni sul fornitore o altri dettagli.
1. I metadati aggiuntivi recuperati da PIM vengono quindi aggiornati in AEM Assets utilizzando l&#39;[API di authoring di Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

## Prerequisiti

Per completare questa esercitazione, è necessario:

- Ambiente AEM as a Cloud Service con [evento AEM abilitato](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Inoltre, il progetto [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) di esempio deve essere distribuito su di esso.

- Accesso a [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installato nel computer locale.

## Passaggi di sviluppo

Le fasi di sviluppo di alto livello sono:

1. [Creare un progetto in Adobe Developer Console (ADC)](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [Inizializzare il progetto per lo sviluppo locale](./runtime-action.md#initialize-project-for-local-development)
1. Configurare il progetto in ADC
1. Configura il servizio di authoring AEM per abilitare la comunicazione dei progetti ADC
1. Sviluppa un’azione di runtime che orchestra il recupero e l’aggiornamento dei metadati
1. Carica una risorsa nel servizio Author dell’AEM e verifica che i metadati siano stati aggiornati

Per informazioni dettagliate sui passaggi 1-2, fare riferimento all&#39;esempio [Azione Adobe I/O Runtime ed Eventi AEM](./runtime-action.md#) e per i passaggi 3-6 fare riferimento alle sezioni seguenti.

### Configurare il progetto in Adobe Developer Console (ADC)

Per ricevere gli eventi AEM Assets ed eseguire l’azione Adobe I/O Runtime creata nel passaggio precedente, configura il progetto in ADC.

- In ADC, passa al [progetto](https://developer.adobe.com/console/projects). Selezionare l&#39;area di lavoro `Stage`, dove è stata distribuita l&#39;azione di runtime.

- Fare clic sul pulsante **Aggiungi servizio** e selezionare l&#39;opzione **Evento**. Nella finestra di dialogo **Aggiungi eventi**, seleziona **Experience Cloud** > **AEM Assets**, quindi fai clic su **Avanti**. Segui i passaggi di configurazione aggiuntivi, seleziona l’istanza AEMCS, l’evento _Elaborazione risorsa completata_, il tipo di autenticazione server-to-server OAuth e altri dettagli.

  ![Evento AEM Assets - Aggiungi evento](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- Infine, nel passaggio **Ricezione degli eventi**, espandi l&#39;opzione **Azione di runtime** e seleziona l&#39;azione _generica_ creata nel passaggio precedente. Fai clic su **Salva eventi configurati**.

  ![Evento AEM Assets - ricevi evento](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- Fare clic sul pulsante **Aggiungi servizio** e selezionare l&#39;opzione **API**. Nella finestra modale **Aggiungi API**, seleziona **Experience Cloud** > **API AEM as a Cloud Service** e fai clic su **Avanti**.

  ![Aggiungi API AEM as a Cloud Service - Configura progetto](../assets/examples/assets-pim-integration/add-aem-api.png)

- Quindi seleziona **OAuth Server-to-Server** per il tipo di autenticazione e fai clic su **Avanti**.

- Selezionare quindi il profilo di prodotto **Amministratori AEM-XXX** e fare clic su **Salva API configurata**. Per aggiornare la risorsa in questione, il profilo di prodotto selezionato deve essere associato all’ambiente AEM Assets da cui viene prodotto l’evento e disporre di accesso sufficiente per aggiornare le risorse in tale ambiente.

  ![Aggiungi API AEM as a Cloud Service - Configura progetto](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

### Configura il servizio di authoring AEM per abilitare la comunicazione dei progetti ADC

Per aggiornare i metadati delle risorse in AEM dal progetto ADC di cui sopra, configura il servizio Author AEM con l’ID client del progetto ADC. L&#39;ID _client_ è stato aggiunto come variabile di ambiente utilizzando l&#39;interfaccia utente [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html#add-variables).

- Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/), seleziona **Programma** > **Ambiente** > **Puntini di sospensione** > **Visualizza dettagli** > **Configurazione**.

  ![Adobe Cloud Manager - Configurazione ambiente](../assets/examples/assets-pim-integration/cloud-manager-environment-configuration.png)

- Quindi **Aggiungi configurazione** e immetti i dettagli della variabile come

  | Nome | Valore | Servizio AEM | Tipo |
  | ----------- | ----------- | ----------- | ----------- |
  | ADOBE_SUPPLIED_CLIENT_ID | &lt;COPY_FROM_ADC_PROJECT_CREDENTIALS> | Autore | Variabile |

  ![Adobe Cloud Manager - Configurazione ambiente](../assets/examples/assets-pim-integration/add-environment-variable.png)

- Fai clic su **Aggiungi** e **Salva** la configurazione.

### Azione di sviluppo runtime

Per eseguire il recupero e l&#39;aggiornamento dei metadati, aggiornare il codice azione _generico_ creato automaticamente nella cartella `src/dx-excshell-1/actions/generic`.

Per il codice completo, fare riferimento al file [WKND-Assets-PIM-Integration.zip](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip) allegato e nella sezione seguente sono evidenziati i file chiave.

- Il file `src/dx-excshell-1/actions/generic/mockPIMCommunicator.js` ironizza sulla chiamata API PIM per recuperare metadati aggiuntivi come SKU e nome del fornitore. Questo file viene utilizzato a scopo dimostrativo. Una volta che il flusso end-to-end funziona, sostituisci questa funzione con una chiamata al tuo vero sistema PIM per recuperare i metadati per la risorsa.

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

- Il file `src/dx-excshell-1/actions/generic/aemCommunicator.js` aggiorna i metadati delle risorse in AEM utilizzando [Assets Author API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

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

  Nel file `.env` sono memorizzati i dettagli delle credenziali server-to-server OAuth del progetto ADC e vengono passati come parametri all&#39;azione utilizzando il file `ext.config.yaml`. Consulta [File di configurazione di App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) per la gestione dei segreti e dei parametri delle azioni.

- La cartella `src/dx-excshell-1/actions/model` contiene `aemAssetEvent.js` e `errors.js` file, utilizzati dall&#39;azione rispettivamente per analizzare l&#39;evento ricevuto e gestire gli errori.

- Il file `src/dx-excshell-1/actions/generic/index.js` utilizza i moduli precedentemente menzionati per orchestrare il recupero e l&#39;aggiornamento dei metadati.

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

- Per visualizzare i metadati fittizi forniti da PIM come SKU e Nome fornitore, creare uno schema metadati in AEM Assets. Vedere [Schema metadati](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/metadata-schemas.html) che visualizza le proprietà dei metadati SKU e Nome fornitore.

- Carica una risorsa nel servizio di authoring AEM e verifica l’aggiornamento dei metadati.

  ![Aggiornamento metadati AEM Assets](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## Concetto e soluzioni chiave

La sincronizzazione dei metadati delle risorse tra AEM e altri sistemi come PIM è spesso necessaria all’interno dell’azienda. Utilizzo dell’AEM In caso di necessità, tali requisiti possono essere soddisfatti.

- Il codice di recupero dei metadati delle risorse viene eseguito al di fuori dell’AEM, evitando il carico sul servizio di authoring dell’AEM e quindi sull’architettura basata sugli eventi e scalabile in modo indipendente.
- La nuova API di authoring di Assets viene utilizzata per aggiornare i metadati delle risorse nell’AEM.
- L&#39;autenticazione API utilizza OAuth server-to-server (flusso di credenziali client). Vedere [Guida all&#39;implementazione delle credenziali server-to-server OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/).
- Al posto di Adobe I/O Runtime Actions, è possibile utilizzare altri webhook o Amazon EventBridge per ricevere l&#39;evento AEM Assets ed elaborare l&#39;aggiornamento dei metadati.
- Eventi Asset tramite AEM Eventing consentono alle aziende di automatizzare e semplificare i processi critici, promuovendo l’efficienza e la coerenza nell’ecosistema dei contenuti.
