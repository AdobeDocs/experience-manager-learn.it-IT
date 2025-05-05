---
title: Pagine di errore personalizzate
description: Scopri come implementare pagine di errore personalizzate per il sito web ospitato da AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Brand Experiences, Configuring, Developing
topic: Content Management, Development
role: Developer
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-12-04T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: c3bfbe59-f540-43f9-81f2-6d7731750fc6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1657'
ht-degree: 0%

---

# Pagine di errore personalizzate

Scopri come implementare pagine di errore personalizzate per il sito web ospitato da AEM as a Cloud Service.

In questa esercitazione imparerai:

- Pagine di errore predefinite
- Da dove vengono distribuite le pagine di errore
   - Tipo di servizio AEM: authoring, pubblicazione, anteprima
   - CDN gestita da Adobe
- Opzioni per personalizzare le pagine di errore
   - ErrorDocument Direttiva Apache
   - ACS AEM Commons - Gestore pagina di errore
   - Pagine errore CDN

## Pagine di errore predefinite

Esaminiamo quando vengono visualizzate le pagine di errore, le pagine di errore predefinite e da dove vengono distribuite.

Le pagine di errore vengono visualizzate quando:

- la pagina non esiste (404)
- non autorizzato ad accedere ad una pagina (403)
- errore del server (500) a causa di problemi di codice o server non raggiungibile.

AEM as a Cloud Service fornisce _pagine di errore predefinite_ per gli scenari di cui sopra. Si tratta di una pagina generica che non corrisponde al tuo marchio.

La pagina di errore predefinita _viene servita_ dal _tipo di servizio AEM_(creazione, pubblicazione, anteprima) o dal _CDN gestito da Adobe_. Per ulteriori informazioni, consulta la tabella seguente.

| Pagina di errore trasmessa da | Dettagli |
|---------------------|:-----------------------:|
| Tipo di servizio AEM: authoring, pubblicazione, anteprima | Quando la richiesta di pagina viene servita dal tipo di servizio AEM e si verifica uno degli scenari di errore precedenti, la pagina di errore viene servita dal tipo di servizio AEM. Per impostazione predefinita, la pagina di errore 5XX viene sostituita dalla pagina di errore CDN gestita da Adobe, a meno che l&#39;intestazione `x-aem-error-pass: true` non sia impostata. |
| CDN gestita da Adobe | Quando la rete CDN gestita da Adobe _non riesce a raggiungere il tipo di servizio AEM_ (server di origine), la pagina di errore viene trasmessa dalla rete CDN gestita da Adobe. **Si tratta di un evento improbabile ma per il quale vale la pena pianificare la pianificazione.** |

>[!NOTE]
>
>In AEM as Cloud Service, la CDN fornisce una pagina di errore generica quando viene ricevuto un errore 5XX dal backend. Per consentire la trasmissione della risposta effettiva del backend, è necessario aggiungere la seguente intestazione alla risposta: `x-aem-error-pass: true`.
>Questo funziona solo per le risposte provenienti da AEM o dal livello Apache/Dispatcher. Altri errori imprevisti provenienti dai livelli intermedi dell’infrastruttura visualizzano ancora la pagina di errore generico.


Ad esempio, le pagine di errore predefinite servite dal tipo di servizio AEM e dalla rete CDN gestita da Adobe sono le seguenti:

![Pagine di errore predefinite di AEM](./assets/aem-default-error-pages.png)

Tuttavia, puoi _personalizzare sia il tipo di servizio AEM che le pagine di errore CDN gestite da Adobe_ per adattarle al tuo marchio e fornire un&#39;esperienza utente migliore.

## Opzioni per personalizzare le pagine di errore

Per personalizzare le pagine di errore sono disponibili le seguenti opzioni:

