---
title: Progetto editor SPA | Guida introduttiva all’editor di SPA AEM e all’Angular
description: Scopri come utilizzare un progetto Maven di Adobe Experience Manager (AEM) come punto di partenza per un’applicazione di Angular integrata con l’editor di SPA di AEM.
sub-product: sites
feature: Editor SPA, AEM Archetipo di progetto
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1111'
ht-degree: 3%

---


# Progetto editor SPA {#create-project}

Scopri come utilizzare un progetto Maven di Adobe Experience Manager (AEM) come punto di partenza per un’applicazione di Angular integrata con l’editor di SPA di AEM.

## Obiettivo

1. Comprendi la struttura di un nuovo progetto AEM editor SPA generato da un archetipo Maven.
2. Distribuisci il progetto iniziale in un’istanza locale di AEM.

## Cosa verrà creato

In questo capitolo, verrà implementato un nuovo progetto AEM, basato su [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Il progetto AEM verrà avviato con un punto di partenza molto semplice per il SPA Angular. Il progetto utilizzato in questo capitolo servirà da base per l&#39;attuazione del SPA WKND e sarà basato sui prossimi capitoli.

![Progetto iniziale WKND SPA Angular](./assets/create-project/what-you-will-build.png)

*Un classico messaggio Hello World.*

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment). Assicurati che una nuova istanza di Adobe Experience Manager, avviata in modalità **author**, sia in esecuzione localmente.

## Ottieni il progetto

Sono disponibili diverse opzioni per creare un progetto Maven Multi-Module per AEM. Questa esercitazione ha utilizzato l&#39;ultimo [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) come base per il codice dell&#39;esercitazione. Sono state apportate modifiche al codice del progetto per supportare più versioni di AEM. Rivedi [la nota sulla compatibilità con le versioni precedenti](overview.md#compatibility).

