---
title: Capitolo 6 - Esposizione dei contenuti su AEM Publish as JSON - Content Services
description: Il capitolo 6 dell’esercitazione AEM Headless tratta la garanzia che tutti i pacchetti, le configurazioni e i contenuti necessari siano disponibili su AEM Publish per consentire il consumo dall’app mobile.
feature: Frammenti di contenuto, API
topic: Senza testa, gestione dei contenuti
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# Capitolo 6 - Esposizione dei contenuti su AEM Publish for Delivery

Il capitolo 6 dell’esercitazione AEM Headless tratta la garanzia che tutti i pacchetti, le configurazioni e i contenuti necessari siano disponibili su AEM Publish per consentire il consumo da parte dell’app mobile.

## Pubblicazione dei contenuti per AEM Content Services

La configurazione e il contenuto creati per guidare gli eventi tramite AEM Content Services devono essere pubblicati su AEM Publish in modo che l’app mobile possa accedervi.

Poiché AEM Content Services è stato creato da Configurazione (Modelli di frammenti di contenuto, Modelli modificabili), Risorse (Frammenti di contenuto, Immagini) e Pagine, tutte queste parti beneficiano automaticamente delle funzionalità di gestione dei contenuti di AEM, tra cui:

* Flusso di lavoro per la revisione e l&#39;elaborazione
* e attivazione/disattivazione per inviare e estrarre contenuti dagli endpoint di AEM Publish Content Services

1. Assicurati che i **[!DNL WKND Mobile]Pacchetti applicativi**, elencati in [Capitolo 1](./chapter-1.md#wknd-mobile-application-packages), siano installati su **AEM Publish** utilizzando [!UICONTROL Gestione pacchetti].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Pubblica il **[!DNL WKND Mobile Events API]modello modificabile**
   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**
   1. Seleziona il modello **[!DNL Event API]**
   1. Tocca **[!UICONTROL Pubblica]** nella barra delle azioni superiore
   1. Pubblica i **modelli** e **tutti i riferimenti** (criteri del contenuto, mappature dei criteri del contenuto e modelli)

1. Pubblica i frammenti di contenuto **[!DNL WKND Mobile Events]**.

Questo è necessario in quanto l’API Eventi utilizza il componente Elenco frammenti di contenuto , che non fa riferimento specifico ai frammenti di contenuto.
1. Passa a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Seleziona tutti i frammenti di contenuto **[!DNL Event]**
1. Tocca **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
1. Lasciando l&#39;azione predefinita **Pubblica** così com&#39;è, tocca **[!UICONTROL Avanti]** nella barra delle azioni superiore
1. Seleziona **tutti i frammenti di contenuto**
1. Tocca **[!UICONTROL Pubblica]** nella barra delle azioni superiore
* *Il [!DNL Events] modello di frammento di contenuto e i riferimenti alle immagini evento verranno pubblicati automaticamente insieme ai frammenti di contenuto.*

1. Pubblica la **[!DNL Events API]pagina**.
   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Siti] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleziona la pagina **[!DNL Events]**
   1. Tocca **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lasciando invariata l&#39;azione predefinita **Pubblica**, tocca **[!UICONTROL Avanti]** nella barra delle azioni superiore
   1. Seleziona la pagina **[!DNL Events]**
   1. Tocca **[!DNL Publish]** nella barra delle azioni superiore

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verifica pubblicazione AEM

1. In un nuovo browser Web, assicurati di essere disconnesso da AEM Publish e richiedi i seguenti URL (sostituendo `http://localhost:4503` qualsiasi host:port AEM Publish sia in esecuzione).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Queste richieste devono restituire la stessa risposta JSON di quando sono stati esaminati i corrispondenti endpoint di AEM Author. In caso contrario, assicurati che tutte le pubblicazioni siano riuscite (controlla le code di replica), che il pacchetto [!DNL WKND Mobile] `ui.apps` sia installato su AEM Publish e controlla `error.log` per AEM Publish.

## Passaggio successivo

Non ci sono pacchetti aggiuntivi da installare. Assicurati che il contenuto e la configurazione descritti in questa sezione siano pubblicati in AEM Publish, altrimenti i capitoli successivi non funzioneranno.

* [Capitolo 7 - Utilizzo di AEM Content Services da un’app mobile](./chapter-7.md)
