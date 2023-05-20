---
title: Implementazione di produzione tramite un servizio di pubblicazione AEM - Guida introduttiva di AEM Headless - GraphQL
description: Scopri i servizi AEM Author e Publish e il modello di distribuzione consigliato per le applicazioni headless. In questa esercitazione, scopri come utilizzare le variabili di ambiente per modificare dinamicamente un endpoint di GraphQL in base all’ambiente di destinazione. Scopri come configurare correttamente l’AEM per la condivisione CORS (Cross-Origin Resource Sharing).
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2357'
ht-degree: 9%

---

# Implementazione di produzione con un servizio di pubblicazione AEM

In questo tutorial, configurerai un ambiente locale per simulare la distribuzione di contenuti da un’istanza Author a un’istanza Publish. Genererai inoltre la build di produzione di un’app React configurata per utilizzare contenuti dall’ambiente AEM Publish utilizzando le API di GraphQL. Imparerai a utilizzare in modo efficace le variabili di ambiente e ad aggiornare le configurazioni AEM CORS.

## Prerequisiti

Questo tutorial fa parte di un tutorial in più parti. Si presume che i passaggi descritti nelle parti precedenti siano stati completati.

## Obiettivi

Scopri come:

* Scopri l’architettura di authoring e pubblicazione di AEM.
* Scopri le best practice per la gestione delle variabili di ambiente.
* Scopri come configurare correttamente l’AEM per la condivisione CORS (Cross-Origin Resource Sharing).

## Pattern di distribuzione pubblicazione autore {#deployment-pattern}

Un ambiente AEM completo è costituito da authoring, pubblicazione e Dispatcher. Il servizio di authoring è il luogo in cui gli utenti interni creano, gestiscono e visualizzano in anteprima i contenuti. Il servizio di pubblicazione è considerato l’ambiente &quot;live&quot; ed è normalmente ciò con cui gli utenti finali interagiscono. I contenuti vengono modificati e approvati nel servizio di authoring e quindi distribuiti al servizio di pubblicazione.

Il modello di implementazione più comune per le applicazioni headless AEM consiste nella connessione della versione di produzione dell’applicazione a un servizio di pubblicazione AEM.

![Modello di distribuzione di alto livello](assets/publish-deployment/high-level-deployment.png)

Il diagramma riportato sopra mostra questo diffuso pattern di distribuzione.

1. A **Autore del contenuto** utilizza il servizio di authoring AEM per creare, modificare e gestire i contenuti.
2. L’**autore del contenuto** e altri utenti interni possono visualizzare in anteprima il contenuto direttamente sul servizio di authoring. È possibile impostare una versione di anteprima dell’applicazione che si connette al servizio di authoring.
3. Una volta approvato, il contenuto può essere **pubblicato** al servizio di pubblicazione AEM.
4. **Gli utenti finali interagiscono con la versione di produzione dell’applicazione.** L’applicazione di produzione si connette al servizio di pubblicazione e utilizza le API GraphQL per richiedere e utilizzare i contenuti.

Il tutorial simula la distribuzione precedente aggiungendo un’istanza AEM Publish alla configurazione corrente. Nei capitoli precedenti, l’app React fungeva da anteprima collegandosi direttamente all’istanza di authoring. Una build di produzione dell’app React viene distribuita a un server Node.js statico che si connette alla nuova istanza Publish.

Alla fine, sono in esecuzione tre server locali:

* http://localhost:4502 - Istanza Autore
* http://localhost:4503 - Pubblica istanza
* http://localhost:5000 - App di reazione in modalità di produzione, con connessione all’istanza Publish.

## Installare l’SDK dell’AEM - Modalità di pubblicazione {#aem-sdk-publish}

Attualmente disponiamo di un’istanza in esecuzione dell’SDK in **Autore** modalità. L’SDK può essere avviato anche in **Pubblica** per simulare un ambiente AEM Publish.

