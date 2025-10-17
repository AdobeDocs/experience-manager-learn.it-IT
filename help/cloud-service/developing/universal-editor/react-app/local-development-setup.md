---
title: Configurazione dello sviluppo locale
description: Scopri come configurare un ambiente di sviluppo locale per l'Editor universale per modificare il contenuto di un'app React di esempio.
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 189
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 47bef697-5253-493a-b9f9-b26c27d2db56
source-git-commit: 7c58c5cb6a3d99a9577206b3e5e0b8dcd55a850e
workflow-type: ht
source-wordcount: '787'
ht-degree: 100%

---

# Configurazione dello sviluppo locale

Scopri come configurare un ambiente di sviluppo locale per modificare i contenuti di un&#39;app React utilizzando l&#39;Editor universale di AEM.

## Prerequisiti

Per seguire questo tutorial sono necessari i seguenti elementi:

- Competenze di base di HTML e JavaScript.
- È necessario installare localmente i seguenti strumenti:
   - [Node.js](https://nodejs.org/en/download/)
   - [Git](https://git-scm.com/downloads)
   - Un IDE o un editor di codice, ad esempio [Visual Studio Code](https://code.visualstudio.com/)
- Scarica e installa quanto segue:
   - [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk): contiene il file Jar Quickstart utilizzato per eseguire l&#39;authoring di AEM e la pubblicazione locale a scopo di sviluppo.
   - [Servizio Editor universale](https://experienceleague.adobe.com/it/docs/experience-cloud/software-distribution/home): una copia locale del servizio Editor universale, con un sottoinsieme di funzionalità e scaricabile dal portale di distribuzione software.
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy): un semplice proxy HTTP SSL locale che utilizza un certificato autofirmato per lo sviluppo locale. L&#39;Editor universale di AEM richiede l&#39;URL HTTPS dell&#39;app React per caricarlo nell&#39;editor.

## Configurazione locale

Per configurare l&#39;ambiente di sviluppo locale, segui questi passaggi:

### SDK AEM

Per fornire i contenuti per l&#39;app WKND Teams React, installa i seguenti pacchetti nel SDK AEM locale.

- [Team WKND - Pacchetto di contenuti](./assets/basic-tutorial-solution.content.zip): contiene i modelli per frammenti di contenuto, frammenti di contenuto e query GraphQL persistenti.
- [Team WKND - Pacchetto di configurazione](./assets/basic-tutorial-solution.ui.config.zip): contiene le configurazioni CORS (Cross-Origin Resource Sharing) e del gestore di autenticazione token. CORS facilita nel proprietà web non AEM per l&#39;esecuzione di chiamate lato client basate sul browser alle API GraphQL di AEM e il gestore di autenticazione token viene utilizzato per autenticare ogni richiesta in AEM.

  ![Team WKND - Pacchetti](./assets/wknd-teams-packages.png)

### App React

Per configurare l&#39;app WKND Teams React, effettua le seguenti operazioni:

1. Clona l&#39;app [WKND Teams React](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) dal ramo della soluzione `basic-tutorial`.

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Passa alla directory `basic-tutorial` e aprila nell&#39;editor di codice.

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. Installa le dipendenze e avvia l&#39;app React.

   ```bash
   $ npm install
   $ npm start
   ```

1. Apri l&#39;app WKND Teams React nel browser in [http://localhost:3000](http://localhost:3000). Viene visualizzato un elenco dei membri del gruppo e i relativi dettagli. Il contenuto dell&#39;app React è fornito dal SDK AEM locale tramite le API GraphQL (`/graphql/execute.json/my-project/all-teams`), che puoi verificare utilizzando la scheda di rete del browser.

   ![Team WKND - App React](./assets/wknd-teams-react-app.png)

### Servizio Editor universale

Per configurare il servizio Editor universale **locale**, effettua le seguenti operazioni:

1. Scarica la versione più recente del servizio Editor universale dal [Portale di distribuzione software](https://experience.adobe.com/downloads).

   ![Distribuzione software - Servizio Editor universale](./assets/universal-editor-service.png)

1. Estrai il file zip scaricato e copia il file `universal-editor-service.cjs` in una nuova directory denominata `universal-editor-service`.

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. Crea il file `.env` nella directory `universal-editor-service` e aggiungi le seguenti variabili di ambiente:

   ```bash
   # The port on which the Universal Editor service runs
   UES_PORT=8000
   # Disable SSL verification
   UES_TLS_REJECT_UNAUTHORIZED=false
   ```

1. Avvia il servizio dell’editor universale locale.

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

Il comando precedente avvia il servizio dell’editor universale sulla porta `8000`. Dovrebbe essere visualizzato il seguente output:

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### Proxy HTTP SSL locale

L’editor universale AEM richiede che l’app React venga distribuita tramite HTTPS. Viene configurato un proxy HTTP SSL locale che utilizza un certificato autofirmato per lo sviluppo locale.

Segui i passaggi seguenti per configurare il proxy HTTP SSL locale e distribuire il servizio Editor universale e AEM SDK tramite HTTPS:

1. Installare il pacchetto `local-ssl-proxy` a livello globale.

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. Avvia due istanze del proxy HTTP SSL locale per i servizi seguenti:

   - Proxy HTTP SSL locale di AEM SDK sulla porta `8443`.
   - Proxy HTTP SSL locale del servizio Editor universale sulla porta `8001`.

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### Aggiornare l’app React per utilizzare HTTPS

Per abilitare HTTPS per l’app React WKND Teams, segui i passaggi seguenti:

1. Interrompi React premendo `Ctrl + C` nel terminale.
1. Aggiorna il file `package.json` per includere la variabile di ambiente `HTTPS=true` nello script `start`.

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. Aggiorna `REACT_APP_HOST_URI` nel file `.env.development` per utilizzare il protocollo HTTPS e la porta proxy HTTP SSL locale di AEM SDK.

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. Aggiorna il file `../src/proxy/setupProxy.auth.basic.js` per utilizzare le impostazioni SSL flessibili utilizzando l’opzione `secure: false`.

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

1. App React WKND Teams su [https://localhost:3000](https://localhost:3000)
1. AEM SDK su [https://localhost:8443](https://localhost:8443)
1. Servizio Editor universale su [https://localhost:8001](https://localhost:8001)

### Caricare l’app React WKND Teams nell’editor universale

Carichiamo l’app React WKND Teams nell’editor universale per verificare la configurazione:

1. Apri l’editor universale https://experience.adobe.com/#/aem/editor nel browser. Se richiesto, effettua l’accesso con il tuo Adobe ID.

1. Immetti l’URL dell’app React WKND Teams nel campo di input per l’URL del sito dell’editor universale e fai clic su `Open`.

   ![Editor universale - URL sito](./assets/universal-editor-site-url.png)

1. L’app React WKND Teams viene caricata nell’editor universale **ma non è ancora possibile modificare il contenuto**. È necessario dotare l’app React di strumenti che consentano di modificare i contenuti mediante l’editor universale.

   ![Editor universale - App React WKND Teams](./assets/universal-editor-wknd-teams.png)


## Passaggio successivo

Scopri come [dotare l’app React per modificare il contenuto](./instrument-to-edit-content.md).
