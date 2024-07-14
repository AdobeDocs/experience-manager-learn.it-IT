---
title: Crea progetto | Guida introduttiva all’Editor SPA dell’AEM e React
description: Scopri come generare un progetto Adobe Experience Manager (AEM) Maven come punto di partenza per un’applicazione React integrata con l’Editor AEM SPA.
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
jira: KT-413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
duration: 250
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 1%

---

# Crea progetto {#spa-editor-project}

Scopri come generare un progetto Adobe Experience Manager (AEM) Maven come punto di partenza per un’applicazione React integrata con l’Editor AEM SPA.

## Obiettivo

1. Genera un progetto abilitato per l’editor SPA utilizzando l’archetipo di progetto AEM.
2. Distribuisci il progetto iniziale in un’istanza locale dell’AEM.

## Cosa verrà creato {#what-build}

In questo capitolo viene generato un nuovo progetto AEM basato sull&#39;[archetipo progetto AEM](https://github.com/adobe/aem-project-archetype). Il progetto AEM viene avviato con un punto di partenza molto semplice per l&#39;SPA React.

**Cos’è un progetto Maven?** - [Apache Maven](https://maven.apache.org/) è uno strumento di gestione software per la creazione di progetti. *Tutte le implementazioni di Adobe Experience Manager* utilizzano i progetti Maven per generare, gestire e distribuire codice personalizzato in aggiunta all&#39;AEM.

**Che cos&#39;è un archetipo Maven?** - Un [archetipo Maven](https://maven.apache.org/archetype/index.html) è un modello o un modello per la generazione di nuovi progetti. L’archetipo del progetto AEM ci consente di generare un nuovo progetto con uno spazio dei nomi personalizzato e di includere una struttura di progetto che segue le best practice, accelerando notevolmente il nostro progetto.

## Prerequisiti

Esaminare gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment). Verificare che una nuova istanza di Adobe Experience Manager, avviata in modalità **author**, sia in esecuzione localmente.

## Creare il progetto {#create}

>[!NOTE]
>
>Questa esercitazione utilizza la versione **35** dell&#39;archetipo.

