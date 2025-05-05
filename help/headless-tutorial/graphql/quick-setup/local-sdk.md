---
title: Configurazione rapida AEM Headless tramite AEM SDK locale
description: Introduzione a Adobe Experience Manager (AEM) e GraphQL. Installa AEM SDK, aggiungi contenuti di esempio e distribuisci un’applicazione che utilizzi contenuti da AEM utilizzando le relative API GraphQL. Scopri come AEM potenzia le esperienze omni-channel.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
duration: 242
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 1%

---

# Configurazione rapida AEM Headless tramite AEM SDK locale {#setup}

La configurazione rapida AEM Headless ti consente di utilizzare AEM Headless utilizzando il contenuto del progetto di esempio del sito WKND e un esempio di app React (a SPA) che consuma il contenuto rispetto alle API AEM Headless GraphQL. Questa guida utilizza [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=it).

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v18](https://nodejs.org/it/)
* [Git](https://git-scm.com/)

## 1. Installare AEM SDK {#aem-sdk}

Questa installazione utilizza [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=it&#aem-as-a-cloud-service-sdk) per esplorare le API GraphQL di AEM. Questa sezione fornisce una guida rapida all’installazione di AEM SDK e alla sua esecuzione in modalità Creazione. Una guida più dettagliata per la configurazione di un ambiente di sviluppo locale [è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it#local-development-environment-set-up).

>[!NOTE]
>
> È inoltre possibile seguire l&#39;esercitazione con un [ambiente AEM as a Cloud Service](./cloud-service.md). Nell’esercitazione sono incluse note aggiuntive sull’utilizzo di un ambiente Cloud.

1. Passa al **[portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** e scarica la versione più recente di **AEM SDK**.

   ![Portale di distribuzione software](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Decomprimere il download e copiare il file jar Quickstart (`aem-sdk-quickstart-XXX.jar`) in una cartella dedicata, ovvero `~/aem-sdk/author`.
1. Rinomina il file jar in `aem-author-p4502.jar`.

   Il nome `author` specifica che il file jar Quickstart viene avviato in modalità Creazione. `p4502` specifica che Quickstart viene eseguito sulla porta 4502.

1. Per installare e avviare l’istanza di AEM, apri un prompt dei comandi nella cartella che contiene il file jar ed esegui il comando seguente:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Specificare una password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare `admin` per lo sviluppo locale per ridurre la necessità di riconfigurare.
1. Al termine dell&#39;installazione del servizio AEM, dovrebbe essere aperta una nuova finestra del browser all&#39;indirizzo [http://localhost:4502](http://localhost:4502).
1. Accedi con il nome utente `admin` e la password selezionati durante l&#39;avvio iniziale di AEM (in genere `admin`).

## 2. Installare il contenuto di esempio {#install-sample-content}

Il contenuto di esempio del **sito di riferimento WKND** viene utilizzato per accelerare l&#39;esercitazione. Il WKND è un brand fittizio in stile vita, spesso utilizzato con la formazione di AEM.

Il sito WKND include le configurazioni necessarie per esporre un [endpoint GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html?lang=it). In un&#39;implementazione reale, segui i passaggi documentati per [includere gli endpoint di GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html?lang=it) nel progetto del cliente. Anche un [CORS](#cors-config) è stato incluso nel pacchetto come parte del sito WKND. Per concedere l&#39;accesso a un&#39;applicazione esterna è necessaria una configurazione CORS. Ulteriori informazioni su [CORS](#cors-config) sono disponibili di seguito.

1. Scarica il pacchetto AEM più recente compilato per il sito WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Assicurati di scaricare la versione standard compatibile con AEM as a Cloud Service e **non** la versione `classic`.

1. Dal menu **AEM Start**, passa a **Strumenti** > **Distribuzione** > **Pacchetti**.

   ![Passa a pacchetti](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Fare clic su **Carica pacchetto** e scegliere il pacchetto WKND scaricato nel passaggio precedente. Per installare il pacchetto, fai clic su **Installa**.

1. Dal menu **AEM Start**, passa a **Assets** > **File** > **WKND Shared** > **Inglese** > **Avventure**.

   ![Visualizzazione cartelle delle avventure](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Questa è una cartella di tutte le risorse che comprendono le varie avventure promosse dal brand WKND. Sono inclusi tipi di file multimediali tradizionali come immagini e video e file multimediali specifici di AEM come **Frammenti di contenuto**.

1. Fai clic nella cartella **Sci in discesa Wyoming** e fai clic sulla scheda **Frammento di contenuto di Sci in discesa Wyoming**:

   ![Scheda Frammento di contenuto](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. Si apre l’editor frammento di contenuto per l’avventura di Downhill Skiing Wyoming.

   ![Editor frammento di contenuto](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Osserva che vari campi come **Titolo**, **Descrizione** e **Attività** definiscono il frammento.

   I **frammenti di contenuto** sono uno dei modi in cui il contenuto può essere gestito in AEM. I frammenti di contenuto sono contenuti riutilizzabili, indipendenti dalla presentazione e composti da elementi dati strutturati come testo, testo RTF, date o riferimenti ad altri frammenti di contenuto. I frammenti di contenuto vengono esaminati più dettagliatamente più avanti nella configurazione rapida.

1. Fai clic su **Annulla** per chiudere il frammento. Puoi spostarti tra le altre cartelle ed esplorare gli altri contenuti di Adventure.

>[!NOTE]
>
> Se utilizzi un ambiente Cloud Service, consulta la documentazione per sapere come [distribuire una base di codice come il sito di riferimento WKND in un ambiente Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=it#coding-against-the-right-aem-version).

## 3. Scaricare ed eseguire l’app WKND React {#sample-app}

Uno degli obiettivi di questo tutorial è mostrare come utilizzare i contenuti AEM da un’applicazione esterna utilizzando le API GraphQL. Questa esercitazione utilizza un esempio di app React. L’app React è intenzionalmente semplice, per concentrarti sull’integrazione con le API GraphQL di AEM.

1. Apri un nuovo prompt dei comandi e clona l’app React di esempio da GitHub:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Aprire l&#39;app React in `aem-guides-wknd-graphql/react-app` nell&#39;IDE desiderato.
1. Nell&#39;IDE, aprire il file `.env.development` in `/.env.development`. Verificare che la riga `REACT_APP_AUTHORIZATION` non contenga commenti e che il file dichiari le seguenti variabili:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Assicurati che `REACT_APP_HOST_URI` punti al tuo SDK AEM locale. Per comodità, questo avvio rapido collega l&#39;app React a **AEM Author**. I servizi **Author** richiedono l&#39;autenticazione, pertanto l&#39;app utilizza l&#39;utente `admin` per stabilire la connessione. La connessione di un’app ad AEM Author è una pratica comune durante lo sviluppo, in quanto facilita la rapida iterazione del contenuto senza la necessità di pubblicare modifiche.

   >[!NOTE]
   >
   > In uno scenario di produzione, l&#39;app si connetterà a un ambiente AEM **Publish**. Questo argomento è trattato più dettagliatamente nella sezione _Distribuzione di produzione_.


1. Installa e avvia l’app React:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Una nuova finestra del browser apre automaticamente l&#39;app in [http://localhost:3000](http://localhost:3000).

   ![React app iniziale](assets/quick-setup/aem-sdk/react-app__home-view.png)

   Viene visualizzato un elenco dei contenuti dell’avventura da AEM.

1. Fai clic su una delle immagini dell’avventura per visualizzarne i dettagli. Viene effettuata una richiesta ad AEM per restituire i dettagli di un’avventura.

   ![Visualizzazione dettagli avventura](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Utilizza gli strumenti di sviluppo del browser per esaminare le richieste **Network**. Visualizza le richieste **XHR** e osserva più richieste GET a `/graphql/execute.json/...`. Questo prefisso di percorso richiama l’endpoint di query persistente di AEM, selezionando la query persistente da eseguire utilizzando il nome e i parametri codificati che seguono il prefisso.

   ![Richiesta XHR endpoint GraphQL](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. Modificare i contenuti in AEM

Quando l’app React è in esecuzione, aggiorna il contenuto in AEM e osserva che la modifica si riflette nell’app.

1. Passa a AEM [http://localhost:4502](http://localhost:4502).
1. Passa a **Assets** > **File** > **WKND Shared** > **Inglese** > **Avventure** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Cartella campo di Bali](assets/setup/bali-surf-camp-folder.png)

1. Fai clic sul frammento di contenuto **Bali Surf Camp** per aprire l&#39;editor frammenti di contenuto.
1. Modifica il **Titolo** e la **Descrizione** dell&#39;avventura.

   ![Modifica frammento di contenuto](assets/setup/modify-content-fragment-bali.png)

1. Fai clic su **Salva** per salvare le modifiche.
1. Aggiorna l&#39;app React all&#39;indirizzo [http://localhost:3000](http://localhost:3000) per visualizzare le modifiche:

   ![Avventura campo surf di Bali aggiornata](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. Esplorare GraphiQL {#graphiql}

1. Apri [GraphiQL](http://localhost:4502/aem/graphiql.html) passando a **Strumenti** > **Generale** > **Editor query GraphQL**
1. Seleziona le query persistenti esistenti a sinistra ed eseguili per visualizzare i risultati.

   >[!NOTE]
   >
   > Lo strumento GraphiQL e l&#39;API GraphQL sono [descritti più avanti nell&#39;esercitazione](../multi-step/explore-graphql-api.md).

## Congratulazioni.{#congratulations}

Congratulazioni, ora disponi di un’applicazione esterna che utilizza contenuti AEM con GraphQL. Puoi controllare il codice nell’app React e continuare a sperimentare la modifica dei frammenti di contenuto esistenti.

### Passaggi successivi

* [Avvia l’esercitazione di AEM Headless](../multi-step/overview.md)
