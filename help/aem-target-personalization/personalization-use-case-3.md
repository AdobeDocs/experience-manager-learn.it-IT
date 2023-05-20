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

# Personalizzazione mediante Visual Experience Composer

In questo capitolo, esploreremo la creazione di esperienze utilizzando **Compositore esperienza visivo** trascinando, scambiando e modificando il layout e il contenuto di una pagina web dall’interno di Target.

## Panoramica dello scenario

La pagina Home del sito WKND mostra le attività locali o le operazioni migliori da eseguire in una città sotto forma di layout di schede. In qualità di addetto al marketing, ti è stata assegnata l’attività di modifica della home page, riorganizzando i layout delle schede per vedere in che modo influisce sul coinvolgimento degli utenti e favorisce la conversione.

### Utenti interessati

Per questo esercizio, è necessario coinvolgere i seguenti utenti e per eseguire alcune attività potrebbe essere necessario l&#39;accesso amministrativo.

* **Content Producer/Editor contenuti** (Adobe Experience Manager)
* **Addetto marketing** (Adobe Target / Team di ottimizzazione)

### Home page del sito WKND

![Scenario 1 obiettivo AEM](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Prerequisiti

* **AEM**
   * [Istanza di pubblicazione AEM](./implementation.md#getting-aem) in esecuzione su 4503
   * [AEM integrato con Adobe Target tramite Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accesso al Adobe Experience Cloud delle organizzazioni - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con [Adobe Target](https://experiencecloud.adobe.com)

## Attività degli addetti marketing

1. L’addetto al marketing crea un’attività di destinazione A/B in Adobe Target.
   1. Dalla finestra di Adobe Target, passa a **Attività** scheda.
   2. Clic **Crea attività** e seleziona il tipo di attività come **Test A/B**

      ![Adobe Target - Crea attività](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleziona la **Web** e scegli il **Compositore esperienza visivo**.
   4. Inserisci il **URL attività** e fai clic su **Successivo** per aprire il Compositore esperienza visivo.
      ![Adobe Target - Crea attività](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Per **Compositore esperienza visivo** per caricare, abilita **Consenti caricamento script non sicuri** sul browser e ricarica la pagina.
      ![Attività Targeting esperienza](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Nell’editor del Compositore esperienza visivo viene aperta la pagina Home del sito WKND.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Esperienza A** fornisce la home page WKND predefinita e modifichiamo il layout del contenuto per **Esperienza B**.
      ![Esperienza B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Fai clic su uno dei contenitori di layout scheda (*I migliori torrefattori*) e seleziona **Ridisponi** opzione.
      ![Selezione contenitore](assets/personalization-use-case-3/container-selection.png)
   9. Fai clic sul contenitore da ridisporre e trascinalo nella posizione desiderata. Ridisponiamo il *I migliori torrefattori* contenitore dalla prima riga della prima colonna alla prima riga della terza colonna. Ora il *I migliori torrefattori* il contenitore è accanto a *Mostre fotografiche* contenitore.
      ![Scambio contenitore](assets/personalization-use-case-3/container-swap.png)

      **Dopo lo scambio**
      ![Contenitore scambiato](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Analogamente, ridisporre le posizioni per gli altri contenitori di carte.
      ![Contenitore scambiato](assets/personalization-use-case-3/after-swap-all.png)
   11. Aggiungiamo anche un testo di intestazione sotto il componente Carosello e sopra il layout della scheda.
   12. Fai clic sul contenitore del carosello e seleziona la **Inserisci dopo > HTML** per aggiungere HTML.
      ![Aggiungi testo](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Aggiungi testo](assets/personalization-use-case-3/after-changes.png)
   13. Clic **Successivo** per continuare con l’attività.
   14. Seleziona la **Metodo di allocazione traffico** come manuale e allocare il 100% del traffico a **Esperienza B**.
      ![Traffico esperienza B](assets/personalization-use-case-2/traffic.png)
   15. Fai clic su **Avanti**.
   16. Fornire **Metriche obiettivo** per l’attività e salvare e chiudere il test A/B.
      ![Metrica per obiettivo test A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Immetti un nome (**Aggiornamento home page WKND**) per l&#39;attività e salva le modifiche.
   18. Dalla schermata Dettagli attività, assicurati di: **Attiva** attività.
      ![Attiva attività](assets/personalization-use-case-3/save-activity.png)
   19. Passa alla home page WKND (http://localhost:4503/content/wknd/en.html) e osserva le modifiche aggiunte all’attività Test A/B di aggiornamento della home page WKND.
      ![Home page WKND aggiornata](assets/personalization-use-case-3/activity-result.png)
   20. Apri la console del browser ed esamina la scheda rete per cercare la risposta di destinazione per l’attività Test A/B di aggiornamento pagina iniziale WKND.
      ![Attività di rete](assets/personalization-use-case-3/activity-result.png)

## Riepilogo

In questo capitolo, un addetto marketing è stato in grado di creare un’esperienza utilizzando il Compositore esperienza visivo trascinando, scambiando e modificando il layout e il contenuto di una pagina web senza modificare alcun codice per eseguire un test.
