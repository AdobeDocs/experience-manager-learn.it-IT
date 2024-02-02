---
title: Azione Adobe I/O Runtime ed eventi AEM
description: Scopri come ricevere eventi AEM utilizzando l’azione Adobe I/O Runtime e rivedere i dettagli dell’evento come payload, intestazioni e metadati.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
source-git-commit: f0930e517254b6353fe50c3bbf9ae915d9ef6ca3
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---


# Azione Adobe I/O Runtime ed eventi AEM

Scopri come ricevere gli eventi AEM utilizzando [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Agisci e controlla i dettagli dell’evento come payload, intestazioni e metadati.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime è una piattaforma senza server che consente l’esecuzione del codice in risposta a eventi Adobi I/O. In questo modo è possibile creare applicazioni basate su eventi senza preoccuparsi dell&#39;infrastruttura.

In questo esempio, puoi creare un Adobe I/O Runtime [Azione](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) che riceve gli Eventi AEM e registra i dettagli dell’evento.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

I passaggi di alto livello sono i seguenti:

- Creare un progetto nella console di Adobe Developer
- Inizializza progetto per sviluppo locale
- Configurare un progetto nella console di Adobe Developer
- Attivare l’evento AEM e verificare l’esecuzione dell’azione

## Prerequisiti

Per completare questa esercitazione, è necessario:

- Ambiente as a Cloud Service AEM con [Evento AEM abilitato](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Accesso a [Console Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) installato nel computer locale.

>[!IMPORTANT]
>
>L’evento AEM as a Cloud Service è disponibile solo per gli utenti registrati in modalità pre-release. Per abilitare gli eventi AEM nell’ambiente as a Cloud Service AEM, contattare [Squadra AEM-Eventing](mailto:grp-aem-events@adobe.com).

## Creare un progetto nella console di Adobe Developer

Per creare un progetto nella console Adobe Developer, effettua le seguenti operazioni:

- Accedi a [Console Adobe Developer](https://developer.adobe.com/) e fai clic su **Console** pulsante.

- In **Guida rapida** , fare clic su **Crea progetto da modello**. Quindi, nella **Sfoglia modelli** finestra di dialogo, seleziona **App Builder** modello.

- Se necessario, aggiorna il titolo del progetto, il nome dell’app e l’area di lavoro Aggiungi. Quindi, fai clic su **Salva**.

  ![Creare un progetto nella console di Adobe Developer](../assets/examples/runtime-action/create-project.png)


## Inizializza progetto per sviluppo locale

Per aggiungere un&#39;azione Adobe I/O Runtime al progetto, è necessario inizializzare il progetto per lo sviluppo locale. Nel terminale aperto del computer locale, individua il punto in cui inizializzare il progetto e segui la procedura riportata di seguito:

- Inizializza progetto eseguendo

  ```bash
  aio app init
  ```

- Seleziona la `Organization`, il `Project` create nel passaggio precedente e nell&#39;area di lavoro. In entrata `What templates do you want to search for?` passaggio, seleziona `All Templates` opzione.

  ![Org-Project-Selection - Inizializza progetto](../assets/examples/runtime-action/all-templates.png)

- Dall’elenco dei modelli, seleziona `@adobe/generator-app-excshell` opzione.

  ![Modello di estensibilità - Inizializza progetto](../assets/examples/runtime-action/extensibility-template.png)

- Apri il progetto nell’IDE preferito, ad esempio VSCode.

- Il valore selezionato _Modello di estensibilità_ (`@adobe/generator-app-excshell`) fornisce un&#39;azione di runtime generica, il codice è in `src/dx-excshell-1/actions/generic/index.js` file. Aggiorniamolo per semplificarlo, registra i dettagli dell’evento e restituisce una risposta di successo. Tuttavia, nell’esempio successivo, viene migliorato per elaborare gli Eventi AEM ricevuti.

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- Infine, distribuisci l’azione aggiornata in Adobe I/O Runtime eseguendo.

  ```bash
  aio app deploy
  ```

## Configurare un progetto nella console di Adobe Developer

Per ricevere gli eventi AEM ed eseguire l’azione Adobe I/O Runtime creata nel passaggio precedente, configura il progetto nella console Adobe Developer.

- Nella console Adobe Developer, passa a [progetto](https://developer.adobe.com/console/projects) creato nel passaggio precedente e fare clic per aprirlo. Seleziona la `Stage` workspace, è qui che è stata implementata l’azione.

- Clic **Aggiungi servizio** e seleziona **API** opzione. In **Aggiungere un’API** modale, seleziona **Servizi Adobe** > **API di gestione I/O** e fai clic su **Successivo**, segui i passaggi di configurazione aggiuntivi e fai clic su **Salva API configurata**.

  ![Aggiungi servizio - Configura progetto](../assets/examples/runtime-action/add-io-management-api.png)

- Allo stesso modo, fai clic su **Aggiungi servizio** e seleziona **Evento** opzione. In **Aggiungi eventi** finestra di dialogo, seleziona **Experience Cloud** > **AEM Sites** e fai clic su **Successivo**. Segui i passaggi di configurazione aggiuntivi, seleziona l’istanza AEMCS, i tipi di evento e altri dettagli.

- Infine, nella sezione **Come ricevere gli eventi** step, expand **Azione runtime** e selezionare il _generico_ azione creata nel passaggio precedente. Clic **Salva eventi configurati**.

  ![Azione runtime: configurare il progetto ](../assets/examples/runtime-action/select-runtime-action.png)

- Rivedi i dettagli di registrazione dell’evento, nonché **Traccia debug** e verificare il **Sonda di verifica** richiesta e risposta.

  ![Dettagli registrazione evento](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## Attivare gli eventi AEM

Per attivare gli eventi AEM dall’ambiente as a Cloud Service AEM registrato nel progetto Adobe Developer Console di cui sopra, effettua le seguenti operazioni:

- Accesso all’ambiente di authoring as a Cloud Service per l’AEM tramite [Cloud Manager](https://my.cloudmanager.adobe.com/).

- A seconda del **Eventi sottoscritti**, creare, aggiornare, eliminare, pubblicare o annullare la pubblicazione di un frammento di contenuto.

## Rivedi dettagli evento

Dopo aver completato i passaggi precedenti, dovresti vedere gli Eventi AEM consegnati all’azione generica.

Puoi rivedere i dettagli dell’evento in **Traccia debug** scheda dei dettagli di registrazione dell’evento.

![Dettagli evento AEM](../assets/examples/runtime-action/aem-event-details.png)


## Passaggi successivi

Nell’esempio successivo, miglioriamo questa azione per elaborare gli eventi AEM, richiamare il servizio di authoring AEM per ottenere i dettagli del contenuto, archiviare i dettagli nell’archiviazione Adobe I/O Runtime e visualizzarli tramite l’applicazione a pagina singola (SPA).

