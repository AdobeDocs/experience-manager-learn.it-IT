---
title: Analizzare i dati con Analysis Workspace
description: Scopri come mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni nelle suite di rapporti di Adobe Analytics. Scopri come creare un dashboard di reporting dettagliato utilizzando la funzione Analysis Workspace di Adobe Analytics.
feature: analisi
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6409
thumbnail: KT-6296.jpg
topic: Integrations (Integrazioni)
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '2204'
ht-degree: 0%

---


# Analizzare i dati con Analysis Workspace

Scopri come mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni nelle suite di rapporti di Adobe Analytics. Scopri come creare un dashboard di reporting dettagliato utilizzando la funzione Analysis Workspace di Adobe Analytics.

## Cosa verrà creato

Il team di marketing WKND desidera capire quali pulsanti di invito all’azione (CTA, Call to Action) eseguono al meglio sulla home page. In questa esercitazione, creeremo un nuovo progetto in Analysis Workspace per visualizzare le prestazioni dei diversi pulsanti CTA e capire il comportamento degli utenti sul sito. Le seguenti informazioni vengono acquisite utilizzando Adobe Analytics quando un utente fa clic su un pulsante Invito all’azione (CTA) nella home page WKND.

**Variabili di Analytics**

Di seguito sono elencate le variabili di Analytics attualmente monitorate:

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

