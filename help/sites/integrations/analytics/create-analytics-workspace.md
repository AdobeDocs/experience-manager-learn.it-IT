---
title: Analizzare i dati con Analysis Workspace
description: Scopri come mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni nelle suite di rapporti di Adobe Analytics. Scopri come creare un dashboard di reporting dettagliato utilizzando la funzione Analysis Workspace di Adobe Analytics.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="Integrazione" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 443
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 0%

---

# Analizzare i dati con Analysis Workspace

Scopri come mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni nelle suite di rapporti di Adobe Analytics. Scopri come creare un dashboard di reporting dettagliato utilizzando la funzione Analysis Workspace di Adobe Analytics.

## Cosa intendi creare {#what-build}

Il team di marketing WKND è interessato a sapere quali pulsanti `Call to Action (CTA)` offrono le migliori prestazioni nella home page. In questo tutorial, crea un progetto in **Analysis Workspace** per visualizzare le prestazioni di diversi pulsanti di CTA e comprendere il comportamento degli utenti sul sito. Le seguenti informazioni vengono acquisite tramite Adobe Analytics quando un utente fa clic su un pulsante di invito all’azione (CTA) nella home page di WKND.

**Variabili di Analytics**

Di seguito sono riportate le variabili di Analytics attualmente tracciate:

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA Fare clic su Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Obiettivi {#objective}

1. Crea una suite di rapporti o usane una esistente.
1. Configurare [Variabili di conversione (eVar)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=it) e [Eventi di successo (Eventi)](https://experienceleague.adobe.com/it/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event) nella suite di rapporti.
1. Crea un [progetto Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=it) per analizzare i dati con l&#39;aiuto di strumenti che ti consentono di generare, analizzare e condividere rapidamente le informazioni.
1. Condividi il progetto Analysis Workspace con altri membri del team.

## Prerequisiti

Questo tutorial è una continuazione del componente [Traccia clic con Adobe Analytics](./track-clicked-component.md) e presuppone che tu abbia:

* **Proprietà tag** con [estensione Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=it) abilitata
* **Adobe Analytics** ID suite di rapporti test/dev e server di tracciamento. Consulta la seguente documentazione per [creare una suite di rapporti](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=it).
* Estensione del browser [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=it) configurata con una proprietà tag caricata nel [sito WKND](https://wknd.site/us/en.html) o in un sito AEM con Adobe Data Layer abilitato.

## Variabili di conversione (eVar) ed Eventi di successo (Evento)

La variabile di conversione Custom Insight (o eVar) viene inserita nel codice Adobe nelle pagine web selezionate del sito. Il suo scopo principale è segmentare le metriche di successo della conversione nei rapporti di marketing personalizzati. Un’eVar può essere basata su visite e funziona in modo simile ai cookie. I valori trasmessi nelle variabili di eVar seguono l’utente per un periodo predeterminato.

Quando un eVar è impostato sul valore di un visitatore, Adobe ricorda automaticamente tale valore fino alla scadenza. Eventuali eventi di successo riscontrati da un visitatore mentre il valore eVar è attivo vengono conteggiati per il valore eVar.

Le eVar vengono utilizzate in modo ottimale per misurare la causa e l’effetto, ad esempio:

* Quali campagne interne hanno influenzato i ricavi
* Quali banner pubblicitari hanno portato a una registrazione
* Il numero di volte in cui è stata utilizzata una ricerca interna prima di effettuare un ordine

Gli eventi di successo sono azioni che possono essere tracciate. Puoi determinare cos’è un evento di successo. Ad esempio, se un visitatore fa clic su un pulsante CTA, l’evento clic potrebbe essere considerato un evento di successo.

### Configurare le eVar

1. Dalla pagina Home di Adobe Experience Cloud, seleziona la tua organizzazione e avvia Adobe Analytics.

   ![AEP di Analytics](assets/create-analytics-workspace/analytics-aep.png)

1. Dalla barra degli strumenti di Analytics, fai clic su **Amministratore** > **Suite di rapporti** e individua la tua suite di rapporti.

   ![Suite di rapporti di Analytics](assets/create-analytics-workspace/select-report-suite.png)

1. Seleziona Suite di rapporti > **Modifica impostazioni** > **Conversione** > **Variabili di conversione**

   ![Variabili di conversione Analytics](assets/create-analytics-workspace/conversion-variables.png)

1. Utilizzando l&#39;opzione **Aggiungi nuovo**, creiamo le variabili di conversione per mappare lo schema come segue:

   * `eVar5` - `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![Aggiungi nuove eVar](assets/create-analytics-workspace/add-new-evars.png)

1. Fornisci un nome e una descrizione appropriati per ogni eVar e **Salva** le modifiche. Nel progetto Analysis Workspace vengono utilizzate le eVar con il nome appropriato, pertanto un nome descrittivo rende le variabili facilmente individuabili.

   ![eVar](assets/create-analytics-workspace/evars.png)

### Configurare eventi di successo

Quindi, creiamo un evento per tenere traccia del clic sul pulsante CTA.

1. Dalla finestra **Report Suite Manager**, seleziona il **ID Report Suite** e fai clic su **Modifica impostazioni**.
1. Fai clic su **Conversione** > **Eventi di successo**
1. Utilizzando l&#39;opzione **Aggiungi nuovo**, crea un evento di successo personalizzato per tenere traccia del clic sul pulsante CTA e quindi **Salva** le modifiche.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## Creare un progetto in Analysis Workspace {#workspace-project}

Analysis Workspace è uno strumento flessibile per browser che consente di generare analisi e condividere rapidamente le informazioni. Tramite l’interfaccia di trascinamento, puoi creare le analisi, aggiungere visualizzazioni per dare vita ai dati, curare un set di dati, condividere e pianificare progetti con chiunque all’interno della tua organizzazione.

Quindi, crea un [progetto](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html?lang=it#analysis-workspace) per creare un dashboard per analizzare le prestazioni dei pulsanti di CTA in tutto il sito.

1. Dalla barra degli strumenti di Analytics, seleziona **Workspace** e fai clic su **Crea nuovo progetto**.

   ![Workspace](assets/create-analytics-workspace/create-workspace.png)

1. Scegli di iniziare da un **progetto vuoto** o seleziona uno dei modelli predefiniti, forniti da Adobe o dai modelli personalizzati creati dalla tua organizzazione. Sono disponibili diversi modelli, a seconda dell’analisi o del caso d’uso a cui stai pensando. [Ulteriori informazioni](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html?lang=it) sulle diverse opzioni di modello disponibili.

   Nel progetto Workspace, puoi accedere a pannelli, tabelle, visualizzazioni e componenti dalla barra a sinistra. Costituiscono gli elementi di base del progetto.

   * **[Componenti](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html?lang=it)** - I componenti sono dimensioni, metriche, segmenti o intervalli di date, che possono essere combinati in una tabella a forma libera per iniziare a rispondere a specifiche domande di business. Prima di iniziare con le analisi, impara a conoscere i diversi tipi di componente. Dopo aver acquisito dimestichezza con la terminologia dei componenti, puoi iniziare a trascinarli per generare le analisi in una tabella a forma libera.
   * **[Visualizzazioni](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html?lang=it)** - Le visualizzazioni, ad esempio i grafici a barre o a linee, vengono quindi aggiunte ai dati per riprodurli visivamente. Nella barra a sinistra, seleziona l’icona centrale Visualizzazioni per visualizzare l’elenco di tutte le visualizzazioni disponibili.
   * **[Pannelli](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html?lang=it)** - Un pannello è una raccolta di tabelle e visualizzazioni. Puoi accedere ai pannelli dall’icona in alto a sinistra in Workspace. I pannelli sono utili per organizzare i progetti in base a periodi di tempo, suite di rapporti o casi di utilizzo di analisi. In Analysis Workspace sono disponibili i seguenti tipi di pannello:

   ![Selezione modello](assets/create-analytics-workspace/workspace-tools.png)

### Aggiungere visualizzazione dati con Analysis Workspace

Quindi, creare una tabella per creare una rappresentazione visiva del modo in cui gli utenti interagiscono con i pulsanti `Call to Action (CTA)` nella home page del sito WKND. Per creare tale rappresentazione, utilizziamo i dati raccolti nel componente [Traccia clic con Adobe Analytics](./track-clicked-component.md). Di seguito è riportato un rapido riepilogo dei dati tracciati per le interazioni dell’utente con i pulsanti di invito all’azione per il sito WKND.

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. Trascina il componente dimensione **Pagina** nella tabella a forma libera. Ora dovresti essere in grado di visualizzare una visualizzazione che mostra il Nome pagina (eVar9) e le Visualizzazioni pagina corrispondenti (Occorrenze) visualizzati all’interno della tabella.

   ![Pagina Dimension](assets/create-analytics-workspace/evar9-dimension.png)

1. Trascina la metrica **Clic su CTA** (evento8) nella metrica delle occorrenze e sostituiscila. Ora puoi visualizzare una visualizzazione che visualizza il Nome pagina (eVar9) e un numero corrispondente di eventi di clic CTA su una pagina.

   ![Metrica pagina - Clic su CTA](assets/create-analytics-workspace/evar8-cta-click.png)

1. Suddividiamo la pagina in base al tipo di modello. Seleziona la metrica del modello di pagina dai componenti, quindi trascina la metrica Modello pagina nella dimensione Nome pagina. Ora puoi visualizzare il nome della pagina suddiviso per il relativo tipo di modello.

   * **Prima**

     ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **Dopo**

     ![Metriche eVar5](assets/create-analytics-workspace/evar5-metrics.png)

1. Per capire come gli utenti interagiscono con i pulsanti di CTA quando si trovano nelle pagine del sito WKND, è necessario aggiungere la metrica ID pulsante (eVar8).

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. Di seguito è riportata una rappresentazione visiva del sito WKND suddivisa per il modello di pagina e per l’interazione dell’utente con i pulsanti Click to Action (CTA) del sito WKND.

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Puoi sostituire il valore ID pulsante con un nome più semplice utilizzando le classificazioni di Adobe Analytics. Ulteriori informazioni su come creare una classificazione per una metrica specifica [qui](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html?lang=it). In questo caso, è stata impostata una metrica di classificazione `Button Section (Button ID)` per `eVar8` che mappa l&#39;ID pulsante su un nome intuitivo.

   ![Sezione pulsante](assets/create-analytics-workspace/button-section.png)

## Aggiungere classificazione a una variabile analitica

### Classificazioni di conversione

La classificazione di Analytics è un modo per classificare i dati delle variabili di Analytics e visualizzarli in modi diversi quando si generano i rapporti. Per migliorare la modalità di visualizzazione dell’ID pulsante nel rapporto Workspace di Analytics, creiamo una variabile di classificazione per l’ID pulsante (eVar8). Durante la classificazione, stabilisci una relazione tra la variabile e i metadati correlati a tale variabile.

Quindi, creiamo una variabile di classificazione per Analytics.

1. Dal menu della barra degli strumenti **Amministratore**, seleziona **Suite per report**
1. Seleziona **ID suite di rapporti** dalla finestra **Gestione suite di rapporti** e fai clic su **Modifica impostazioni** > **Conversione** > **Classificazioni di conversione**

   ![Classificazione conversione](assets/create-analytics-workspace/conversion-classification.png)

1. Dall&#39;elenco a discesa **Seleziona tipo di classificazione**, seleziona la variabile (ID pulsante eVar8) per aggiungere una classificazione.
1. Fai clic sulla freccia accanto alla variabile di classificazione elencata nella sezione Classificazioni per aggiungere una nuova classificazione.

   ![Tipo di classificazione conversione](assets/create-analytics-workspace/select-classification-variable.png)

1. Nella finestra di dialogo **Modifica classificazione**, specifica un nome appropriato per la classificazione di testo. Viene creato un componente dimensione con il nome Classificazione di testo.

   ![Tipo di classificazione conversione](assets/create-analytics-workspace/new-classification.png)

1. **Salva** le modifiche.

### Importatore di classificazione

Utilizza l’importazione per caricare le classificazioni in Adobe Analytics. Puoi anche esportare i dati per l’aggiornamento prima di un’importazione. I dati importati con lo strumento di importazione devono essere in un formato specifico. Adobe consente di scaricare un modello di dati con tutti i dettagli di intestazione corretti in un file di dati con valori delimitati da tabulazioni. Puoi aggiungere i nuovi dati a questo modello e quindi importare il file di dati nel browser utilizzando l’FTP.

#### Modello di classificazione

Prima di importare le classificazioni nei rapporti di marketing, puoi scaricare un modello per la creazione di un file di dati delle classificazioni. Il file di dati utilizza le classificazioni desiderate come intestazioni di colonna, quindi organizza il set di dati di reporting sotto le intestazioni di classificazione appropriate.

Quindi, scariciamo il modello di classificazione per la variabile Button Id (eVar8)

1. Passa a **Amministratore** > **Importazione classificazioni**
1. Scarica un modello di classificazione per la variabile di conversione dalla scheda **Scarica modello**.
   ![Tipo di classificazione conversione](assets/create-analytics-workspace/classification-importer.png)

1. Nella scheda Scarica modello, specifica la configurazione del modello di dati.
   * **Seleziona suite di rapporti** : seleziona la suite di rapporti da utilizzare nel modello. La suite di rapporti e il set di dati devono corrispondere.
   * **Set di dati da classificare**: selezionare il tipo di dati per il file di dati. Il menu include tutti i rapporti nelle suite di rapporti configurati per le classificazioni.
   * **Codifica**: selezionare la codifica dei caratteri per il file di dati. Il formato di codifica predefinito è UTF-8.

1. Fai clic su **Scarica** e salva il file modello nel sistema locale. Il file modello è un file di dati delimitato da tabulazioni (estensione .tab) supportato dalla maggior parte delle applicazioni per fogli di calcolo.
1. Apri il file di dati delimitato da tabulazioni utilizzando un editor a tua scelta.
1. Aggiungi l’ID pulsante (eVar9) e il nome del pulsante corrispondente al file delimitato da tabulazioni per ogni valore di eVar9 dal passaggio 9 della sezione.

   ![Valore chiave](assets/create-analytics-workspace/key-value.png)

1. **Salva** il file delimitato da tabulazioni.
1. Passare alla scheda **Importa file**.
1. Configura la destinazione per l’importazione del file.
   * **Seleziona suite di rapporti** : AEM del sito WKND (suite di rapporti)
   * **Set di dati da classificare**: ID pulsante (variabile di conversione eVar8)
1. Fare clic sull&#39;opzione **Scegli file** per caricare il file delimitato da tabulazioni dal sistema e quindi fare clic su **Importa file**

   ![Importazione file](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Un’importazione corretta visualizza immediatamente le modifiche appropriate in un’esportazione. Tuttavia, le modifiche ai dati nei rapporti richiedono fino a quattro ore quando si utilizza un’importazione browser e fino a 24 ore quando si utilizza un’importazione FTP.

#### Sostituisci variabile di conversione con variabile di classificazione

1. Dalla barra degli strumenti di Analytics, seleziona **Workspace** e apri l&#39;area di lavoro creata nella sezione [Crea un progetto in Analysis Workspace](#create-a-project-in-analysis-workspace) di questa esercitazione.

   ![ID pulsante Workspace](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Quindi, sostituisci la metrica **ID pulsante** nell&#39;area di lavoro che visualizza l&#39;ID di un pulsante di invito all&#39;azione (CTA) con il nome della classificazione creato nel passaggio precedente.

1. Dal Finder dei componenti, cercare **Pulsanti CTA WKND** e trascinare la dimensione **Pulsanti CTA WKND (ID pulsante)** nella metrica ID pulsante e sostituirla.

   * **Prima**

     ![Pulsante Workspace Prima](assets/create-analytics-workspace/wknd-button-before.png)
   * **Dopo**

     ![Pulsante Workspace dopo](assets/create-analytics-workspace/wknd-button-after.png)

1. Puoi notare che la metrica Id pulsante che conteneva l’ID pulsante di un pulsante di invito all’azione (CTA) ora viene sostituita con un nome corrispondente fornito nel modello di classificazione.
1. Confrontiamo la tabella Workspace di Analytics con la pagina Home di WKND e comprendiamo il conteggio dei clic dei pulsanti di CTA e la relativa analisi. In base ai dati della tabella a forma libera dell&#39;area di lavoro, è chiaro che 22 volte gli utenti hanno fatto clic sul pulsante **SKI NOW** e quattro volte per il campeggio WKND Home Page Camping nell&#39;Australia occidentale **Ulteriori informazioni**.

   ![Rapporto CTA](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Salva il progetto Adobe Analytics Workspace e specifica un nome e una descrizione corretti. Facoltativamente, puoi aggiungere tag a un progetto Workspace.

   ![Salva progetto](assets/create-analytics-workspace/save-project.png)

1. Dopo aver salvato correttamente il progetto, puoi condividere il progetto Workspace con altri collaboratori o compagni utilizzando l’opzione Condividi.

   ![Condividi progetto](assets/create-analytics-workspace/share.png)

## Congratulazioni.

Hai appena imparato a mappare i dati acquisiti da un sito Adobe Experience Manager a metriche e dimensioni nelle suite di rapporti di Adobe Analytics. Inoltre, ha eseguito una classificazione per le metriche e generato una dashboard di reporting dettagliata utilizzando la funzione Analysis Workspace di Adobe Analytics.
