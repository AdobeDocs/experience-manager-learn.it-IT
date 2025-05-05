---
title: Personalization con Frammenti esperienza AEM e Adobe Target
description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando Frammenti di esperienza Adobe Experience Manager e Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
duration: 1088
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1663'
ht-degree: 0%

---

# Personalization con Frammenti esperienza AEM e Adobe Target

Con la possibilità di esportare frammenti di esperienza AEM in offerte Adobe Target as HTML, puoi combinare la facilità d’uso e la potenza dell’AEM con le potenti capacità di intelligenza automatizzata (AI) e apprendimento automatico (ML) di Target per testare e personalizzare le esperienze su larga scala.

L’AEM riunisce tutti i contenuti e le risorse in una posizione centrale per alimentare la tua strategia di personalizzazione. AEM consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un&#39;unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo: l’AEM regola automaticamente ogni esperienza utilizzando il contenuto.

Target consente di fornire esperienze personalizzate in scala su una combinazione di approcci di apprendimento automatico basati sulle regole e guidati da intelligenze automatizzate che incorporano variabili comportamentali, contestuali e offline.  Con Target puoi facilmente configurare ed eseguire attività di test A/B e multivariati (MVT) per determinare le offerte, i contenuti e le esperienze migliori.

I frammenti di esperienza rappresentano un enorme passo avanti per collegare i creatori di contenuti con gli esperti di marketing che stanno guidando i risultati di business utilizzando Target.

## Panoramica dello scenario

Il sito WKND sta pianificando di annunciare una **sfida SkateFest** in tutta l&#39;America tramite il proprio sito Web e desidera che gli utenti del sito si iscrivano per l&#39;audizione condotta in ogni stato. In qualità di addetto al marketing, ti è stato assegnato il compito di eseguire una campagna nella home page del sito WKND, con messaggi di banner relativi alla posizione degli utenti e un collegamento alla pagina dei dettagli dell’evento. Esplora la home page del sito WKND e scopri come creare e fornire a un utente un’esperienza personalizzata in base alla sua posizione corrente.

### Utenti interessati

Per questo esercizio, è necessario coinvolgere i seguenti utenti e per eseguire alcune attività potrebbe essere necessario l&#39;accesso amministrativo.

* **Produttore contenuti/Editor contenuti** (Adobe Experience Manager)
* **Addetto marketing** (Adobe Target / Team di ottimizzazione)

### Prerequisiti

