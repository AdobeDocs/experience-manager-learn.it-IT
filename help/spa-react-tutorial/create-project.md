---
title: Progetto per l’editor di applicazioni a pagina singola | Guida introduttiva dell’Editor SPA di AEM e React
description: Scopri come utilizzare un progetto Maven di Adobe Experience Manager (AEM) come punto di partenza per un’applicazione React integrata con l’Editor SPA di AEM.
sub-product: sites
feature: Editor SPA, AEM Project Archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 3%

---


# Progetto editor SPA {#spa-editor-project}

Scopri come utilizzare un progetto Maven di Adobe Experience Manager (AEM) come punto di partenza per un’applicazione React integrata con l’Editor SPA di AEM.

## Obiettivo

1. Comprendi la struttura di un nuovo progetto AEM SPA Editor generato da un archetipo Maven.
2. Distribuisci il progetto iniziale in un’istanza locale di AEM.

## Cosa verrà creato

In questo capitolo, verrà implementato un nuovo progetto AEM, basato su [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Il progetto AEM verrà avviato con un punto di partenza molto semplice per React SPA. Il progetto utilizzato in questo capitolo servirà da base per l’attuazione della SPA WKND e sarà basato sui prossimi capitoli.

![Progetto di avvio per reazioni SPA WKND](./assets/create-project/wknd-spa-react.png)

*Avvio della gerarchia del sito per l’applicazione a pagina singola WKND.*

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment). Assicurati che una nuova istanza di Adobe Experience Manager, avviata in modalità **author**, sia in esecuzione localmente.

## Ottieni il progetto

Esistono diverse opzioni per creare un progetto Maven Multi-Module per AEM. Questa esercitazione ha utilizzato l&#39;ultimo [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) come base per il codice dell&#39;esercitazione. Sono state apportate modifiche al codice del progetto per supportare più versioni di AEM. Rivedi [la nota sulla compatibilità con le versioni precedenti](overview.md#compatibility).

>[!CAUTION]
>
> È consigliabile utilizzare la versione **più recente** dell’ [archetipo](https://github.com/adobe/aem-project-archetype) per generare un nuovo progetto per un’implementazione nel mondo reale. I progetti AEM devono essere destinati a una singola versione di AEM utilizzando la proprietà `aemVersion` dell’archetipo.

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/create-project-start
   ```

2. La seguente struttura di file e cartelle rappresenta il progetto AEM generato dall’archetipo Maven sul file system locale:

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. Le seguenti proprietà sono state utilizzate durante la generazione del progetto AEM da [AEM Project Archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | Proprietà | Valore |
   |-----------------|-------------------------------------|
   | aemVersion | nuvole |
   | appTitle | Reazione SPA WKND |
   | appId | wknd-spa-react |
   | groupId | com.adobe.aem.guides |
   | frontendModule | react |
   | pacchetto | com.adobe.aem.guides.wknd.spa.react |
   | includeExample | n |

   >[!NOTE]
   >
   > Osserva la proprietà `frontendModule=react` . Questo comunica ad AEM Project Archetype di avviare il progetto con un avviatore [React code base](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) da utilizzare con AEM SPA Editor.

## Crea il progetto

Successivamente, compila, genera e distribuisci il codice del progetto in un’istanza locale di AEM utilizzando Maven.

1. Assicurati che un&#39;istanza di AEM sia in esecuzione localmente sulla porta **4502**.
2. Dal terminale della riga di comando verificare che Maven sia installato:

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Esegui il seguente comando Maven dalla directory `aem-guides-wknd-spa` per generare e distribuire il progetto in AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   I diversi moduli del progetto devono essere compilati e distribuiti in AEM.

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-react 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-react ..................................... SUCCESS [  0.523 s]
    [INFO] WKND SPA React - Core .............................. SUCCESS [  8.069 s]
    [INFO] wknd-spa-react.ui.frontend - UI Frontend ........... SUCCESS [01:23 min]
    [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  0.830 s]
    [INFO] WKND SPA React - UI apps ........................... SUCCESS [  4.654 s]
    [INFO] WKND SPA React - UI content ........................ SUCCESS [  1.607 s]
    [INFO] WKND SPA React - All ............................... SUCCESS [  0.384 s]
    [INFO] WKND SPA React - Integration Tests Bundles ......... SUCCESS [  0.770 s]
    [INFO] WKND SPA React - Integration Tests Launcher ........ SUCCESS [  1.407 s]
    [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.055 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  01:44 min
   ```

   Il profilo Maven ***autoInstallSinglePackage*** compila i singoli moduli del progetto e distribuisce un singolo pacchetto all&#39;istanza AEM. Per impostazione predefinita questo pacchetto verrà distribuito a un&#39;istanza AEM in esecuzione localmente sulla porta **4502** e con le credenziali di **admin:admin**.

4. Passa a **[!UICONTROL Gestione pacchetti]** nell&#39;istanza AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Dovresti visualizzare tre pacchetti per `wknd-spa-react.all`, `wknd-spa-react.ui.apps` e `wknd-spa-react.ui.content`.

   ![Pacchetti SPA WKND](./assets/create-project/package-manager.png)

   *Gestione pacchetti AEM*

   Tutto il codice personalizzato necessario per il progetto verrà incluso in questi pacchetti e installato nel runtime AEM.

6. Dovresti inoltre visualizzare diversi pacchetti per `spa.project.core` e `core.wcm.components`. Queste dipendenze vengono incluse automaticamente dall&#39;archetipo. Ulteriori informazioni sui [componenti core di AEM sono disponibili qui](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/introduction.html).

   `spa.project.core` è una dipendenza necessaria per generare l’API del modello JSON che l’Editor SPA si aspetta.

## Contenuto autore

Quindi, apri l’applicazione a pagina singola iniziale generata dall’archetipo e aggiorna parte del contenuto.

1. Passa alla console **[!UICONTROL Sites]** : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   L’applicazione a pagina singola WKND include una struttura di base del sito con paese, lingua e home page. Questa gerarchia si basa sui valori predefiniti dell&#39;archetipo per `language_country` e `isSingleCountryWebsite`. Questi valori possono essere sovrascritti aggiornando le proprietà [disponibili](https://github.com/adobe/aem-project-archetype#available-properties) durante la generazione di un progetto.

2. Apri la pagina **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA React Home Page]** selezionando la pagina e facendo clic sul pulsante **[!UICONTROL Modifica]** nella barra dei menu:

   ![console del sito](./assets/create-project/open-home-page.png)

