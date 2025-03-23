---
title: Configurare il file manifest.yml di un progetto Asset Compute
description: Il file manifest.yml del progetto Asset Compute descrive tutti i processi di lavoro del progetto da distribuire.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# Configurare manifest.yml

`manifest.yml`, che si trova nella directory principale del progetto Asset Compute, descrive tutti i processi di lavoro di questo progetto da distribuire.

![manifest.yml](./assets/manifest/manifest.png)

## Definizione lavoratore predefinita

I processi di lavoro sono definiti come voci di azioni Adobe I/O Runtime in `actions` e sono costituiti da un set di configurazioni.

I processi di lavoro che accedono ad altre integrazioni Adobe I/O devono impostare la proprietà `annotations -> require-adobe-auth` su `true` in quanto [espone le credenziali Adobe I/O del processo di lavoro](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) tramite l&#39;oggetto `params.auth`. Questo è in genere necessario quando il lavoratore effettua una chiamata alle API di Adobe I/O, come le API di Adobe Photoshop, Lightroom o Sensei, e può essere attivato per lavoratore.

1. Aprire e rivedere il processo di lavoro generato automaticamente `manifest.yml`. I progetti che contengono più processi di lavoro Asset Compute devono definire una voce per ogni processo di lavoro sotto l&#39;array `actions`.

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

Ogni lavoratore può configurare i [limiti](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) per il proprio contesto di esecuzione in Adobe I/O Runtime. Questi valori devono essere regolati in modo da fornire al lavoratore un dimensionamento ottimale, in base al volume, al tasso e al tipo di risorse che calcolerà, nonché al tipo di lavoro svolto.

Rivedi le [linee guida per il dimensionamento di Adobe](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) prima di impostare i limiti. I processi di lavoro di Asset Compute possono esaurire la memoria durante l’elaborazione delle risorse, causando l’interruzione dell’esecuzione di Adobe I/O Runtime, in modo da garantire che il processo di lavoro venga ridimensionato in modo appropriato per gestire tutte le risorse candidate.

1. Aggiungere una sezione `inputs` alla nuova voce delle azioni `wknd-asset-compute`. Questo consente di ottimizzare le prestazioni complessive e l’allocazione delle risorse del processo di lavoro Asset Compute.

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

L&#39;aspetto finale di `manifest.yml` è il seguente:

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

Il `.manifest.yml` finale è disponibile su Github all&#39;indirizzo:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Convalida del manifesto.yml

Una volta aggiornato l&#39;Asset Compute `manifest.yml` generato, eseguire lo strumento di sviluppo locale e verificare che inizi correttamente con le impostazioni `manifest.yml` aggiornate.

Per avviare Asset Compute Development Tool per il progetto Asset Compute:

1. Apri una riga di comando nella directory principale del progetto Asset Compute (in VS Code può essere aperta direttamente nell’IDE tramite Terminal > New Terminal) ed esegui il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo Asset Compute locale verrà aperto nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![esecuzione app aio](assets/environment-variables/aio-app-run.png)

1. Esaminare l&#39;output della riga di comando e il browser Web per i messaggi di errore durante l&#39;inizializzazione dello strumento di sviluppo.
1. Per arrestare lo strumento di sviluppo Asset Compute, tocca `Ctrl-C` nella finestra che ha eseguito `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [Rientro YAML non corretto](../troubleshooting.md#incorrect-yaml-indentation)
+ [Il limite memorySize è impostato su un valore troppo basso](../troubleshooting.md#memorysize-limit-is-set-too-low)
