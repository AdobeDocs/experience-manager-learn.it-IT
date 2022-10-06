---
title: Personalizzazione tramite Adobe Target
seo-title: Personalization using Adobe Target
description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---

# Personalizzazione delle esperienze di pagine web complete con Adobe Target

Nel capitolo precedente, abbiamo imparato a creare un’attività basata sulla geolocalizzazione in Adobe Target utilizzando il contenuto creato come Frammenti esperienza ed esportato da AEM come Offerte HTML.

In questo capitolo, esploreremo la creazione di attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.

## Panoramica dello scenario

Il sito WKND ha riprogettato la propria home page e desidera reindirizzare i visitatori della home page corrente alla nuova home page. Allo stesso tempo, è inoltre importante comprendere in che modo la home page riprogettata contribuisce a migliorare il coinvolgimento e i ricavi degli utenti. In qualità di addetto al marketing, ti è stata assegnata l’attività di creazione di un’attività per reindirizzare i visitatori alla nuova home page. Esploriamo la home page del sito WKND e impariamo a creare un’attività utilizzando Adobe Target.

### Utenti coinvolti

Per questo esercizio è necessario coinvolgere i seguenti utenti ed eseguire alcune attività, potrebbe essere necessario un accesso amministrativo.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Addetto marketing** (Adobe Target / Team di ottimizzazione)

### Home page sito WKND

![Scenario AEM Target 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Prerequisiti

* **AEM**
   * [AEM istanza di authoring e pubblicazione](./implementation.md#getting-aem) in esecuzione rispettivamente su localhost 4502 e 4503.
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
   1. Dalla finestra di Adobe Target, passa a **Attività** scheda .
   2. Fai clic su **Crea attività** e seleziona il tipo di attività come **Test A/B**

      ![Adobe Target - Creare un’attività](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleziona la **Web** e scegli il **Compositore esperienza visivo**.
   4. Inserisci il **URL attività** e fai clic su **Successivo** per aprire il Compositore esperienza visivo.
      ![Adobe Target - Creare un’attività](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Per **Compositore esperienza visivo** per caricare, abilita **Consenti caricamento script non sicuri** nel browser e ricarica la pagina.
      ![Attività Targeting esperienze](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Osserva la home page del sito WKND aperta nell’editor del Compositore esperienza visivo.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Passa il cursore **Esperienza B** e selezionare visualizza altre opzioni.
      ![Esperienza B](assets/personalization-use-case-2/redirect-url.png)
   8. Seleziona **Reindirizzare a URL** e immetti l’URL della nuova home page WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Esperienza B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Salva** le modifiche e continuare con i passaggi successivi della Creazione di attività.
   10. Seleziona la **Metodo di allocazione traffico** come manuale e assegna il 100% di traffico a **Esperienza B**.
      ![Traffico B](assets/personalization-use-case-2/traffic.png)
   11. Fai clic su **Avanti**.
   12. Fornire **Metriche dell&#39;obiettivo** per la tua attività e salva e chiudi il test A/B.
      ![Metrica per obiettivo di test A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Immetti un nome (**Riprogettazione della home page WKND**) per l’attività e salva le modifiche.
   14. Dalla schermata dei dettagli dell’attività, assicurati di: **Attiva** la tua attività.
      ![Attiva attività](assets/personalization-use-case-2/ab-activate.png)
   15. Passa alla home page WKND (http://localhost:4503/content/wknd/en.html) e vieni reindirizzato alla home page del sito WKND riprogettata (http://localhost:4503/content/wknd/en1.html).
      ![Home page WKND riprogettata](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Riepilogo

In questo capitolo, un addetto al marketing è stato in grado di creare un’attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.
