---
title: Configurare il file manifest.yml di un progetto di Asset compute
description: Il file manifest.yml del progetto di Asset compute descrive tutti i processi di lavoro di questo progetto da distribuire.
feature: Microservizi di Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integrazioni, Sviluppo
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# Configura il file manifest.yml

Il `manifest.yml`, che si trova nella directory principale del progetto di Asset compute, descrive tutti i processi di lavoro del progetto da distribuire.

![manifest.yml](./assets/manifest/manifest.png)

## Definizione predefinita del processo di lavoro

I lavoratori sono definiti come voci di azione Adobe I/O Runtime in `actions` e sono composti da un set di configurazioni.

I lavoratori che accedono ad altre integrazioni di Adobe I/O devono impostare la proprietà `annotations -> require-adobe-auth` su `true` in quanto questo [espone le credenziali Adobi I/O del lavoratore](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) tramite l&#39;oggetto `params.auth` . Questo è tipicamente necessario quando il lavoratore chiama API di Adobe I/O come le API Adobe Photoshop, Lightroom o Sensei e può essere attivato per ogni lavoratore.

1. Apri e rivedi il processo di lavoro generato automaticamente `manifest.yml`. I progetti che contengono più processi di lavoro di Asset compute devono definire una voce per ciascun processo di lavoro sotto la matrice `actions`.

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

Ogni processo di lavoro può configurare i [limiti](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) per il proprio contesto di esecuzione in Adobe I/O Runtime. Questi valori devono essere regolati in modo da fornire un dimensionamento ottimale per il lavoratore, in base al volume, al tasso e al tipo di risorse che elabora, nonché al tipo di lavoro che esegue.

Rivedi [Guida alle dimensioni degli Adobi](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) prima di impostare i limiti. I processi di lavoro di Asset compute possono esaurire la memoria durante l’elaborazione delle risorse, causando l’interruzione dell’esecuzione di Adobe I/O Runtime. In tal modo, assicurati che il processo di lavoro sia dimensionato in modo appropriato per gestire tutte le risorse candidate.

1. Aggiungi una sezione `inputs` alla nuova voce di azioni `wknd-asset-compute`. Ciò consente di ottimizzare le prestazioni complessive del lavoratore Asset compute e l&#39;allocazione delle risorse.

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

## Il file manifest.yml finito

L&#39;aspetto finale `manifest.yml` è il seguente:

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

L’ ultima versione `.manifest.yml` è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Convalida del file manifest.yml

Una volta aggiornato l&#39;Asset compute generato `manifest.yml`, esegui lo strumento di sviluppo locale e assicurati che venga avviato con successo con le impostazioni aggiornate `manifest.yml`.

Per avviare Asset compute Development Tool per il progetto Asset compute:

1. Apri una riga di comando nella directory principale del progetto di Asset compute (in Codice VS che può essere aperta direttamente nell’IDE tramite Terminal > Nuovo terminale) ed esegui il comando:

   ```
   $ aio app run
   ```

1. Lo strumento di sviluppo Asset compute locale si aprirà nel browser Web predefinito all&#39;indirizzo __http://localhost:9000__.

   ![esecuzione di app aio](assets/environment-variables/aio-app-run.png)

1. Osserva l’output della riga di comando e il browser Web per verificare la presenza di messaggi di errore durante l’inizializzazione dello strumento di sviluppo.
1. Per interrompere lo strumento di sviluppo Asset compute, tocca `Ctrl-C` nella finestra che ha eseguito `aio app run` per terminare il processo.

## Risoluzione dei problemi

+ [rientro YAML errato](../troubleshooting.md#incorrect-yaml-indentation)
+ [il limite memorySize è impostato troppo basso](../troubleshooting.md#memorysize-limit-is-set-too-low)
