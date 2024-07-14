---
title: Verifica webhook Github.com
description: Scopri come verificare una richiesta webhook da Github.com in un’azione App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
exl-id: 5492dc7b-f034-4a7f-924d-79e083349e26
source-git-commit: 8f64864658e521446a91bb4c6475361d22385dc1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Verifica webhook Github.com

I webhook ti consentono di creare o configurare integrazioni che si abbonano a determinati eventi su GitHub.com. Quando viene attivato uno di questi eventi, GitHub invia un payload POST HTTP all’URL configurato del webhook. Tuttavia, per motivi di sicurezza, è importante verificare che la richiesta del webhook in ingresso provenga effettivamente da GitHub e non da un attore dannoso. Questa esercitazione ti guida attraverso i passaggi per verificare una richiesta webhook GitHub.com in un&#39;azione App Builder di Adobe utilizzando un segreto condiviso.

## Configurare il segreto Github in AppBuilder

1. **Aggiungi segreto al file `.env`:**

   Nel file `.env` del progetto App Builder, aggiungi una chiave personalizzata per il segreto del webhook GitHub.com:

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **Aggiorna il file `ext.config.yaml`:**

   È necessario aggiornare il file `ext.config.yaml` per verificare la richiesta del webhook GitHub.com.

   - Impostare la configurazione dell&#39;azione AppBuilder `web` su `raw` per ricevere il corpo della richiesta non elaborato da GitHub.com.
   - In `inputs` nella configurazione dell&#39;azione AppBuilder, aggiungere la chiave `GITHUB_SECRET`, mappandola al campo `.env` contenente il segreto. Il valore di questa chiave è il nome del campo `.env` con prefisso `$`.
   - Impostare l&#39;annotazione `require-adobe-auth` nella configurazione dell&#39;azione AppBuilder su `false` per consentire la chiamata dell&#39;azione senza richiedere l&#39;autenticazione Adobe.

   Il file `ext.config.yaml` risultante dovrebbe avere un aspetto simile al seguente:

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## Aggiungi codice di verifica all&#39;azione AppBuilder

Quindi, aggiungi il codice JavaScript fornito di seguito (copiato dalla documentazione di [GitHub.com](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)) alla tua azione AppBuilder. Assicurarsi di esportare la funzione `verifySignature`.

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## Implementare la verifica nell’azione di AppBuilder

Verificare quindi che la richiesta provenga da GitHub confrontando la firma nell&#39;intestazione della richiesta con la firma generata dalla funzione `verifySignature`.

Nell&#39;azione AppBuilder `index.js`, aggiungere il codice seguente alla funzione `main`:


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## Configurare il webhook in GitHub

Tornando a GitHub.com, fornisci lo stesso valore segreto a GitHub.com durante la creazione del webhook. Utilizza il valore segreto specificato nella chiave `GITHUB_SECRET` del file `.env`.

In GitHub.com, vai alle impostazioni dell’archivio e modifica il webhook. Nelle impostazioni del webhook, specificare il valore segreto nel campo `Secret`. Fai clic su __Aggiorna webhook__ in basso per salvare le modifiche.

![Segreto webhook Github](./assets/github-webhook-verification/github-webhook-settings.png)

Seguendo questi passaggi, assicurati che la tua azione App Builder possa verificare in modo sicuro che le richieste dei webhook in ingresso provengano effettivamente dal webhook GitHub.com.
