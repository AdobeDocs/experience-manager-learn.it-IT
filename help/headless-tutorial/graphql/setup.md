---
title: Configurazione rapida - Guida introduttiva a AEM headless - GraphQL
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Installa l’SDK AEM, aggiungi contenuto di esempio e distribuisci un’applicazione che consuma contenuto da AEM utilizzando le relative API GraphQL. Scopri come AEM le esperienze omnicanale.
sub-product: sites
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
translation-type: tm+mt
source-git-commit: 2ea667d3bdb73fa4da87b877f14db77d896448a7
workflow-type: tm+mt
source-wordcount: '1590'
ht-degree: 2%

---


# Configurazione rapida {#setup}

>[!CAUTION]
>
> L&#39;API AEM GraphQL per la distribuzione dei frammenti di contenuto verrà rilasciata all&#39;inizio del 2021.
> La documentazione correlata è disponibile a scopo di anteprima.

Questo capitolo offre una rapida configurazione di un ambiente locale per visualizzare un&#39;applicazione esterna consuma contenuto da AEM utilizzando AEM API GraphQL. I capitoli successivi dell&#39;esercitazione verranno completati da questa configurazione.

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext= Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/it/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Obiettivi {#objectives}

1. Scarica e installa l’SDK AEM.
1. Scaricate e installate contenuti di esempio dal sito WKND Reference.
1. Scaricate e installate un&#39;app di esempio per utilizzare il contenuto tramite le API GraphQL.

## Installare l&#39;SDK AEM{#aem-sdk}

Questa esercitazione utilizza la [AEM come Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk) per esplorare AEM API GraphQL. Questa sezione fornisce una guida rapida all’installazione dell’SDK AEM ed esecuzione in modalità Autore. Una guida più dettagliata per la configurazione di un ambiente di sviluppo locale [è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

>[!NOTE]
>
> È inoltre possibile seguire l&#39;esercitazione con un AEM come ambiente Cloud Service. Durante l&#39;esercitazione sono incluse note aggiuntive sull&#39;utilizzo di un ambiente Cloud.

1. Andate al **[portale di distribuzione del software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html)** > **AEM come Cloud Service** e scaricate la versione più recente dell&#39; **AEM SDK**.

   ![Portale di distribuzione software](assets/setup/software-distribution-portal-download.png)

1. Decomprimete il download e copiate il file JAR di avvio rapido (`aem-sdk-quickstart-XXX.jar`) in una cartella dedicata, ad esempio `~/aem-sdk/author`.
1. Rinominare il file jar su `aem-author-p4502.jar`.
1. Aprite una nuova finestra del terminale e individuate la cartella che contiene il file JAR. Eseguite il comando seguente per installare e avviare l&#39;istanza AEM:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Immettete una password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare la password predefinita per lo sviluppo locale per ridurre la necessità di riconfigurarla.
1. Dopo alcuni minuti l&#39;istanza AEM finirà l&#39;installazione e una nuova finestra del browser dovrebbe aprirsi in [http://localhost:4502](Http://localhost:4502).
1. Accedete con il nome utente `admin` e la password `admin`.

>[!CAUTION]
>
> Per continuare la configurazione, la funzione GraphQL deve ora essere abilitata manualmente nell’SDK di Quickstart. Rivolgiti al tuo contatto  Adobe per ulteriori istruzioni. Questo passaggio manuale è necessario solo fino al rilascio della funzione nel 2021.

## Installare il contenuto di esempio{#wknd-site}

Il contenuto di esempio del sito WKND Reference verrà installato per accelerare l&#39;esercitazione. Il WKND è un marchio fittizio in stile di vita, spesso utilizzato in combinazione con AEM formazione.

1. Scarica l&#39;ultimo pacchetto AEM compilato per il sito WKND: [aem-guide-wknd.all-x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Accertatevi di scaricare la versione standard compatibile con AEM come Cloud Service e **not** la versione `classic`.

1. Dal menu **AEM Start** passare a **Strumenti** > **Distribuzione** > **Pacchetti**.

   ![Passa ai pacchetti](assets/setup/navigate-to-packages.png)

1. Fate clic su **Carica pacchetto** e scegliete il pacchetto WKND scaricato nel passaggio precedente. Fate clic su **Installa** per installare il pacchetto.

1. Dal menu **AEM Start** passare a **Risorse** > **File**.
1. Fare clic tra le cartelle per passare a **WKND Site** > **English** > **Adventures**.

   ![Vista cartella delle avventure](assets/setup/folder-view-adventures.png)

   Si tratta di una cartella di tutte le risorse che comprendono le varie Avventure promosse dal marchio WKND. Sono inclusi i tipi di supporti tradizionali come immagini e video, nonché i supporti specifici per AEM come **Frammenti di contenuto**.

1. Fare clic nella cartella **Sci in discesa del Wyoming** e fare clic sulla scheda **Sci in discesa del frammento di contenuto del Wyoming**:

   ![Scorrimento della scheda frammento di contenuto](assets/setup/downhill-skiing-cntent-fragment.png)

1. L’interfaccia utente dell’editor dei frammenti di contenuto si aprirà per l’avventura Sci in discesa nel Wyoming.

   ![Frammento contenuto sciistico in discesa](assets/setup/down-hillskiing-fragment.png)

   Tenere presente che vari campi come **Title**, **Description** e **Activity** definiscono il frammento.

   **I** frammenti di contenuto sono uno dei modi in cui il contenuto può essere gestito in AEM. I frammenti di contenuto sono contenuti riutilizzabili e non presentano elementi di dati strutturati come testo, RTF, date o riferimenti ad altri frammenti di contenuto. I frammenti di contenuto verranno esplorati più dettagliatamente più avanti nell&#39;esercitazione.

1. Fare clic su **Annulla** per chiudere il frammento. Sentitevi liberi di navigare in alcune delle altre cartelle ed esplorare gli altri contenuti di Avventura.

>[!NOTE]
>
> Se utilizzi un ambiente di Cloud Service, consulta la documentazione relativa alla [implementazione di una base di codice come il sito WKND Reference in un ambiente di Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#deploying).

## Installazione di GraphQL Endpoints{#graphql-endpoint}

È necessario configurare gli endpoint GraphQL. Questo offre flessibilità al progetto per determinare l&#39;endpoint esatto in cui è esposta l&#39;API GraphQL. È inoltre necessario un [CORS](#cors-config) per concedere l&#39;accesso a un&#39;applicazione esterna. Per accelerare l&#39;esercitazione, è stato già creato un pacchetto.

1. Scaricate il pacchetto [aem-guide-wknd-graphql.all-1.0.0-SNAPSHOT.zip](./assets/setup/aem-guides-wknd-graphql.all-1.0.0-SNAPSHOT.zip).
1. Dal menu **AEM Start** passare a **Strumenti** > **Distribuzione** > **Pacchetti**.
1. Fate clic su **Carica pacchetto** e scegliete il pacchetto scaricato nel passaggio precedente. Fate clic su **Installa** per installare il pacchetto.

Il pacchetto di cui sopra contiene anche [GraphiQL tool](https://github.com/graphql/graphiql) che verrà utilizzato in capitoli successivi. Ulteriori informazioni sulla configurazione CORS sono disponibili [sotto](#cors-config).

## Installa l&#39;app di esempio{#sample-app}

Uno degli obiettivi di questa esercitazione è mostrare come utilizzare AEM contenuto da un&#39;applicazione esterna utilizzando le API GraphQL. In questa esercitazione viene utilizzato un esempio di React App parzialmente completato per accelerare l&#39;esercitazione. Le stesse lezioni e gli stessi concetti si applicano alle app create con iOS, Android o qualsiasi altra piattaforma. L&#39;app React è intenzionalmente semplice, per evitare inutili complessità; non si tratta di un&#39;implementazione di riferimento.

1. Aprite una nuova finestra di terminale e clonate il ramo iniziale dell&#39;esercitazione utilizzando Git:

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Nell&#39;IDE di vostra scelta, aprite il file `.env.development` in `aem-guides-wknd-graphql/react-app/.env.development`. Rimuovete il commento dalla riga `REACT_APP_AUTHORIZATION` in modo che il file abbia l&#39;aspetto seguente:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/endpoint.gql
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Assicurarsi che `React_APP_HOST_URI` corrisponda all&#39;istanza AEM locale. In questo capitolo, l&#39;app React verrà collegata direttamente all&#39;ambiente AEM **Author** e dovrà pertanto essere autenticata. Si tratta di una pratica comune durante lo sviluppo, in quanto ci consente di apportare rapidamente modifiche all&#39;ambiente AEM e di visualizzarle immediatamente riflesse nell&#39;app.

   >[!NOTE]
   >
   > In uno scenario di produzione, l&#39;app si connetterà a un ambiente AEM **Pubblica**. Viene descritto più dettagliatamente, più avanti nell’esercitazione.

1. Individuate la cartella `aem-guides-wknd-graphql/react-app`. Installate e avviate l&#39;app:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Una nuova finestra del browser dovrebbe avviare automaticamente l&#39;app in [http://localhost:3000](Http://localhost:3000).

   ![React starter app](assets/setup/react-starter-app.png)

   Deve essere visualizzato un elenco dei contenuti Adventure correnti da AEM.

1. Fai clic su una delle immagini dell&#39;avventura per vedere i dettagli dell&#39;avventura. Viene AEM una richiesta per restituire i dettagli di un&#39;avventura.

   ![Visualizzazione Dettagli avventura](assets/setup/adventure-details-view.png)

1. Utilizzate gli strumenti di sviluppo del browser per esaminare le richieste **Network**. Visualizzare le richieste **XHR** e osservare più richieste di POST a `/content/graphql/endpoint.gql`, l&#39;endpoint GraphQL configurato per AEM.

   ![Richiesta XHR dell&#39;endpoint GraphQL](assets/setup/endpoint-gql.png)

1. Puoi anche visualizzare i parametri e la risposta JSON controllando la richiesta di rete. Potrebbe essere utile installare un&#39;estensione del browser come [GraphQL Network](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm) per Chrome per ottenere una migliore comprensione della query e della risposta.

   ![GraphQL Network Extension](assets/setup/GraphQL-extension.png)

   *Utilizzo dell&#39;estensione Chrome GraphQL Network*

## Modifica di un frammento di contenuto

Ora che l&#39;app React è in esecuzione, effettua un aggiornamento al contenuto in AEM e visualizza la modifica riflessa nell&#39;app.

1. Andate a AEM [http://localhost:4502](Http://localhost:4502).
1. Passare a **Risorse** > **File** > **WKND Site** > **English** > **Avventure** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Cartella Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Fare clic sul frammento di contenuto **Bali Surf Camp** per aprire l&#39;Editor frammento di contenuto.
1. Modificare il **Titolo** e la **Descrizione** dell&#39;avventura

   ![Modifica frammento di contenuto](assets/setup/modify-content-fragment-bali.png)

1. Fare clic su **Salva** per salvare le modifiche.
1. Torna all&#39;app React all&#39;indirizzo [http://localhost:3000](Http://localhost:3000) e aggiorna per visualizzare le modifiche:

   ![Bali Surf Camp Adventure](assets/setup/overnight-bali-surf-camp-changes.png)

## Congratulazioni! {#congratulations}

Congratulazioni, ora disponete di un&#39;applicazione esterna che consuma AEM contenuto con GraphQL. Potete controllare il codice nell’app React e continuare a provare a modificare i frammenti di contenuto esistenti.

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [Defining Content Fragment Models](content-fragment-models.md) (Definizione di modelli di frammenti di contenuto), viene illustrato come modellare il contenuto e creare uno schema con **Content Fragment Models**. Verranno esaminati i modelli esistenti e verrà creato un nuovo modello. Verranno inoltre illustrati i diversi tipi di dati che è possibile utilizzare per definire uno schema come parte del modello.

## (Bonus) Configurazione CORS {#cors-config}

AEM, essendo sicura per impostazione predefinita, blocca le richieste tra le origini, impedendo alle applicazioni non autorizzate di connettersi e visualizzarne il contenuto.

Per consentire all&#39;app React di questa esercitazione di interagire con AEM endpoint API GraphQL, nel pacchetto degli endpoint GraphQL è stata definita una configurazione per la condivisione di risorse tra origini.

![Configurazione della condivisione delle risorse tra origini](./assets/setup/cross-origin-resource-sharing-configuration.png)

Per configurare manualmente questo comando:

1. Andate AEM console Web dell&#39;SDK in **Strumenti** > **Operazioni** > **Console Web**
1. Fare clic sulla riga **Adobe Granite Cross-Origin Resource Sharing Policy** per creare una nuova configurazione
1. Aggiornate i campi seguenti, lasciando gli altri con i relativi valori predefiniti:
   * Origini consentite: `localhost:3000`
   * Origini consentite (Regex): `.* `
   * Percorsi consentiti: `/content/graphql/endpoint.gql`
   * Metodi Consentiti: `GET`, `HEAD`, `POST`
      * È necessario solo `POST` per GraphQL, ma gli altri metodi possono essere utili quando interagiscono con AEM in modo headless.
   * Supporta le credenziali: `Yes`
      * Questo è richiesto perché la nostra app React comunicherà con i punti finali GraphQL protetti sul servizio AEM Author.
1. Fai clic su **Salva**

Questa configurazione consente di `POST` richieste HTTP provenienti da `localhost:3000` al servizio AEM Author sul percorso `/content/graphql/endpoint.gql`.

Questa configurazione e gli endpoint GraphQL vengono generati da un progetto AEM. [Visualizza i dettagli qui](https://github.com/adobe/aem-guides-wknd-graphql/tree/master/aem-project).
