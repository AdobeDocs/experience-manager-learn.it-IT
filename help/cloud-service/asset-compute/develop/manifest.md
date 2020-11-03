---
title: Configurare manifest.yml di un progetto Asset Compute
description: Il file manifest.yml del progetto Asset Compute descrive tutti i lavoratori del progetto da distribuire.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Configurare manifest.yml

Nella `manifest.yml`directory principale del progetto Asset Compute vengono descritti tutti i lavoratori del progetto da distribuire.

![manifest.yml](./assets/manifest/manifest.png)

## Definizione di lavoratore predefinita

I lavoratori sono definiti come voci dell&#39;azione Adobe I/O Runtime in `actions`e sono composti di un insieme di configurazioni.

I lavoratori che accedono ad altre integrazioni di I/O  Adobe devono impostare la `annotations -> require-adobe-auth` proprietà su `true` perché [espone le credenziali](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) di I/O  Adobe del lavoratore tramite l&#39; `params.auth` oggetto. Questo è in genere richiesto quando il lavoratore chiama  API I/O di Adobe, come le API Adobe Photoshop , Lightroom o Sensei, e può essere attivato per ogni lavoratore.

1. Aprire e rivedere il lavoratore generato automaticamente `manifest.yml`. I progetti che contengono più lavoratori di elaborazione risorse devono definire una voce per ciascun lavoratore all&#39;interno dell&#39; `actions` array.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## Definire i limiti

Ogni lavoratore può configurare i [limiti](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) per il proprio contesto di esecuzione in Adobe I/O Runtime. Questi valori devono essere sintonizzati per fornire un dimensionamento ottimale per il lavoratore, in base al volume, al tasso e al tipo di risorse che elaborerà, nonché al tipo di lavoro che esegue.

Rivedete [guida](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) al ridimensionamento del Adobe prima di impostare i limiti. I lavoratori di elaborazione delle risorse possono esaurire la memoria durante l&#39;elaborazione delle risorse, causando l&#39;arresto dell&#39;esecuzione Adobe I/O Runtime, pertanto accertatevi che il lavoratore sia ridimensionato in modo appropriato per gestire tutte le risorse candidate.

1. Aggiungere una `inputs` sezione alla nuova voce `wknd-asset-compute` Azioni. Questo consente di ottimizzare le prestazioni complessive e l&#39;allocazione delle risorse del lavoratore di elaborazione delle risorse.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## manifest.yml completato

L&#39; `manifest.yml` aspetto finale è:

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.yml su Github

La finale `.manifest.yml` è disponibile su Github al seguente indirizzo:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Convalida del file manifest.yml

Una volta aggiornato il calcolo risorse generato, `manifest.yml` eseguite lo strumento di sviluppo locale e assicuratevi che inizi correttamente con le `manifest.yml` impostazioni aggiornate.

Per avviare lo strumento di sviluppo del calcolo delle risorse per il progetto Asset Compute:

1. Aprite una riga di comando nella directory principale del progetto Asset Compute (in Codice VS questo può essere aperto direttamente nell’IDE tramite Terminal > Nuovo terminale) ed eseguite il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo del calcolo risorse locale si aprirà nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![avvio app aio](assets/environment-variables/aio-app-run.png)

1. Osservate l&#39;output della riga di comando e il browser Web per visualizzare i messaggi di errore durante l&#39;inizializzazione dello strumento di sviluppo.
1. Per arrestare lo strumento di sviluppo di calcolo risorse, toccate `Ctrl-C` nella finestra che è stata eseguita `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Rientro YAML errato](../troubleshooting.md#incorrect-yaml-indentation)
+ [il limite memorySize è impostato su low](../troubleshooting.md#memorysize-limit-is-set-too-low)
