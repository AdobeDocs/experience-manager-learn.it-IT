---
title: Personalizzazione tramite frammenti di esperienza AEM e Adobe Target
seo-title: Personalization using Adobe Experience Manager (AEM) Experience Fragments and Adobe Target
description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando Frammenti esperienza Adobe Experience Manager e Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Experience Manager Experience Fragments and Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1691'
ht-degree: 1%

---

# Personalizzazione tramite frammenti di esperienza AEM e Adobe Target

Grazie alla possibilità di esportare AEM Frammenti esperienza in Adobe Target come offerte di HTML, puoi combinare la facilità d’uso e la potenza del AEM con le potenti funzionalità di intelligenza automatizzata (AI) e apprendimento automatico (ML) di Target per testare e personalizzare le esperienze su larga scala.

AEM riunisce tutti i contenuti e le risorse in una posizione centrale per alimentare la tua strategia di personalizzazione. AEM consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un’unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo: AEM regola automaticamente ogni esperienza utilizzando il contenuto.

Target ti consente di fornire esperienze personalizzate su scala basata su una combinazione di approcci di apprendimento automatico basati su regole e basati su intelligenza artificiale che incorporano variabili comportamentali, contestuali e offline.  Con Target puoi facilmente impostare ed eseguire attività A/B e multivariate (MVT) per determinare le offerte, i contenuti e le esperienze migliori.

I frammenti di esperienza rappresentano un enorme passo in avanti per collegare i creatori di contenuti con gli addetti al marketing che utilizzano Target per conseguire risultati di business.

## Panoramica dello scenario

Il sito WKND prevede di annunciare un **Sfida SkateFest** in tutta l&#39;America attraverso il loro sito web e vorrebbe che i loro utenti del sito si iscrivano per l&#39;audizione condotta in ogni stato. In qualità di addetto al marketing, ti è stata assegnata l’attività di eseguire una campagna nella home page del sito WKND, con messaggi di banner relativi alla posizione degli utenti e un collegamento alla pagina dei dettagli dell’evento. Esploriamo la home page del sito WKND e impariamo a creare e distribuire un’esperienza personalizzata per un utente in base alla sua posizione corrente.

### Utenti coinvolti

Per questo esercizio è necessario coinvolgere i seguenti utenti ed eseguire alcune attività, potrebbe essere necessario un accesso amministrativo.

* **Content Producer / Editor di contenuti** (Adobe Experience Manager)
* **Addetto marketing** (Adobe Target / Team di ottimizzazione)

### Prerequisiti