Una guida più dettagliata per la creazione di un ambiente di sviluppo locale [si trova qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. Nel file system locale, crea una cartella dedicata per installare l’istanza Publish, denominata `~/aem-sdk/publish`.
1. Copia il file jar Quickstart utilizzato per l’istanza di authoring nei capitoli precedenti e incollalo in `publish` directory. In alternativa, accedi a [Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html) e scarica l’SDK più recente ed estrae il file jar Quickstart.
1. Rinomina il file jar in `aem-publish-p4503.jar`.

   Il `publish` string specifica che il file jar Quickstart viene avviato in modalità di pubblicazione. Il `p4503` specifica che il server Quickstart viene eseguito sulla porta 4503.

1. Apri una nuova finestra del terminale e passa alla cartella che contiene il file jar. Installa e avvia l’istanza AEM:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Immetti una password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l’impostazione predefinita per lo sviluppo locale per evitare configurazioni aggiuntive.
1. Al termine dell’installazione dell’istanza AEM, si aprirà una nuova finestra del browser all’indirizzo [http://localhost:4503/content.html](http://localhost:4503/content.html)

   Dovrebbe restituire una pagina 404 Non trovato. Si tratta di una nuova istanza dell’AEM e non è stato installato alcun contenuto.

## Installare contenuti di esempio ed endpoint GraphQL {#wknd-site-content-endpoints}

Proprio come nell’istanza Autore, l’istanza Pubblica deve avere gli endpoint GraphQL abilitati e deve avere contenuti di esempio. Quindi, installa il sito di riferimento WKND sull’istanza Publish.

1. Scarica l’ultimo pacchetto AEM compilato per il sito WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Assicurati di scaricare la versione standard compatibile con AEM as a Cloud Service e **non** il `classic` versione.

1. Accedi all’istanza Publish direttamente in: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) con il nome utente `admin` e password `admin`.
1. Quindi, passa a Gestione pacchetti all’indirizzo [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Clic **Carica pacchetto** e scegli il pacchetto WKND scaricato nel passaggio precedente. Per installare il pacchetto, fai clic su **Installa**.
1. Dopo aver installato il pacchetto, il sito di riferimento WKND è ora disponibile all’indirizzo [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Esci come `admin` facendo clic sul pulsante Disconnetti nella barra dei menu.

   ![Sito di riferimento per l’uscita da WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   A differenza dell’istanza di AEM Author, per impostazione predefinita le istanze di AEM Publish hanno accesso anonimo in sola lettura. Vogliamo simulare l’esperienza di un utente anonimo durante l’esecuzione dell’applicazione React.

## Aggiornare le variabili di ambiente per puntare all’istanza Publish {#react-app-publish}

Quindi, aggiorna le variabili di ambiente utilizzate dall’applicazione React in modo che puntino all’istanza Publish. L’app React dovrebbe **solo** connettersi all’istanza Publish in modalità di produzione.

Aggiungi un nuovo file `.env.production.local` per simulare l’esperienza di produzione.

1. Apri l’app WKND GraphQL React nell’IDE.

1. Sotto `aem-guides-wknd-graphql/react-app`, aggiungi un file denominato `.env.production.local`.
1. Popolare `.env.production.local` con le seguenti caratteristiche:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Aggiungi nuovo file di variabile di ambiente](assets/publish-deployment/env-production-local-file.png)

   L’utilizzo delle variabili di ambiente consente di alternare facilmente l’endpoint GraphQL tra un ambiente Author o Publish senza aggiungere ulteriore logica all’interno del codice dell’applicazione. Ulteriori informazioni su [Le variabili di ambiente personalizzate per React sono disponibili qui](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Tieni presente che non sono incluse informazioni di autenticazione, in quanto per impostazione predefinita gli ambienti di pubblicazione forniscono accesso anonimo al contenuto.

## Distribuire un server Nodo statico {#static-server}

L’app React può essere avviata utilizzando il server Webpack, ma solo per lo sviluppo. Quindi, simula una distribuzione di produzione utilizzando [servire](https://github.com/vercel/serve) ospitare una build di produzione dell’app React utilizzando Node.js.

1. Apri una nuova finestra del terminale e passa alla `aem-guides-wknd-graphql/react-app` directory

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Installa [servire](https://github.com/vercel/serve) con il comando seguente:

   ```shell
   $ npm install serve --save-dev
   ```

1. Apri il file `package.json` a `react-app/package.json`. Aggiungi uno script denominato `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   Il `serve` lo script esegue due azioni. Innanzitutto, viene generata una build di produzione dell’app React. In secondo luogo, il server Node.js viene avviato e utilizza la build di produzione.

1. Torna al terminale e immetti il comando per avviare il server statico:

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. Apri un nuovo browser e passa a [http://localhost:5000/](Http://localhost:5000/). Dovresti vedere l’app React distribuita.

   ![App React distribuita](assets/publish-deployment/react-app-served-port5000.png)

   La query GraphQL funziona sulla home page. Inspect **XHR** tramite gli strumenti di sviluppo. Osserva che GraphQL POST si trova nell’istanza Publish in `http://localhost:4503/content/graphql/global/endpoint.json`.

   Tuttavia, tutte le immagini sono rotte nella home page.

1. Fai clic su una delle pagine Dettagli avventura.

   ![Errore dettagli avventura](assets/publish-deployment/adventure-detail-error.png)

   Osserva che viene generato un errore GraphQL per `adventureContributor`. Negli esercizi successivi, le immagini interrotte e `adventureContributor` i problemi sono stati risolti.

## Riferimenti immagine assoluti {#absolute-image-references}

Le immagini appaiono interrotte perché `<img src` L&#39;attributo è impostato su un percorso relativo e punta al server statico Node in corrispondenza di `http://localhost:5000/`. Queste immagini devono invece puntare all’istanza AEM Publish. Esistono diverse possibili soluzioni a questo problema. Quando si utilizza il server di sviluppo Webpack, il file `react-app/src/setupProxy.js` configurare un proxy tra il server webpack e l&#39;istanza di authoring AEM per le richieste di `/content`. Una configurazione proxy può essere utilizzata in un ambiente di produzione, ma deve essere configurata a livello di server web. Ad esempio: [Modulo proxy di Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

L’app può essere aggiornata per includere un URL assoluto utilizzando `REACT_APP_HOST_URI` variabile di ambiente. Invece, usiamo una funzione dell’API GraphQL dell’AEM per richiedere un URL assoluto all’immagine.

1. Arresta il server Node.js.
1. Torna all’IDE e apri il file `Adventures.js` a `react-app/src/components/Adventures.js`.
1. Aggiungi il `_publishUrl` proprietà per il `ImageRef` all&#39;interno del `allAdventuresQuery`:

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` e `_authorUrl` sono valori incorporati in `ImageRef` per semplificare l&#39;inclusione degli url assoluti.

1. Ripeti i passaggi precedenti per modificare la query utilizzata nella `filterQuery(activity)` funzione per includere `_publishUrl` proprietà.
1. Modifica il `AdventureItem` componente in `function AdventureItem(props)` per fare riferimento a `_publishUrl` invece del `_path` durante la costruzione di `<img src=''>` tag:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Apri il file `AdventureDetail.js` a `react-app/src/components/AdventureDetail.js`.
1. Ripeti gli stessi passaggi per modificare la query GraphQL e aggiungere `_publishUrl` proprietà per l&#39;avventura

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. Modifica le due opzioni `<img>` tag per l’immagine principale di Adventure e riferimento per l’immagine collaboratore in `AdventureDetail.js`:

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. Tornare al terminale e avviare il server statico:

   ```shell
   $ npm run serve
   ```

1. Accedi a [http://localhost:5000/](Http://localhost:5000/) e osservano che le immagini appaiono e che il `<img src''>` attribuire punti a `http://localhost:4503`.

   ![Immagini interrotte corrette](assets/publish-deployment/broken-images-fixed.png)

## Simula pubblicazione di contenuti {#content-publish}

Ricorda che viene generato un errore GraphQL per `adventureContributor` quando viene richiesta una pagina Dettagli avventura. Il **Collaboratore** Il modello per frammenti di contenuto non esiste ancora nell’istanza Publish. Aggiornamenti apportati al **Avventura** Nell’istanza Publish non è disponibile neanche il modello per frammenti di contenuto. Queste modifiche sono state apportate direttamente all’istanza Author e devono essere distribuite all’istanza Publish.

È un aspetto da considerare quando si implementano nuovi aggiornamenti a un’applicazione che si basa sugli aggiornamenti a un frammento di contenuto o a un modello per frammenti di contenuto.

Quindi, simula la pubblicazione di contenuti tra le istanze Autore locale e Pubblica.

1. Avvia l’istanza di authoring (se non è già stata avviata) e passa a Gestione pacchetti all’indirizzo [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Scaricare il pacchetto [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) e installarlo utilizzando Gestione pacchetti.

   Questo pacchetto installa una configurazione che consente all’istanza di authoring di pubblicare i contenuti nell’istanza di pubblicazione. Passaggi manuali per [questa configurazione si trova qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > In un ambiente AEM as a Cloud Service, il livello di authoring viene impostato automaticamente per distribuire i contenuti al livello di pubblicazione.

1. Dalla sezione **Inizio AEM** menu, passa a **Strumenti** > **Risorse** > **Modelli per frammenti di contenuto**.

1. Fai clic su nella **Sito WKND** cartella.

1. Selezionate tutti e tre i modelli e fate clic su **Pubblica**:

   ![Pubblicare modelli per frammenti di contenuto](assets/publish-deployment/publish-contentfragment-models.png)

   Viene visualizzata una finestra di dialogo di conferma, fai clic su **Pubblica**.

1. Accedi al frammento di contenuto del Bali Surf Camp all’indirizzo [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Fai clic su **Pubblica** nella barra dei menu superiore.

   ![Fai clic sul pulsante Pubblica nell’Editor frammento di contenuto](assets/publish-deployment/publish-bali-content-fragment.png)

1. La Pubblicazione guidata mostra tutte le risorse dipendenti da pubblicare. In questo caso, il frammento a cui si fa riferimento **stacey-roswells** è elencato e sono presenti riferimenti a diverse immagini. Le risorse a cui si fa riferimento vengono pubblicate insieme al frammento.

   ![Risorse di riferimento da pubblicare](assets/publish-deployment/referenced-assets.png)

   Fai clic su **Pubblica** per pubblicare il frammento di contenuto e le risorse dipendenti.

1. Torna all’app React in esecuzione il [http://localhost:5000/](Http://localhost:5000/). Ora puoi fare clic sul Bali Surf Camp per visualizzare i dettagli dell’avventura.

1. Torna all’istanza di AEM Author all’indirizzo [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e aggiorna **Titolo** del frammento. **Salva e chiudi** il frammento. Then **pubblicare** il frammento.
1. Torna a [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e osserva le modifiche pubblicate.

   ![Aggiornamento pubblicazione campo surf di Bali](assets/publish-deployment/bali-surf-camp-update.png)

## Aggiorna configurazione COR

L’AEM è protetto per impostazione predefinita e non consente alle proprietà web non AEM di effettuare chiamate lato client. La configurazione CORS (Cross-Origin Resource Sharing) dell’AEM può consentire a domini specifici di effettuare chiamate all’AEM.

Quindi, prova la configurazione CORS dell’istanza AEM Publish.

1. Torna alla finestra del terminale in cui l’app React è in esecuzione con il comando `npm run serve`:

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   Tieni presente che vengono forniti due URL. Uno che utilizza `localhost` e un altro utilizzando l&#39;indirizzo IP della rete locale.

1. Passa all’indirizzo che inizia con [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). L&#39;indirizzo è leggermente diverso per ogni computer locale. Osserva che si è verificato un errore CORS durante il recupero dei dati. Questo perché la configurazione CORS corrente consente solo richieste da `localhost`.

   ![Errore CORS](assets/publish-deployment/cors-error-not-fetched.png)

   Quindi, aggiorna la configurazione AEM Publish CORS per consentire le richieste dall’indirizzo IP della rete.

1. Accedi a [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) e accedi con il nome utente `admin` e password `admin`.

1. Accedi a [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) e trovare la configurazione WKND GraphQL all’indirizzo `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Aggiornare il **Origini consentite** per includere l&#39;indirizzo IP di rete:

   ![Aggiorna configurazione CORS](assets/publish-deployment/cors-update.png)

   È inoltre possibile includere un’espressione regolare per consentire tutte le richieste da un sottodominio specifico. Salva le modifiche.

1. Cerca **Filtro referrer Apache Sling** e controlla la configurazione. Il **Consenti vuoto** è necessaria anche la configurazione per abilitare le richieste di GraphQL da un dominio esterno.

   ![Filtro referrer sling](assets/publish-deployment/sling-referrer-filter.png)

   Questi sono stati configurati come parte del sito di riferimento WKND. Puoi visualizzare l’intero set di configurazioni OSGi tramite [archivio GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > Le configurazioni OSGi vengono gestite in un progetto AEM impegnato nel controllo del codice sorgente. Un progetto AEM può essere implementato nell’AEM come ambienti di Cloud Service utilizzando Cloud Manager. Il [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) può aiutare a generare un progetto per un’implementazione specifica.

1. Torna all’app React che inizia con [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) e osserva che l’applicazione non genera più un errore CORS.

   ![Errore CORS corretto](assets/publish-deployment/cors-error-corrected.png)

## Congratulazioni.  {#congratulations}

Congratulazioni. Ora hai simulato una distribuzione di produzione completa utilizzando un ambiente AEM Publish. Hai anche imparato a utilizzare la configurazione CORS in AEM.

## Altre risorse

Per ulteriori dettagli su Frammenti di contenuto e GraphQL, consulta le risorse seguenti:

* [Distribuzione di contenuti headless tramite frammenti di contenuto con GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=it)
* [API GraphQL di AEM per l’utilizzo con Frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=it)
* [Autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [Distribuzione del codice in AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
