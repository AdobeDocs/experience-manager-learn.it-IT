---
title: Personalizzazione tramite il Compositore esperienza visivo di Adobe Target
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando il Compositore esperienza visivo di Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 2%

---

# Personalizzazione tramite Compositore esperienza visivo

In questo capitolo, esploreremo la creazione di esperienze utilizzando **Compositore esperienza visivo** trascinando, rilasciando, scambiando e modificando il layout e il contenuto di una pagina web dall’interno di Target.

## Panoramica dello scenario

La home page del sito WKND mostra le attività locali o la cosa migliore da fare intorno a una città sotto forma di layout di carte. In qualità di addetto al marketing, ti è stato assegnato il compito di modificare la home page riorganizzando i layout delle schede per vedere in che modo influisce sul coinvolgimento dell’utente e favorisce la conversione.

### Utenti coinvolti

Per questo esercizio è necessario coinvolgere i seguenti utenti ed eseguire alcune attività, potrebbe essere necessario un accesso amministrativo.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Addetto marketing** (Adobe Target / Team di ottimizzazione)

### Home page sito WKND

![Scenario AEM Target 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Prerequisiti

* **AEM**
   * [istanza di pubblicazione AEM](./implementation.md#getting-aem) in esecuzione su 4503
   * [AEM integrato con Adobe Target tramite Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con [Adobe Target](https://experiencecloud.adobe.com)

## Attività di marketing

1. L’addetto al marketing crea un’attività di destinazione A/B in Adobe Target.
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
   7. **Esperienza A** fornisce la home page WKND predefinita e modifichiamo il layout del contenuto per **Esperienza B**.
      ![Esperienza B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Fai clic su uno dei contenitori di layout della scheda (*Migliori Roasters*) e seleziona **Ridisponi** opzione .
      ![Selezione del contenitore](assets/personalization-use-case-3/container-selection.png)
   9. Fai clic sul contenitore che desideri ridisporre e trascinalo nella posizione desiderata. Ridisponiamo il *Migliori Roasters* contenitore dalla prima riga prima colonna alla prima riga terza colonna. Ora il *Migliori Roasters* accanto a *Mostra Fotografia* contenitore.
      ![Scambio contenitore](assets/personalization-use-case-3/container-swap.png)

      **Dopo lo scambio**
      ![Contenitore scambiato](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Analogamente, ridisporre le posizioni per gli altri contenitori di carte.
      ![Contenitore scambiato](assets/personalization-use-case-3/after-swap-all.png)
   11. Aggiungiamo anche un testo di intestazione sotto il componente carosello e sopra il layout di scheda.
   12. Fai clic sul contenitore del carosello e seleziona il **Inserisci dopo > HTML** per aggiungere HTML.
      ![Aggiungi testo](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Aggiungi testo](assets/personalization-use-case-3/after-changes.png)
   13. Fai clic su **Successivo** per continuare con la tua attività.
   14. Seleziona la **Metodo di allocazione traffico** come manuale e assegna il 100% di traffico a **Esperienza B**.
      ![Traffico B](assets/personalization-use-case-2/traffic.png)
   15. Fai clic su **Avanti**.
   16. Fornire **Metriche dell&#39;obiettivo** per la tua attività e salva e chiudi il test A/B.
      ![Metrica per obiettivo di test A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Immetti un nome (**Aggiornamento home page WKND**) per l’attività e salva le modifiche.
   18. Dalla schermata dei dettagli dell’attività, assicurati di: **Attiva** la tua attività.
      ![Attiva attività](assets/personalization-use-case-3/save-activity.png)
   19. Passa alla home page WKND (http://localhost:4503/content/wknd/en.html) e noterai le modifiche aggiunte all’attività Test A/B di aggiornamento pagina Home WKND.
      ![Home page WKND aggiornata](assets/personalization-use-case-3/activity-result.png)
   20. Apri la console del browser e controlla la scheda di rete per cercare la risposta di destinazione per l’attività Test A/B di aggiornamento pagina iniziale WKND.
      ![Attività di rete](assets/personalization-use-case-3/activity-result.png)

## Riepilogo

In questo capitolo, un addetto al marketing è stato in grado di creare un’esperienza utilizzando Visual Experience Composer (Compositore esperienza visivo) trascinando e rilasciando, scambiando e modificando il layout e il contenuto di una pagina web senza modificare il codice necessario per eseguire un test.