* **AEM**
   * [Istanza di creazione e pubblicazione AEM](./implementation.md#getting-aem) in esecuzione rispettivamente su localhost 4502 e 4503.
* **Experience Cloud**
   * Accesso al Adobe Experience Cloud delle organizzazioni - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

### Home page del sito WKND

![Scenario di destinazione AEM 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. L’addetto al marketing avvia la discussione sulla campagna WKND SkateFest con l’Editor di contenuti dell’AEM e ne descrive i requisiti.
   * ***Requisito***: promuovere la campagna WKND SkateFest nella home page del sito WKND con contenuti personalizzati per i visitatori di ogni stato degli Stati Uniti. Aggiungi un nuovo blocco di contenuto sotto il carosello della home page contenente un’immagine di sfondo, un testo e un pulsante.
      * **Immagine di sfondo**: l&#39;immagine deve essere rilevante per lo stato da cui l&#39;utente accede alla pagina del sito WKND.
      * **Testo**: &quot;Iscriviti alle Audition&quot;
      * **Pulsante**: &quot;Dettagli evento&quot; che punta alla pagina WKND SkateFest
      * **Pagina SkateFest WKND**: una nuova pagina con i dettagli dell&#39;evento, inclusi il luogo dell&#39;audizione, la data e l&#39;ora.
1. In base ai requisiti, l’Editor di contenuti AEM crea un frammento di esperienza per il blocco di contenuti e lo esporta in Adobe Target come offerta. Per distribuire contenuti personalizzati per tutti gli stati degli Stati Uniti, l’autore di contenuti può creare una variante principale Frammento esperienza e quindi altre 50 varianti, una per ogni stato. Il contenuto di ogni variante di stato con immagini e testo pertinenti può quindi essere modificato manualmente. Durante l’authoring di un frammento di esperienza, gli editor di contenuti possono accedere rapidamente a tutte le risorse disponibili in AEM Assets utilizzando l’opzione Asset Finder. Quando un frammento di esperienza viene esportato in Adobe Target, anche tutte le sue varianti vengono inviate ad Adobe Target come Offerte.

1. Dopo aver esportato il frammento di esperienza dall’AEM ad Adobe Target as Offers, gli esperti di marketing possono creare attività in Target utilizzando queste offerte. In base alla campagna SkateFest del sito WKND, l’addetto al marketing deve creare e fornire un’esperienza personalizzata ai visitatori del sito WKND da ogni stato. Per creare un’attività Targeting esperienza, l’addetto al marketing deve identificare i tipi di pubblico. Per la nostra campagna WKND SkateFest, dobbiamo creare 50 tipi di pubblico separati, in base alla loro posizione da cui visitano il sito web WKND.
   * [I tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=it#section_3F32DA46BDF947878DD79DBB97040D01) definiscono la destinazione dell&#39;attività e vengono utilizzati ovunque sia disponibile il targeting. I tipi di pubblico di destinazione sono un set definito di criteri per i visitatori. Le offerte possono essere indirizzate a tipi di pubblico (o segmenti) specifici. Solo i visitatori che appartengono a quel pubblico visualizzano l&#39;esperienza di destinazione.  Ad esempio, puoi inviare un’offerta a un pubblico composto da visitatori che utilizzano un browser particolare o da una geolocalizzazione specifica.
   * Una [Offerta](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=it#section_973D4CC4CEB44711BBB9A21BF74B89E9) è il contenuto che viene visualizzato nelle pagine Web durante le campagne o le attività. Quando esegui il test delle pagine web, misuri il successo di ogni esperienza con offerte diverse nelle tue posizioni. Un’offerta può contenere diversi tipi di contenuto, tra cui:
      * Immagine
      * Testo
      * **HTML**
         * *Le offerte HTML sono utilizzate per l&#39;attività di questo scenario*
      * Collegamento
      * Pulsante

## Attività editor contenuti

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Publish il frammento di esperienza prima di esportarlo in Adobe Target.

## Attività degli addetti marketing

### Creare un pubblico con il geotargeting {#marketer-audience}

1. Accedi alle tue organizzazioni [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Accedi con il tuo Adobe ID e accertati di essere nell’organizzazione corretta.
1. Dal commutatore della soluzione, fai clic su **Target** e quindi su **avvia** Adobe Target.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Passa alla scheda **Offerte** e cerca le offerte &quot;WKND&quot;. Dovresti essere in grado di visualizzare l’elenco delle varianti di Frammenti esperienza, esportate da AEM come Offerte HTML. Ogni offerta corrisponde a uno stato. Ad esempio, *WKND SkateFest California* è l&#39;offerta che viene distribuita a un visitatore del sito WKND dalla California.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Dalla navigazione principale, fai clic su **Tipi di pubblico**.

   Un addetto al marketing deve creare 50 tipi di pubblico separati per i visitatori del sito WKND provenienti da ogni stato degli Stati Uniti d’America.

1. Per creare un pubblico, fai clic sul pulsante **Crea pubblico** e fornisci un nome per il pubblico.

   **Formato nome pubblico: WKND-\&lt;*stato*\>**

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Fai clic su **Aggiungi regola > Geo**.
1. Fai clic su **Seleziona**, quindi seleziona una delle seguenti opzioni:
   * Paese
   * **Stato** *(Seleziona stato per la campagna SkateFest del sito WKND)*
   * Città
   * Codice postale
   * Latitudine
   * Longitudine
   * DMA
   * Operatore di telefonia mobile

   **Geo** - Utilizza i tipi di pubblico per eseguire il targeting degli utenti in base alla loro posizione geografica, compreso paese, stato/provincia, città, CAP, DMA o gestore di telefonia mobile. I parametri di geolocalizzazione consentono di eseguire il targeting di attività ed esperienze in base alla posizione geografica dei visitatori. Questi dati vengono inviati con ogni richiesta di Target e si basano sull’indirizzo IP del visitatore. Seleziona questi parametri come qualsiasi altro valore di targeting.

   >[!NOTE]
   >L’indirizzo IP di un visitatore viene trasmesso con una richiesta mbox, una volta per visita (sessione), per risolvere i parametri di geotargeting per quel visitatore.

1. Seleziona l&#39;operatore come **corrisponde**, specifica un valore appropriato (ad esempio: California) e **Salva** le modifiche. Nel nostro caso, fornisci il nome dello stato.

   ![Adobe Target- Regola geografica](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >A un pubblico possono essere assegnate più regole.

1. Ripeti i passaggi da 6 a 9 per creare tipi di pubblico per gli altri stati.

   ![Adobe Target - Tipi di pubblico WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

A questo punto, abbiamo creato con successo i tipi di pubblico per tutti i visitatori del sito WKND nei diversi stati degli Stati Uniti d’America e abbiamo anche l’offerta HTML corrispondente per ogni stato. Ora creiamo un’attività Targeting esperienza per indirizzare il pubblico con un’offerta corrispondente per la home page del sito WKND.

### Creare un’attività con il geotargeting

1. Dalla finestra di Adobe Target, passa alla scheda **Attività**.
1. Fai clic su **Crea attività** e seleziona il tipo di attività **Targeting esperienza**.
1. Seleziona il canale **Web** e scegli il **Compositore esperienza visivo**.
1. Immetti l&#39;**URL attività** e fai clic su **Avanti** per aprire il Compositore esperienza visivo.

   URL Publish della home page del sito WKND: http://localhost:4503/content/wknd/en.html

   ![Attività Targeting Esperienza](assets/personalization-use-case-1/target-activity.png)

1. Per il caricamento di **Visual Experience Composer**, abilita **Allow Load Unsafe scripts** nel browser e ricarica la pagina.

   ![Attività Targeting Esperienza](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Nell’editor del Compositore esperienza visivo viene aperta la pagina Home del sito WKND.

   ![Compositore esperienza visivo](assets/personalization-use-case-1/vec.png)

1. Per aggiungere un pubblico al Compositore esperienza visivo, fai clic su **Aggiungi targeting esperienza** in Pubblico, quindi seleziona il pubblico WKND-California e fai clic su **Avanti**.

   ![Compositore esperienza visivo](assets/personalization-use-case-1/vec-select-audience.png)

1. Fai clic sulla pagina del sito WKND all&#39;interno del Compositore esperienza visivo, seleziona l&#39;elemento HTML per aggiungere l&#39;offerta per il pubblico WKND-California, quindi scegli l&#39;opzione **Sostituisci con** e seleziona l&#39;offerta **HTML**.

   ![Attività Targeting Esperienza](assets/personalization-use-case-1/vec-selecting-div.png)

1. Seleziona l&#39;offerta HTML **WKND SkateFest California** per il pubblico **WKND-California** dall&#39;interfaccia utente di selezione dell&#39;offerta e fai clic su **Fine**.
1. Ora dovresti essere in grado di visualizzare l&#39;offerta HTML **WKND SkateFest California** aggiunta alla pagina del sito WKND per il pubblico WKND-California.
1. Ripeti i passaggi 7-10 per aggiungere Targeting esperienza per gli altri stati e scegli l’offerta HTML corrispondente.
1. Fai clic su **Successivo** per continuare ed è possibile visualizzare una mappatura per Tipi di pubblico su Esperienze.
1. Fai clic su **Avanti** per passare a Obiettivi e impostazioni.
1. Scegli l’origine per la generazione di rapporti e identifica un obiettivo principale per l’attività. Per il nostro scenario, selezioniamo il Source di reporting come **Adobe Target**, misurando l&#39;attività come **Conversione**, l&#39;azione come visualizzata in una pagina e l&#39;URL che punta alla pagina WKND SkateFest Details.

   ![Obiettivo e destinazione - Destinazione](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Puoi anche scegliere Adobe Analytics come origine per la generazione di rapporti.

1. Passa il puntatore del mouse sul nome dell&#39;attività corrente per rinominarlo in **WKND SkateFest - Stati Uniti**, quindi **Salva e chiudi** le modifiche.
1. Dalla schermata dei dettagli dell&#39;attività, assicurati di **attivare** l&#39;attività.

   ![Attiva attività](assets/personalization-use-case-1/activate-activity.png)

1. La tua campagna WKND SkateFest è ora live per tutti i visitatori del sito WKND.
1. Passa alla [home page del sito WKND](http://localhost:4503/content/wknd/en.html) e dovresti essere in grado di visualizzare l&#39;offerta WKND SkateFest in base alla tua posizione geografica (*stato: California*).

   ![Controllo qualità attività](assets/personalization-use-case-1/wknd-california.png)

### Controllo di qualità attività di Target

1. Nella scheda **Dettagli attività > Panoramica**, fai clic sul pulsante **Controllo di qualità attività** per ottenere il collegamento diretto per il controllo qualità di tutte le esperienze.

   ![Controllo qualità attività](assets/personalization-use-case-1/activity-qa.png)

1. Passa alla [home page del sito WKND](http://localhost:4503/content/wknd/en.html) e dovresti essere in grado di visualizzare l&#39;offerta SkateFest WKND in base alla geolocalizzazione (stato).
1. Guarda il video seguente per comprendere come viene distribuita un’offerta alla pagina, come personalizzare i token di risposta ed eseguire un controllo di qualità.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Riepilogo

In questo capitolo, un editor di contenuti è stato in grado di creare tutti i contenuti per supportare la campagna WKND SkateFest all’interno di Adobe Experience Manager ed esportarli in Adobe Target as HTML Offers, per la creazione di Experience Targeting, in base alla geolocalizzazione degli utenti.
