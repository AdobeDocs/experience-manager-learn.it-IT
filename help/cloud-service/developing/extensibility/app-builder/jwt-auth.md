---
title: Genera token di accesso nell’azione di App Builder
description: Scopri come generare un token di accesso utilizzando le credenziali JWT da utilizzare in un’azione di App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
source-git-commit: 40679e80fd9270dd9fad8174a986fd1fdd5e3d29
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 1%

---


# Genera token di accesso nell’azione di App Builder

Le azioni di App Builder potrebbero dover interagire con le API di Adobe associate ai progetti Adobe Developer Console in cui viene distribuita anche l’app di App Builder.

Potrebbe essere necessaria l’azione App Builder per generare il proprio token di accesso associato al progetto Adobe Developer Console desiderato.

>[!IMPORTANT]
>
> Revisione [Documentazione sulla sicurezza di App Builder](https://developer.adobe.com/app-builder/docs/guides/security/) per capire quando è appropriato generare token di accesso rispetto all’utilizzo dei token di accesso forniti.
>
> L’azione personalizzata potrebbe dover fornire i propri controlli di sicurezza per garantire che solo i consumatori autorizzati possano accedere all’azione di App Builder e ai servizi Adobe dietro di essa.


## file .env

Nel progetto App Builder `.env` aggiungi chiavi personalizzate per ciascuna delle credenziali JWT del progetto Adobe Developer Console. I valori delle credenziali JWT possono essere ottenuti dal progetto Adobe Developer Console __Credenziali__ > __Account di servizio (JWT)__ per una determinata area di lavoro.

![Credenziali del servizio JWT della console Adobe Developer](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

I valori per `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` può essere copiato direttamente dalla schermata Credenziali JWT del progetto Adobe Developer Console.

### Metascopi

Determina le API di Adobe e i relativi metascopi con cui interagisce l’azione App Builder. Elencare metascopi con delimitatori virgola nel `JWT_METASCOPES` chiave. I metascopi validi sono elencati in [Documentazione del metascopio JWT di Adobe](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


Ad esempio, è possibile aggiungere il seguente valore al `JWT_METASCOPES` nella `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Chiave privata

La `JWT_PRIVATE_KEY` devono essere formattati in modo particolare in quanto si tratta nativamente di un valore su più righe, che non è supportato in `.env` file. Il modo più semplice è codificare la chiave privata in base64. Codifica base64 della chiave privata (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) può essere eseguita utilizzando strumenti nativi forniti dal sistema operativo in uso.

>[!BEGINTABS]

>[!TAB macOS]

1. Apri `Terminal`
1. Esegui il comando `base64 -i /path/to/private.key | pbcopy`
1. L&#39;output base64 viene copiato automaticamente negli Appunti
1. Incolla in `.env` come valore della chiave corrispondente

>[!TAB Windows]

1. Apri `Command Prompt`
1. Esegui il comando `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. Esegui il comando `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. Copia l&#39;output base64 negli appunti
1. Incolla in `.env` come valore della chiave corrispondente

>[!TAB Linux®]

1. Terminale aperto
1. Esegui il comando `base64 private.key`
1. Copia l&#39;output base64 negli appunti
1. Incolla in `.env` come valore della chiave corrispondente

>[!ENDTABS]

Ad esempio, la seguente chiave privata con codifica base64 potrebbe essere aggiunta al `JWT_PRIVATE_KEY` nella `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Mappatura degli ingressi

Con il valore delle credenziali JWT impostato in `.env` devono essere mappati sugli input dell&#39;azione AppBuilder per poter essere letti nell&#39;azione stessa. A questo scopo, aggiungi voci per ogni variabile nella `ext.config.yaml` action `inputs` nel formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

Esempio:

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

Le chiavi definite in `inputs` sono disponibili nella `params` oggetto fornito all’azione App Builder.


## Credenziali JWT per accedere al token

Nell’azione App Builder , le credenziali JWT sono disponibili nella `params` e utilizzabile da [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) per generare un token di accesso, che a sua volta può accedere ad altre API e servizi di Adobe.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
