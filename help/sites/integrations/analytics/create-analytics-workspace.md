---
title: Analizzare i dati con  Analysis Workspace
description: Scopri come mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni  suite di rapporti Adobe Analytics. Scoprite come creare un dashboard di reporting dettagliato utilizzando la funzione Analysis Workspace  di  Adobe Analytics.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6409
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 55beee99b91c44f96cd37d161bb3b4ffe38d2687
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 0%

---


# Analizzare i dati con  Analysis Workspace

Scopri come mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni  suite di rapporti Adobe Analytics. Scoprite come creare un dashboard di reporting dettagliato utilizzando la funzione Analysis Workspace  di  Adobe Analytics.

## Cosa verrà creato

Il team di marketing WKND vuole capire quali pulsanti Invito all’azione (CTA, Call To Action) eseguono meglio sulla pagina principale. In questa esercitazione, creeremo un nuovo progetto nell&#39;Analysis Workspace  per visualizzare le prestazioni dei diversi pulsanti CTA e comprendere il comportamento degli utenti sul sito. Le informazioni seguenti vengono acquisite utilizzando  Adobe Analytics quando un utente fa clic sul pulsante Invito all’azione (CTA) nella home page del WKND.

**Variabili di Analytics**

Di seguito sono riportate le variabili di Analytics attualmente monitorate:

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

