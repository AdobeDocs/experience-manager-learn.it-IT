---
title: Personalizzazione mediante  Adobe Target Visual Experience Composer
seo-title: Personalizzazione mediante  Adobe Target Visual Experience Composer (VEC)
description: Un'esercitazione end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando  Adobe Target Visual Experience Composer (VEC).
seo-description: Un'esercitazione end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando  Adobe Target Visual Experience Composer (VEC).
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---


# Personalizzazione mediante Visual Experience Composer (Compositore esperienza visivo)

In questo capitolo, esploreremo la creazione di esperienze utilizzando **Visual Experience Composer** trascinando, rilasciando, scambiando e modificando il layout e il contenuto di una pagina Web dall&#39;interno di Target.

## Panoramica scenario

La home page del sito WKND mostra le attività locali o la cosa migliore da fare intorno a una città sotto forma di layout di scheda. Come esperto di marketing, ti è stata assegnata l&#39;attività di modificare la pagina principale, ridisponendo i layout della scheda per vedere in che modo ciò influenzi il coinvolgimento degli utenti e stimola la conversione.

### Utenti coinvolti

Per questo esercizio, è necessario coinvolgere i seguenti utenti e per eseguire alcune attività potrebbe essere necessario l&#39;accesso amministrativo.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Marketing** ( Adobe Target / Gruppo di ottimizzazione)

### Home page sito WKND

![AEM Target 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Prerequisiti

* **AEM**
   * [AEM ](./implementation.md#getting-aem) istancerunning di pubblicazione su 4503
   * [AEM integrato con  Adobe Target mediante  Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experienceecCloud.adobe.com
   *  Experience Cloud fornito con [ Adobe Target](https://experiencecloud.adobe.com)

## Attività di marketing

1. L&#39;esperto di marketing crea un&#39;attività target A/B in  Adobe Target.
   1. Dalla finestra  Adobe Target, passare alla scheda **Attività**.
   2. Fare clic sul pulsante **Crea attività** e selezionare il tipo di attività come **A/B Test**

      ![ Adobe Target - Crea attività](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selezionate il canale **Web** e scegliete **Visual Experience Composer (Compositore esperienza visivo)**.
   4. Inserite l&#39; **URL attività** e fate clic su **Avanti** per aprire Visual Experience Composer (Compositore esperienza visivo).
      ![ Adobe Target - Crea attività](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Per consentire il caricamento di **Visual Experience Composer**, abilitate **Allow Load Unsafe scripts** nel browser e ricaricate la pagina.
      ![Attività di targeting delle esperienze](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Notate che la home page del sito WKND si apre nell&#39;editor di Visual Experience Composer (Compositore esperienza visivo).
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Experience** Fornisce la home page WKND predefinita e modifichiamo il layout del contenuto per l&#39; **Esperienza B**.
      ![Esperienza B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Fate clic su uno dei contenitori di layout scheda (*Migliori Roasters*) e selezionate l&#39;opzione **Ridisponi**.
      ![Selezione contenitore](assets/personalization-use-case-3/container-selection.png)
   9. Fate clic sul contenitore che desiderate ridisporre e trascinatelo nella posizione desiderata. Ridisponiamo il contenitore *Migliori Roasters* dalla prima riga alla prima riga terza colonna. Ora il contenitore *Best Roasters* sarà accanto al contenitore *Photography Exhibitions*.
      ![Scambio contenitore](assets/personalization-use-case-3/container-swap.png)

      **Dopo lo scambio**
      ![Contenitore scambiato](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Allo stesso modo, ridisponete le posizioni per gli altri contenitori della scheda.
      ![Contenitore scambiato](assets/personalization-use-case-3/after-swap-all.png)
   11. Aggiungete anche un testo di intestazione sotto il componente carosello e sopra il layout della scheda.
   12. Fate clic sul contenitore del carosello e selezionate l&#39;opzione **Inserisci dopo > HTML** per aggiungere il codice HTML.
      ![Aggiungi testo](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Aggiungi testo](assets/personalization-use-case-3/after-changes.png)
   13. Fare clic su **Next** per continuare con l&#39;attività.
   14. Selezionare il **Metodo di allocazione del traffico** come manuale e assegnare il 100% del traffico a **Esperienza B**.
      ![Experience B Traffic](assets/personalization-use-case-2/traffic.png)
   15. Fai clic su **Avanti**.
   16. Fornire **Metriche obiettivo** per l&#39;attività e salvare e chiudere il test A/B.
      ![Metrica obiettivo test A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Immettete un nome (**Aggiornamento pagina iniziale WKND**) per l&#39;attività e salvate le modifiche.
   18. Dalla schermata Dettagli attività, accertatevi di **Attivare** l&#39;attività.
      ![Attiva attività](assets/personalization-use-case-3/save-activity.png)
   19. Passate alla home page WKND (http://localhost:4503/content/wknd/en.html) e notate le modifiche aggiunte all&#39;attività di test A/B di aggiornamento pagina iniziale WKND.
      ![Home page WKND aggiornata](assets/personalization-use-case-3/activity-result.png)
   20. Aprite la console del browser e ispezionate la scheda di rete per cercare la risposta di destinazione per l&#39;attività di test A/B di aggiornamento pagina iniziale WKND.
      ![Attività di rete](assets/personalization-use-case-3/activity-result.png)

## Riepilogo

In questo capitolo, un esperto di marketing è stato in grado di creare un&#39;esperienza utilizzando Visual Experience Composer (Compositore esperienza visivo) trascinando, scambiando e modificando il layout e il contenuto di una pagina Web senza modificare il codice per eseguire un test.
