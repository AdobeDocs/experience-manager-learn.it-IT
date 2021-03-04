---
title: Estensibilità dei microservizi Asset Compute per AEM as a Cloud Service
description: Questa esercitazione illustra la creazione di un semplice processo di lavoro Asset Compute che crea un rendering delle risorse ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il processo di lavoro sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un processo di lavoro Asset Compute personalizzato da utilizzare con AEM as a Cloud Service.
feature: Microservizi Asset Compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrazioni, Sviluppo
role: Developer (Sviluppatore)
level: Intermedio, esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 0%

---


# Estensibilità dei microservizi Asset Compute

I microservizi Asset Compute di AEM as a Cloud Service supportano lo sviluppo e l’implementazione di processi di lavoro personalizzati che consentono di leggere e manipolare i dati binari delle risorse memorizzate in AEM, in genere, per creare rappresentazioni personalizzate delle risorse.

Mentre in AEM 6.x i processi di flusso di lavoro AEM personalizzati sono stati utilizzati per leggere, trasformare e riscrivere le rappresentazioni delle risorse, in AEM as a Cloud Service Asset Compute i processi di lavoro soddisfano questa esigenza.

## Cosa farai

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Questa esercitazione illustra la creazione di un semplice processo di lavoro Asset Compute che crea un rendering delle risorse ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il processo di lavoro sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un processo di lavoro Asset Compute personalizzato da utilizzare con AEM as a Cloud Service.

### Obiettivi {#objective}

1. Provisioning e configurazione degli account e dei servizi necessari per creare e distribuire un processo di lavoro Asset Compute
1. Creare e configurare un progetto Asset Compute
1. Sviluppa un processo di lavoro Asset Compute che genera un rendering personalizzato
1. Creazione di test per e apprendimento di come eseguire il debug del processo di lavoro Asset Compute personalizzato
1. Distribuire il processo di lavoro Asset Compute e integrarlo con il servizio Author di AEM as a Cloud Service tramite Profili di elaborazione

## Configurazione

Scopri come prepararsi in modo appropriato all’estensione dei processi di lavoro di Asset Compute e quali servizi e account devono essere forniti e configurati e software installati localmente per lo sviluppo.

### Provisioning di account e servizi{#accounts-and-services}

I seguenti account e servizi richiedono il provisioning e l’accesso a per completare l’esercitazione, l’ambiente di sviluppo AEM as a Cloud Service o il programma Sandbox, l’accesso ad Adobe Project Firefly e l’archiviazione BLOB di Microsoft Azure.

+ [Prestazione di account e servizi](./set-up/accounts-and-services.md)

### Ambiente di sviluppo locale

Lo sviluppo locale di progetti Asset Compute richiede un set di strumenti per sviluppatori specifico, diverso dallo sviluppo AEM tradizionale, che include: Microsoft Visual Studio Code, Docker Desktop, Node.js e il supporto di moduli npm.

