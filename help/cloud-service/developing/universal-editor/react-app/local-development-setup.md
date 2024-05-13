---
title: Configurazione sviluppo locale
description: Scopri come impostare un ambiente di sviluppo locale per Universal Editor per modificare il contenuto di un’app React di esempio.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 1%

---


# Configurazione sviluppo locale

Scopri come impostare un ambiente di sviluppo locale per modificare i contenuti di un’app React utilizzando l’editor universale dell’AEM.

## Prerequisiti

Per seguire questa esercitazione sono necessari i seguenti elementi:

- Competenze di base in ambito HTML e JavaScript.
- I seguenti strumenti devono essere installati localmente:
   - [Node.js](https://nodejs.org/it/download/)
   - [Git](https://git-scm.com/downloads)
   - Un IDE o un editor di codice, ad esempio [Codice di Visual Studio](https://code.visualstudio.com/)
- Scarica e installa quanto segue:
   - [SDK AS A CLOUD SERVICE AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk): contiene il file JAR Quickstart utilizzato per eseguire AEM Author e Publish localmente a scopo di sviluppo.
   - [Servizio Universal Editor](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home): copia locale del servizio Universal Editor, con un sottoinsieme di funzioni, scaricabile dal portale di distribuzione software.
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy): proxy HTTP SSL locale semplice che utilizza un certificato autofirmato per lo sviluppo locale. L’editor universale dell’AEM richiede l’URL HTTPS dell’app React per caricarlo nell’editor.

## Configurazione locale

Per configurare l’ambiente di sviluppo locale, segui i passaggi seguenti:

### SDK AEM

Per fornire i contenuti per l’app WKND Teams React, installa i seguenti pacchetti nell’SDK AEM locale.

- [Team WKND - Pacchetto di contenuti](./assets/basic-tutorial-solution.content.zip): contiene i modelli per frammenti di contenuto, frammenti di contenuto e query GraphQL persistenti.
- [Team WKND - Pacchetto di configurazione](./assets/basic-tutorial-solution.ui.config.zip): contiene le configurazioni del gestore Cross-Origin Resource Sharing (CORS) e del gestore di autenticazione token. Il CORS facilita l’esecuzione di chiamate lato client alle API GraphQL dell’AEM da parte di proprietà web non AEM, mentre il gestore di autenticazione token viene utilizzato per autenticare ogni richiesta all’AEM.

  ![Team WKND - Pacchetti](./assets/wknd-teams-packages.png)

### React app

Per configurare l’app WKND Teams React, effettua le seguenti operazioni:

1. Clona il [App di reazione team WKND](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) dal `basic-tutorial` ramo della soluzione.

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Accedi a `basic-tutorial` e aprirla nell’editor di codice.

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. Installa le dipendenze e avvia l’app React.

   ```bash
   $ npm install
   $ npm start
   ```

1. Apri l’app WKND Teams React nel browser all’indirizzo [http://localhost:3000](http://localhost:3000). Viene visualizzato un elenco dei membri del team e dei relativi dettagli. Il contenuto dell’app React è fornito dall’SDK AEM locale tramite API GraphQL (`/graphql/execute.json/my-project/all-teams`), che puoi verificare utilizzando la scheda di rete del browser.

   ![Team WKND - App di reazione](./assets/wknd-teams-react-app.png)

### Servizio editor universale

Per impostare **locale** Per il servizio Universal Editor, effettuare le seguenti operazioni:

1. Scaricare l&#39;ultima versione del servizio Universal Editor dalla [Portale di distribuzione software](https://experience.adobe.com/downloads).

   ![Distribuzione di software - Servizio Universal Editor](./assets/universal-editor-service.png)

1. Estrai il file zip scaricato e copia il `universal-editor-service.cjs` in una nuova directory denominata `universal-editor-service`.

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. Crea `.env` file in `universal-editor-service` e aggiungi le seguenti variabili di ambiente:

   ```bash
   # The port on which the Universal Editor service runs
   EXPRESS_PORT=8000
   # Disable SSL verification
   NODE_TLS_REJECT_UNAUTHORIZED=0
   ```

1. Avviare il servizio Universal Editor locale.

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

Il comando precedente avvia il servizio Universal Editor sulla porta `8000` e dovresti visualizzare il seguente output:

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### Proxy HTTP SSL locale

L’editor universale dell’AEM richiede che l’app React sia gestita tramite HTTPS. Configuriamo un proxy HTTP SSL locale che utilizza un certificato autofirmato per lo sviluppo locale.

Segui i passaggi seguenti per configurare il proxy HTTP SSL locale e distribuire l’SDK AEM e il servizio Universal Editor tramite HTTPS:

1. Installare `local-ssl-proxy` a livello globale.

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. Avvia due istanze del proxy HTTP SSL locale per i servizi seguenti:

   - Proxy HTTP SSL locale dell’SDK AEM sulla porta `8443`.
   - Proxy HTTP SSL locale del servizio Universal Editor sulla porta `8001`.

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### Aggiornare l’app React per utilizzare HTTPS

Per abilitare HTTPS per l’app WKND Teams React, effettua le seguenti operazioni:

1. Interrompere la reazione premendo `Ctrl + C` nel terminale.
1. Aggiornare il `package.json` file da includere `HTTPS=true` variabile di ambiente in `start` script.

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. Aggiornare il `REACT_APP_HOST_URI` nel `.env.development` per utilizzare il protocollo HTTPS e la porta proxy HTTP SSL locale dell’SDK AEM.

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. Aggiornare il `../src/proxy/setupProxy.auth.basic.js` file per utilizzare impostazioni SSL flessibili tramite `secure: false` opzione.

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. Avvia l’app React.

   ```bash
   $ npm start
   ```

## Verificare la configurazione

Dopo aver configurato l’ambiente di sviluppo locale seguendo i passaggi precedenti, verifichiamo la configurazione.

### Verifica locale

Assicurati che i seguenti servizi siano in esecuzione localmente su HTTPS. Potrebbe essere necessario accettare l’avviso di sicurezza nel browser per il certificato autofirmato:

1. App di reazione dei team WKND su [https://localhost:3000](https://localhost:3000)
1. SDK per AEM su [https://localhost:8443](https://localhost:8443)
1. Servizio Editor universale attivato [https://localhost:8001](https://localhost:8001)

### Caricare l’app WKND Teams React nell’editor universale

Carichiamo l’app WKND Teams React nell’editor universale per verificare la configurazione:

1. Apri Universal Editor https://experience.adobe.com/#/aem/editor nel browser. Se richiesto, effettua l’accesso con il tuo Adobe ID.

1. Immetti l’URL dell’app WKND Teams React nel campo di input URL del sito dell’editor universale e fai clic su `Open`.

   ![Editor universale - URL sito](./assets/universal-editor-site-url.png)

1. L’app WKND Teams React viene caricata nell’editor universale **ma non è ancora possibile modificare il contenuto**. È necessario dotare l’app React di strumenti che consentano di modificare i contenuti mediante l’editor universale.

   ![Editor universale - App di reazione team WKND](./assets/universal-editor-wknd-teams.png)


## Passaggio successivo

Scopri come [dotare l’app React di uno strumento per modificare il contenuto](./instrument-to-edit-content.md).