>[!CAUTION]
>
>È consigliabile utilizzare la versione **più recente** dell’ [archetipo](https://github.com/adobe/aem-project-archetype) per generare un nuovo progetto per un’implementazione nel mondo reale. I progetti AEM devono essere destinati a una singola versione di AEM utilizzando la proprietà `aemVersion` dell’archetipo.

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. La seguente struttura di file e cartelle rappresenta il Progetto AEM generato dall&#39;archetipo Maven sul file system locale:

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

3. Le seguenti proprietà sono state utilizzate durante la generazione del progetto AEM da [AEM archetipo di progetto](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | Proprietà | Valore |
   |-----------------|---------------------------------------|
   | aemVersion | nuvole |
   | appTitle | Angular SPA WKND |
   | appId | angular wknd-spa |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | pacchetto | com.adobe.aem.guides.wknd.spa.angular |
   | includeExample | n |

   >[!NOTE]
   >
   > Osserva la proprietà `frontendModule=angular` . Questo comunica a AEM Project Archetype di avviare il progetto con un codice di Angular [base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) da utilizzare con AEM SPA Editor.

## Crea il progetto

Quindi, compila, genera e distribuisci il codice del progetto in un’istanza locale di AEM utilizzando Maven.

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
   $ mvn -PautoInstallSinglePackage clean install
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   I moduli multipli del progetto devono essere compilati e distribuiti per AEM.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Il profilo Maven ***autoInstallSinglePackage*** compila i singoli moduli del progetto e distribuisce un singolo pacchetto all&#39;istanza AEM. Per impostazione predefinita questo pacchetto verrà distribuito in un&#39;istanza AEM in esecuzione localmente sulla porta **4502** e con le credenziali di **admin:admin**.

4. Passa a **[!UICONTROL Gestione pacchetti]** nell&#39;istanza AEM locale: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Dovresti visualizzare tre pacchetti per `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` e `wknd-spa-angular.ui.content`.

   ![Pacchetti SPA WKND](./assets/create-project/package-manager.png)

   Tutto il codice personalizzato necessario per il progetto verrà raggruppato in questi pacchetti e installato nel runtime AEM.

6. Dovresti inoltre visualizzare diversi pacchetti per `spa.project.core` e `core.wcm.components`. Si tratta di dipendenze incluse automaticamente dall&#39;archetipo. Ulteriori informazioni sui [AEM componenti core sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it).

## Contenuto autore

Quindi, apri il SPA iniziale generato dall’archetipo e aggiorna parte del contenuto.

1. Passa alla console **[!UICONTROL Sites]** : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   Il SPA WKND include una struttura di base del sito con un paese, una lingua e una home page. Questa gerarchia si basa sui valori predefiniti dell&#39;archetipo per `language_country` e `isSingleCountryWebsite`. Questi valori possono essere sovrascritti aggiornando le proprietà [disponibili](https://github.com/adobe/aem-project-archetype#available-properties) durante la generazione di un progetto.

2. Apri la pagina **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** selezionando la pagina e facendo clic sul pulsante **[!UICONTROL Modifica]** nella barra dei menu:

   ![console del sito](./assets/create-project/open-home-page.png)

3. Alla pagina è già stato aggiunto un componente **[!UICONTROL Testo]** . Puoi modificare questo componente come qualsiasi altro componente in AEM.

   ![Aggiorna componente testo](./assets/create-project/update-text-component.gif)

4. Aggiungi alla pagina un componente aggiuntivo **[!UICONTROL Testo]** .

   L’esperienza di authoring è simile a quella di una pagina AEM Sites tradizionale. Attualmente è disponibile un numero limitato di componenti da utilizzare. Nel corso dell’esercitazione verranno aggiunti ulteriori elementi.

## Applicazione a pagina singola Inspect

Successivamente, verifica che si tratti di un&#39;applicazione a pagina singola con l&#39;uso degli strumenti di sviluppo del browser.

1. In **[!UICONTROL Editor pagina]**, fai clic sul menu **[!UICONTROL Informazioni pagina]** > **[!UICONTROL Visualizza come pubblicato]**:

   ![Pulsante Visualizza come pubblicato](./assets/create-project/view-as-published.png)

   Viene aperta una nuova scheda con il parametro di query `?wcmmode=disabled` che disattiva efficacemente l’editor AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. Visualizzare l’origine della pagina e notare che il contenuto di testo **[!DNL Hello World]** o qualsiasi altro contenuto non è trovato. Dovresti invece visualizzare l’HTML come segue:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` è l’Angular SPA caricato sulla pagina e responsabile del rendering del contenuto.

   *Da dove viene il contenuto?*

3. Torna alla scheda : [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. Apri gli strumenti di sviluppo del browser ed esamina il traffico di rete della pagina durante un aggiornamento. Visualizza le richieste **XHR**:

   ![Richieste XHR](./assets/create-project/xhr-requests.png)

   È necessaria una richiesta a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Contiene tutto il contenuto, formattato in JSON, che guiderà il SPA.

5. In una nuova scheda, apri [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   La richiesta `en.model.json` rappresenta il modello di contenuto che guiderà l&#39;applicazione. Inspect l&#39;output JSON e dovresti essere in grado di trovare lo snippet che rappresenta i componenti **[!UICONTROL Testo]** .

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   Nel capitolo successivo esamineremo come il contenuto JSON viene mappato da Componenti AEM a Componenti SPA per formare la base dell’esperienza di Editor SPA AEM.

   >[!NOTE]
   >
   > Potrebbe essere utile installare un’estensione del browser per formattare automaticamente l’output JSON.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo AEM SPA Editor Project!

È abbastanza semplice in questo momento, ma nei prossimi capitoli saranno aggiunte ulteriori funzionalità.

### Passaggi successivi {#next-steps}

[Integrare il SPA](integrate-spa.md)  - Scopri come il codice sorgente SPA è integrato con il progetto AEM e come è possibile sviluppare rapidamente il SPA.