1. Apri un terminale della riga di comando e immetti il seguente comando Maven:

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=35 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Se si esegue il targeting di AEM 6.5.5+, sostituire `aemVersion="cloud"` con `aemVersion="6.5.5"`. Se si esegue il targeting della versione 6.4.8 e successive, utilizzare `aemVersion="6.4.8"`.

   Osserva la proprietà `frontendModule=react`. Questo comunica all&#39;Archetipo progetto AEM di avviare il progetto con una base di codice iniziale [React](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) da utilizzare con l&#39;Editor SPA dell&#39;AEM. Proprietà come `appTitle`, `appId`, `artifactId` e `groupId` vengono utilizzate per identificare il progetto e lo scopo.

   Un elenco completo delle proprietà disponibili per la configurazione di un progetto [ è disponibile qui](https://github.com/adobe/aem-project-archetype#available-properties).

1. La seguente struttura di file e cartelle è generata dall’archetipo Maven sul file system locale:

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   Ogni cartella rappresenta un singolo modulo Maven. In questo tutorial verrà utilizzato principalmente il modulo `ui.frontend`, che è l&#39;app React. Ulteriori dettagli sui singoli moduli sono disponibili nella [documentazione di Archetipo progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it).

## Distribuire e generare il progetto

Quindi, compila, genera e implementa il codice del progetto in un’istanza locale dell’AEM utilizzando Maven.

1. Verificare che un&#39;istanza dell&#39;AEM sia in esecuzione localmente sulla porta **4502**.
1. Dalla riga di comando passare alla directory del progetto `aem-guides-wknd-spa.react`.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Esegui il comando seguente per generare e distribuire l’intero progetto a AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La build richiederà circa un minuto e dovrebbe terminare con il seguente messaggio:

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Il profilo Maven `autoInstallSinglePackage` compila i singoli moduli del progetto e distribuisce un singolo pacchetto all&#39;istanza AEM. Per impostazione predefinita, questo pacchetto viene distribuito a un&#39;istanza AEM in esecuzione localmente sulla porta **4502** e con le credenziali di `admin:admin`.

1. Passa a **Gestione pacchetti** nell&#39;istanza AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Dovrebbero essere presenti più pacchetti con prefisso `aem-guides-wknd-spa.react`.

   ![Pacchetti SPA WKND](assets/create-project/package-manager.png)

   *Gestione pacchetti AEM*

   Tutto il codice personalizzato necessario per il progetto è incluso in questi pacchetti e installato nell’ambiente AEM.

## Contenuto autore

Quindi, apri l’SPA iniziale generato dall’archetipo e aggiorna alcuni contenuti.

1. Passare alla console **Sites**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   L’SPA del WKND include una struttura di base del sito con un paese, una lingua e una pagina Home. Questa gerarchia si basa sui valori predefiniti dell&#39;archetipo per `language_country` e `isSingleCountryWebsite`. Questi valori possono essere sovrascritti aggiornando le [proprietà disponibili](https://github.com/adobe/aem-project-archetype#available-properties) durante la generazione di un progetto.

2. Apri la pagina **us** > **en** > **Pagina iniziale WKND SPA React** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![console del sito](./assets/create-project/open-home-page.png)

3. Un componente **Testo** è già stato aggiunto alla pagina. Puoi modificare questo componente come qualsiasi altro componente nell’AEM.

   ![Aggiorna componente testo](./assets/create-project/update-text-component.gif)

4. Aggiungi un componente aggiuntivo **Testo** alla pagina.

   L’esperienza di authoring è simile a quella di una pagina AEM Sites tradizionale. Attualmente è disponibile un numero limitato di componenti da utilizzare. Nel corso dell’esercitazione verranno aggiunte ulteriori informazioni.

## Inspect: applicazione a pagina singola

Quindi, verifica che si tratti di un’applicazione a pagina singola con l’utilizzo degli strumenti di sviluppo del browser.

1. Nell&#39;**Editor pagine**, fai clic sul pulsante **Informazioni pagina** > **Visualizza come pubblicato**:

   ![Pulsante Visualizza come pubblicato](./assets/create-project/view-as-published.png)

   Verrà aperta una nuova scheda con il parametro di query `?wcmmode=disabled` che disattiva l&#39;editor AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Visualizzare l&#39;origine della pagina e notare che il contenuto di testo **[!DNL Hello World]** o qualsiasi altro contenuto non è stato trovato. Dovresti invece visualizzare HTML come segue:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` è l&#39;SPA React caricato sulla pagina e responsabile del rendering del contenuto.

   Tuttavia, *da dove proviene il contenuto?*

3. Torna alla scheda: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Apri gli strumenti di sviluppo del browser e controlla il traffico di rete della pagina durante un aggiornamento. Visualizza le richieste **XHR**:

   ![Richieste XHR](./assets/create-project/xhr-requests.png)

   È necessaria una richiesta a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Contiene tutti i contenuti, formattati in JSON, che determineranno l’SPA.

5. In una nuova scheda apri [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   La richiesta `en.model.json` rappresenta il modello di contenuto che guiderà l&#39;applicazione. Inspect ha aggiunto l&#39;output JSON e dovresti essere in grado di trovare lo snippet che rappresenta i **[!UICONTROL Componenti testo]**.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   Nel prossimo capitolo verrà esaminato come questo contenuto JSON viene mappato dai Componenti AEM ai Componenti SPA per formare la base dell’esperienza dell’Editor SPA dell’AEM.

   >[!NOTE]
   >
   > Potrebbe essere utile installare un’estensione del browser per formattare automaticamente l’output JSON.

## Congratulazioni. {#congratulations}

Congratulazioni, hai appena creato il tuo primo progetto di editor SPA per l’AEM!

L&#39;SPA è piuttosto semplice. Nei capitoli successivi vengono aggiunte ulteriori funzionalità.

### Passaggi successivi {#next-steps}

[Integrare un SPA](integrate-spa.md): scopri come il codice sorgente dell&#39;SPA è integrato con il progetto AEM e quali strumenti sono disponibili per sviluppare rapidamente l&#39;SPA.