![CTA Click  Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Obiettivi {#objective}

1. Crea una nuova suite di rapporti o usa una esistente.
1. Configurare le variabili di conversione (eVar)[ e ](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html)eventi di successo (Events)[ nella suite di rapporti.](https://docs.adobe.com/help/en/analytics/admin/admin-tools/success-events/success-event.html)
1. Crea un [ progetto Analysis Workspace](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/home.html) per analizzare i dati con l&#39;aiuto di strumenti che ti consentono di creare, analizzare e condividere rapidamente informazioni.
1. Condividete il progetto Analysis Workspace  con altri membri del team.

## Prerequisiti

Questa esercitazione è una continuazione del [Tenere traccia del componente su cui è stato fatto clic con  Adobe Analytics](./track-clicked-component.md) e presuppone che si disponga di:

* A **Proprietà lancio** con l&#39;estensione [ Adobe Analytics](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) abilitata
* **ID suite di rapporti** Analytics/dev di Adobe e server di tracciamento. Consulta la seguente documentazione per [creare una nuova suite di rapporti](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [&#39;estensione ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) del browser del debugger di Experience Platform configurata con la proprietà Launch caricata in  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlor un sito AEM con  Adobe livello dati di.

## Variabili di conversione (eVar) ed eventi di successo (evento)

La variabile di conversione Custom Insight (o eVar ) viene inserita nel codice di Adobe  delle pagine Web selezionate del sito. Il suo scopo principale è segmentare le metriche di successo della conversione nei report di marketing personalizzati. Un eVar  può essere basato sulla visita e funzionare in modo simile ai cookie. I valori passati  variabili di eVar seguono l&#39;utente per un periodo predeterminato.

Quando un eVar  è impostato sul valore di un visitatore,  Adobe ricorda automaticamente tale valore fino alla scadenza. Eventuali eventi di successo riscontrati da un visitatore mentre il valore  eVar è attivo vengono conteggiati verso il valore  eVar.

Le eVar vengono utilizzate in modo ottimale per misurare la causa e l&#39;effetto, ad esempio:

* Quali campagne interne hanno influenzato le entrate
* Quali banner pubblicitari sono risultati in ultima istanza una registrazione
* Numero di volte in cui è stata utilizzata una ricerca interna prima di eseguire un ordine

Gli eventi di successo sono azioni che possono essere tracciate. È possibile determinare l&#39;evento di successo. Ad esempio, se un visitatore fa clic su un pulsante CTA, l&#39;evento click potrebbe essere considerato un evento di successo.

### Configurare le eVar

1. Dalla home page di Adobe Experience Cloud, selezionate la vostra organizzazione e avviate  Adobe Analytics.

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Dalla barra degli strumenti di Analytics, fate clic su **Admin** > **Suite di rapporti** e individuate la suite di rapporti.

   ![Analytics Report Suite](assets/create-analytics-workspace/select-report-suite.png)

1. Selezionare Suite di rapporti > **Modifica impostazioni** > **Conversione** > **Variabili di conversione**

   ![Variabili di conversione di Analytics](assets/create-analytics-workspace/conversion-variables.png)

1. Utilizzando l&#39;opzione **Aggiungi nuovo**, creiamo variabili di conversione per mappare lo schema come segue:

   * `eVar5` -   `Page Template`
   * `eVar6` -  `Page ID`
   * `eVar7` -  `Last Modified Date`
   * `eVar8` -  `Button Id`
   * `eVar9` -  `Page Name`

   ![Aggiungi nuove eVar](assets/create-analytics-workspace/add-new-evars.png)

1. Fornire un nome e una descrizione appropriati per ciascuna eVar e **Salvare** le modifiche. Nella sezione successiva verranno utilizzate queste eVar per creare un progetto Analysis Workspace . Quindi, un nome semplice rende le variabili facilmente individuabili.

   ![eVar](assets/create-analytics-workspace/evars.png)

### Configurare gli eventi di successo

Quindi, creiamo un evento even per tenere traccia del clic del pulsante CTA.

1. Dalla finestra **Report Suite Manager**, selezionare l&#39; **Report Suite Id** e fare clic su **Edit Settings** (Modifica impostazioni).
1. Fare clic su **Conversione** > **Eventi di successo**
1. Utilizzando l&#39;opzione **Aggiungi nuovo**, create un nuovo evento di successo personalizzato per tenere traccia del clic sul pulsante CTA e quindi **Salva** delle modifiche.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## Creare un nuovo progetto in  Analysis Workspace {#workspace-project}

 Analysis Workspace è uno strumento browser flessibile che consente di creare analisi e condividere informazioni rapidamente. Tramite l’interfaccia di trascinamento, puoi creare analisi, aggiungere visualizzazioni per dare vita ai dati, curare un dataset, condividere e pianificare progetti con chiunque all’interno dell’organizzazione.

Quindi, create un nuovo [progetto](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/t-freeform-project.html) per creare un dashboard per analizzare le prestazioni dei pulsanti CTA in tutto il sito.

1. Dalla barra degli strumenti di Analytics, selezionare **Workspace** e fare clic su **Crea un nuovo progetto**.

   ![Area di lavoro](assets/create-analytics-workspace/create-workspace.png)

1. Scegliete di iniziare da un **progetto vuoto** oppure selezionate uno dei modelli predefiniti, forniti da  Adobe o modelli personalizzati creati dalla vostra organizzazione. Sono disponibili diversi modelli, a seconda dell’analisi o del caso di utilizzo previsto. [Ulteriori ](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) informazioni sulle diverse opzioni di modello disponibili.

   Nel progetto Workspace, i pannelli, le tabelle, le visualizzazioni e i componenti sono accessibili dalla barra a sinistra. Questi sono i vostri elementi costitutivi del progetto.

   * **[Componenti](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** : i componenti sono dimensioni, metriche, segmenti o intervalli di date, che possono essere combinati in una tabella a forma libera per iniziare a rispondere alle tue domande aziendali. Prima di approfondire l’analisi, è necessario acquisire familiarità con ciascun tipo di componente. Una volta acquisita la terminologia del componente, puoi iniziare a trascinare per creare l’analisi in una tabella a forma libera.
   * **[Visualizzazioni](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)**  - Le visualizzazioni, ad esempio un grafico a barre o a linee, vengono aggiunte sopra ai dati per renderle visibili. Nella barra a sinistra, seleziona l&#39;icona Visualizza in mezzo per visualizzare l&#39;elenco completo delle visualizzazioni disponibili.
   * **[Pannelli](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/panels.html)**  - Un pannello è un insieme di tabelle e visualizzazioni. È possibile accedere ai pannelli dall’icona in alto a sinistra nell’area di lavoro. I pannelli sono utili per organizzare i progetti in base a periodi di tempo, suite di rapporti o casi di utilizzo dell’analisi. In  Analysis Workspace sono disponibili i seguenti tipi di pannelli:

   ![Selezione dei modelli](assets/create-analytics-workspace/workspace-tools.png)

### Aggiunta di visualizzazioni dati con  Analysis Workspace

Quindi, create una tabella per creare una rappresentazione visiva del modo in cui gli utenti interagiscono con i pulsanti Invito all’azione (CTA) nella home page del sito WKND. Per creare una rappresentazione di questo tipo, utilizziamo i dati raccolti nel componente [con clic sul tracciamento con  Adobe Analytics](./track-clicked-component.md). Di seguito è riportato un breve riepilogo dei dati tracciati per le interazioni degli utenti con i pulsanti Invito all’azione per il sito WKND.

* `eVar5` -   `Page template`
* `eVar6` -  `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

1. Trascinare il componente Dimensione **Pagina** nella tabella a forma libera. È ora possibile visualizzare una visualizzazione con il Nome pagina ( eVar 9) e le Visualizzazioni pagina corrispondenti (occorrenze) visualizzate all’interno della tabella.

   ![Dimension pagina](assets/create-analytics-workspace/evar9-dimension.png)

1. Trascinare la metrica **CTA Click** (event8) sulla metrica delle occorrenze e sostituirla. È ora possibile visualizzare una visualizzazione che mostra il Nome pagina ( eVar 9) e il corrispondente conteggio degli eventi CTA Click su una pagina.

   ![Metrica pagina - CTA Click](assets/create-analytics-workspace/evar8-cta-click.png)

1. Suddividiamo la pagina per tipo di modello. Selezionate la metrica del modello di pagina dai componenti e trascinate la metrica Modello pagina nella dimensione Nome pagina. Ora è possibile visualizzare il nome della pagina suddiviso per il tipo di modello.

   * **Prima**

      ![ eVar 5](assets/create-analytics-workspace/evar5.png)

   * **Dopo**

      ![Metriche  eVar 5](assets/create-analytics-workspace/evar5-metrics.png)

1. Per comprendere in che modo gli utenti interagiscono con i pulsanti CTA quando si trovano sulle pagine del sito WKND, è necessario suddividere ulteriormente la metrica Modello pagina aggiungendo la metrica ID pulsante ( eVar 8).

   ![ eVar 8](assets/create-analytics-workspace/evar8.png)

1. Di seguito è riportata una rappresentazione visiva del sito WKND suddivisa per modello di pagina e ulteriormente suddivisa per interazione con l&#39;utente con i pulsanti CTA (Site Click to Action, Clic to Action) del sito WKND.

   ![ eVar 8](assets/create-analytics-workspace/evar8-metric.png)

1. È possibile sostituire il valore ID pulsante con un nome più semplice utilizzando le  classificazioni Adobe Analytics. Per ulteriori informazioni su come creare una classificazione per una metrica specifica, vedere [here](https://docs.adobe.com/content/help/en/analytics/components/classifications/c-classifications.html). In questo caso, abbiamo una metrica di classificazione `Button Section (Button ID)` impostata per `eVar8` che mappa l&#39;ID del pulsante con un nome semplice.

   ![Sezione pulsante](assets/create-analytics-workspace/button-section.png)

## Aggiungi classificazione a una variabile Analytics

### Classificazioni conversione

La classificazione di Analytics è un modo per classificare i dati variabili di Analytics, e quindi per visualizzare i dati in modi diversi quando si generano i rapporti. Per migliorare la modalità di visualizzazione dell&#39;ID pulsante nel rapporto di Analytics Workspace, creiamo una variabile di classificazione per l&#39;ID pulsante ( eVar 8). Quando si classifica, si crea una relazione tra la variabile e i metadati correlati a tale variabile.

Quindi, creiamo una variabile Classificazione per Analytics.

1. Dal menu **Admin** della barra degli strumenti, selezionare **Suite di rapporti**
1. Selezionare l&#39; **ID suite di rapporti** dalla finestra **Report Suite Manager** e fare clic su **Edit Settings** > **Conversion** > **Conversion Classifications**

   ![Classificazione conversione](assets/create-analytics-workspace/conversion-classification.png)

1. Dall&#39;elenco a discesa **Seleziona tipo classificazione**, selezionare la variabile ( ID pulsante di eVar 8) per aggiungere una classificazione.
1. Fare clic sulla freccia accanto alla variabile Classificazione elencata nella sezione Classificazioni per aggiungere una nuova classificazione.

   ![Tipo classificazione conversione](assets/create-analytics-workspace/select-classification-variable.png)

1. Nella finestra di dialogo **Edit a Classification**, specificare un nome adatto per la classificazione di testo. Viene creato un componente dimensione con il nome classificazione testo.

   ![Tipo classificazione conversione](assets/create-analytics-workspace/new-classification.png)

1. **** Salvare le modifiche.

### Importatore classificazione

Utilizzate Importazione per caricare le classificazioni in  Adobe Analytics. È inoltre possibile esportare i dati per l’aggiornamento prima di un’importazione. I dati importati con lo strumento di importazione devono essere in un formato specifico.  Adobe offre l’opzione per scaricare un modello di dati con tutti i dettagli di intestazione corretti in un file di dati delimitato da tabulazioni. Puoi aggiungere i nuovi dati a questo modello e quindi importare il file di dati nel browser tramite FTP.

#### Modello classificazione

Prima di importare le classificazioni nei rapporti di marketing, puoi scaricare un modello che ti aiuta a creare un file di dati per le classificazioni. Il file di dati utilizza le classificazioni desiderate come intestazioni di colonna, quindi organizza il set di dati di reporting sotto le intestazioni di classificazione appropriate.

Quindi, scaricate il modello di classificazione per la variabile ID pulsante ( eVar 8)

1. Passa a **Amministratore** > **Importatore classificazione**
1. Scaricate un modello di classificazione per la variabile di conversione dalla scheda **Scarica modello**.
   ![Tipo classificazione conversione](assets/create-analytics-workspace/classification-importer.png)

1. Nella scheda Scarica modello, specifica la configurazione del modello di dati.
   * **Seleziona suite**  di rapporti: Selezionate la suite di rapporti da usare nel modello. La suite di rapporti e il set di dati devono corrispondere.
   * **Set di dati da classificare** : Selezionare il tipo di dati per il file di dati. Il menu include tutti i rapporti nelle suite di rapporti configurati per le classificazioni.
   * **Codifica** : Selezionare la codifica dei caratteri per il file di dati. Il formato di codifica predefinito è UTF-8.

1. Fare clic su **Scarica** e salvare il file modello nel sistema locale. Il file modello è un file di dati delimitato da tabulazioni (estensione del nome file .tab) supportato dalla maggior parte delle applicazioni per fogli di calcolo.
1. Aprite il file di dati delimitato da tabulazioni utilizzando un editor di vostra scelta.
1. Aggiungete l&#39;ID pulsante ( eVar 9) e un nome di pulsante corrispondente al file delimitato da tabulazioni per ciascun valore  eVar 9 dal passaggio 9 della sezione.

   ![Valore chiave](assets/create-analytics-workspace/key-value.png)

1. **Salvate** il file delimitato da tabulazioni.
1. Passare alla scheda **Importa file**.
1. Configurare la destinazione per l’importazione del file.
   * **Seleziona suite**  di rapporti: AEM sito WKND (Suite di rapporti)
   * **Set di dati da classificare** : Id Pulsante (Variabile Di Conversione  eVar 8)
1. Fare clic sull&#39;opzione **Scegli file** per caricare il file delimitato da tabulazioni dal sistema, quindi fare clic su **Importa file**

   ![Importazione file](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Un&#39;importazione corretta visualizza immediatamente le modifiche appropriate in un&#39;esportazione. Tuttavia, le modifiche ai dati nei rapporti richiedono fino a quattro ore quando si utilizza un&#39;importazione browser e fino a 24 ore quando si utilizza un&#39;importazione FTP.

#### Sostituire la variabile di conversione con la variabile di classificazione

1. Dalla barra degli strumenti di Analytics, selezionate **Workspace** e aprite l&#39;area di lavoro creata in [Crea un nuovo progetto nella sezione  Analysis Workspace](#workspace-project) di questa esercitazione.

   ![ID pulsante Area di lavoro](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Successivamente, sostituire la metrica **ID pulsante** nell&#39;area di lavoro che visualizza l&#39;ID di un pulsante Invito all&#39;azione (CTA) con il nome di classificazione creato nel passaggio precedente.

1. Nel mirino componenti, cercate la dimensione **Pulsanti CTA WKND** e trascinate la dimensione **Pulsanti CTA WKND (ID pulsante)** nella metrica ID pulsante e sostituitela.

   * **Prima**

      ![Pulsante Area di lavoro prima](assets/create-analytics-workspace/wknd-button-before.png)
   * **Dopo**

      ![Pulsante Workspace dopo](assets/create-analytics-workspace/wknd-button-after.png)

1. È possibile notare che la metrica ID pulsante che conteneva l&#39;ID pulsante di un pulsante Invito all&#39;azione (CTA) ora viene sostituita con un nome corrispondente fornito nel modello di classificazione.
1. Confrontiamo la tabella di Analytics Workspace con la home page WKND e comprendiamo il numero di clic del pulsante CTA e la relativa analisi. In base ai dati della tabella a forma libera dell&#39;area di lavoro, è chiaro che 22 volte gli utenti hanno fatto clic sul pulsante **SKI NOW** e quattro volte sul pulsante WKND Home Page Camping in Australia Occidentale **Leggi tutto**.

   ![Report CTA](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Assicurati di salvare il progetto  Adobe Analytics Workspace e di fornire un nome e una descrizione corretti. Facoltativamente, è possibile aggiungere tag a un progetto dell&#39;area di lavoro.

   ![Salva progetto](assets/create-analytics-workspace/save-project.png)

1. Dopo aver salvato il progetto, potete condividere il progetto dell’area di lavoro con altri colleghi o colleghi di lavoro utilizzando l’opzione Condividi.

   ![Condividi progetto](assets/create-analytics-workspace/share.png)

## Congratulazioni!

Hai appena imparato a mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni  suite di rapporti Adobe Analytics, a eseguire una classificazione per le metriche e a creare un dashboard di reporting dettagliato utilizzando la funzione  Analysis Workspace di  Adobe Analytics.