3. Alla pagina è già stato aggiunto un componente **[!UICONTROL Testo]** . Puoi modificare questo componente come qualsiasi altro componente in AEM.

   ![Aggiorna componente testo](./assets/create-project/update-text-component.gif)

4. Aggiungi alla pagina un componente aggiuntivo **[!UICONTROL Testo]** .

   L’esperienza di authoring è simile a quella di una pagina AEM Sites tradizionale. Attualmente è disponibile un numero limitato di componenti da utilizzare. Nel corso dell’esercitazione verranno aggiunti ulteriori elementi.

## Ispezionare l’applicazione a pagina singola

Successivamente, verifica che si tratti di un&#39;applicazione a pagina singola con l&#39;uso degli strumenti di sviluppo del browser.

1. In **[!UICONTROL Editor pagina]**, fai clic sul pulsante **[!UICONTROL Informazioni pagina]** > **[!UICONTROL Visualizza come pubblicato]**:

   ![Pulsante Visualizza come pubblicato](./assets/create-project/view-as-published.png)

   Viene aperta una nuova scheda con il parametro di query `?wcmmode=disabled` che disattiva efficacemente l’editor AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Visualizzare l’origine della pagina e notare che il contenuto di testo **[!DNL Hello World]** o qualsiasi altro contenuto non è trovato. Dovresti invece visualizzare l’HTML come segue:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` è l’applicazione a pagina singola React che viene caricata sulla pagina e responsabile del rendering del contenuto.

   Tuttavia, *da dove viene il contenuto?*

3. Torna alla scheda : [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Apri gli strumenti di sviluppo del browser ed esamina il traffico di rete della pagina durante un aggiornamento. Visualizza le richieste **XHR**:

   ![Richieste XHR](./assets/create-project/xhr-requests.png)

   È necessaria una richiesta a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Contiene tutto il contenuto, formattato in JSON, che guiderà l’applicazione a pagina singola.

5. In una nuova scheda apri [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   La richiesta `en.model.json` rappresenta il modello di contenuto che guiderà l&#39;applicazione. Controlla l&#39;output JSON e dovresti essere in grado di trovare lo snippet che rappresenta i componenti **[!UICONTROL Testo]** .

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

   Nel capitolo successivo esamineremo come questo contenuto JSON viene mappato dai componenti AEM ai componenti SPA per formare la base dell’esperienza dell’Editor SPA di AEM.

   >[!NOTE]
   >
   > Potrebbe essere utile installare un’estensione del browser per formattare automaticamente l’output JSON.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo progetto AEM SPA Editor!

La SPA è abbastanza semplice. Nei prossimi capitoli saranno aggiunte ulteriori funzionalità.

### Passaggi successivi {#next-steps}

[Integrare l’applicazione a pagina singola](integrate-spa.md) : scopri come il codice sorgente dell’applicazione a pagina singola è integrato con il progetto AEM e scopri gli strumenti disponibili per sviluppare rapidamente l’applicazione a pagina singola.
