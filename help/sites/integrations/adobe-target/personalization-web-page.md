---
title: Personalization dell’esperienza dell’intera pagina web
description: Scopri come creare un’attività Target per reindirizzare le pagine del sito web AEM a nuove pagine utilizzando Adobe Target.
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Personalization dell’esperienza dell’intera pagina web {#personalization-fpe}

Scopri come creare un’attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.

## Prerequisiti

Per personalizzare pagine intere di un sito web AEM, è necessario completare la seguente configurazione:

1. [Aggiungere Adobe Target al sito Web AEM](./add-target-launch-extension.md)
1. [Attivare una chiamata Adobe Target dai tag](./load-and-fire-target.md)

## Panoramica dello scenario

Il sito WKND ha riprogettato la propria home page e desidera reindirizzare i visitatori della home page corrente alla nuova home page. Allo stesso tempo, scopri anche come la home page riprogettata consente di migliorare il coinvolgimento degli utenti e i ricavi. In qualità di addetto al marketing, ti è stato assegnato il compito di creare un’attività per reindirizzare i visitatori alla nuova home page. Esplora la home page del sito WKND e scopri come creare un’attività utilizzando Adobe Target.

## Passaggi per creare un test A/B utilizzando il Compositore esperienza visivo

1. Accedi ad Adobe Target e passa alla scheda Attività
1. Fai clic sul pulsante **Crea attività** e scegli **Attività test A/B**

   ![Attività A/B](assets/ab-target-activity.png)

1. Seleziona l&#39;opzione **Compositore esperienza visivo**, specifica l&#39;URL attività, quindi fai clic su **Avanti**

   ![URL attività](assets/ab-test-url.png)

1. Dopo aver creato un&#39;attività, nel Compositore esperienza visivo vengono visualizzate due schede a sinistra: *Esperienza A* e *Esperienza B*. Seleziona un&#39;esperienza dall&#39;elenco. Puoi aggiungere nuove esperienze all&#39;elenco utilizzando il pulsante **Aggiungi esperienza**.

   ![Opzioni esperienza](assets/experience-options.png)

1. Visualizza le opzioni disponibili per l&#39;Esperienza A, quindi seleziona l&#39;opzione **Reindirizza all&#39;URL** e fornisci un URL per la nuova home page del sito WKND.

   ![URL di reindirizzamento](assets/redirect-url.png)

1. Rinomina *Esperienza A* in *Nuova home page WKND* e *Esperienza B* in *Home page WKND*

   ![Avventure](assets/new-experiences.png)

1. Fai clic su **Avanti** per passare al targeting e mantenere un&#39;allocazione manuale del traffico di 50-50 tra le due esperienze.

   ![Targeting](assets/targeting.png)

1. Per Obiettivi e impostazioni, scegli l’origine per la generazione di rapporti come Adobe Target e seleziona la metrica Obiettivo come Conversione con un’azione di visualizzazione della pagina.

   ![Obiettivi](assets/goals.png)

1. Immetti un nome per l&#39;attività e Salva.
1. Attiva l&#39;attività salvata per inviare in diretta le modifiche.

   ![Obiettivi](assets/activate.png)

1. Apri la pagina del sito (URL attività dal passaggio 3) in una nuova scheda e dovresti essere in grado di visualizzare una delle esperienze (home page WKND o nuova home page WKND) dalla nostra attività di test A/B. `us/en.html` reindirizza a `us/home.html`.

   ![Obiettivi](assets/redirect-test.png)

## Riepilogo

In qualità di addetto al marketing, hai potuto creare un’attività per reindirizzare le pagine del sito ospitate su AEM a una nuova pagina utilizzando Adobe Target.

## Collegamenti di supporto

* [Debugger Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
