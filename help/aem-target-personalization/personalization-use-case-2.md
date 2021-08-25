---
title: Personalizzazione tramite Adobe Target
seo-title: Personalization using Adobe Target
description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 2%

---


# Personalizzazione delle esperienze di pagine web complete con Adobe Target

Nel capitolo precedente, abbiamo imparato a creare un’attività basata sulla geolocalizzazione in Adobe Target utilizzando il contenuto creato come Frammenti esperienza ed esportato da AEM come Offerte HTML.

In questo capitolo, esploreremo la creazione di attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.

## Panoramica dello scenario

Il sito WKND ha riprogettato la propria home page e desidera reindirizzare i visitatori della home page corrente alla nuova home page. Allo stesso tempo, è inoltre importante comprendere in che modo la home page riprogettata contribuisce a migliorare il coinvolgimento e i ricavi degli utenti. In qualità di addetto al marketing, ti è stata assegnata l’attività di creazione di un’attività per reindirizzare i visitatori alla nuova home page. Esploriamo la home page del sito WKND e impariamo a creare un’attività utilizzando Adobe Target.

### Utenti coinvolti

Per questo esercizio è necessario coinvolgere i seguenti utenti ed eseguire alcune attività, potrebbe essere necessario un accesso amministrativo.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Addetto al marketing**  (Adobe Target / Team di ottimizzazione)

### Home page sito WKND

![Scenario AEM Target 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Prerequisiti

* **AEM**
   * [AEM autore e pubblicare instancerunning ](./implementation.md#getting-aem) su localhost 4502 e 4503 rispettivamente.
   * [AEM integrato con Adobe Target tramite Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

## Attività dell’editor di contenuti

1. L’addetto al marketing avvia la discussione sulla riprogettazione della home page WKND con AEM editor di contenuti e descrive i requisiti.
   * ***Requisito*** : Consente di riprogettare la home page del sito WKND con un design basato su schede.
2. In base ai requisiti, AEM Editor di contenuto crea quindi una nuova home page del sito WKND con una progettazione basata su scheda e pubblica la nuova home page.

## Attività di marketing

1. L’addetto al marketing crea un’attività di destinazione A/B con l’offerta di reindirizzamento come esperienza e il 100% di traffico del sito web assegnato alla nuova home page con obiettivo di successo e metriche aggiunte.
   1. Dalla finestra di Adobe Target, passa alla scheda **Attività** .
   2. Fai clic sul pulsante **Crea attività** e seleziona il tipo di attività come **Test A/B**

      ![Adobe Target - Creare un’attività](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleziona il canale **Web** e scegli il **Compositore esperienza visivo**.
   4. Inserisci l&#39; **URL attività** e fai clic su **Avanti** per aprire il Compositore esperienza visivo.
      ![Adobe Target - Creare un’attività](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Affinché **Compositore esperienza visivo** possa essere caricato, abilita **Consenti il caricamento di script non sicuri** nel browser e ricarica la pagina.
      ![Attività Targeting esperienze](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Osserva la home page del sito WKND aperta nell’editor del Compositore esperienza visivo.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Passa il puntatore del mouse su **Esperienza B** e seleziona visualizza altre opzioni.
      ![Esperienza B](assets/personalization-use-case-2/redirect-url.png)
   8. Seleziona l’opzione **Reindirizza all’URL** e immetti l’URL nella nuova home page WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Esperienza B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** Salva le modifiche e continua con i passaggi successivi della Creazione di attività.
   10. Seleziona il **Metodo di allocazione del traffico** come manuale e assegna il 100% del traffico a **Esperienza B**.
      ![Traffico B](assets/personalization-use-case-2/traffic.png)
   11. Fai clic su **Avanti**.
   12. Fornisci **Metriche obiettivo** per la tua attività e Salva e chiudi il test A/B.
      ![Metrica per obiettivo di test A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Specifica un nome (**WKND Home Page Redesign**) per l&#39;attività e salva le modifiche.
   14. Dalla schermata dei dettagli dell&#39;attività, assicurati di **Attivare** l&#39;attività.
      ![Attiva attività](assets/personalization-use-case-2/ab-activate.png)
   15. Passa alla home page WKND (http://localhost:4503/content/wknd/en.html) e verrai reindirizzato alla home page del sito WKND riprogettata (http://localhost:4503/content/wknd/en1.html).
      ![Home page WKND riprogettata](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Riepilogo

In questo capitolo, un addetto al marketing è stato in grado di creare un’attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.
