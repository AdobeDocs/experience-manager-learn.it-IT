---
title: Personalizzazione tramite  Adobe Target
seo-title: Personalizzazione tramite  Adobe Target
description: Un'esercitazione completa che mostra come creare e distribuire esperienze personalizzate utilizzando  Adobe Target.
seo-description: Un'esercitazione completa che mostra come creare e distribuire esperienze personalizzate utilizzando  Adobe Target.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 2%

---


# Personalizzazione di esperienze con pagine Web complete tramite  Adobe Target

Nel capitolo precedente, abbiamo imparato a creare un&#39;attività basata sulla geolocalizzazione in  Adobe Target utilizzando il contenuto creato come frammenti esperienza ed esportato da AEM come offerte HTML.

In questo capitolo, esploreremo la creazione di attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando  Adobe Target.

## Panoramica scenario

Il sito WKND ha riprogettato la propria pagina principale e desidera reindirizzare i visitatori della pagina principale corrente alla nuova pagina principale. Allo stesso tempo, puoi comprendere anche in che modo la nuova home page consente di migliorare il coinvolgimento e le entrate degli utenti. In qualità di esperto di marketing, vi è stata assegnata l&#39;attività di creazione di un&#39;attività per reindirizzare i visitatori alla nuova home page. Esaminate la home page del sito WKND e scoprite come creare un&#39;attività utilizzando  Adobe Target.

### Utenti coinvolti

Per questo esercizio, è necessario coinvolgere i seguenti utenti e per eseguire alcune attività potrebbe essere necessario l&#39;accesso amministrativo.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketing** ( Adobe Target / Gruppo di ottimizzazione)

### Home page sito WKND

![AEM Target 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Prerequisiti

* **AEM**
   * [AEM istanza](./implementation.md#getting-aem) di creazione e pubblicazione eseguita rispettivamente su localhost 4502 e 4503.
   * [AEM integrato con  Adobe Target mediante  Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experienceecCloud.adobe.com
   *  Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

## Attività Editor contenuto

1. L’addetto al marketing avvia la discussione sulla riprogettazione della pagina principale WKND con AEM Content Editor e descrive dettagliatamente i requisiti.
   * ***Requisito*** : Riprogettare la home page del sito WKND con un design basato su schede.
2. In base ai requisiti, AEM Content Editor crea quindi una nuova home page del sito WKND con un design basato su scheda e pubblica la nuova home page.

## Attività di marketing

1. L&#39;esperto di marketing crea un&#39;attività target A/B con l&#39;offerta di reindirizzamento come Esperienza e il traffico del sito Web al 100% assegnato alla nuova pagina principale con l&#39;aggiunta dell&#39;obiettivo di successo e delle metriche.
   1. Dalla finestra  Adobe Target, passare alla scheda **Attività** .
   2. Fate clic sul pulsante **Crea attività** e selezionate il tipo di attività come test **A/B**

      ![Adobe Target - Crea attività](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selezionate il canale **Web** e scegliete **Visual Experience Composer (Compositore esperienza visivo)**.
   4. Immettete l&#39;URL **dell&#39;** attività e fate clic su **Avanti** per aprire Visual Experience Composer (Compositore esperienza visivo).
      ![Adobe Target - Crea attività](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Affinché **Visual Experience Composer (Compositore esperienza visivo) venga caricato** , abilitate **Consenti caricamento di script** non sicuri nel browser e ricaricate la pagina.
      ![Attività di targeting delle esperienze](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Notate che la home page del sito WKND si apre nell&#39;editor di Visual Experience Composer (Compositore esperienza visivo).
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Passa il puntatore del mouse sull&#39; **Esperienza B** e seleziona Visualizza altre opzioni.
      ![Esperienza B](assets/personalization-use-case-2/redirect-url.png)
   8. Selezionate l’opzione **Reindirizza a URL** e immettete l’URL per la nuova home page WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Esperienza B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Salvare** le modifiche e continuare con i passaggi successivi della creazione di attività.
   10. Selezionate il metodo **di allocazione del** traffico come manuale e assegnate il traffico al 100% all&#39; **Esperienza B**.
      ![Experience B Traffic](assets/personalization-use-case-2/traffic.png)
   11. Fai clic su **Avanti**.
   12. Fornite le metriche **** degli obiettivi per l&#39;attività e salvate e chiudete il test A/B.
      ![Metrica obiettivo test A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Specificate un nome (**WKND Home Page Redesign**) per l&#39;attività e salvate le modifiche.
   14. Dalla schermata Dettagli attività, accertatevi di **attivare** l&#39;attività.
      ![Attiva attività](assets/personalization-use-case-2/ab-activate.png)
   15. Passate alla home page WKND (http://localhost:4503/content/wknd/en.html) e verrete reindirizzati alla home page del sito WKND riprogettata (http://localhost:4503/content/wknd/en1.html).
      ![Home page WKND riprogettata](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Riepilogo

In questo capitolo, un esperto di marketing è stato in grado di creare un&#39;attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando  Adobe Target.
