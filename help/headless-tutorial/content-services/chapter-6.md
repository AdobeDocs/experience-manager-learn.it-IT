---
title: Capitolo 6 - Esposizione dei contenuti su AEM Publish as JSON - Content Services
description: Il capitolo 6 del tutorial AEM headless descrive come verificare che tutti i pacchetti, la configurazione e i contenuti necessari siano disponibili su AEM Publish per consentire l’utilizzo dall’app mobile.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Capitolo 6 - Esporre i contenuti della pubblicazione AEM per la distribuzione

Il capitolo 6 del tutorial AEM Headless copre la verifica che tutti i pacchetti, la configurazione e i contenuti necessari siano disponibili su AEM Publish per consentire l’utilizzo da parte dell’app mobile.

## Pubblicazione dei contenuti per AEM Content Services

La configurazione e il contenuto creati per indirizzare gli Eventi tramite AEM Content Services devono essere pubblicati in AEM Publish in modo che l’app mobile possa accedervi.

Poiché AEM Content Services è creato da Configurazione (Modelli per frammenti di contenuto, Modelli modificabili), Risorse (Frammenti di contenuto, Immagini) e Pagine, tutti questi elementi godono automaticamente delle funzionalità di gestione dei contenuti AEM, tra cui:

* Flusso di lavoro per revisione ed elaborazione
* e attivazione/disattivazione per inviare e richiamare contenuti dagli endpoint di AEM Content Services della pubblicazione AEM

1. Assicurati che **[!DNL WKND Mobile]Pacchetti applicazioni**, elencati in [Capitolo 1](./chapter-1.md#wknd-mobile-application-packages), sono installati in **Pubblicazione AEM** utilizzo [!UICONTROL Gestione pacchetti].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Pubblica il **[!DNL WKND Mobile Events API]Modello modificabile**
   1. Accedi a **[!UICONTROL AEM] > [!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**
   1. Seleziona la **[!DNL Event API]** modello
   1. Tocca **[!UICONTROL Pubblica]** nella barra delle azioni superiore
   1. Pubblica il **modello** e **tutti i riferimenti** (criteri per i contenuti, mapping dei criteri per i contenuti e modelli)

1. Pubblica il **[!DNL WKND Mobile Events]frammenti di contenuto**.

   Questo è necessario in quanto l’API degli eventi utilizza il componente Elenco frammenti di contenuto, che non fa specifico riferimento a Frammenti di contenuto.

   1. Accedi a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Seleziona tutte le opzioni **[!DNL Event]** frammenti di contenuto
   1. Tocca il **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lasciando l&#39;impostazione predefinita **Pubblica** azione così com’è, tocca **[!UICONTROL Successivo]** nella barra delle azioni superiore
   1. Seleziona **tutto** frammenti di contenuto
   1. Tocca **[!UICONTROL Pubblica]** nella barra delle azioni superiore
      * *Il [!DNL Events] Il modello per frammenti di contenuto e i riferimenti a immagini evento verranno pubblicati automaticamente insieme ai frammenti di contenuto.*

1. Pubblica il **[!DNL Events API]pagina**.
   1. Accedi a **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleziona la **[!DNL Events]** pagina
   1. Tocca il **[!UICONTROL Gestisci pubblicazione]** nella barra delle azioni superiore
   1. Lasciando l&#39;impostazione predefinita **Pubblica** azione così com’è, tocca **[!UICONTROL Successivo]** nella barra delle azioni superiore
   1. Seleziona la **[!DNL Events]** pagina
   1. Tocca **[!DNL Publish]** nella barra delle azioni superiore

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Verifica pubblicazione AEM

1. In un nuovo browser web, accertati di aver effettuato la disconnessione da Pubblicazione AEM e richiedi i seguenti URL (sostituendo `http://localhost:4503` per qualsiasi host:porta su cui è in esecuzione Pubblicazione AEM).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Queste richieste devono restituire la stessa risposta JSON di quando sono stati esaminati gli end-point corrispondenti di AEM Author. In caso contrario, verificare che tutte le pubblicazioni siano state completate (controllare le code di replica), [!DNL WKND Mobile] `ui.apps` viene installato nella pubblicazione AEM e rivedi il `error.log` per pubblicazione AEM.

## Passaggio successivo

Nessun pacchetto aggiuntivo da installare. Assicurati che il contenuto e la configurazione descritti in questa sezione siano pubblicati in Pubblicazione AEM, altrimenti i capitoli successivi non funzioneranno.

* [Capitolo 7 - Utilizzo di servizi di contenuti AEM da un’app mobile](./chapter-7.md)
