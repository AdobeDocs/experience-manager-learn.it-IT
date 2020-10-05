---
title: Personalizzazione mediante AEM frammenti esperienza e  Adobe Target
seo-title: Personalizzazione mediante frammenti esperienza Adobe Experience Manager (AEM) e  Adobe Target
description: Un'esercitazione completa che mostra come creare e distribuire esperienze personalizzate utilizzando i frammenti esperienza Adobe Experience Manager e  Adobe Target.
seo-description: Un'esercitazione completa che mostra come creare e distribuire esperienze personalizzate utilizzando i frammenti esperienza Adobe Experience Manager e  Adobe Target.
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 1%

---


# Personalizzazione mediante AEM frammenti esperienza e  Adobe Target

Grazie alla possibilità di esportare AEM frammenti esperienza in  Adobe Target come offerte HTML, è possibile combinare la facilità d&#39;uso e la potenza dei AEM con le potenti funzionalità di Automated Intelligence (AI) e Machine Learning (ML) di Target per testare e personalizzare le esperienze su scala.

AEM riunisce tutti i contenuti e le risorse in una posizione centrale per rafforzare la strategia di personalizzazione. AEM consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un&#39;unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo; AEM regola automaticamente ogni esperienza utilizzando il contenuto.

Target consente di fornire esperienze personalizzate su scala basata su una combinazione di approcci di machine learning basati su regole e basati su AI che includono variabili comportamentali, contestuali e offline.  Con Target potete facilmente impostare ed eseguire attività A/B e multivariato (MVT) per determinare le migliori offerte, contenuti ed esperienze.

I frammenti esperienza rappresentano un enorme passo avanti verso il collegamento tra i creatori di contenuti e gli esperti di marketing che stanno portando avanti il business utilizzando Target.

## Panoramica scenario

Il sito WKND sta pianificando di annunciare una **SkateFest Challenge** in America attraverso il loro sito web e vorrebbe che i loro utenti del sito si iscrivano per l&#39;audizione condotta in ogni stato. Come esperto di marketing, vi è stata assegnata l&#39;attività di eseguire una campagna sulla home page del sito WKND, con messaggi di banner relativi alla posizione degli utenti e un collegamento alla pagina dei dettagli dell&#39;evento. Esplora la home page del sito WKND e scopri come creare e distribuire un’esperienza personalizzata per un utente in base alla sua posizione corrente.

### Utenti coinvolti

Per questo esercizio, è necessario coinvolgere i seguenti utenti e per eseguire alcune attività potrebbe essere necessario l&#39;accesso amministrativo.

* **Content Producer / Content Editor** (Adobe Experience Manager)
* **Marketing** ( Adobe Target / Gruppo di ottimizzazione)

### Prerequisiti