![CTA Fai clic su Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Obiettivi {#objective}

1. Crea una nuova suite di rapporti o utilizzane una esistente.
1. Configura le [variabili di conversione (eVar)](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html) e [Eventi di successo (Eventi)](https://docs.adobe.com/help/en/analytics/admin/admin-tools/success-events/success-event.html) nella suite di rapporti.
1. Crea un [progetto Analysis Workspace](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/home.html) per analizzare i dati con l&#39;aiuto di strumenti che ti consentono di generare, analizzare e condividere informazioni rapidamente.
1. Condividi il progetto Analysis Workspace con altri membri del team.

## Prerequisiti

Questa esercitazione è una continuazione del [Tieni traccia del componente su cui hai fatto clic con Adobe Analytics](./track-clicked-component.md) e presuppone che tu abbia:

* A **Proprietà Launch** con l&#39;estensione [Adobe Analytics](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) abilitata
* **Adobe ID suite di rapporti** Analytics/dev e server di tracciamento. Consulta la seguente documentazione per [creare una nuova suite di rapporti](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform estensione ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Debuggerbrowser configurata con la proprietà Launch caricata su  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlor un sito AEM con l’Adobe Data Layer abilitato.

## Variabili di conversione (eVar) ed eventi di successo (evento)

La variabile di conversione Custom Insight (o eVar) viene inserita nel codice di Adobe nelle pagine web selezionate del sito. Il suo scopo principale è segmentare le metriche di successo della conversione nei rapporti di marketing personalizzati. Un eVar può essere basato su visite e funziona in modo simile ai cookie. I valori trasmessi nelle variabili eVar seguono l&#39;utente per un periodo predeterminato.

Quando un eVar è impostato sul valore di un visitatore, Adobe ricorda automaticamente tale valore fino alla scadenza. Eventuali eventi di successo riscontrati da un visitatore mentre il valore eVar è attivo vengono conteggiati per il valore eVar.

Le eVar vengono utilizzate in modo più efficace per misurare la causa e l’effetto, ad esempio:

* Quali campagne interne hanno influenzato i ricavi
* Quali banner pubblicitari sono stati alla fine il risultato di una registrazione
* Numero di volte in cui è stata utilizzata una ricerca interna prima di effettuare un ordine

Gli eventi di successo sono azioni che possono essere tracciate. È possibile determinare l&#39;evento di successo. Ad esempio, se un visitatore fa clic su un pulsante CTA, l’evento clic potrebbe essere considerato un evento riuscito.

### Configurare eVar

1. Dalla home page di Adobe Experience Cloud, seleziona l’organizzazione e avvia Adobe Analytics.

   ![AEP di Analytics](assets/create-analytics-workspace/analytics-aep.png)

1. Dalla barra degli strumenti di Analytics, fai clic su **Amministratore** > **Suite di rapporti** e trova la tua Suite di rapporti.

   ![Suite di rapporti di Analytics](assets/create-analytics-workspace/select-report-suite.png)

1. Seleziona Suite di rapporti > **Modifica impostazioni** > **Conversione** > **Variabili di conversione**

   ![Variabili di conversione di Analytics](assets/create-analytics-workspace/conversion-variables.png)

1. Utilizzando l&#39;opzione **Aggiungi nuovo**, creiamo le variabili di conversione per mappare lo schema come segue:

   * `eVar5` -   `Page Template`
   * `eVar6` -  `Page ID`
   * `eVar7` -  `Last Modified Date`
   * `eVar8` -  `Button Id`
   * `eVar9` -  `Page Name`

   ![Aggiungi nuove eVar](assets/create-analytics-workspace/add-new-evars.png)

1. Immetti un nome e una descrizione appropriati per ciascuna eVar e **Salva** le modifiche. Useremo questi eVar per creare un progetto Analysis Workspace nella sezione successiva. Quindi, un nome facile da usare rende le variabili facilmente individuabili.

   ![eVar](assets/create-analytics-workspace/evars.png)

### Configurare eventi di successo

Quindi, creiamo un pari per tenere traccia del clic sul pulsante CTA.

1. Dalla finestra **Report Suite Manager**, seleziona **Report Suite Id** e fai clic su **Edit Settings** (Modifica impostazioni).
1. Fai clic su **Conversione** > **Eventi di successo**
1. Utilizzando l&#39;opzione **Aggiungi nuovo**, crea un nuovo evento di successo personalizzato per tenere traccia del clic sul pulsante CTA, quindi **Salva** le modifiche.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## Creare un nuovo progetto in Analysis Workspace {#workspace-project}

Analysis Workspace è uno strumento browser flessibile che ti consente di creare analisi e condividere informazioni rapidamente. Tramite l’interfaccia a trascinamento della selezione puoi creare le analisi, aggiungere visualizzazioni per dare vita ai dati, curare un set di dati, condividere e pianificare progetti con chiunque all’interno della tua organizzazione.

Quindi, crea un nuovo [progetto](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/t-freeform-project.html) per creare un dashboard per analizzare le prestazioni dei pulsanti CTA in tutto il sito.

1. Dalla barra degli strumenti di Analytics, seleziona **Area di lavoro** e fai clic su **Crea un nuovo progetto**.

   ![Area di lavoro](assets/create-analytics-workspace/create-workspace.png)

1. Scegli di iniziare da un **progetto vuoto** oppure seleziona uno dei modelli predefiniti, forniti da un Adobe o da modelli personalizzati creati dalla tua organizzazione. Sono disponibili diversi modelli, a seconda dell’analisi o del caso d’uso previsto. [Ulteriori ](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) informazioni sulle diverse opzioni di modello disponibili.

   Nel progetto Workspace, puoi accedere a pannelli, tabelle, visualizzazioni e componenti dalla barra a sinistra. Questi sono gli elementi di base del tuo progetto.

   * **[Componenti](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** : i componenti sono dimensioni, metriche, segmenti o intervalli di date che possono essere combinati in una tabella a forma libera per iniziare a rispondere a domande di business. Prima di iniziare l’analisi, acquisisci familiarità con ogni tipo di componente. Dopo aver acquisito dimestichezza con la terminologia dei componenti, puoi iniziare a trascinarli per creare l’analisi in una tabella a forma libera.
   * **[Visualizzazioni](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** : le visualizzazioni, ad esempio un grafico a barre o a linee, vengono quindi aggiunte ai dati per riprodurli visivamente. Nella barra a sinistra, seleziona l’icona Visualizzazioni centrale per visualizzare l’elenco completo delle visualizzazioni disponibili.
   * **[Pannelli](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/panels.html)**  - Un pannello è un insieme di tabelle e visualizzazioni. Puoi accedere ai pannelli dall’icona in alto a sinistra nell’area di lavoro. I pannelli sono utili quando desideri organizzare i progetti in base a periodi di tempo, suite di rapporti o casi di utilizzo dell’analisi. In Analysis Workspace sono disponibili i seguenti tipi di pannelli:

   ![Selezione del modello](assets/create-analytics-workspace/workspace-tools.png)

### Aggiungere visualizzazione dati con Analysis Workspace

Quindi, crea una tabella per creare una rappresentazione visiva del modo in cui gli utenti interagiscono con i pulsanti Invito all’azione (CTA) nella home page del sito WKND. Per creare una rappresentazione di questo tipo, utilizziamo i dati raccolti nel componente [Tieni traccia del clic con Adobe Analytics](./track-clicked-component.md). Di seguito è riportato un rapido riepilogo dei dati tracciati per le interazioni dell’utente con i pulsanti Invito all’azione per il sito WKND.

* `eVar5` -   `Page template`
* `eVar6` -  `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

1. Trascina il componente Dimensione **Pagina** nella tabella a forma libera. Ora dovresti essere in grado di visualizzare una visualizzazione con il Nome pagina (eVar9) e le Viste pagina corrispondenti (Occorrenze) visualizzate all’interno della tabella.

   ![Dimension pagina](assets/create-analytics-workspace/evar9-dimension.png)

1. Trascina la metrica **CTA Click** (event8) nella metrica delle occorrenze e sostituiscila. Ora puoi visualizzare una visualizzazione che mostra il Nome pagina (eVar9) e un conteggio corrispondente degli eventi CTA Click su una pagina.

   ![Metrica pagina - Clic CTA](assets/create-analytics-workspace/evar8-cta-click.png)

1. Suddividiamo il per pagina per tipo di modello. Seleziona la metrica del modello di pagina dai componenti e trascina la metrica Modello pagina sulla dimensione Nome pagina. Ora puoi visualizzare il nome della pagina suddiviso per il relativo tipo di modello.

   * **Prima**

      ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **Dopo**

      ![Metriche eVar5](assets/create-analytics-workspace/evar5-metrics.png)

1. Per comprendere in che modo gli utenti interagiscono con i pulsanti CTA quando si trovano sulle pagine del sito WKND, è necessario suddividere ulteriormente la metrica Modello pagina aggiungendo la metrica ID pulsante (eVar8).

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. Di seguito è riportata una rappresentazione visiva del sito WKND suddivisa per il relativo modello di pagina e ulteriormente suddivisa per l’interazione dell’utente con i pulsanti di azione Click to Action (CTA) del sito WKND.

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Puoi sostituire il valore ID pulsante con un nome più semplice utilizzando le classificazioni di Adobe Analytics. Puoi trovare ulteriori informazioni su come creare una classificazione per una metrica specifica [qui](https://docs.adobe.com/content/help/en/analytics/components/classifications/c-classifications.html). In questo caso, disponiamo di una configurazione della metrica di classificazione `Button Section (Button ID)` per `eVar8` che mappa l’ID del pulsante con un nome semplice.

   ![Sezione pulsante](assets/create-analytics-workspace/button-section.png)

## Aggiungere una classificazione a una variabile Analytics

### Classificazioni di conversione

La classificazione di Analytics è un modo per classificare i dati delle variabili di Analytics e visualizzarli in modi diversi quando si generano i rapporti. Per migliorare la modalità di visualizzazione dell’ID pulsante nel rapporto di Analysis Workspace, creiamo una variabile di classificazione per ID pulsante (eVar8). Durante la classificazione viene stabilita una relazione tra la variabile e i metadati correlati a essa.

Quindi, creiamo una classificazione per la variabile Analytics.

1. Dal menu della barra degli strumenti **Amministratore**, seleziona **Suite di rapporti**
1. Seleziona **ID suite di rapporti** dalla finestra **Gestione suite di rapporti** e fai clic su **Modifica impostazioni** > **Conversione** > **Classificazioni di conversione**

   ![Classificazione della conversione](assets/create-analytics-workspace/conversion-classification.png)

1. Dall’elenco a discesa **Seleziona tipo di classificazione** , seleziona la variabile (ID pulsante eVar8) per aggiungere una classificazione.
1. Fai clic sulla freccia accanto alla variabile di classificazione elencata nella sezione Classificazioni per aggiungere una nuova classificazione.

   ![Tipo di classificazione della conversione](assets/create-analytics-workspace/select-classification-variable.png)

1. Nella finestra di dialogo **Modifica una classificazione**, specificare un nome adatto per la classificazione del testo. Viene creato un componente dimensione con il nome della classificazione del testo.

   ![Tipo di classificazione della conversione](assets/create-analytics-workspace/new-classification.png)

1. **** Salva le modifiche.

### Importatore di classificazione

Utilizza l’importazione per caricare le classificazioni in Adobe Analytics. Puoi anche esportare i dati per l’aggiornamento prima di un’importazione. I dati importati con lo strumento di importazione devono essere in un formato specifico. Adobe offre l’opzione per scaricare un modello di dati con tutti i dettagli di intestazione corretti in un file di dati delimitato da tabulazioni. Puoi aggiungere i nuovi dati a questo modello e quindi importare il file di dati nel browser utilizzando l’FTP.

#### Modello di classificazione

Prima di importare le classificazioni nei rapporti di marketing, puoi scaricare un modello che ti aiuta a creare un file di dati delle classificazioni. Il file di dati utilizza le classificazioni desiderate come intestazioni di colonna, quindi organizza il set di dati di reporting sotto le intestazioni di classificazione appropriate.

Quindi, scarica il modello di classificazione per la variabile ID pulsante (eVar8)

1. Passa a **Amministratore** > **Importazione delle classificazioni**
1. Scarichiamo un modello di classificazione per la variabile di conversione dalla scheda **Scarica modello** .
   ![Tipo di classificazione della conversione](assets/create-analytics-workspace/classification-importer.png)

1. Nella scheda Scarica modello , specifica la configurazione del modello di dati.
   * **Seleziona Suite**  di rapporti: Seleziona la suite di rapporti da utilizzare nel modello. La suite di rapporti e il set di dati devono corrispondere.
   * **Set di dati da classificare** : Selezionare il tipo di dati per il file di dati. Il menu include tutti i rapporti nelle suite di rapporti configurati per le classificazioni.
   * **Codifica** : Selezionare la codifica dei caratteri per il file di dati. Il formato di codifica predefinito è UTF-8.

1. Fai clic su **Scarica** e salva il file modello nel sistema locale. Il file modello è un file di dati delimitato da tabulazioni (estensione del nome file .tab) supportato dalla maggior parte delle applicazioni per fogli di calcolo.
1. Apri il file di dati delimitato da tabulazioni utilizzando un editor di tua scelta.
1. Aggiungi l’ID pulsante (eVar9) e il nome corrispondente del pulsante al file delimitato da tabulazioni per ciascun valore eVar9 dal passaggio 9 della sezione .

   ![Valore chiave](assets/create-analytics-workspace/key-value.png)

1. **** Salva il file delimitato da tabulazioni.
1. Passa alla scheda **Importa file** .
1. Configura la destinazione per l’importazione del file.
   * **Seleziona Suite**  di rapporti: AEM sito WKND (Suite di rapporti)
   * **Set di dati da classificare** : Id Pulsante (Conversion Variable eVar8)
1. Fai clic sull&#39;opzione **Scegli file** per caricare dal sistema il file delimitato da tabulazioni, quindi fai clic su **Importa file**

   ![Importazione file](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Un’importazione corretta visualizza immediatamente le modifiche appropriate in un’esportazione. Tuttavia, le modifiche ai dati nei rapporti richiedono fino a quattro ore quando si utilizza un’importazione browser e fino a 24 ore quando si utilizza un’importazione FTP.

#### Sostituire la variabile di conversione con la variabile di classificazione

1. Dalla barra degli strumenti di Analytics, seleziona **Area di lavoro** e apri l&#39;area di lavoro creata in [Crea un nuovo progetto nella sezione Analysis Workspace](#workspace-project) di questa esercitazione.

   ![ID pulsante Area di lavoro](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Quindi, sostituisci la metrica **ID pulsante** nell’area di lavoro che visualizza l’ID di un pulsante di invito all’azione (CTA) con il nome di classificazione creato nel passaggio precedente.

1. Dal componente finder, cerca la dimensione **Pulsanti CTA WKND** e trascina la dimensione **Pulsanti CTA WKND (Button Id)** nella metrica ID pulsante e sostituiscilo.

   * **Prima**

      ![Pulsante Area di lavoro prima](assets/create-analytics-workspace/wknd-button-before.png)
   * **Dopo**

      ![Pulsante Workspace dopo](assets/create-analytics-workspace/wknd-button-after.png)

1. È possibile notare che la metrica ID pulsante che conteneva l’ID pulsante di un pulsante Invito all’azione (CTA) ora viene sostituita con un nome corrispondente fornito nel modello di classificazione.
1. Confrontiamo la tabella di Analysis Workspace con la home page WKND e comprendiamo il conteggio dei clic del pulsante CTA e la relativa analisi. In base ai dati della tabella a forma libera dell&#39;area di lavoro, è chiaro che 22 volte gli utenti hanno fatto clic sul pulsante **SKI NOW** e quattro volte per il pulsante WKND Home Page Camping in Western Australia **Read More** .

   ![Rapporto CTA](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Assicurati di salvare il progetto Adobe Analytics Workspace e di fornire un nome e una descrizione corretti. Facoltativamente, puoi aggiungere tag a un progetto Workspace.

   ![Salva progetto](assets/create-analytics-workspace/save-project.png)

1. Dopo aver salvato correttamente il progetto, puoi condividere il progetto Workspace con altri colleghi o team utilizzando l’opzione Condividi .

   ![Condividi progetto](assets/create-analytics-workspace/share.png)

## Congratulazioni!

Hai appena imparato a mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni nelle suite di rapporti Adobe Analytics, a eseguire una classificazione per le metriche e a creare un dashboard di reporting dettagliato utilizzando la funzione Analysis Workspace di Adobe Analytics.

