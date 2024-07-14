---
title: Capitolo 6 - Esposizione dei contenuti su AEM Publish come JSON - Content Services
description: Il capitolo 6 del tutorial AEM headless descrive come verificare che tutti i pacchetti, la configurazione e i contenuti necessari siano disponibili sul Publish AEM per consentire l’utilizzo dall’app mobile.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 196
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Capitolo 6 - Presentazione dei contenuti del Publish AEM

Il capitolo 6 del tutorial AEM headless descrive come verificare che tutti i pacchetti, la configurazione e i contenuti necessari siano disponibili sul Publish AEM per consentire l’utilizzo da parte dell’app mobile.

## Pubblicazione dei contenuti per AEM Content Services

La configurazione e il contenuto creati per indirizzare gli Eventi attraverso i Content Services dell’AEM devono essere pubblicati su Publish dell’AEM in modo che l’app mobile possa accedervi.

Poiché AEM Content Services è creato da Configurazione (Modelli per frammenti di contenuto, Modelli modificabili), Assets (Frammenti di contenuto, Immagini) e Pagine, tutti questi elementi godono automaticamente delle funzionalità di gestione dei contenuti AEM, tra cui:

* Flusso di lavoro per revisione ed elaborazione
* e attivazione/disattivazione per inviare e richiamare contenuti dagli endpoint di AEM Content Services di Publish dell’AEM

1. Verificare che i **[!DNL WKND Mobile]pacchetti applicazione**, elencati nel [capitolo 1](./chapter-1.md#wknd-mobile-application-packages), siano installati nel **Publish AEM** utilizzando [!UICONTROL Gestione pacchetti].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publish il **[!DNL WKND Mobile Events API]modello modificabile**
   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**
   1. Seleziona il modello **[!DNL Event API]**
   1. Tocca **[!UICONTROL Publish]** nella barra delle azioni superiore
   1. Publish il **modello** e **tutti i riferimenti** (criteri contenuto, mapping criteri contenuto e modelli)

1. Publish i **[!DNL WKND Mobile Events]frammenti di contenuto**.

   Questo è necessario in quanto l’API degli eventi utilizza il componente Elenco frammenti di contenuto, che non fa specifico riferimento a Frammenti di contenuto.

   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Seleziona tutti i **[!DNL Event]** frammenti di contenuto
   1. Tocca **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lasciando invariata l&#39;azione predefinita **Publish**, tocca **[!UICONTROL Avanti]** nella barra delle azioni superiore
   1. Seleziona **tutti** i frammenti di contenuto
   1. Tocca **[!UICONTROL Publish]** nella barra delle azioni superiore
      * *Il modello per frammenti di contenuto [!DNL Events] e le immagini evento di riferimento verranno pubblicati automaticamente insieme ai frammenti di contenuto.*

1. Publish la pagina **[!DNL Events API]**.
   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Siti] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleziona la pagina **[!DNL Events]**
   1. Tocca **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lasciando invariata l&#39;azione predefinita **Publish**, tocca **[!UICONTROL Avanti]** nella barra delle azioni superiore
   1. Seleziona la pagina **[!DNL Events]**
   1. Tocca **[!DNL Publish]** nella barra delle azioni superiore

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Verifica AEM Publish

1. In un nuovo browser Web, verificare di aver effettuato la disconnessione da AEM Publish e richiedere i seguenti URL (sostituendo `http://localhost:4503` per qualsiasi host:porta AEM Publish in esecuzione).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Queste richieste devono restituire la stessa risposta JSON di quando sono stati esaminati gli end-point corrispondenti di AEM Author. In caso contrario, verificare che tutte le pubblicazioni siano state completate correttamente (controllare le code di replica), che il pacchetto [!DNL WKND Mobile] `ui.apps` sia installato nel Publish AEM e che `error.log` sia visualizzato nel Publish AEM.

## Passaggio successivo

Nessun pacchetto aggiuntivo da installare. Assicurati che il contenuto e la configurazione descritti in questa sezione siano pubblicati in AEM Publish, altrimenti i capitoli successivi non funzioneranno.

* [Capitolo 7 - Utilizzo di servizi di contenuti AEM da un’app mobile](./chapter-7.md)
