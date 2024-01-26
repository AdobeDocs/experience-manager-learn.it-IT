---
title: Generare il token di accesso nell’azione di App Builder
description: Scopri come generare un token di accesso utilizzando le credenziali JWT per l’utilizzo in un’azione di App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 161
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Generare il token di accesso nell’azione di App Builder

Potrebbe essere necessario che le azioni di App Builder interagiscano con le API Adobe associate ai progetti della console Adobe Developer. Anche l&#39;app di App Builder viene distribuita.

Questo potrebbe richiedere che l’azione Generatore app generi il proprio token di accesso associato al progetto Adobe Developer Console desiderato.

>[!IMPORTANT]
>
> Revisione [Documentazione sulla sicurezza di App Builder](https://developer.adobe.com/app-builder/docs/guides/security/) per capire quando è appropriato generare token di accesso anziché utilizzare quelli forniti.
>
> L’azione personalizzata potrebbe dover fornire propri controlli di sicurezza per garantire che solo i consumatori autorizzati possano accedere all’azione di App Builder e ai servizi Adobe associati.


## file .env

Nel progetto App Builder di `.env` , aggiungi le chiavi personalizzate per ciascuna delle credenziali JWT del progetto Adobe Developer Console. I valori delle credenziali JWT possono essere ottenuti dal file della console di Adobe Developer __Credenziali__ > __Account di servizio (JWT)__ per una determinata area di lavoro.

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

I valori per `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` possono essere copiate direttamente dalla schermata delle credenziali JWT del progetto Adobe Developer Console.

### Metascopi

Determina le API Adobe e i relativi metascopi con cui l&#39;azione di App Builder interagisce. Elencare metascopi con delimitatori virgola in `JWT_METASCOPES` chiave. I metascopi validi sono elencati in [Documentazione di Adobe JWT Metascope](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


Ad esempio, il seguente valore potrebbe essere aggiunto al `JWT_METASCOPES` chiave nella `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Chiave privata

Il `JWT_PRIVATE_KEY` deve essere formattato in modo speciale in quanto si tratta di un valore nativo su più righe non supportato in `.env` file. Il modo più semplice è codificare la chiave privata in base64. Codifica Base64 della chiave privata (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) può essere eseguita utilizzando gli strumenti nativi forniti dal sistema operativo in uso.

>[!BEGINTABS]

>[!TAB macOS]

1. Apri `Terminal`
1. Eseguire il comando `base64 -i /path/to/private.key | pbcopy`
1. L&#39;output base64 viene automaticamente copiato negli Appunti
1. Incolla in `.env` come valore della chiave corrispondente

>[!TAB Windows]

1. Apri `Command Prompt`
1. Eseguire il comando `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. Eseguire il comando `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. Copiare l&#39;output base64 negli Appunti
1. Incolla in `.env` come valore della chiave corrispondente

>[!TAB Linux®]

1. Apri terminale
1. Eseguire il comando `base64 private.key`
1. Copiare l&#39;output base64 negli Appunti
1. Incolla in `.env` come valore della chiave corrispondente

>[!ENDTABS]

Ad esempio, la seguente chiave privata con codifica base64 può essere aggiunta al `JWT_PRIVATE_KEY` chiave nella `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Mappatura degli input

Con il valore delle credenziali JWT impostato in `.env` , devono essere mappati agli input dell&#39;azione AppBuilder per poter essere letti nell&#39;azione stessa. A questo scopo, aggiungi voci per ciascuna variabile nella `ext.config.yaml` azione `inputs` nel formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

Ad esempio:

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

Le chiavi definite in `inputs` sono disponibili sul `params` oggetto fornito all&#39;azione di App Builder.


## Credenziali JWT per accedere al token

Nell’azione App Builder, le credenziali JWT sono disponibili nel `params` e utilizzabile da [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) generare un token di accesso, che a sua volta può accedere ad altre API e servizi Adobe.

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
