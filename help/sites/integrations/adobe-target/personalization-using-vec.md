---
title: Personalizzazione tramite Compositore esperienza visivo
description: Scopri come creare un’attività Adobe Target utilizzando il Compositore esperienza visivo.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 1%

---

# Personalizzazione tramite Compositore esperienza visivo {#personalization-vec}

Scopri come creare un’attività Target di test A/B utilizzando il Compositore esperienza visivo.

## Prerequisiti

Per utilizzare il Compositore esperienza visivo su un sito web AEM, è necessario completare la seguente configurazione:

1. [Aggiungere Adobe Target al sito Web AEM](./add-target-launch-extension.md)
1. [Attivare una chiamata Adobe Target da Launch](./load-and-fire-target.md)

## Panoramica dello scenario

La home page del sito WKND mostra le attività locali o la cosa migliore da fare intorno a una città sotto forma di schede informative. In qualità di addetto al marketing, ti è stato assegnato il compito di modificare la home page, apportando modifiche testuali al teaser della sezione avventura e comprendendo come migliora la conversione.

## Passaggi per creare un test A/B utilizzando il Compositore esperienza visivo

1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com/), tocca __Target__, passa alla __Attività__ scheda

   + Se non vedi __Target__ nel dashboard di Experience Cloud, assicurati che l’organizzazione Adobe corretta sia selezionata nel commutatore di organizzazione in alto a destra e che all’utente sia stato concesso l’accesso a Target in [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Fai clic su **Crea attività** quindi scegli **Test A/B** attività

   ![Attività A/B](assets/ab-target-activity.png)

1. Seleziona la **Compositore esperienza visivo** , fornisci l’URL attività e fai clic su **Successivo**

   ![URL attività](assets/ab-test-url.png)

1. Dopo aver creato una nuova attività, nel Compositore esperienza visivo sono visualizzate due schede a sinistra: *Esperienza A* e *Esperienza B*. Seleziona un’esperienza dall’elenco. È possibile aggiungere nuove esperienze all’elenco utilizzando **Aggiungi esperienza** pulsante .

   ![Esperienza A](assets/experience.png)

1. Seleziona un’immagine o un testo sulla pagina per iniziare a apportare modifiche o utilizza l’editor di codice per scegliere e utilizzare l’elemento di HTML.

   ![Elemento](assets/select-element.png)

1. Modificare il testo da *Campeggio in Australia Occidentale* a *Avventure dell&#39;Australia*. Un elenco delle modifiche aggiunte a un’esperienza viene visualizzato in Modifiche . Puoi fare clic sull’elemento modificato e modificarlo per visualizzarne il selettore CSS e il nuovo contenuto ad esso aggiunto.

   ![Avventure](assets/adventures.png)

1. Rinomina *Esperienza A* a *Avventura*
1. Analogamente, aggiorna il testo su *Esperienza B* da *Campeggio in Australia Occidentale* a *Esplora il selvaggio australiano*.

   ![Esplora](assets/explore.png)

1. Fai clic su **Successivo** per passare a Targeting e mantieni un’allocazione manuale del traffico di 50-50 tra le due esperienze.

   ![Impostazione destinazione](assets/targeting.png)

1. Per Obiettivi e impostazioni, scegli l’origine per la generazione di rapporti come Adobe Target e seleziona la metrica per l’obiettivo come Conversione con un’azione di visualizzazione della pagina.

   ![Obiettivi](assets/goals.png)

1. Specifica un nome per l’attività e Salva.
1. Attiva l&#39;attività salvata per attivare le modifiche.

   ![Obiettivi](assets/activate.png)

1. Apri la pagina del sito (URL attività dal passaggio 3) in una nuova scheda e dovresti essere in grado di visualizzare una delle esperienze (Avventura o Esplora) dalla nostra attività di test A/B.

   ![Obiettivi](assets/publish.png)

## Riepilogo

In questo capitolo, un addetto al marketing è stato in grado di creare un’esperienza utilizzando Visual Experience Composer (Compositore esperienza visivo) trascinando e rilasciando, scambiando e modificando il layout e il contenuto di una pagina web senza modificare il codice necessario per eseguire un test.

## Collegamenti di supporto

+ [Debugger Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Debugger Adobe Experience Cloud - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
