---
title: Personalizzazione dell'esperienza della pagina Web completa
description: Scoprite come creare un'attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando  Adobe Target.
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---


# Personalizzazione dell&#39;esperienza della pagina Web completa {#personalization-fpe}

Scoprite come creare un&#39;attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando  Adobe Target.

## Panoramica scenario

Il sito WKND ha riprogettato la propria pagina principale e desidera reindirizzare i visitatori della pagina principale corrente alla nuova pagina principale. Allo stesso tempo, puoi comprendere anche in che modo la nuova home page consente di migliorare il coinvolgimento e le entrate degli utenti. In qualità di esperto di marketing, vi è stata assegnata l&#39;attività di creazione di un&#39;attività per reindirizzare i visitatori alla nuova home page. Esaminate la home page del sito WKND e scoprite come creare un&#39;attività utilizzando  Adobe Target.

## Passaggi per creare un test A/B utilizzando Visual Experience Composer (VEC)

1. Accedi a  Adobe Target e passa alla scheda Attività
2. Fate clic sul pulsante **Crea attività** e scegliete Attività test **** A/B

   ![Attività A/B](assets/ab-target-activity.png)

3. Selezionate l&#39;opzione **Visual Experience Composer (Compositore esperienza visivo)** , fornite l&#39;URL dell&#39;attività, quindi fate clic su **Next (Avanti)**

   ![URL attività](assets/ab-test-url.png)

4. In Visual Experience Composer (Compositore esperienza visivo) sono visualizzate due schede a sinistra dopo la creazione di una nuova attività: *Esperienza A* ed *Esperienza B*. Selezionate un&#39;esperienza dall&#39;elenco. Puoi aggiungere nuove esperienze all&#39;elenco tramite il pulsante **Aggiungi esperienza** .

   ![Opzioni esperienza](assets/experience-options.png)

5. Visualizzate le opzioni disponibili per l&#39;Esperienza A, quindi selezionate l&#39;opzione **Reindirizza a URL** e fornite un URL per la nuova home page del sito WKND.

   ![URL di reindirizzamento](assets/redirect-url.png)

6. Rinomina *Esperienza A* in *nuova home page* WKND e *Esperienza B* in *WKND Home Page*

   ![Avventure](assets/new-experiences.png)

7. Fate clic su **Avanti** per passare a Targeting e mantenere un&#39;allocazione manuale del traffico di 50-50 tra le due esperienze.

   ![Impostazione destinazione](assets/targeting.png)

8. Per Obiettivi e impostazioni, scegli l&#39;origine di reporting come  Adobe Target e seleziona la metrica Obiettivo come Conversione con un&#39;azione di visualizzazione della pagina.

   ![Obiettivi](assets/goals.png)

9. Specificate un nome per l&#39;attività e Salva.
10. Attivate l&#39;attività salvata per rendere attive le modifiche.

   ![Obiettivi](assets/activate.png)

11. Aprite la pagina del sito (URL attività dal passaggio 3) in una nuova scheda e dovreste essere in grado di visualizzare una delle esperienze (pagina iniziale WKND o nuova pagina iniziale WKND) dalla nostra attività di test A/B. `us/en.html` reindirizza a `us/home.html`.

   ![Obiettivi](assets/redirect-test.png)

## Riepilogo

In qualità di esperto di marketing, è stata creata un&#39;attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando  Adobe Target.

## Collegamenti di supporto

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

