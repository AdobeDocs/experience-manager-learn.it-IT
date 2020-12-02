---
title: Personalizzazione mediante Visual Experience Composer (Compositore esperienza visivo)
description: Scoprite come creare un'attività Adobe Target  utilizzando Visual Experience Composer (Compositore esperienza visivo).
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---


# Personalizzazione mediante Visual Experience Composer (Compositore esperienza visivo) {#personalization-vec}

Scoprite come creare un&#39;attività A/B Test Target utilizzando Visual Experience Composer (VEC).

## Prerequisiti

Per utilizzare VEC su un sito Web AEM, è necessario completare la seguente configurazione:

1. [Aggiungere  Adobe Target al sito Web AEM](./add-target-launch-extension.md)
1. [Attivazione di una chiamata Adobe Target  da Launch](./load-and-fire-target.md)

## Panoramica scenario

La home page del sito WKND mostra le attività locali o la cosa migliore da fare intorno a una città sotto forma di schede informative. Come esperto di marketing, ti è stato assegnato il compito di modificare la pagina principale, apportando modifiche testuali al teaser della sezione avventura e comprendendo come migliora la conversione.

## Passaggi per creare un test A/B utilizzando Visual Experience Composer (VEC)

1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com/), tocca __Target__, vai alla scheda __Activities__

   + Se non vedete __Target__ nel dashboard del Experience Cloud di , verificate che l&#39;organizzazione  Adobe corretta sia selezionata nello switcher di organizzazione in alto a destra e che all&#39;utente sia stato concesso l&#39;accesso a Target in [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Fare clic sul pulsante **Crea attività**, quindi scegliere l&#39;attività **A/B Test**

   ![Attività A/B](assets/ab-target-activity.png)

1. Selezionate l&#39;opzione **Visual Experience Composer (Compositore esperienza visivo)**, fornite l&#39;URL attività, quindi fate clic su **Next**

   ![URL attività](assets/ab-test-url.png)

1. In Visual Experience Composer (Compositore esperienza visivo) sono visualizzate due schede a sinistra dopo la creazione di una nuova attività: *Esperienza A* e *Esperienza B*. Selezionate un&#39;esperienza dall&#39;elenco. Puoi aggiungere nuove esperienze all&#39;elenco utilizzando il pulsante **Aggiungi esperienza**.

   ![Esperienza A](assets/experience.png)

1. Selezionate un’immagine o un testo sulla pagina per iniziare a modificarlo o utilizzate l’editor di codice per selezionare e usare l’elemento HTML.

   ![Elemento](assets/select-element.png)

1. Modificate il testo da *Campeggio in Australia Occidentale* a *Avventure dell&#39;Australia*. Un elenco delle modifiche aggiunte a un&#39;esperienza verrà visualizzato in Modifiche. Potete fare clic sull’elemento modificato e modificarlo per visualizzarne il selettore CSS e il nuovo contenuto ad esso aggiunto.

   ![Avventure](assets/adventures.png)

1. Rinominare *Esperienza A* in *Avventura*
1. Allo stesso modo, aggiornate il testo sull&#39; *Esperienza B* da *Campeggio in Australia Occidentale* a *Esplorare la Wilderness Australiana*.

   ![Esplora](assets/explore.png)

1. Fare clic su **Next** per passare al Targeting e mantenere un&#39;allocazione manuale del traffico di 50-50 tra le due esperienze.

   ![Impostazione destinazione](assets/targeting.png)

1. Per Obiettivi e impostazioni, scegli l&#39;origine di reporting come  Adobe Target e seleziona la metrica Obiettivo come Conversione con un&#39;azione di visualizzazione della pagina.

   ![Obiettivi](assets/goals.png)

1. Specificate un nome per l&#39;attività e Salva.
1. Attivate l&#39;attività salvata per rendere attive le modifiche.

   ![Obiettivi](assets/activate.png)

1. Aprite la pagina del sito (URL attività dal passaggio 3) in una nuova scheda e dovreste essere in grado di visualizzare una delle esperienze (Avventura o Esplora) dalla nostra attività di test A/B.

   ![Obiettivi](assets/publish.png)

## Riepilogo

In questo capitolo, un esperto di marketing è stato in grado di creare un&#39;esperienza utilizzando Visual Experience Composer (Compositore esperienza visivo) trascinando, scambiando e modificando il layout e il contenuto di una pagina Web senza modificare il codice per eseguire un test.

## Collegamenti di supporto

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
