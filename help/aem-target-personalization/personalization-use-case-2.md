---
title: Personalization con Adobe Target
description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 1%

---

# Personalization di esperienze di pagine web complete con Adobe Target

Nel capitolo precedente, abbiamo imparato a creare un’attività basata su geolocalizzazione all’interno di Adobe Target utilizzando contenuti creati come frammenti di esperienza ed esportati da AEM come offerte HTML.

In questo capitolo verrà esplorata la creazione di attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.

## Panoramica dello scenario

Il sito WKND ha riprogettato la propria home page e desidera reindirizzare i visitatori della home page corrente alla nuova home page. Allo stesso tempo, scopri anche come la home page riprogettata consente di migliorare il coinvolgimento degli utenti e i ricavi. In qualità di addetto al marketing, ti è stato assegnato il compito di creare un’attività per reindirizzare i visitatori alla nuova home page. Esplora la home page del sito WKND e scopri come creare un’attività utilizzando Adobe Target.

### Utenti interessati

Per questo esercizio, è necessario coinvolgere i seguenti utenti e per eseguire alcune attività potrebbe essere necessario l&#39;accesso amministrativo.

* **Produttore contenuti/Editor contenuti** (Adobe Experience Manager)
* **Addetto marketing** (Adobe Target / Team di ottimizzazione)

### Home page del sito WKND

![Scenario di destinazione AEM 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Prerequisiti

* **AEM**
   * [Istanza di creazione e pubblicazione AEM](./implementation.md#getting-aem) in esecuzione rispettivamente su localhost 4502 e 4503.
   * [AEM integrato con Adobe Target tramite tag](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accesso al Adobe Experience Cloud delle organizzazioni - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

## Attività dell’editor di contenuti

1. L’addetto al marketing avvia la discussione sulla riprogettazione della home page WKND con l’editor di contenuti AEM e ne descrive i requisiti.
   * ***Requisito***: riprogettare la home page del sito WKND con una progettazione basata su schede.
2. In base ai requisiti, AEM Content Editor crea quindi una nuova home page del sito WKND con una progettazione basata su schede e pubblica la nuova home page.

## Attività degli addetti al marketing

1. L’addetto al marketing crea un’attività di destinazione A/B con l’offerta di reindirizzamento come esperienza e il 100% del traffico del sito web assegnato alla nuova home page, con l’aggiunta dell’obiettivo di successo e delle metriche.
   1. Dalla finestra di Adobe Target, passa alla scheda **Attività**.
   2. Fai clic sul pulsante **Crea attività** e seleziona il tipo di attività come **Test A/B**
      ![Adobe Target - Crea attività](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleziona il canale **Web** e scegli il **Compositore esperienza visivo**.
   4. Immetti l&#39;**URL attività** e fai clic su **Avanti** per aprire il Compositore esperienza visivo.
      ![Adobe Target - Crea attività](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Per il caricamento di **Visual Experience Composer**, abilita **Allow Load Unsafe scripts** nel browser e ricarica la pagina.
      ![Attività Targeting Esperienza](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Nell’editor del Compositore esperienza visivo viene aperta la pagina Home del sito WKND.
      ![Compositore esperienza visivo](assets/personalization-use-case-2/vec.png)
   7. Passa il puntatore del mouse su **Esperienza B** e seleziona Visualizza altre opzioni.
      ![Esperienza B](assets/personalization-use-case-2/redirect-url.png)
   8. Seleziona l&#39;opzione **Reindirizza all&#39;URL** e immetti l&#39;URL per la nuova home page WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Esperienza B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Salva** le modifiche e continua con i passaggi successivi della creazione di attività.
   10. Seleziona il **metodo di allocazione traffico** come manuale e assegna il 100% di traffico a **Esperienza B**.
      ![Traffico Esperienza B](assets/personalization-use-case-2/traffic.png)
   11. Fai clic su **Avanti**.
   12. Fornisci **Metriche obiettivo** per l&#39;attività e salva e chiudi il test A/B.
      ![Metrica Obiettivo Test A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Specifica un nome (**Riprogettazione home page WKND**) per l&#39;attività e salva le modifiche.
   14. Dalla schermata dei dettagli dell&#39;attività, assicurati di **attivare** l&#39;attività.
      ![Attiva attività](assets/personalization-use-case-2/ab-activate.png)
   15. Passa alla home page WKND (http://localhost:4503/content/wknd/en.html) e vieni reindirizzato alla home page del sito WKND riprogettata (http://localhost:4503/content/wknd/en1.html).
      ![Home page WKND riprogettata](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Riepilogo

In questo capitolo, un addetto marketing è riuscito a creare un’attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.
