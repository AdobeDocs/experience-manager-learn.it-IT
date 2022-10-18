---
title: Crea progetto | Guida introduttiva all'editor di SPA AEM e React
description: Scopri come generare un progetto Maven di Adobe Experience Manager (AEM) come punto di partenza per un’applicazione React integrata con l’editor di SPA di AEM.
sub-product: sites
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 3%

---

# Crea progetto {#spa-editor-project}

Scopri come generare un progetto Maven di Adobe Experience Manager (AEM) come punto di partenza per un’applicazione React integrata con l’editor di SPA di AEM.

## Obiettivo

1. Genera un progetto abilitato SPA Editor utilizzando AEM Project Archetype.
2. Distribuisci il progetto iniziale in un’istanza locale di AEM.

## Cosa verrà creato {#what-build}

In questo capitolo, viene generato un nuovo progetto AEM, basato su [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype). Il progetto AEM viene avviato con un punto di partenza molto semplice per il SPA React.

**Cos&#39;è un progetto Maven?** - [Apache Maven](https://maven.apache.org/) è uno strumento di gestione software per la creazione di progetti. *Tutte le Adobe Experience Manager* le implementazioni utilizzano i progetti Maven per generare, gestire e distribuire codice personalizzato in cima alle AEM.

**Cos&#39;è un archetipo Maven?** - A [Archetipo Maven](https://maven.apache.org/archetype/index.html) è un modello o un pattern per la generazione di nuovi progetti. L’archetipo AEM progetto ci consente di generare un nuovo progetto con uno spazio dei nomi personalizzato e di includere una struttura di progetto che segue le best practice, accelerando notevolmente il nostro progetto.

## Prerequisiti

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment). Assicurati che una nuova istanza di Adobe Experience Manager sia stata avviata in **autore** è in esecuzione localmente.

## Crea il progetto {#create}

>[!NOTE]
>
>Questa esercitazione utilizza la versione **39** dell&#39;archetipo. È sempre consigliabile utilizzare le **più recente** versione dell’archetipo per generare un nuovo progetto.

