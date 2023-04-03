---
title: Capitolo 6 - Esposizione dei contenuti su AEM Publish as JSON - Content Services
description: Il capitolo 6 dell’esercitazione AEM headless tratta la garanzia che tutti i pacchetti, le configurazioni e i contenuti necessari siano disponibili su AEM Publish per consentire il consumo dall’app mobile.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Capitolo 6 - Esposizione dei contenuti su AEM Publish for Delivery

Il capitolo 6 dell’esercitazione AEM headless tratta la garanzia che tutti i pacchetti, le configurazioni e i contenuti necessari siano disponibili su AEM Publish per consentire il consumo da parte dell’app mobile.

## Pubblicazione dei contenuti per AEM Content Services

La configurazione e il contenuto creati per guidare gli eventi tramite AEM Content Services devono essere pubblicati in AEM Publish in modo che l’app mobile possa accedervi.

Poiché AEM Content Services è basato su Configurazione (Modelli di frammenti di contenuto, Modelli modificabili), Risorse (Frammenti di contenuto, Immagini) e Pagine, tutte queste parti dispongono automaticamente di funzionalità AEM gestione dei contenuti, tra cui:

* Flusso di lavoro per la revisione e l&#39;elaborazione
* e attivazione/disattivazione per spingere ed estrarre contenuti dagli endpoint di AEM Publish AEM Content Services

1. Assicurati che **[!DNL WKND Mobile]Pacchetti applicazione**, elencati in [Capitolo 1](./chapter-1.md#wknd-mobile-application-packages), sono installati in **Pubblicazione AEM** utilizzo [!UICONTROL Gestione pacchetti].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Pubblica il **[!DNL WKND Mobile Events API]Modello modificabile**
   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**
   1. Seleziona la **[!DNL Event API]** template
   1. Tocca **[!UICONTROL Pubblica]** nella barra delle azioni superiore
   1. Pubblica il **template** e **tutti i riferimenti** (criteri del contenuto, mappature dei criteri del contenuto e modelli)

1. Pubblica il **[!DNL WKND Mobile Events]frammenti di contenuto**.

   Questo è necessario in quanto l’API Eventi utilizza il componente Elenco frammenti di contenuto , che non fa riferimento specifico ai frammenti di contenuto.

   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Seleziona tutti i **[!DNL Event]** frammenti di contenuto
   1. Tocca **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lascia il valore predefinito **Pubblica** azione esistente, tocca **[!UICONTROL Successivo]** nella barra delle azioni superiore
   1. Seleziona **tutto** frammenti di contenuto
   1. Tocca **[!UICONTROL Pubblica]** nella barra delle azioni superiore
      * *La [!DNL Events] Modello frammento di contenuto e fa riferimento a Immagini evento verranno pubblicati automaticamente insieme ai frammenti di contenuto.*

1. Pubblica il **[!DNL Events API]page**.
   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleziona la **[!DNL Events]** page
   1. Tocca **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lascia il valore predefinito **Pubblica** azione esistente, tocca **[!UICONTROL Successivo]** nella barra delle azioni superiore
   1. Seleziona la **[!DNL Events]** page
   1. Tocca **[!DNL Publish]** nella barra delle azioni superiore

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Verifica pubblicazione AEM

1. In un nuovo browser Web, assicurati di essere disconnesso da AEM Publish e richiedi i seguenti URL (sostituendo `http://localhost:4503` per qualsiasi host:port su cui AEM Publish è in esecuzione).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Queste richieste devono restituire la stessa risposta JSON di quando sono stati esaminati i corrispondenti endpoint di AEM Author. In caso contrario, assicurati che tutte le pubblicazioni siano state completate (controlla le code di replica), [!DNL WKND Mobile] `ui.apps` il pacchetto è installato su AEM Publish e controlla il `error.log` per AEM Publish.

## Passaggio successivo

Non ci sono pacchetti aggiuntivi da installare. Assicurati che il contenuto e la configurazione descritti in questa sezione siano pubblicati in AEM Publish, altrimenti i capitoli successivi non funzioneranno.

* [Capitolo 7 - Utilizzo di AEM Content Services da un’app mobile](./chapter-7.md)
