---
title: Configurazione rapida AEM Headless tramite SDK locale
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Installa l'SDK AEM, aggiungi contenuto di esempio e distribuisci un'applicazione che consuma contenuti da AEM utilizzando le API GraphQL. Scopri come AEM le esperienze omni-channel.
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 64086f3f7b340b143bd281e2f6f802af07554ecf
workflow-type: tm+mt
source-wordcount: '1258'
ht-degree: 2%

---

# Configurazione rapida AEM Headless tramite SDK locale {#setup}

La configurazione rapida AEM Headless ti permette di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un esempio di app React (a SPA) che consuma il contenuto sopra AEM API GraphQL headless. Questa guida utilizza [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/it/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 1. Installare l’SDK AEM {#aem-sdk}

Questa configurazione utilizza [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) per esplorare AEM API GraphQL. Questa sezione fornisce una guida rapida all’installazione e all’esecuzione dell’SDK AEM in modalità Autore. Una guida più dettagliata per la creazione di un ambiente di sviluppo locale [si trova qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> È inoltre possibile seguire l’esercitazione con un [AEM ambiente as a Cloud Service](./cloud-service.md). Durante l’esercitazione sono incluse ulteriori note sull’utilizzo di un ambiente Cloud.

1. Passa a **[Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** e scarica la versione più recente del **SDK AEM**.

   ![Portale di distribuzione software](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Decomprimi il download e copia il jar Quickstart (`aem-sdk-quickstart-XXX.jar`) in una cartella dedicata, ovvero `~/aem-sdk/author`.
1. Rinomina il file jar in `aem-author-p4502.jar`.

   La `author` name specifica che il jar Quickstart viene avviato in modalità Autore. La `p4502` specifica l&#39;esecuzione di Quickstart sulla porta 4502.

1. Per installare e avviare l&#39;istanza AEM, apri un prompt dei comandi nella cartella che contiene il file jar ed esegui il seguente comando :

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Fornisci una password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare `admin` per lo sviluppo locale per ridurre la necessità di riconfigurare.
1. Al termine dell&#39;installazione del servizio AEM, dovrebbe essere aperta una nuova finestra del browser in [http://localhost:4502](Http://localhost:4502).
1. Accedi con il nome utente `admin` e la password selezionata durante AEM&#39;avvio iniziale (in genere `admin`).

## 2. Installare il contenuto di esempio {#install-sample-content}

Contenuto di esempio dal **Sito di riferimento WKND** viene utilizzato per accelerare l’esercitazione. Il WKND è un marchio fittizio in stile di vita, spesso utilizzato con formazione AEM.

Il sito WKND include configurazioni necessarie per esporre un [Endpoint GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). In un’implementazione reale, segui i passaggi documentati per [includere gli endpoint GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) nel progetto del cliente. A [CORS](#cors-config) è stato anche confezionato come parte del sito WKND. È necessaria una configurazione CORS per concedere l’accesso a un’applicazione esterna, ulteriori informazioni su [CORS](#cors-config) di seguito.

1. Scarica l&#39;ultimo pacchetto di AEM compilato per il sito WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Assicurati di scaricare la versione standard compatibile con AEM as a Cloud Service e **not** la `classic` versione.

1. Da **Inizio AEM** menu, vai a **Strumenti** > **Distribuzione** > **Pacchetti**.

   ![Passa a Pacchetti](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Fai clic su **Carica pacchetto** e scegli il pacchetto WKND scaricato nel passaggio precedente. Per installare il pacchetto, fai clic su **Installa**.

1. Da **Inizio AEM** menu, vai a **Risorse** > **File** > **WKND condiviso** > **Inglese** > **Avventure**.

   ![Vista a cartelle delle avventure](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Questa è una cartella di tutte le risorse che comprendono le varie Avventure promosse dal marchio WKND. Questo include i tipi di media tradizionali come immagini e video e i supporti specifici per AEM come **Frammenti di contenuto**.

1. Fai clic su **Sci in discesa Wyoming** e fai clic su **Frammento di contenuto per lo sci in discesa** scheda:

   ![Scheda Frammento di contenuto](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. L’editor dei frammenti di contenuto si apre per l’avventura Sci in discesa nel Wyoming.

   ![Editor frammento di contenuto](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Osserva che vari campi, come **Titolo**, **Descrizione** e **Attività** definire il frammento.

   **Frammenti di contenuto** sono uno dei modi in cui il contenuto può essere gestito in AEM. I frammenti di contenuto sono contenuti riutilizzabili e non soggetti a presentazione composti da elementi di dati strutturati quali testo, testo RTF, date o riferimenti ad altri frammenti di contenuto. I frammenti di contenuto vengono esaminati più dettagliatamente più avanti nella configurazione rapida.

1. Fai clic su **Annulla** per chiudere il frammento. Sentitevi liberi di navigare in alcune delle altre cartelle ed esplorare l&#39;altro contenuto Avventura.

>[!NOTE]
>
> Se utilizzi un ambiente di Cloud Service, consulta la documentazione su come [distribuire una base di codice come il sito WKND Reference in un ambiente di Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3. Scaricare ed eseguire l&#39;app WKND React {#sample-app}

Uno degli obiettivi di questa esercitazione è quello di mostrare come utilizzare AEM contenuto da un’applicazione esterna utilizzando le API GraphQL. Questa esercitazione utilizza un&#39;app React di esempio. L’app React è intenzionalmente semplice e si concentra sull’integrazione con AEM API GraphQL.

1. Apri un nuovo prompt dei comandi e duplica l’app React di esempio da GitHub:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Apri l’app React in `aem-guides-wknd-graphql/react-app` nell’IDE che preferisci.
1. Nell’IDE, apri il file . `.env.development` a `/.env.development`. Verifica la `REACT_APP_AUTHORIZATION` La riga non viene commentata e il file dichiara le seguenti variabili:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Assicurati `REACT_APP_HOST_URI` rimanda all’SDK AEM locale. Per comodità, questo avvio rapido collega l’app React a  **AEM Author**. **Autore** i servizi richiedono l’autenticazione, quindi l’app utilizza `admin` per stabilire la connessione. La connessione di un’app ad AEM Author è una pratica comune durante lo sviluppo, in quanto consente di eseguire rapidamente l’iterazione dei contenuti senza dover pubblicare le modifiche.

   >[!NOTE]
   >
   > In uno scenario di produzione, l’app si connetterà a un AEM **Pubblica** ambiente. Questo viene trattato più dettagliatamente nella _Distribuzione di produzione_ sezione .


1. Installa e avvia l&#39;app React:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Una nuova finestra del browser apre automaticamente l’app su [http://localhost:3000](Http://localhost:3000).

   ![Reagisci con l’app iniziale](assets/quick-setup/aem-sdk/react-app__home-view.png)

   Viene visualizzato un elenco dei contenuti dell’avventura da AEM.

1. Fai clic su una delle immagini dell&#39;avventura per visualizzare i dettagli dell&#39;avventura. Viene AEM una richiesta per restituire i dettagli di un&#39;avventura.

   ![Vista Dettagli avventura](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Utilizza gli strumenti di sviluppo del browser per ispezionare il **Rete** richieste. Visualizza la **XHR** richiede e osserva più richieste di GET a `/graphql/execute.json/...`. Questo prefisso del percorso richiama AEM endpoint di query persistente, selezionando la query persistente da eseguire utilizzando il nome e i parametri codificati che seguono il prefisso .

   ![Richiesta XHR di GraphQL Endpoint](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. Modificare il contenuto in AEM

Quando l’app React è in esecuzione, aggiorna il contenuto di AEM e osserva che la modifica si riflette nell’app.

1. Passa a AEM [http://localhost:4502](Http://localhost:4502).
1. Passa a **Risorse** > **File** > **WKND condiviso** > **Inglese** > **Avventure** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Cartella Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Fai clic su **Bali Surf Camp** frammento di contenuto per aprire l’editor frammento di contenuto.
1. Modifica la **Titolo** e **Descrizione** dell&#39;avventura.

   ![Modificare un frammento di contenuto](assets/setup/modify-content-fragment-bali.png)

1. Fai clic su **Salva** per salvare le modifiche.
1. Aggiorna l’app React all’indirizzo [http://localhost:3000](Http://localhost:3000) per visualizzare le modifiche:

   ![Bali Surf Camp Avventura aggiornata](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. Esplora GraphiQL {#graphiql}

1. Apri [GraphiQL](http://localhost:4502/aem/graphiql.html) passando a **Strumenti** > **Generale** > **Editor query GraphQL**
1. Seleziona le query persistenti esistenti a sinistra ed eseguine per visualizzare i risultati.

   >[!NOTE]
   >
   > Lo strumento GraphiQL e l&#39;API GraphQL è [più avanti nell’esercitazione è stato esplorato più dettagliatamente](../multi-step/explore-graphql-api.md).

## Congratulazioni!{#congratulations}

Congratulazioni, ora disponi di un’applicazione esterna che consuma AEM contenuti con GraphQL. Puoi controllare il codice nell’app React e continuare a provare a modificare i frammenti di contenuto esistenti.

### Passaggi successivi

* [Avvia l&#39;esercitazione AEM Headless](../multi-step/overview.md)