+ [Configurare l&#39;ambiente di sviluppo locale](./set-up/development-environment.md)

### Adobe Project Firefly

I progetti Asset Compute sono progetti Adobe Project Firefly appositamente definiti e, come tali, richiedono l’accesso ad Adobe Project Firefly in Adobe Developer Console per configurarli e distribuirli.

+ [Configurazione Adobe Project Firefly](./set-up/firefly.md)

## Sviluppa

Scopri come creare e configurare un progetto Asset Compute e quindi sviluppare un processo di lavoro personalizzato per la generazione di un rendering delle risorse personalizzato.

### Creare un nuovo progetto Asset Compute

I progetti Asset Compute, che contengono uno o più processi di lavoro Asset Compute, vengono generati utilizzando l’interfaccia CLI interattiva di Adobe I/O. I progetti Asset Compute sono progetti Adobe Project Firefly appositamente strutturati, che a loro volta sono progetti Node.js.

+ [Creare un nuovo progetto Asset Compute](./develop/project.md)

### Configurare le variabili di ambiente

Le variabili dell’ambiente vengono mantenute nel file `.env` per lo sviluppo locale e vengono utilizzate per fornire le credenziali di Adobe I/O e dell’archiviazione cloud richieste per lo sviluppo locale.

+ [Configurare le variabili di ambiente](./develop/environment-variables.md)

### Configura il file manifest.yml

I progetti Asset Compute contengono manifesti che definiscono tutti i processi di lavoro Asset Compute contenuti nel progetto, nonché le risorse disponibili quando vengono distribuiti in Adobe I/O Runtime per l’esecuzione.

+ [Configura il file manifest.yml](./develop/manifest.md)

### Sviluppare un processo di lavoro

Lo sviluppo di un processo di lavoro Asset Compute è il nucleo dell’estensione dei microservizi Asset Compute, in quanto il processo di lavoro contiene il codice personalizzato che genera, o orchestra, la generazione del rendering risultante delle risorse.

+ [Sviluppare un processo di lavoro Asset Compute](./develop/worker.md)

### Utilizzare lo strumento di sviluppo Asset Compute

Lo strumento di sviluppo Asset Compute fornisce un cablaggio web locale per distribuire, eseguire e visualizzare in anteprima rappresentazioni generate dai processi di lavoro e supportare lo sviluppo rapido e iterativo dei processi di lavoro di Asset Compute.

+ [Utilizzare lo strumento di sviluppo Asset Compute](./develop/development-tool.md)

## Test e debug

Scopri come testare i processi di lavoro Asset Compute personalizzati in modo che siano sicuri del loro funzionamento e come eseguire il debug dei processi di lavoro Asset Compute per comprendere e risolvere i problemi relativi all’esecuzione del codice personalizzato.

### Test di un processo di lavoro

Asset Compute fornisce un framework di test per la creazione di suite di test per i lavoratori, rendendo più semplice la definizione di test che garantiscano un comportamento corretto.

+ [Test di un processo di lavoro](./test-debug/test.md)

### Debug di un processo di lavoro

I processi di lavoro di Asset Compute forniscono vari livelli di debug dall’output tradizionale `console.log(..)` alle integrazioni con __VS Code__ e __wskdebug__, consentendo agli sviluppatori di passare attraverso il codice di lavoro mentre viene eseguito in tempo reale.

+ [Debug di un processo di lavoro](./test-debug/debug.md)

## Implementa

Scopri come integrare i processi di lavoro Asset Compute personalizzati con AEM as a Cloud Service, prima distribuendoli in Adobe I/O Runtime e poi richiamandoli da AEM as a Cloud Service Author tramite i profili di elaborazione di AEM Assets.

### Distribuzione ad Adobe I/O Runtime

Per poter essere utilizzati con AEM as a Cloud Service, i processi di lavoro di Asset Compute devono essere implementati in Adobe I/O Runtime.

+ [Utilizzo dei profili di elaborazione](./deploy/runtime.md)

### Integrare i processi di lavoro tramite i profili di elaborazione AEM

Una volta implementati in Adobe I/O Runtime, i processi di lavoro di Asset Compute possono essere registrati in AEM as a Cloud Service tramite [Profili di elaborazione delle risorse](../../assets/configuring/processing-profiles.md). I profili di elaborazione vengono, a loro volta, applicati alle cartelle di risorse che si applicano alle risorse in esse contenute.

+ [Integrazione con i profili di elaborazione AEM](./deploy/processing-profiles.md)

## Avanzate 

Questi tutorial abbreviati affrontano casi d’uso più avanzati sulla base di insegnamenti fondamentali stabiliti nei capitoli precedenti.

+ [Sviluppare un ](./advanced/metadata.md) workerer di metadati Asset Compute in grado di riscrivere i metadati

## Codebase su Github

La base di codice dell’esercitazione è disponibile su Github all’indirizzo:

+ [ramo principale adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

Il codice sorgente non contiene i file `.env` o `config.json` richiesti. Questi devono essere aggiunti e configurati utilizzando le informazioni [account e servizi](#accounts-and-services) .

## Altro materiale di riferimento

Di seguito sono riportate diverse risorse Adobe che forniscono ulteriori informazioni e API e SDK utili per lo sviluppo di processi di lavoro Asset Compute.

### Documentazione

+ [Documentazione di Asset Compute Service](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [File readme dello strumento di sviluppo Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [Lavoratori di esempio di Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### API e SDK

+ [SDK di Asset Compute](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Libreria Wrapper di Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Libreria di tentativi di recupero dei nodi Adobe](https://github.com/adobe/node-fetch-retry)
+ [Lavoratori di esempio di Asset Compute](https://github.com/adobe/asset-compute-example-workers)
