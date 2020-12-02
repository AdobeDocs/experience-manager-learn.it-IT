---
title: Guida introduttiva a AEM headless - Capitolo 6 - Esposizione dei contenuti su AEM Publish come JSON
description: Il capitolo 6 dell'esercitazione AEM headless tratta la garanzia che tutti i pacchetti, la configurazione e il contenuto necessari siano disponibili in AEM Publish per consentire il consumo dall'app mobile.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---


# Capitolo 6 - Esposizione dei contenuti su AEM Publish for Delivery

Il capitolo 6 dell&#39;esercitazione AEM headless tratta la garanzia che tutti i pacchetti, la configurazione e il contenuto necessari siano disponibili su AEM Publish per consentire l&#39;utilizzo da parte dell&#39;app mobile.

## Pubblicazione dei contenuti per AEM Content Services

La configurazione e il contenuto creati per guidare gli eventi tramite AEM Content Services devono essere pubblicati su AEM Publish in modo che l&#39;app mobile possa accedervi.

Poiché AEM Content Services è basato su Configuration (Modelli di frammenti di contenuto, Modelli modificabili), Assets (Frammenti di contenuto, Immagini) e Pages, tutti questi elementi sono dotati automaticamente AEM funzionalità di gestione del contenuto, tra cui:

* Flusso di lavoro per revisione ed elaborazione
* attivazione/disattivazione per inviare e recuperare contenuti dai punti finali di AEM Publish AEM Content Services

1. Assicurati che **[!DNL WKND Mobile]Application Packages**, elencato in [Chapter 1](./chapter-1.md#wknd-mobile-application-packages), siano installati in **AEM Publish** utilizzando [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Pubblicare il modello **[!DNL WKND Mobile Events API]modificabile**
   1. Andate su **[!UICONTROL AEM] > [!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**
   1. Selezionare il modello **[!DNL Event API]**
   1. Toccate **[!UICONTROL Pubblica]** nella barra delle azioni superiore
   1. Pubblicate i **modelli** e **tutti i riferimenti** (criteri di contenuto, mappature criteri di contenuto e modelli)

1. Pubblicate i frammenti di contenuto **[!DNL WKND Mobile Events]**.

Questo è richiesto perché l&#39;API Events utilizza il componente Elenco frammenti di contenuto, che non fa riferimento in modo specifico ai frammenti di contenuto.
1. Andate su **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Selezionare tutti i frammenti di contenuto **[!DNL Event]**
1. Toccate **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
1. Lasciando invariata l&#39;azione **Pubblica** predefinita, toccate **[!UICONTROL Avanti]** nella barra delle azioni superiore
1. Selezionare **tutti i frammenti di contenuto**
1. Toccate **[!UICONTROL Pubblica]** nella barra delle azioni superiore
* *Il [!DNL Events] modello di frammento di contenuto e i riferimenti alle immagini evento verranno pubblicati automaticamente insieme ai frammenti di contenuto.*

1. Pubblicate la pagina **[!DNL Events API]**.
   1. Passare a **[!UICONTROL AEM] > [!UICONTROL Siti] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selezionare la pagina **[!DNL Events]**
   1. Toccate **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lasciando l&#39;azione **Pubblica** predefinita così com&#39;è, toccate **[!UICONTROL Avanti]** nella barra delle azioni superiore
   1. Selezionare la pagina **[!DNL Events]**
   1. Toccate **[!DNL Publish]** nella barra delle azioni superiore

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verifica pubblicazione AEM

1. In un nuovo browser Web, verifica di essere disconnesso da AEM Publish e richiedi i seguenti URL (sostituendo `http://localhost:4503` qualsiasi host:porta su cui sia in esecuzione AEM Publish).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Queste richieste devono restituire la stessa risposta JSON di quando sono stati revisionati i punti finali di AEM Author corrispondenti. In caso contrario, accertatevi che tutte le pubblicazioni siano state completate (controllate le code di replica), che il pacchetto [!DNL WKND Mobile] `ui.apps` &lt;a1/> sia installato in AEM Publish, quindi controllate `error.log` per AEM Publish.

## Passaggio successivo

Non ci sono pacchetti aggiuntivi da installare. Assicurati che il contenuto e la configurazione descritti in questa sezione siano pubblicati su AEM Publish, altrimenti i capitoli successivi non funzioneranno.

* [Capitolo 7 - Utilizzo di AEM Content Services da un&#39;app mobile](./chapter-7.md)
