---
title: Personalization con Compositore esperienza visivo
description: Scopri come creare un’attività di Adobe Target utilizzando il Compositore esperienza visivo.
version: Cloud Service
jira: KT-6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
duration: 101
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Personalization con Compositore esperienza visivo {#personalization-vec}

Scopri come creare un’attività di test A/B di Target utilizzando il Compositore esperienza visivo.

## Prerequisiti

Per utilizzare il Compositore esperienza visivo su un sito web dell’AEM, è necessario completare la seguente configurazione:

1. [Aggiungere Adobe Target al sito Web AEM](./add-target-launch-extension.md)
1. [Attivare una chiamata Adobe Target dai tag](./load-and-fire-target.md)

## Panoramica dello scenario

Nella home page del sito WKND, sotto forma di schede informative, vengono visualizzate le attività locali o le operazioni migliori da eseguire in una città. In qualità di addetto al marketing, ti è stato assegnato il compito di modificare la home page, apportando modifiche testuali al teaser della sezione avventura e comprendendo in che modo migliora la conversione.

## Passaggi per creare un test A/B utilizzando il Compositore esperienza visivo

1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com/), tocca __Target__, passa alla scheda __Attività__

   + Se non vedi __Target__ nel dashboard di Experience Cloud, accertati che nel commutatore dell&#39;organizzazione in alto a destra sia selezionata l&#39;organizzazione di Adobe corretta e che all&#39;utente sia stato concesso l&#39;accesso a Target in [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Fai clic sul pulsante **Crea attività** e scegli **Attività test A/B**

   ![Attività A/B](assets/ab-target-activity.png)

1. Seleziona l&#39;opzione **Compositore esperienza visivo**, specifica l&#39;URL attività, quindi fai clic su **Avanti**

   ![URL attività](assets/ab-test-url.png)

1. Dopo aver creato un&#39;attività, nel Compositore esperienza visivo vengono visualizzate due schede a sinistra: *Esperienza A* e *Esperienza B*. Seleziona un&#39;esperienza dall&#39;elenco. Puoi aggiungere nuove esperienze all&#39;elenco utilizzando il pulsante **Aggiungi esperienza**.

   ![Esperienza A](assets/experience.png)

1. Seleziona un&#39;immagine o un testo nella pagina per iniziare ad apportare modifiche o puoi utilizzare l&#39;editor di codice per scegliere un elemento HTML.

   ![Elemento](assets/select-element.png)

1. Cambia il testo da *Campeggio in Australia Occidentale* a *Avventure dell&#39;Australia*. In Modifiche viene visualizzato un elenco delle modifiche aggiunte a un’esperienza. Puoi fare clic sull’elemento modificato e modificarlo per visualizzarne il selettore CSS e il nuovo contenuto aggiunto.

   ![Avventure](assets/adventures.png)

1. Rinomina *Esperienza A* in *Avventura*
1. Allo stesso modo, aggiorna il testo su *Esperienza B* da *Campeggio in Australia Occidentale* a *Esplora il deserto australiano*.

   ![Esplora](assets/explore.png)

1. Fai clic su **Avanti** per passare al targeting e mantieni un&#39;allocazione manuale del traffico di 50-50 tra le due esperienze.

   ![Targeting](assets/targeting.png)

1. Per Obiettivi e impostazioni, scegli l’origine per la generazione di rapporti come Adobe Target e seleziona la metrica Obiettivo come Conversione con un’azione di visualizzazione della pagina.

   ![Obiettivi](assets/goals.png)

1. Immetti un nome per l&#39;attività e Salva.
1. Attiva l&#39;attività salvata per inviare in diretta le modifiche.

   ![Obiettivi](assets/activate.png)

1. Apri la pagina del sito (URL attività dal passaggio 3) in una nuova scheda e dovresti essere in grado di visualizzare una delle esperienze (Avventura o Esplora) dall’attività di test A/B.

   ![Obiettivi](assets/publish.png)

## Riepilogo

In questo capitolo, un addetto al marketing può creare un’esperienza utilizzando il Compositore esperienza visivo trascinando, scambiando e modificando il layout e il contenuto di una pagina web senza modificare il codice per eseguire un test.

## Collegamenti di supporto

+ [Debugger Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
