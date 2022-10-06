---
title: Distribuzione di produzione utilizzando un servizio AEM Publish - Guida introduttiva a AEM Headless - GraphQL
description: Scopri i servizi Author e Publish di AEM e il modello di implementazione consigliato per le applicazioni headless. In questa esercitazione, scopri come utilizzare le variabili di ambiente per modificare dinamicamente un endpoint GraphQL in base all’ambiente di destinazione. Scopri come configurare AEM per la condivisione di risorse tra origini diverse (CORS, Cross-Origin resource sharing).
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
ht-degree: 8%

---

# Distribuzione di produzione con un servizio AEM Publish

In questa esercitazione verrà impostato un ambiente locale per simulare il contenuto distribuito da un’istanza di authoring a un’istanza di pubblicazione. Puoi anche generare la build di produzione di un’app React configurata per utilizzare contenuti dall’ambiente di pubblicazione AEM utilizzando le API GraphQL . Man mano che procedi, imparerai a utilizzare in modo efficace le variabili di ambiente e come aggiornare le configurazioni AEM CORS.

## Prerequisiti

Questa esercitazione fa parte di un tutorial in più parti. Si presume che i passaggi descritti nelle parti precedenti siano stati completati.

## Obiettivi

Scopri come:

* Comprendere l’architettura di authoring e pubblicazione di AEM.
* Scopri le best practice per la gestione delle variabili di ambiente.
* Scopri come configurare AEM in modo appropriato per la condivisione di risorse tra origini diverse (CORS, Cross-Origin resource sharing).

## Pattern di distribuzione di pubblicazione dell’autore {#deployment-pattern}

Un ambiente AEM completo è costituito da Author, Publish e Dispatcher. Il servizio di authoring è il luogo in cui gli utenti interni creano, gestiscono e visualizzano in anteprima i contenuti. Il servizio Publish è considerato l’ambiente &quot;Live&quot; ed è tipicamente quello con cui gli utenti finali interagiscono. I contenuti vengono modificati e approvati nel servizio di authoring e quindi distribuiti al servizio di pubblicazione.

Il modello di implementazione più comune per le applicazioni headless AEM consiste nella connessione della versione di produzione dell’applicazione a un servizio di pubblicazione AEM.

![Pattern di distribuzione di alto livello](assets/publish-deployment/high-level-deployment.png)

Il diagramma riportato sopra mostra questo diffuso pattern di distribuzione.

1. A **Autore del contenuto** utilizza il servizio di authoring AEM per creare, modificare e gestire i contenuti.
2. L’**autore del contenuto** e altri utenti interni possono visualizzare in anteprima il contenuto direttamente sul servizio di authoring. È possibile impostare una versione di anteprima dell’applicazione che si connette al servizio di authoring.
3. Una volta che il contenuto è stato approvato, può essere **pubblicato** al servizio AEM Publish.
4. **Gli utenti finali interagiscono con la versione di produzione dell’applicazione.** L’applicazione Production si connette al servizio Publish e utilizza le API GraphQL per richiedere e utilizzare i contenuti.

L’esercitazione simula la distribuzione di cui sopra aggiungendo un’istanza di AEM Publish alla configurazione corrente. Nei capitoli precedenti l’app React ha agito come anteprima mediante la connessione diretta all’istanza di authoring. Una build di produzione dell’app React viene distribuita in un server Node.js statico che si connette alla nuova istanza Publish.

Alla fine, tre server locali sono in esecuzione:

* http://localhost:4502 - Istanza di authoring
* http://localhost:4503 - Pubblica istanza
* http://localhost:5000 - React App in modalità di produzione, connettendosi all&#39;istanza Publish.

## Installare AEM SDK - Modalità di pubblicazione {#aem-sdk-publish}

Attualmente disponiamo di un&#39;istanza in esecuzione dell&#39;SDK in **Autore** modalità. L’SDK può essere avviato anche in **Pubblica** per simulare un ambiente AEM Publish.