* **AEM**
   * [AEM istanza di authoring e pubblicazione](./implementation.md#getting-aem) in esecuzione rispettivamente su localhost 4502 e 4503.
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

### Home page sito WKND

![Scenario AEM Target 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. L’addetto al marketing avvia la discussione sulla campagna WKND SkateFest con AEM Content Editor e ne descrive i requisiti.
   * ***Requisito***: Promuovi la campagna WKND SkateFest sulla home page del sito WKND con contenuti personalizzati per i visitatori di ogni stato negli Stati Uniti. Aggiungi un nuovo blocco di contenuto sotto il carosello Home Page contenente un’immagine di sfondo, un testo e un pulsante.
      * **Immagine di sfondo**: L’immagine deve essere pertinente allo stato da cui l’utente sta visitando la pagina del sito WKND.
      * **Testo**: &quot;Iscriviti alle Audition&quot;
      * **Pulsante**: &quot;Dettagli evento&quot; che punta alla pagina WKND SkateFest
      * **Pagina SkateFest WKND**: una nuova pagina con i dettagli dell’evento, inclusa la sede dell’audizione, la data e l’ora.
1. In base ai requisiti, AEM Editor di contenuto crea un frammento esperienza per il blocco di contenuto ed lo esporta in Adobe Target come offerta. Per distribuire contenuti personalizzati per tutti gli stati degli Stati Uniti, l’autore del contenuto può creare una variante principale del frammento esperienza e quindi creare altre 50 varianti, una per ogni stato. Il contenuto di ogni variante di stato con immagini e testo pertinenti può quindi essere modificato manualmente. Quando crei un frammento esperienza, gli editor di contenuti possono accedere rapidamente a tutte le risorse disponibili in AEM Assets utilizzando l’opzione Asset Finder. Quando un frammento esperienza viene esportato in Adobe Target, anche tutte le sue varianti vengono inviate ad Adobe Target come offerte.

1. Dopo aver esportato un frammento esperienza da AEM ad Adobe Target come offerte, gli addetti al marketing possono creare un’attività in Target utilizzando queste offerte. In base alla campagna SkateFest del sito WKND, l’addetto al marketing deve creare e fornire un’esperienza personalizzata ai visitatori del sito WKND da ogni stato. Per creare un’attività Targeting esperienze, l’addetto al marketing deve identificare i tipi di pubblico. Per la nostra campagna WKND SkateFest, dobbiamo creare 50 tipi di pubblico separati, in base alla loro posizione da cui stanno visitando il sito web WKND.
   * [Tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) definisci il target per l’attività e viene utilizzato ovunque sia disponibile il targeting. I tipi di pubblico di Target sono un insieme definito di criteri per i visitatori. Le offerte possono essere indirizzate a tipi di pubblico specifici (o segmenti). Solo i visitatori che appartengono a quel pubblico visualizzano l’esperienza a loro destinata.  Ad esempio, puoi offrire un’offerta a un pubblico composto da visitatori che utilizzano un particolare browser o da una specifica geolocalizzazione.
   * Un [Offerta](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) è il contenuto che viene visualizzato sulle pagine web durante campagne o attività. Quando sottoponi a test le pagine web, misuri il successo di ogni esperienza con offerte diverse nelle tue posizioni. Un’offerta può contenere diversi tipi di contenuto, tra cui:
      * Immagine
      * Testo
      * **HTML**
         * *Le offerte HTML vengono utilizzate per l’attività di questo scenario*
      * Collegamento
      * Pulsante

## Attività dell’editor di contenuti

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Pubblica il frammento esperienza prima di esportarlo in Adobe Target.

## Attività di marketing

### Creare un pubblico con il geotargeting {#marketer-audience}

1. Passa alle organizzazioni [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Accedi utilizzando il tuo Adobe ID e assicurati di essere nell&#39;organizzazione corretta.
1. Dal commutatore della soluzione, fai clic su **Target** e poi **lanciare** Adobe Target.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Passa a **Offerte** e cerca le offerte &quot;WKND&quot;. Dovresti essere in grado di visualizzare l’elenco delle varianti Frammenti esperienza, esportate da AEM come Offerte HTML. Ogni offerta corrisponde a uno stato. Ad esempio: *WKND SkateFest California* è l’offerta che viene fornita a un visitatore del sito WKND dalla California.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Dalla navigazione principale, fai clic su **Tipi di pubblico**.

   Un addetto al marketing deve creare 50 tipi di pubblico separati per i visitatori del sito WKND provenienti da ogni stato degli Stati Uniti d’America.

1. Per creare un pubblico, fai clic su **Crea pubblico** e specifica un nome per il pubblico.

   **Formato del nome del pubblico : WKND-\&lt;*stato*\>**

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Fai clic su **Aggiungi regola > Geo**.
1. Fai clic su **Seleziona**, quindi seleziona una delle seguenti opzioni:
   * Paese
   * **Stato** *(Selezionare lo stato per la campagna SkateFest del sito WKND)*
   * Città
   * Codice postale
   * Latitudine
   * Longitudine
   * DMA
   * Operatore di telefonia mobile

   **Geo** - Utilizza i tipi di pubblico per indirizzare l’attività agli utenti in base alla loro posizione geografica, compreso paese, stato/provincia, città, CAP, DMA o gestore di telefonia mobile. I parametri di geolocalizzazione consentono di eseguire il targeting di attività ed esperienze in base alla posizione geografica dei visitatori. Questi dati vengono inviati con ogni richiesta Target e si basano sull&#39;indirizzo IP del visitatore. Seleziona questi parametri come qualsiasi altro valore di targeting.

   >[!NOTE]
   >L’indirizzo IP di un visitatore viene trasmesso con una richiesta mbox, una volta per visita (sessione), per risolvere i parametri di geotargeting per quel visitatore.

1. Seleziona l’operatore come **corrisponde**, fornire un valore appropriato (ad esempio: California) e **Salva** le modifiche. Nel nostro caso, fornisci il nome dello stato.

   ![Adobe Target - Regola Geo](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >Puoi assegnare più regole a un pubblico.

1. Ripeti i passaggi 6-9 per creare tipi di pubblico per gli altri stati.

   ![Adobe Target - Tipi di pubblico WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

A questo punto, abbiamo creato con successo tipi di pubblico per tutti i visitatori del sito WKND in diversi stati degli Stati Uniti d’America e abbiamo anche l’offerta HTML corrispondente per ogni stato. Ora creiamo un’attività Targeting esperienza per indirizzare il pubblico con un’offerta corrispondente per la home page del sito WKND.

### Creare un’attività con il geotargeting

1. Dalla finestra di Adobe Target, passa a **Attività** scheda .
1. Fai clic su **Crea attività** e seleziona la **Targeting esperienza** tipo di attività.
1. Seleziona la **Web** e scegli il **Compositore esperienza visivo**.
1. Inserisci il **URL attività** e fai clic su **Successivo** per aprire il Compositore esperienza visivo.

   URL di pubblicazione della home page del sito WKND: http://localhost:4503/content/wknd/en.html

   ![Attività Targeting esperienze](assets/personalization-use-case-1/target-activity.png)

1. Per **Compositore esperienza visivo** per caricare, abilita **Consenti caricamento script non sicuri** nel browser e ricarica la pagina.

   ![Attività Targeting esperienze](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Osserva la home page del sito WKND aperta nell’editor del Compositore esperienza visivo.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Per aggiungere un pubblico al Compositore esperienza visivo, fai clic su **Aggiungi targeting esperienza** in Audiences, quindi seleziona il pubblico WKND-California e fai clic su **Successivo**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Fai clic sulla pagina del sito WKND all’interno del Compositore esperienza visivo, seleziona l’elemento HTML per aggiungere l’offerta per il pubblico WKND-California, quindi scegli **Sostituisci con** quindi seleziona l’opzione **Offerta HTML**.

   ![Attività Targeting esperienze](assets/personalization-use-case-1/vec-selecting-div.png)

1. Seleziona la **WKND SkateFest California** offerta HTML per **WKND-California** pubblico dall’interfaccia utente selezionata dell’offerta e fai clic su **Fine**.
1. Ora dovresti essere in grado di vedere il **WKND SkateFest California** Offerta HTML aggiunta alla pagina del sito WKND per il pubblico WKND-California.
1. Ripeti i passaggi da 7 a 10 per aggiungere il targeting delle esperienze per gli altri stati e scegli l’offerta HTML corrispondente.
1. Fai clic su **Successivo** per continuare, puoi vedere una mappatura per tipi di pubblico per esperienze.
1. Fai clic su **Successivo** per passare a Obiettivi e impostazioni.
1. Scegli l’origine per la generazione di rapporti e identifica un obiettivo principale per l’attività. Per il nostro scenario, selezioniamo l’origine per la generazione di rapporti come **Adobe Target**, misura attività come **Conversione**, azione visualizzata come pagina e URL che punta alla pagina WKND SkateFest Details (Dettagli SkateFest).

   ![Obiettivo e targeting - Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Puoi anche scegliere Adobe Analytics come origine per la generazione di rapporti.

1. Passa il puntatore del mouse sul nome dell&#39;attività corrente e lo puoi rinominare in **WKND SkateFest - USA** e quindi **Salva e chiudi** le modifiche.
1. Dalla schermata dei dettagli dell’attività, assicurati di: **Attiva** la tua attività.

   ![Attiva attività](assets/personalization-use-case-1/activate-activity.png)

1. La campagna WKND SkateFest è ora live per tutti i visitatori del sito WKND.
1. Passa a [Home page sito WKND](http://localhost:4503/content/wknd/en.html), e dovresti essere in grado di visualizzare l’offerta WKND SkateFest in base alla tua geolocalizzazione (*stato: California*).

   ![Controllo di qualità delle attività](assets/personalization-use-case-1/wknd-california.png)

### Controllo di qualità delle attività di Target

1. Sotto **Dettagli attività > Panoramica** , fai clic su **Controllo di qualità delle attività** e puoi ottenere il collegamento di controllo qualità diretto a tutte le tue esperienze.

   ![Controllo di qualità delle attività](assets/personalization-use-case-1/activity-qa.png)

1. Passa a [Home page sito WKND](http://localhost:4503/content/wknd/en.html), e dovresti essere in grado di visualizzare l’offerta WKND SkateFest in base alla tua geolocalizzazione (stato).
1. Guarda il video seguente per comprendere come un’offerta viene consegnata alla tua pagina, come personalizzare i token di risposta ed eseguire un controllo di qualità.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Riepilogo

In questo capitolo, un editor di contenuti è stato in grado di creare tutti i contenuti per supportare la campagna WKND SkateFest in Adobe Experience Manager ed esportarla in Adobe Target as HTML Offers, per creare Experience Targeting, in base alla geolocalizzazione degli utenti.
