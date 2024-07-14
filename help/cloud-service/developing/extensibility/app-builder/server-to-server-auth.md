---
title: Generare un token di accesso server-to-server nell’azione di App Builder
description: Scopri come generare un token di accesso utilizzando le credenziali server-to-server OAuth per l’utilizzo in un’azione App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Generare un token di accesso server-to-server nell’azione di App Builder

Le azioni App Builder potrebbero dover interagire con le API Adobe che supportano **credenziali server-to-server OAuth** e sono associate a progetti Adobe Developer Console in cui è distribuita l&#39;app App Builder.

Questa guida spiega come generare un token di accesso utilizzando _credenziali server-to-server OAuth_ per l&#39;utilizzo in un&#39;azione App Builder.

>[!IMPORTANT]
>
> Le credenziali dell’account di servizio (JWT) sono state dichiarate obsolete e sostituite dalle credenziali server-to-server OAuth. Tuttavia, esistono ancora alcune API di Adobe che supportano solo le credenziali dell’account di servizio (JWT) e la migrazione a OAuth Server-to-Server è in corso. Consulta la documentazione API di Adobe per capire quali credenziali sono supportate.

## Configurazioni dei progetti Adobe Developer Console

Durante l&#39;aggiunta dell&#39;API Adobe desiderata al progetto Adobe Developer Console, nel passaggio _Configura API_ selezionare il tipo di autenticazione **OAuth Server-to-Server**.

![Adobe Developer Console - Server-to-Server OAuth](./assets/s2s-auth/oauth-server-to-server.png)

Per assegnare l’account del servizio di integrazione creato automaticamente di cui sopra, seleziona il profilo di prodotto desiderato. Pertanto, tramite il profilo di prodotto, vengono controllate le autorizzazioni dell’account di servizio.

![Adobe Developer Console - Profilo prodotto](./assets/s2s-auth/select-product-profile.png)

## file .env

Nel file `.env` del progetto App Builder, aggiungi le chiavi personalizzate per le credenziali server-to-server OAuth del progetto Adobe Developer Console. I valori delle credenziali da server a server OAuth possono essere ottenuti dalle __credenziali__ > __OAuth Server-to-Server__ del progetto Adobe Developer Console per una determinata area di lavoro.

![Credenziali server-to-server Adobe Developer Console OAuth](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

I valori per `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET`, `OAUTHS2S_CECREDENTIALS_METASCOPES` possono essere copiati direttamente dalla schermata Credenziali server-to-server OAuth del progetto Adobe Developer Console.

## Mappatura degli input

Con il valore delle credenziali da server a server OAuth impostato nel file `.env`, è necessario mapparle agli input dell&#39;azione AppBuilder in modo che possano essere lette nell&#39;azione stessa. A tale scopo, aggiungere voci per ogni variabile nell&#39;azione `inputs` di `ext.config.yaml` nel formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

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
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

Le chiavi definite in `inputs` sono disponibili nell&#39;oggetto `params` fornito per l&#39;azione App Builder.

## Credenziali server-to-server OAuth per accedere al token

Nell&#39;azione App Builder, le credenziali server-to-server OAuth sono disponibili nell&#39;oggetto `params`. Utilizzando queste credenziali il token di accesso può essere generato utilizzando [Librerie OAuth 2.0](https://oauth.net/code/). Oppure puoi utilizzare la [libreria Node Fetch](https://www.npmjs.com/package/node-fetch) per effettuare una richiesta POST all&#39;endpoint del token Adobe IMS per ottenere il token di accesso.

Nell&#39;esempio seguente viene illustrato come utilizzare la libreria `node-fetch` per effettuare una richiesta POST all&#39;endpoint token Adobe IMS per ottenere il token di accesso.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