1. Apri un terminale della riga di comando e immetti il seguente comando Maven:

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=39 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Se il targeting AEM 6.5.5+ sostituisci `aemVersion="cloud"` con `aemVersion="6.5.5"`. Se il targeting è 6.4.8+, utilizza `aemVersion="6.4.8"`.

   Osserva che `frontendModule=react` proprietà. Questo comunica a AEM Project Archetype di avviare il progetto con un avviatore [Base di codice React](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) da utilizzare con l’editor SPA AEM. Proprietà simili `appTitle`, `appId`, `artifactId`e `groupId` sono utilizzati per identificare il progetto e lo scopo.

   Elenco completo delle proprietà disponibili per la configurazione di un progetto [si trova qui](https://github.com/adobe/aem-project-archetype#available-properties).

1. La seguente struttura di file e cartelle viene generata dall&#39;archetipo Maven sul file system locale:

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

   Ogni cartella rappresenta un singolo modulo Maven. In questa esercitazione lavoreremo principalmente con `ui.frontend` , che è l’app React. Ulteriori dettagli sui singoli moduli sono disponibili nella sezione [Documentazione AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## Distribuzione e creazione del progetto

Quindi, compila, genera e distribuisci il codice del progetto in un’istanza locale di AEM utilizzando Maven.

1. Assicurati che un&#39;istanza di AEM sia in esecuzione localmente sulla porta **4502**.
1. Dalla riga di comando passa alla `aem-guides-wknd-spa.react` directory di progetto.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Esegui il comando seguente per generare e distribuire l’intero progetto in AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La build richiede circa un minuto e termina con il seguente messaggio:

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

   Il profilo Maven `autoInstallSinglePackage` compila i singoli moduli del progetto e distribuisce un singolo pacchetto all’istanza AEM. Per impostazione predefinita questo pacchetto viene distribuito a un&#39;istanza AEM in esecuzione localmente sulla porta **4502** e con le credenziali di `admin:admin`.

1. Passa a **Gestione pacchetti** sull&#39;istanza AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Dovresti visualizzare più pacchetti con prefisso `aem-guides-wknd-spa.react`.

   ![Pacchetti SPA WKND](assets/create-project/package-manager.png)

   *Gestione pacchetti AEM*

   Tutto il codice personalizzato necessario per il progetto viene raggruppato in questi pacchetti e installato nell’ambiente AEM.

## Contenuto autore

Quindi, apri il SPA iniziale generato dall’archetipo e aggiorna parte del contenuto.

1. Passa a **Sites** console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Il SPA WKND include una struttura di base del sito con un paese, una lingua e una home page. Questa gerarchia è basata sui valori predefiniti dell&#39;archetipo per `language_country` e `isSingleCountryWebsite`. Questi valori possono essere sovrascritti aggiornando il [proprietà disponibili](https://github.com/adobe/aem-project-archetype#available-properties) durante la generazione di un progetto.

2. Apri **noi** > **en** > **Home page React SPA WKND** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![console del sito](./assets/create-project/open-home-page.png)

3. A **Testo** è già stato aggiunto alla pagina. Puoi modificare questo componente come qualsiasi altro componente in AEM.

   ![Aggiorna componente testo](./assets/create-project/update-text-component.gif)

4. Aggiungi un **Testo** alla pagina.

   L’esperienza di authoring è simile a quella di una pagina AEM Sites tradizionale. Attualmente è disponibile un numero limitato di componenti da utilizzare. Nel corso dell’esercitazione viene aggiunto altro.

## Applicazione a pagina singola Inspect

Successivamente, verifica che si tratti di un&#39;applicazione a pagina singola con l&#39;uso degli strumenti di sviluppo del browser.

1. In **Editor pagina**, fai clic su **Informazioni pagina** pulsante > **Visualizza come pubblicato**:

   ![Pulsante Visualizza come pubblicato](./assets/create-project/view-as-published.png)

   Verrà aperta una nuova scheda con il parametro di query `?wcmmode=disabled` che disattiva efficacemente l&#39;editor AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Visualizzare l’origine della pagina e notare che il contenuto del testo **[!DNL Hello World]** o uno degli altri contenuti non trovato. Dovresti invece visualizzare HTML come segue:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` è la SPA React caricata sulla pagina e responsabile del rendering del contenuto.

   Tuttavia, *da dove viene il contenuto?*

3. Torna alla scheda : [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Apri gli strumenti di sviluppo del browser ed esamina il traffico di rete della pagina durante un aggiornamento. Visualizza la **XHR** richieste:

   ![Richieste XHR](./assets/create-project/xhr-requests.png)

   Dovrebbe essere richiesta [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Contiene tutto il contenuto, formattato in JSON, che guiderà il SPA.

5. In una nuova scheda aperta [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   Richiesta `en.model.json` rappresenta il modello di contenuto che guiderà l&#39;applicazione. Inspect l’output JSON e dovresti essere in grado di trovare lo snippet che rappresenta il **[!UICONTROL Testo]** componenti.

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

   Nel capitolo successivo esamineremo come questo contenuto JSON viene mappato da componenti AEM a componenti SPA per formare la base dell’esperienza dell’editor SPA AEM.

   >[!NOTE]
   >
   > Potrebbe essere utile installare un’estensione del browser per formattare automaticamente l’output JSON.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo AEM SPA Editor Project!

Il SPA è piuttosto semplice. Nei capitoli successivi vengono aggiunte ulteriori funzionalità.

### Passaggi successivi {#next-steps}

[Integrare un SPA](integrate-spa.md) - Scopri come il codice sorgente SPA è integrato con il progetto AEM e comprendere gli strumenti disponibili per sviluppare rapidamente il SPA.
