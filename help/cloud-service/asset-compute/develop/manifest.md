---
title: 'Configurare manifest.yml di un progetto di Asset compute '
description: Il file manifest.yml del progetto del Asset compute  descrive tutti i lavoratori del progetto da distribuire.
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

La `manifest.yml`, situata nella directory principale del progetto del Asset compute , descrive tutti i lavoratori del progetto da distribuire.

![manifest.yml](./assets/manifest/manifest.png)

## Definizione di lavoratore predefinita

I lavoratori sono definiti come voci dell&#39;azione Adobe I/O Runtime in `actions` e sono composti di un insieme di configurazioni.

I lavoratori che accedono ad altre  integrazioni Adobe I/O devono impostare la proprietà `annotations -> require-adobe-auth` su `true`, in quanto [espone le credenziali Adobe I/O  del lavoratore](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) tramite l&#39;oggetto `params.auth`. Questo è in genere richiesto quando il lavoratore chiama  le API Adobe I/O, come le API Adobe Photoshop , Lightroom o Sensei, e può essere attivato per ogni lavoratore.

1. Aprire e rivedere il lavoratore generato automaticamente `manifest.yml`. I progetti che contengono più lavoratori  Asset compute devono definire una voce per ciascun lavoratore nell&#39;array `actions`.

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

Rivedete [ guida al ridimensionamento del Adobe](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) prima di impostare i limiti.  lavoratori Asset compute possono esaurire la memoria durante l&#39;elaborazione delle risorse, causando l&#39;arresto dell&#39;esecuzione Adobe I/O Runtime, pertanto accertatevi che il lavoratore sia ridimensionato in modo appropriato per gestire tutte le risorse candidate.

1. Aggiungete una sezione `inputs` alla nuova voce di azioni `wknd-asset-compute`. Questo consente di ottimizzare le prestazioni complessive del lavoratore del Asset compute  e l&#39;allocazione delle risorse.

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

L&#39;aspetto finale `manifest.yml` è simile a:

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

L&#39;ultima `.manifest.yml` è disponibile su Github all&#39;indirizzo:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Convalida del file manifest.yml

Una volta aggiornato l&#39;Asset compute  generato `manifest.yml`, eseguire lo strumento di sviluppo locale e assicurarsi che venga avviato correttamente con le impostazioni `manifest.yml` aggiornate.

Per avviare  Strumento di sviluppo Asset compute per il progetto del Asset compute :

1. Aprite una riga di comando nella radice del progetto del Asset compute  (in Codice VS questo può essere aperto direttamente nell&#39;IDE tramite Terminal > Nuovo terminale) ed eseguite il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo  Asset compute locale si aprirà nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![avvio app aio](assets/environment-variables/aio-app-run.png)

1. Osservate l&#39;output della riga di comando e il browser Web per visualizzare i messaggi di errore durante l&#39;inizializzazione dello strumento di sviluppo.
1. Per arrestare lo strumento di sviluppo  Asset compute, toccare `Ctrl-C` nella finestra che ha eseguito `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Rientro YAML errato](../troubleshooting.md#incorrect-yaml-indentation)
+ [il limite memorySize è impostato su low](../troubleshooting.md#memorysize-limit-is-set-too-low)
