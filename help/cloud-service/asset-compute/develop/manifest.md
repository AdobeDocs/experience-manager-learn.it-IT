---
title: Configurare il file manifest.yml di un progetto Asset compute
description: Il file manifest.yml del progetto di Asset compute descrive tutti i processi di lavoro del progetto da distribuire.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 0%

---

# Configurare manifest.yml

Il `manifest.yml`, che si trova nella directory principale del progetto Asset compute, descrive tutti i processi di lavoro del progetto da distribuire.

![manifest.yml](./assets/manifest/manifest.png)

## Definizione lavoratore predefinita

I lavoratori sono definiti come voci di azioni Adobe I/O Runtime in `actions`, e comprende un set di configurazioni.

I lavoratori che accedono ad altre integrazioni Adobe I/O devono impostare `annotations -> require-adobe-auth` proprietà a `true` come questo [espone le credenziali Adobe I/O del lavoratore](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) tramite `params.auth` oggetto. Questo è in genere necessario quando il lavoratore effettua una chiamata alle API di Adobe I/O, come le API di Adobe Photoshop, Lightroom o Sensei, e può essere attivato per lavoratore.

1. Apri e controlla il lavoratore generato automaticamente `manifest.yml`. I progetti che contengono più lavoratori Asset compute devono definire una voce per ogni lavoratore sotto `actions` array.

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

Ogni lavoratore può configurare [limiti](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) per il relativo contesto di esecuzione in Adobe I/O Runtime. Questi valori devono essere regolati in modo da fornire al lavoratore un dimensionamento ottimale, in base al volume, al tasso e al tipo di risorse che calcolerà, nonché al tipo di lavoro svolto.

Revisione [Adobe di indicazioni sul dimensionamento](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) prima di impostare i limiti. I lavoratori Asset compute possono esaurire la memoria durante l’elaborazione delle risorse, causando l’interruzione dell’esecuzione di Adobe I/O Runtime, in modo da garantire che il lavoratore venga ridimensionato in modo appropriato per gestire tutte le risorse candidate.

1. Aggiungi un `inputs` sezione al nuovo `wknd-asset-compute` voce azioni. Ciò consente di ottimizzare le prestazioni complessive e l&#39;allocazione delle risorse del lavoratore Asset compute.

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

## Il manifesto finito.yml

La versione finale `manifest.yml` ha un aspetto simile a:

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

La versione finale `.manifest.yml` è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Convalida del manifesto.yml

Una volta generato l’Asset compute `manifest.yml` viene aggiornato, esegui lo strumento di sviluppo locale e assicurati che inizi correttamente con `manifest.yml` impostazioni.

Per avviare lo strumento di sviluppo Asset compute per il progetto Asset compute:

1. Apri una riga di comando nella directory principale del progetto di Asset compute (in Codice VS può essere aperto direttamente nell’IDE tramite Terminal > New Terminal) ed esegui il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo Asset compute locale verrà aperto nel browser predefinito all’indirizzo __http://localhost:9000__.

   ![esecuzione dell&#39;app aio](assets/environment-variables/aio-app-run.png)

1. Esaminare l&#39;output della riga di comando e il browser Web per i messaggi di errore durante l&#39;inizializzazione dello strumento di sviluppo.
1. Per arrestare lo strumento di sviluppo Asset compute, tocca `Ctrl-C` nella finestra che ha eseguito `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Rientro YAML non corretto](../troubleshooting.md#incorrect-yaml-indentation)
+ [Il limite memorySize è impostato su un valore troppo basso](../troubleshooting.md#memorysize-limit-is-set-too-low)