Una guida più dettagliata per la creazione di un ambiente di sviluppo locale [si trova qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. Nel file system locale, crea una cartella dedicata per installare l’istanza Publish, ad esempio denominata `~/aem-sdk/publish`.
1. Copia il file jar Quickstart utilizzato per l’istanza Author nei capitoli precedenti e incollalo nel `publish` directory. In alternativa, passa alla [Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html) e scarica l&#39;SDK più recente ed estrae il file jar Quickstart.
1. Rinomina il file jar in `aem-publish-p4503.jar`.

   La `publish` string specifica che il jar Quickstart viene avviato in modalità Publish. La `p4503` specifica che il server Quickstart viene eseguito sulla porta 4503.

1. Apri una nuova finestra terminale e passa alla cartella che contiene il file jar. Installa e avvia l&#39;istanza AEM:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Fornisci una password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l&#39;impostazione predefinita per lo sviluppo locale per evitare configurazioni aggiuntive.
1. Al termine dell’installazione dell’istanza AEM, viene aperta una nuova finestra del browser in [http://localhost:4503/content.html](http://localhost:4503/content.html)

   È previsto che venga restituita una pagina 404 Non trovato . Questa è una nuova istanza AEM e non è stato installato alcun contenuto.

## Installare il contenuto di esempio e gli endpoint GraphQL {#wknd-site-content-endpoints}

Come nell’istanza Author, l’istanza Publish deve avere gli endpoint GraphQL abilitati e deve disporre di contenuto di esempio. Quindi, installa il sito di riferimento WKND sull&#39;istanza Publish.

1. Scarica l&#39;ultimo pacchetto di AEM compilato per il sito WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Assicurati di scaricare la versione standard compatibile con AEM as a Cloud Service e **not** la `classic` versione.

1. Accedi all’istanza Publish passando direttamente a: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) con il nome utente `admin` e password `admin`.
1. Quindi, passa a Gestione pacchetti in [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Fai clic su **Carica pacchetto** e scegli il pacchetto WKND scaricato nel passaggio precedente. Per installare il pacchetto, fai clic su **Installa**.
1. Dopo aver installato il pacchetto, il sito di riferimento WKND è ora disponibile in [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Esci come `admin` facendo clic sul pulsante &quot;Esci&quot; nella barra dei menu.

   ![Sito di riferimento per la disconnessione WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   A differenza dell’istanza di AEM Author, le istanze di AEM Publish hanno come impostazione predefinita l’accesso in sola lettura anonimo. Vogliamo simulare l&#39;esperienza di un utente anonimo durante l&#39;esecuzione dell&#39;applicazione React.

## Aggiorna le variabili di ambiente per puntare all&#39;istanza Publish {#react-app-publish}

Quindi, aggiorna le variabili di ambiente utilizzate dall&#39;applicazione React in modo che puntino all&#39;istanza Publish. L&#39;app React deve **only** connettiti all’istanza Publish in modalità di produzione.

Quindi, aggiungi un nuovo file `.env.production.local` per simulare l’esperienza di produzione.

1. Apri l’app WKND GraphQL React nell’IDE.

1. Sotto `aem-guides-wknd-graphql/react-app`, aggiungi un file denominato `.env.production.local`.
1. Popolare `.env.production.local` con le seguenti caratteristiche:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Aggiungi un nuovo file di variabili di ambiente](assets/publish-deployment/env-production-local-file.png)

   L’utilizzo delle variabili di ambiente rende facile attivare/disattivare l’endpoint GraphQL tra un ambiente di authoring o di pubblicazione senza aggiungere ulteriore logica nel codice dell’applicazione. Ulteriori informazioni [le variabili di ambiente personalizzate per React sono disponibili qui](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Osserva che non sono incluse informazioni di autenticazione, in quanto gli ambienti di pubblicazione forniscono l’accesso anonimo al contenuto per impostazione predefinita.

## Distribuire un server dei nodi statico {#static-server}

L’app React può essere avviata utilizzando il server webpack, ma solo per lo sviluppo. Quindi, simulare un&#39;implementazione di produzione utilizzando [servire](https://github.com/vercel/serve) ospitare una build di produzione dell’app React utilizzando Node.js.

1. Apri una nuova finestra terminale e passa alla `aem-guides-wknd-graphql/react-app` directory

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Installa [servire](https://github.com/vercel/serve) con il comando seguente:

   ```shell
   $ npm install serve --save-dev
   ```

1. Apri il file . `package.json` a `react-app/package.json`. Aggiungi uno script denominato `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   La `serve` lo script esegue due azioni. Innanzitutto, viene generata una build di produzione dell’app React. In secondo luogo, il server Node.js avvia e utilizza la build di produzione.

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

1. Apri un nuovo browser e passa a [http://localhost:5000/](Http://localhost:5000/). Dovresti vedere l’app React che viene servita.

   ![React App Served](assets/publish-deployment/react-app-served-port5000.png)

   La query GraphQL funziona sulla home page. Inspect **XHR** richiedi tramite gli strumenti per sviluppatori. Tieni presente che GraphQL POST si trova nell’istanza Publish in `http://localhost:4503/content/graphql/global/endpoint.json`.

   Tuttavia, tutte le immagini sono interrotte sulla home page!

1. Fai clic su in una delle pagine Adventure Detail.

   ![Errore di dettaglio dell&#39;avventura](assets/publish-deployment/adventure-detail-error.png)

   Osserva che viene generato un errore GraphQL per `adventureContributor`. Negli esercizi successivi, le immagini rotte e `adventureContributor` problemi risolti.

## Riferimenti a immagini assolute {#absolute-image-references}

Le immagini appaiono interrotte perché il `<img src` è impostato su un percorso relativo e finisce per puntare al server statico Node in `http://localhost:5000/`. Queste immagini dovrebbero invece puntare all’istanza di AEM Publish. Ci sono diverse soluzioni potenziali a questo problema. Quando si utilizza webpack dev server il file `react-app/src/setupProxy.js` configura un proxy tra il server webpack e l&#39;istanza autore AEM per tutte le richieste a `/content`. Una configurazione proxy può essere utilizzata in un ambiente di produzione ma deve essere configurata a livello di server web. Ad esempio: [Modulo proxy Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

L’app può essere aggiornata per includere un URL assoluto utilizzando `REACT_APP_HOST_URI` variabile di ambiente. Invece, usiamo una funzione dell’API GraphQL AEM per richiedere un URL assoluto all’immagine.

1. Arresta il server Node.js .
1. Torna all’IDE e apri il file . `Adventures.js` a `react-app/src/components/Adventures.js`.
1. Aggiungi il `_publishUrl` della proprietà `ImageRef` all&#39;interno del `allAdventuresQuery`:

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

   `_publishUrl` e `_authorUrl` sono valori incorporati in `ImageRef` per facilitare l’inclusione degli url assoluti.

1. Ripeti i passaggi precedenti per modificare la query utilizzata nella `filterQuery(activity)` per includere `_publishUrl` proprietà.
1. Modifica la `AdventureItem` componente a `function AdventureItem(props)` per fare riferimento al `_publishUrl` anziché `_path` durante la costruzione della `<img src=''>` tag:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Apri il file . `AdventureDetail.js` a `react-app/src/components/AdventureDetail.js`.
1. Ripeti gli stessi passaggi per modificare la query GraphQL e aggiungere la `_publishUrl` proprietà per l&#39;avventura

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

1. Modificare i due `<img>` tag per l’immagine principale di Avventura e il riferimento immagine da collaboratore in `AdventureDetail.js`:

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

1. Torna al terminale e avvia il server statico:

   ```shell
   $ npm run serve
   ```

1. Passa a [http://localhost:5000/](Http://localhost:5000/) e osserva che le immagini appaiono e che la `<img src''>` punti di attributo `http://localhost:4503`.

   ![Correzione di immagini interrotte](assets/publish-deployment/broken-images-fixed.png)

## Simulazione della pubblicazione dei contenuti {#content-publish}

Ricorda che viene generato un errore GraphQL per `adventureContributor` quando viene richiesta una pagina Dettagli avventura. La **Collaboratore** Il modello per frammenti di contenuto non esiste ancora nell’istanza di pubblicazione. Aggiornamenti apportati al **Avventura** Anche il modello per frammenti di contenuto non è disponibile nell’istanza di pubblicazione. Queste modifiche sono state apportate direttamente all’istanza Author e devono essere distribuite all’istanza Publish.

Questo è un aspetto da considerare durante il rollout di nuovi aggiornamenti di un’applicazione che si basa sugli aggiornamenti di un frammento di contenuto o di un modello di frammento di contenuto.

Quindi, consente di simulare la pubblicazione di contenuti tra le istanze Author e Publish locali.

1. Avvia l’istanza di authoring (se non è già stata avviata) e passa a Gestione pacchetti all’indirizzo [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Scarica il pacchetto [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) e installalo utilizzando Gestione pacchetti.

   Questo pacchetto installa una configurazione che consente all’istanza Author di pubblicare contenuti nell’istanza Publish. Passaggi manuali per [questa configurazione si trova qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > In un ambiente AEM as a Cloud Service, il livello Autore viene impostato automaticamente per distribuire il contenuto al livello di pubblicazione.

1. Da **Inizio AEM** menu, vai a **Strumenti** > **Risorse** > **Modelli per frammenti di contenuto**.

1. Fai clic su **Sito WKND** cartella.

1. Seleziona tutti e tre i modelli e fai clic su **Pubblica**:

   ![Pubblicare modelli di frammenti di contenuto](assets/publish-deployment/publish-contentfragment-models.png)

   Viene visualizzata una finestra di dialogo di conferma, fai clic su **Pubblica**.

1. Passa al frammento di contenuto del campo Surf di Bali in [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Fai clic sul pulsante **Pubblica** nella barra dei menu superiore.

   ![Fai clic sul pulsante Pubblica nell’Editor frammento di contenuto](assets/publish-deployment/publish-bali-content-fragment.png)

1. La procedura guidata di pubblicazione mostra tutte le risorse dipendenti da pubblicare. In questo caso, il frammento a cui si fa riferimento **farfalle** viene elencato e viene fatto riferimento anche a diverse immagini. Le risorse a cui si fa riferimento vengono pubblicate insieme al frammento.

   ![Risorse di riferimento da pubblicare](assets/publish-deployment/referenced-assets.png)

   Fai clic sul pulsante **Pubblica** per pubblicare nuovamente il frammento di contenuto e le risorse dipendenti.

1. Torna all’app React in esecuzione in [http://localhost:5000/](Http://localhost:5000/). Ora potete cliccare sul Bali Surf Camp per vedere i dettagli dell&#39;avventura.

1. Torna all’istanza di AEM Author in [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e aggiorna **Titolo** del frammento. **Salva e chiudi** il frammento. Then **pubblicare** il frammento.
1. Torna a [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e osserva le modifiche pubblicate.

   ![Aggiornamento pubblicazione del campo di surf di Bali](assets/publish-deployment/bali-surf-camp-update.png)

## Aggiorna la configurazione del COR

AEM è protetto per impostazione predefinita e non consente alle proprietà web non AEM di effettuare chiamate lato client. AEM la configurazione CORS (Cross-Origin Resource Sharing) può consentire a domini specifici di effettuare chiamate a AEM.

Quindi, prova con la configurazione CORS dell’istanza AEM Publish.

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

   Osserva che sono forniti due URL. Uno che utilizza `localhost` e un altro utilizzando l&#39;indirizzo IP di rete locale.

1. Passa all’indirizzo che inizia con [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). L&#39;indirizzo è leggermente diverso per ogni computer locale. Tieni presente che si verifica un errore CORS durante il recupero dei dati. Questo perché la configurazione CORS corrente consente solo le richieste da `localhost`.

   ![Errore CORS](assets/publish-deployment/cors-error-not-fetched.png)

   Quindi, aggiorna la configurazione AEM Publish CORS per consentire le richieste dall’indirizzo IP di rete.

1. Passa a [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) e accedi con il nome utente `admin` e password `admin`.

1. Passa a [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) e trova la configurazione WKND GraphQL in `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Aggiorna **Origini consentite** campo per includere l&#39;indirizzo IP di rete:

   ![Aggiorna la configurazione CORS](assets/publish-deployment/cors-update.png)

   È inoltre possibile includere un’espressione regolare per consentire tutte le richieste provenienti da un sottodominio specifico. Salva le modifiche.

1. Cerca **Filtro di riferimento Apache Sling** e controlla la configurazione. La **Consenti vuoto** è inoltre necessaria per abilitare le richieste GraphQL da un dominio esterno.

   ![Sling Referrer Filter](assets/publish-deployment/sling-referrer-filter.png)

   Questi sono stati configurati come parte del sito di riferimento WKND. Puoi visualizzare l’intero set di configurazioni OSGi tramite [archivio GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > Le configurazioni OSGi sono gestite in un progetto AEM che si impegna a controllare il codice sorgente. Un progetto AEM può essere distribuito in ambienti di Cloud Service AEM utilizzando Cloud Manager. La [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype) può contribuire a generare un progetto per una specifica implementazione.

1. Torna all’app React a partire da [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) e osserva che l’applicazione non genera più un errore CORS.

   ![Errore CORS corretto](assets/publish-deployment/cors-error-corrected.png)

## Congratulazioni! {#congratulations}

Congratulazioni! Ora hai simulato un’implementazione di produzione completa utilizzando un ambiente di pubblicazione AEM. Hai anche imparato a utilizzare la configurazione CORS in AEM.

## Altre risorse

Per ulteriori dettagli sui frammenti di contenuto e su GraphQL, consulta le risorse seguenti:

* [Distribuzione di contenuti headless tramite frammenti di contenuto con GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=it)
* [API GraphQL di AEM per l’utilizzo con Frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=it)
* [Autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [Distribuzione del codice AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