| Applicabile a | Nome opzione | Descrizione |
|---------------------|:-----------------------:|:-----------------------:|
| Tipi di servizi AEM: pubblicazione e anteprima | Direttiva ErrorDocument | Utilizza la direttiva [ErrorDocument](https://httpd.apache.org/docs/2.4/custom-error.html) nel file di configurazione Apache per specificare il percorso della pagina di errore personalizzata. Applicabile solo ai tipi di servizio di AEM: pubblicazione e anteprima. |
| Tipi di servizi di AEM: authoring, pubblicazione, anteprima | Gestore pagina di errore ACS AEM Commons | Utilizza il [Gestore pagina errori ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html) per personalizzare l&#39;errore in tutti i tipi di servizi AEM. |
| CDN gestita da Adobe | Pagine errore CDN | Utilizza le pagine di errore CDN per personalizzare le pagine di errore quando la rete CDN gestita da Adobe non raggiunge il tipo di servizio AEM (server di origine). |


## Prerequisiti

In questa esercitazione imparerai a personalizzare le pagine di errore utilizzando la direttiva _ErrorDocument_, il _Gestore pagine di errore ACS AEM Commons_ e le opzioni _Pagine di errore CDN_. Per seguire questa esercitazione, è necessario:

- [ambiente di sviluppo AEM locale](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) o ambiente AEM as a Cloud Service. L&#39;opzione _Pagine errore CDN_ è applicabile all&#39;ambiente AEM as a Cloud Service.

- Il [progetto AEM WKND](https://github.com/adobe/aem-guides-wknd) per personalizzare le pagine di errore.

## Configurazione

- Clona e implementa il progetto AEM WKND nell’ambiente di sviluppo AEM locale seguendo i passaggi seguenti:

  ```
  # For local AEM development environment
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallSinglePackage -PautoInstallSinglePackagePublish
  ```

- Per l&#39;ambiente AEM as a Cloud Service, distribuire il progetto WKND di AEM eseguendo la [pipeline full stack](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline). Vedere l&#39;esempio [pipeline non di produzione](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/cloud-manager/cicd-non-production-pipeline).

- Verifica che il rendering delle pagine del sito WKND sia corretto.

## ErrorDocument Direttiva Apache per personalizzare le pagine di errore servite da AEM{#errordocument}

Per personalizzare le pagine di errore AEM fornite, utilizzare la direttiva Apache `ErrorDocument`.

In AEM as a Cloud Service, l&#39;opzione di direttiva Apache `ErrorDocument` è applicabile solo ai tipi di servizio di pubblicazione e anteprima. Non è applicabile al tipo di servizio Author in quanto Apache + Dispatcher non fa parte dell’architettura di distribuzione.

Esaminiamo in che modo il progetto [AEM WKND](https://github.com/adobe/aem-guides-wknd) utilizza la direttiva Apache `ErrorDocument` per visualizzare pagine di errore personalizzate.

- Il modulo `ui.content.sample` contiene le [pagine di errore](https://github.com/adobe/aem-guides-wknd/tree/main/ui.content.sample/src/main/content/jcr_root/content/wknd/language-masters/en/errors) @ `/content/wknd/language-masters/en/errors` con marchio. Esaminali nell&#39;ambiente [AEM](http://localhost:4502/sites.html/content/wknd/language-masters/en/errors) locale o AEM as a Cloud Service `https://author-p<ID>-e<ID>.adobeaemcloud.com/ui#/aem/sites.html/content/wknd/language-masters/en/errors`.

- Il file `wknd.vhost` del modulo `dispatcher` contiene:
   - Direttiva [ErrorDocument](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L139-L143) che punta alle [pagine di errore](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/variables/custom.vars#L7-L8) precedenti.
   - Il valore [DispatcherPassError](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L133) è impostato su 1, pertanto Dispatcher consente ad Apache di gestire tutti gli errori.

  ```
  # In `wknd.vhost` file:
  
  ...
  
  ## ErrorDocument directive
  ErrorDocument 404 ${404_PAGE}
  ErrorDocument 500 ${500_PAGE}
  ErrorDocument 502 ${500_PAGE}
  ErrorDocument 503 ${500_PAGE}
  ErrorDocument 504 ${500_PAGE}
  
  ## Add Header for 5XX error page response
  <IfModule mod_headers.c>
    ### By default, CDN overrides 5XX error pages. To allow the actual response of the backend to pass through, add the header x-aem-error-pass: true
    Header set x-aem-error-pass "true" "expr=%{REQUEST_STATUS} >= 500 && %{REQUEST_STATUS} < 600"
  </IfModule>
  
  ...
  ## DispatcherPassError directive
  <IfModule disp_apache2.c>
      ...
      DispatcherPassError        1
  </IfModule>
  
  # In `custom.vars` file
  ...
  ## Define the error page paths
  Define 404_PAGE /content/wknd/us/en/errors/404.html
  Define 500_PAGE /content/wknd/us/en/errors/500.html
  ...
  ```

- Rivedi le pagine di errore personalizzate del sito WKND immettendo un nome o un percorso di pagina errato nell&#39;ambiente, ad esempio [https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html](https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html).

## ACS AEM Commons-Gestore pagine di errore per personalizzare le pagine di errore servite da AEM{#acs-aem-commons}

Per personalizzare le pagine di errore servite da AEM in _tutti i tipi di servizio AEM_, è possibile utilizzare l&#39;opzione [Gestore pagina di errore ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html).

. Per istruzioni dettagliate, consulta la sezione [Come utilizzare](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html#how-to-use).

## Pagine di errore CDN per personalizzare le pagine di errore CDN distribuite{#cdn-error-pages}

Per personalizzare le pagine di errore servite dalla rete CDN gestita da Adobe, utilizza l’opzione Pagine di errore CDN.

Implementiamo le pagine di errore CDN per personalizzare le pagine di errore quando la rete CDN gestita da Adobe non può raggiungere il tipo di servizio AEM (server di origine).

>[!IMPORTANT]
>
> Il CDN gestito da _Adobe non è in grado di raggiungere il tipo di servizio AEM_ (server di origine), che è un **evento improbabile** per il quale vale la pena pianificare la connessione.

I passaggi di alto livello per implementare le pagine di errore CDN sono:

- Sviluppa un contenuto personalizzato per la pagina di errore come applicazione a pagina singola.
- Ospita i file statici necessari per la pagina di errore CDN in una posizione accessibile al pubblico.
- Configura la regola CDN (errorPages) e fai riferimento ai file statici di cui sopra.
- Distribuisci la regola CDN configurata nell’ambiente AEM as a Cloud Service utilizzando la pipeline Cloud Manager.
- Verifica le pagine di errore CDN.


### Panoramica delle pagine di errore CDN

La pagina di errore CDN viene implementata come applicazione a pagina singola (SPA) dalla rete CDN gestita da Adobe. Il documento SPA HTML consegnato dalla rete CDN gestita da Adobe contiene lo snippet minimo di HTML. Il contenuto personalizzato della pagina di errore viene generato in modo dinamico utilizzando un file JavaScript. Il file JavaScript deve essere sviluppato e ospitato in una posizione accessibile al pubblico dal cliente.

Lo snippet HTML distribuito dalla rete CDN gestita da Adobe ha la seguente struttura:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    
    ...

    <title>{title}</title>
    <link rel="icon" href="{icoUrl}">
    <link rel="stylesheet" href="{cssUrl}">
  </head>
  <body>
    <script src="{jsUrl}"></script>
  </body>
</html>
```

Lo snippet HTML contiene i segnaposto seguenti:

1. **jsUrl**: URL assoluto del file JavaScript per il rendering del contenuto della pagina di errore mediante la creazione dinamica di elementi HTML.
1. **cssUrl**: l&#39;URL assoluto del file CSS per assegnare uno stile al contenuto della pagina di errore.
1. **icoUrl**: URL assoluto del favicon.



### Sviluppare una pagina di errore personalizzata

Sviluppiamo il contenuto della pagina di errore con marchio WKND come applicazione a pagina singola.

A scopo dimostrativo, utilizziamo [React](https://react.dev/), tuttavia puoi utilizzare qualsiasi framework o libreria JavaScript.

- Creare un nuovo progetto React eseguendo il comando seguente:

  ```
  $ npx create-react-app aem-cdn-error-page
  ```

- Apri il progetto nel tuo editor di codice preferito e aggiorna i seguenti file:

   - `src/App.js`: è il componente principale che esegue il rendering del contenuto della pagina di errore.

     ```javascript
     import logo from "./wknd-logo.png";
     import "./App.css";
     
     function App() {
       return (
         <>
           <div className="App">
             <div className="container">
             <img src={logo} className="App-logo" alt="logo" />
             </div>
           </div>
           <div className="container">
             <div className="error-code">CDN Error Page</div>
             <h1 className="error-message">Ruh-Roh! Page Not Found</h1>
             <p className="error-description">
               We're sorry, we are unable to fetch this page!
             </p>
           </div>
         </>
       );
     }
     
     export default App;
     ```

   - `src/App.css`: applicare lo stile al contenuto della pagina di errore.

     ```css
     .App {
       text-align: left;
     }
     
     .App-logo {
       height: 14vmin;
       pointer-events: none;
     }
     
     
     body {
       margin-top: 0;
       padding: 0;
       font-family: Arial, sans-serif;
       background-color: #fff;
       color: #333;
       display: flex;
       justify-content: center;
       align-items: center;
     }
     
     .container {
       text-align: letf;
       padding-top: 10px;
     }
     
     .error-code {
       font-size: 4rem;
       font-weight: bold;
       color: #ff6b6b;
     }
     
     .error-message {
       font-size: 2.5rem;
       margin-bottom: 10px;
     }
     
     .error-description {
       font-size: 1rem;
       margin-bottom: 20px;
     }
     ```

   - Aggiungere il file `wknd-logo.png` alla cartella `src`. Copia il [file](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-512.png) come `wknd-logo.png`.

   - Aggiungere il file `favicon.ico` alla cartella `public`. Copia il [file](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-32.png) come `favicon.ico`.

   - Verifica il contenuto della pagina di errore CDN con marchio WKND eseguendo il progetto:

     ```
     $ npm start
     ```

     Apri il browser e passa a `http://localhost:3000/` per visualizzare il contenuto della pagina di errore CDN.

   - Crea il progetto per generare i file statici:

     ```
     $ npm run build
     ```

     I file statici vengono generati nella cartella `build`.


In alternativa, puoi scaricare il file [aem-cdn-error-page.zip](./assets/aem-cdn-error-page.zip) contenente i file di progetto React precedenti.

Quindi, ospitare i file statici di cui sopra in una posizione accessibile al pubblico.

### Pagina Host: file statici necessari per errore CDN

Ospitiamo i file statici nell’archiviazione BLOB di Azure. Tuttavia, puoi utilizzare qualsiasi servizio di hosting di file statico come [Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/) o [AWS S3](https://aws.amazon.com/s3/).

- Segui la documentazione ufficiale di [Azure Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) per creare un contenitore e caricare i file statici.

  >[!IMPORTANT]
  >
  >Se utilizzi altri servizi di hosting di file statici, segui la relativa documentazione per ospitare i file statici.

- Assicurati che i file statici siano accessibili al pubblico. Le impostazioni dell’account di archiviazione specifico per la demo WKND sono le seguenti:

   - **Nome account di archiviazione**: `aemcdnerrorpageresources`
   - **Nome contenitore**: `static-files`

  ![Archiviazione BLOB di Azure](./assets/azure-blob-storage-settings.png)

- Nel contenitore superiore a `static-files`, vengono caricati i file di seguito della cartella `build`:

   - `error.js`: il file `build/static/js/main.<hash>.js` è stato rinominato in `error.js` e [pubblicamente accessibile](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js).
   - `error.css`: il file `build/static/css/main.<hash>.css` è stato rinominato in `error.css` e [pubblicamente accessibile](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css).
   - `favicon.ico`: il file `build/favicon.ico` è stato caricato così com&#39;è e [accessibile pubblicamente](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico).

Quindi, configura la regola CDN (errorPages) e fai riferimento ai file statici di cui sopra.

### Configurare la regola CDN

Configuriamo la regola CDN `errorPages` che utilizza i file statici di cui sopra per eseguire il rendering del contenuto della pagina di errore CDN.

1. Apri il file `cdn.yaml` dalla cartella principale `config` del progetto AEM. Ad esempio, il file cdn.yaml[&#128279;](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) del progetto WKND.

1. Aggiungi la seguente regola CDN al file `cdn.yaml`:

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     # The CDN Error Page configuration. 
     # The error page is displayed when the Adobe-managed CDN is unable to reach the origin server.
     # It is implemented as a Single Page Application (SPA) and WKND branded content must be generated dynamically using the JavaScript file 
     errorPages:
       spa:
         title: Adobe AEM CDN Error Page # The title of the error page
         icoUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico # The PUBLIC URL of the favicon
         cssUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css # The PUBLIC URL of the CSS file
         jsUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js # The PUBLIC URL of the JavaScript file
   ```

1. Salva, conferma e invia le modifiche all’archivio a monte di Adobe.

### Distribuire la regola CDN

Infine, distribuisci la regola CDN configurata nell’ambiente AEM as a Cloud Service utilizzando la pipeline Cloud Manager.

1. In Cloud Manager, passa alla sezione **Pipeline**.

1. Crea una nuova pipeline o seleziona la pipeline esistente che distribuisce solo i file **Config**. Per i passaggi dettagliati, vedere [Creare una pipeline di configurazione](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

1. Fai clic sul pulsante **Esegui** per distribuire la regola CDN.

![Distribuisci regola CDN](./assets/deploy-cdn-rule-for-cdn-error-pages.png)

### Verifica le pagine di errore CDN

Per verificare le pagine di errore CDN, effettua le seguenti operazioni:

- Nel browser, passa all&#39;URL di pubblicazione di AEM as a Cloud Service, aggiungi `cdnstatus?code=404` all&#39;URL, ad esempio [https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404](https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404), oppure accedi utilizzando l&#39;[URL di dominio personalizzato](https://wknd.enablementadobe.com/cdnstatus?code=404)

  ![WKND - Pagina errore CDN](./assets/wknd-cdn-error-page.png)

- I codici supportati sono: 403, 404, 406, 500 e 503.

- Verifica la scheda di rete del browser per visualizzare i file statici caricati dall’archiviazione BLOB di Azure. Il documento HTML distribuito dalla rete CDN gestita da Adobe contiene il contenuto minimo indispensabile e il file JavaScript crea in modo dinamico il contenuto della pagina di errore con marchio.

  ![Scheda Rete della pagina di errore CDN](./assets/wknd-cdn-error-page-network-tab.png)

## Riepilogo

In questo tutorial hai imparato quali sono le pagine di errore predefinite, da dove vengono distribuite le pagine di errore, e le opzioni per personalizzare le pagine di errore. Hai imparato a implementare pagine di errore personalizzate utilizzando le opzioni `ACS AEM Commons Error Page Handler`, `ErrorDocument` Apache e `CDN Error Pages`.

## Risorse aggiuntive

- [Configurazione delle pagine di errore CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-error-pages)

- [Cloud Manager - Config pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#config-deployment-pipeline)