* **AEM**
   * [AEM istanza](./implementation.md#getting-aem) di creazione e pubblicazione eseguita rispettivamente su localhost 4502 e 4503.
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experienceecCloud.adobe.com
   *  Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

### Home page sito WKND

![AEM Target 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. Marketer avvia la discussione sulla campagna WKND SkateFest con AEM Content Editor e specifica i requisiti.
   * ***Requisito***: Promuovere la campagna WKND SkateFest sulla home page del sito WKND con contenuti personalizzati per i visitatori provenienti da ogni stato negli Stati Uniti. Aggiungete un nuovo blocco di contenuto sotto il carosello Home Page contenente un&#39;immagine di sfondo, un testo e un pulsante.
      * **Immagine** di sfondo: L’immagine deve essere pertinente allo stato dal quale l’utente accede alla pagina del sito WKND.
      * **Testo**: &quot;Iscriviti alle Audition &quot;
      * **Pulsante**: &quot;Dettagli evento&quot; che indica la pagina WKND SkateFest
      * **Pagina** SkateFest WKND: una nuova pagina con i dettagli dell’evento, compresa la sede, la data e l’ora dell’audizione.
1. In base ai requisiti, AEM Content Editor crea un frammento esperienza per il blocco di contenuto ed lo esporta in  Adobe Target come offerta. Per distribuire contenuti personalizzati per tutti gli stati degli Stati Uniti, l’autore del contenuto può creare una variante principale del frammento esperienza e quindi creare altre 50 varianti, una per ciascuno stato. Il contenuto per ogni variazione di stato con immagini e testo rilevanti può essere modificato manualmente. Durante la creazione di un frammento esperienza, gli editor di contenuti possono accedere rapidamente a tutte le risorse disponibili in  AEM Assets tramite l&#39;opzione Asset Finder. Quando un frammento esperienza viene esportato in  Adobe Target, anche tutte le sue varianti vengono inviate  Adobe Target come offerte.

1. Dopo aver esportato un frammento esperienza da AEM a  Adobe Target come offerte, gli addetti al marketing possono creare un&#39;attività in Target utilizzando queste offerte. Sulla base della campagna SkateFest del sito WKND, l&#39;esperto di marketing deve creare e fornire un&#39;esperienza personalizzata ai visitatori del sito WKND da ogni stato. Per creare un&#39;attività Experience Targeting (Targeting dell&#39;esperienza), l&#39;esperto di marketing deve identificare i tipi di pubblico. Per la nostra campagna WKND SkateFest, dobbiamo creare 50 audience separate, in base alla loro posizione da cui stanno visitando il sito web WKND.
   * [Le audience](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) definiscono il target per l&#39;attività e vengono utilizzate ovunque il targeting sia disponibile. Le audience di destinazione sono un insieme definito di criteri per i visitatori. Le offerte possono essere indirizzate a audience specifiche (o segmenti). Solo i visitatori che appartengono a tale audience visualizzano l&#39;esperienza con targeting per loro.  Ad esempio, potete fornire un&#39;offerta a un pubblico composto da visitatori che utilizzano un particolare browser o da una specifica geolocalità.
   * Un&#39; [offerta](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) è il contenuto che viene visualizzato sulle pagine Web durante campagne o attività. Quando sottoponete a test le pagine Web, misurate il successo di ogni esperienza con offerte diverse nelle posizioni. Un&#39;offerta può contenere diversi tipi di contenuto, tra cui:
      * Immagine
      * Testo
      * **HTML**
         * *Le offerte HTML verranno utilizzate per l&#39;attività di questo scenario*
      * Collegamento
      * Pulsante

## Attività Editor contenuto

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Pubblicate il frammento esperienza prima di esportarlo in  Adobe Target.

## Attività di marketing

### Creare un pubblico con il geotargeting {#marketer-audience}

1. Passa a [Adobe Experience Cloud](https://experiencecloud.adobe.com/) organizzazione (<https://>`<yourcompany>`.experienceecCloud.adobe.com)
1. Accedete utilizzando il vostro Adobe ID  e accertatevi di essere nell&#39;organizzazione corretta.
1. Dallo switcher della soluzione, fai clic su **Target** , quindi **avvia**  Adobe Target.

   ![Experience Cloud  -  Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Andate alla scheda **Offerte** e cercate le offerte &quot;WKND&quot;. Dovrebbe essere possibile visualizzare l&#39;elenco delle varianti dei frammenti esperienza, esportate da AEM come offerte HTML. Ogni offerta corrisponde a uno stato. Ad esempio, *WKND SkateFest California* è l&#39;offerta che viene servita a un visitatore del sito WKND dalla California.

   ![Experience Cloud  -  Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Dalla navigazione principale, fate clic su **Audiences**.

   Un esperto di marketing deve creare 50 tipi di pubblico separati per i visitatori del sito WKND provenienti da ogni stato degli Stati Uniti.

1. Per creare un pubblico, fate clic sul pulsante **Crea pubblico** e fornite un nome per il pubblico.

   **Formato Nome audience: WKND-\&lt;*state*\>**

   ![Experience Cloud  -  Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Fate clic su **Aggiungi regola > Geo**.
1. Fate clic su **Seleziona**, quindi selezionate una delle seguenti opzioni:
   * Paese
   * **Stato** *(selezionare lo stato per la campagna SkateFest del sito WKND)*
   * Città
   * CAP
   * Latitudine
   * Longitudine
   * DMA
   * Mobile Carrier

   **Geo** - Utilizzate i tipi di pubblico per eseguire il targeting degli utenti in base alla loro posizione geografica, inclusi paese, stato/provincia, città, CAP, DMA o vettore mobile. I parametri di geolocalizzazione consentono di eseguire il targeting delle attività e delle esperienze in base alla geografia dei visitatori. Questi dati vengono inviati con ogni richiesta Target e si basano sull&#39;indirizzo IP del visitatore. Selezionate questi parametri come qualsiasi altro valore di targeting.

   >[!NOTE]
   >L&#39;indirizzo IP di un visitatore viene passato con una richiesta mbox, una volta per visita (sessione), per risolvere i parametri di geotargeting per quel visitatore.

1. Selezionare l&#39;operatore come **corrisponde**, fornire un valore appropriato (ad esempio: California) e **salvare** le modifiche. Nel nostro caso, immettete il nome dello stato.

   ![Adobe Target - Regola Geo](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >Potete assegnare più regole a un&#39;audience.

1. Ripetete i passaggi da 6 a 9 per creare audience per gli altri stati.

   ![Adobe Target - Pubblico WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

A questo punto, abbiamo creato con successo audience per tutti i visitatori del sito WKND nei diversi stati degli Stati Uniti d&#39;America e abbiamo anche la corrispondente offerta HTML per ogni stato. Quindi ora create un&#39;attività Experience Targeting (Impostazione destinazione dell&#39;esperienza) per indirizzare il pubblico con un&#39;offerta corrispondente per la home page del sito WKND.

### Creazione di un&#39;attività con il geotargeting

1. Dalla finestra  Adobe Target, passare alla scheda **Attività** .
1. Fate clic su **Crea attività** e selezionate il tipo di attività **Targeting** esperienza.
1. Selezionate il canale **Web** e scegliete **Visual Experience Composer (Compositore esperienza visivo)**.
1. Immettete l&#39;URL **dell&#39;** attività e fate clic su **Avanti** per aprire Visual Experience Composer (Compositore esperienza visivo).

   URL pubblicazione home page sito WKND: http://localhost:4503/content/wknd/en.html

   ![Attività di targeting delle esperienze](assets/personalization-use-case-1/target-activity.png)

1. Affinché **Visual Experience Composer (Compositore esperienza visivo) venga caricato** , abilitate **Consenti caricamento di script** non sicuri nel browser e ricaricate la pagina.

   ![Attività di targeting delle esperienze](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Notate che la home page del sito WKND si apre nell&#39;editor di Visual Experience Composer (Compositore esperienza visivo).

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Per aggiungere un&#39;audience al VEC, fai clic su **Aggiungi targeting** esperienza in Audiences, quindi seleziona l&#39;audience WKND-California e fai clic su **Next (Avanti)**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Fate clic sulla pagina del sito WKND all&#39;interno di VEC, selezionate l&#39;elemento HTML per aggiungere l&#39;offerta per il pubblico WKND-California, quindi scegliete **Sostituisci con** opzione e selezionate l&#39;offerta **** HTML.

   ![Attività di targeting delle esperienze](assets/personalization-use-case-1/vec-selecting-div.png)

1. Selezionate l&#39;offerta HTML **WKND SkateFest California** per il pubblico **WKND-California** dall&#39;interfaccia utente selezionata dell&#39;offerta e fate clic su **Fine**.
1. A questo punto dovreste essere in grado di vedere l&#39;offerta HTML **WKND SkateFest California** Offer aggiunto alla vostra pagina WKND Site per il pubblico WKND-California.
1. Ripetete i passaggi da 7 a 10 per aggiungere il targeting delle esperienze per gli altri stati e scegliete l’offerta HTML corrispondente.
1. Fai clic su **Avanti** per continuare e puoi vedere una mappatura per Audience a Esperienze.
1. Fai clic su **Avanti** per passare a Obiettivi e impostazioni.
1. Scegliete l&#39;origine di reporting e identificate un obiettivo principale per l&#39;attività. Per il nostro scenario, selezioniamo l&#39;origine di reporting come **Adobe Target**, misurando l&#39;attività come **Conversione**, l&#39;azione come visualizzata una pagina e l&#39;URL che indica la pagina Dettagli SkateFest WKND.

   ![Obiettivo e targeting - Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Potete anche scegliere  Adobe Analytics come origine di reporting.

1. Passate il puntatore del mouse sul nome dell&#39;attività corrente e rinominatela in **WKND SkateFest - USA**, quindi **salvate e chiudete** le modifiche.
1. Dalla schermata Dettagli attività, accertatevi di **attivare** l&#39;attività.

   ![Attiva attività](assets/personalization-use-case-1/activate-activity.png)

1. La tua campagna SkateFest WKND ora è live per tutti i visitatori del sito WKND.
1. Andate alla home page [del sito](http://localhost:4503/content/wknd/en.html)WKND e dovreste essere in grado di visualizzare l&#39;offerta SkateFest WKND in base alla vostra geolocalità (*stato: California*).

   ![QA attività](assets/personalization-use-case-1/wknd-california.png)

### QA attività Target

1. In Dettagli **attività > scheda Panoramica** , fate clic sul pulsante QA **** attività e potete ottenere il collegamento QA diretto a tutte le esperienze.

   ![QA attività](assets/personalization-use-case-1/activity-qa.png)

1. Andate alla home page [del sito](http://localhost:4503/content/wknd/en.html)WKND e dovreste essere in grado di visualizzare l&#39;offerta SkateFest WKND in base alla geolocalità (stato).
1. Guardate il video seguente per comprendere in che modo un’offerta viene trasmessa alla pagina, come personalizzare i token di risposta ed eseguire un controllo di qualità.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Riepilogo

In questo capitolo, un editor di contenuti è stato in grado di creare tutto il contenuto per supportare la campagna WKND SkateFest in Adobe Experience Manager ed esportarlo in Adobe Target  come offerte HTML, per creare Experience Targeting, in base alla geolocalizzazione degli utenti.
