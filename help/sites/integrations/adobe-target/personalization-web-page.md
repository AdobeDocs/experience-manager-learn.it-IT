---
title: Personalizzazione dell’esperienza di pagina web completa
description: Scopri come creare un’attività Target per reindirizzare le pagine del sito web AEM a nuove pagine utilizzando Adobe Target.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations (Integrazioni)
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---


# Personalizzazione dell’esperienza di pagina web completa {#personalization-fpe}

Scopri come creare un’attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.

## Prerequisiti

Per personalizzare le pagine complete di un sito web AEM, è necessario completare la seguente configurazione:

1. [Aggiungere Adobe Target al sito Web AEM](./add-target-launch-extension.md)
1. [Attivare una chiamata Adobe Target da Launch](./load-and-fire-target.md)

## Panoramica dello scenario

Il sito WKND ha riprogettato la propria home page e desidera reindirizzare i visitatori della home page corrente alla nuova home page. Allo stesso tempo, è inoltre importante comprendere in che modo la home page riprogettata contribuisce a migliorare il coinvolgimento e i ricavi degli utenti. In qualità di addetto al marketing, ti è stata assegnata l’attività di creazione di un’attività per reindirizzare i visitatori alla nuova home page. Esploriamo la home page del sito WKND e impariamo a creare un’attività utilizzando Adobe Target.

## Passaggi per creare un test A/B utilizzando il Compositore esperienza visivo

1. Accedi ad Adobe Target e passa alla scheda Attività
1. Fai clic sul pulsante **Crea attività**, quindi scegli l&#39;attività **Test A/B**

   ![Attività A/B](assets/ab-target-activity.png)

1. Seleziona l&#39;opzione **Compositore esperienza visivo**, fornisci l&#39;URL attività, quindi fai clic su **Avanti**

   ![URL attività](assets/ab-test-url.png)

1. Dopo aver creato una nuova attività, nel Compositore esperienza visivo sono visualizzate due schede a sinistra: *Esperienza A* e *Esperienza B*. Seleziona un’esperienza dall’elenco. Puoi aggiungere nuove esperienze all’elenco utilizzando il pulsante **Aggiungi esperienza** .

   ![Opzioni esperienza](assets/experience-options.png)

1. Visualizza le opzioni disponibili per l’esperienza A, quindi seleziona l’opzione **Reindirizza all’URL** e fornisci un URL per la nuova home page del sito WKND.

   ![URL di reindirizzamento](assets/redirect-url.png)

1. Rinomina *Esperienza A* in *Nuova home page WKND* e *Esperienza B* in *Pagina iniziale WKND*

   ![Avventure](assets/new-experiences.png)

1. Fai clic su **Avanti** per passare a Targeting e mantenere un&#39;allocazione manuale del traffico di 50-50 tra le due esperienze.

   ![Impostazione destinazione](assets/targeting.png)

1. Per Obiettivi e impostazioni, scegli l’origine per la generazione di rapporti come Adobe Target e seleziona la metrica per l’obiettivo come Conversione con un’azione di visualizzazione della pagina.

   ![Obiettivi](assets/goals.png)

1. Specifica un nome per l’attività e Salva.
1. Attiva l&#39;attività salvata per attivare le modifiche.

   ![Obiettivi](assets/activate.png)

1. Apri la pagina del sito (URL attività dal passaggio 3) in una nuova scheda e dovresti essere in grado di visualizzare una delle esperienze (Home page WKND o Nuova home page WKND) dalla nostra attività di test A/B. `us/en.html` reindirizza a  `us/home.html`.

   ![Obiettivi](assets/redirect-test.png)

## Riepilogo

In qualità di addetto al marketing hai creato un’attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.

## Collegamenti di supporto

* [Debugger Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Debugger Adobe Experience Cloud - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

