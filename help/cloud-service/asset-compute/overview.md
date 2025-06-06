---
title: Estensibilità dei microservizi Asset Compute per AEM as a Cloud Service
description: Questo tutorial guida nella creazione di un processo di lavoro Asset Compute semplice che crea la rappresentazione di una risorsa ritagliando quella originale a forma di cerchio e applicando contrasto e luminosità configurabili. Anche se si tratta di un processo di lavoro di base, questo tutorial lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un processo di lavoro Asset Compute personalizzato, da utilizzare con AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '986'
ht-degree: 100%

---

# Estensibilità dei microservizi Asset Compute

I microservizi Asset Compute di AEM as a Cloud Service supportano lo sviluppo e l’implementazione di processi di lavoro personalizzati utilizzati per leggere e manipolare i dati binari delle risorse archiviate in AEM, più frequentemente per creare rappresentazioni personalizzate delle risorse.

Mentre in AEM 6.x venivano utilizzati i processi di flusso di lavoro AEM personalizzati per leggere, trasformare e riscrivere le rappresentazioni delle risorse, in AEM as a Cloud Service questo viene svolto dai processi di lavoro Asset Compute.

## Che cosa farai

>[!VIDEO](https://video.tv.adobe.com/v/3410022?quality=12&learn=on&captions=ita)

Questo tutorial guida nella creazione di un processo di lavoro Asset Compute semplice che crea la rappresentazione di una risorsa ritagliando quella originale a forma di cerchio e applicando contrasto e luminosità configurabili. Anche se si tratta di un processo di lavoro di base, questo tutorial lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un processo di lavoro Asset Compute personalizzato, da utilizzare con AEM as a Cloud Service.

### Obiettivi {#objective}

1. Provisioning e configurazione degli account e dei servizi necessari per creare e distribuire un processo di lavoro Asset Compute
1. Creare e configurare un progetto Asset Compute
1. Sviluppare un processo di lavoro Asset Compute che genera una rappresentazione personalizzata
1. Scrivere i test e scoprire come eseguire il debug del processo di lavoro Asset Compute personalizzato
1. Distribuire il processo di lavoro Asset Compute e integrarlo con il servizio AEM as a Cloud Service Author tramite i profili di elaborazione

## Configurazione

Scopri come prepararti correttamente per l’estensione dei processi di lavoro Asset Compute, comprendi per quali servizi e account è necessario eseguire il provisioning e configurare e quale software installare localmente per lo sviluppo.

### Provisioning di account e servizi{#accounts-and-services}

I seguenti account e servizi richiedono il provisioning e l’accesso per completare il tutorial, l’ambiente di sviluppo o il programma sandbox di AEM as a Cloud Service e accedere all’archiviazione BLOB di App Builder e Microsoft Azure.

+ [Provisioning di account e servizi](./set-up/accounts-and-services.md)

### Ambiente di sviluppo locale

Lo sviluppo locale di progetti Asset Compute richiede un set di strumenti specifico per gli sviluppatori diverso dallo sviluppo AEM tradizionale, tra cui: Microsoft Visual Studio Code, Docker Desktop, Node.js e il supporto di moduli npm.

+ [Configurare l’ambiente di sviluppo locale](./set-up/development-environment.md)

### App Builder

I progetti Asset Compute sono progetti App Builder appositamente definiti e, in quanto tali, richiedono l’accesso ad App Builder in Adobe Developer Console per poter essere configurati e distribuiti.

+ [Configurare App Builder](./set-up/app-builder.md)

## Sviluppare

Scopri come creare e configurare un progetto Asset Compute e in seguito sviluppare un processo di lavoro personalizzato che generi una rappresentazione personalizzata delle risorse.

### Creare un nuovo progetto Asset Compute

I progetti Asset Compute, che contengono uno o più processi di lavoro Asset Compute, vengono generati utilizzando Adobe I/O CLI interattiva. I progetti Asset Compute sono progetti App Builder appositamente strutturati, che a loro volta sono progetti Node.js.

+ [Creare un nuovo progetto Asset Compute](./develop/project.md)

### Configurare le variabili di ambiente

Le variabili di ambiente vengono mantenute nel file `.env` per lo sviluppo locale e utilizzate per fornire le credenziali di Adobe I/O e dell’archiviazione cloud richieste per lo sviluppo locale.

+ [Configurare le variabili di ambiente](./develop/environment-variables.md)

### Configurare il manifest.yml

I progetti Asset Compute contengono manifesti che definiscono tutti i processi di lavoro Asset Compute all’interno del progetto, nonché le risorse che sono disponibili quando vengono distribuiti in Adobe I/O Runtime per l’esecuzione.

+ [Configurare il manifest.yml](./develop/manifest.md)

### Sviluppare un processo di lavoro

Lo sviluppo di un processo di lavoro Asset Compute è fondamentale per l’estensione dei microservizi Asset Compute, in quanto il processo di lavoro contiene il codice personalizzato che genera, o orchestra, la generazione della rappresentazione della risorsa risultante.

+ [Sviluppare un processo di lavoro Asset Compute](./develop/worker.md)

### Utilizzare lo strumento di sviluppo Asset Compute

Lo strumento di lavoro Asset Compute fornisce un Web harness locale per la distribuzione, l’esecuzione e l’anteprima delle rappresentazioni generate dai processi di lavoro, supportando lo sviluppo rapido e iterativo del processo di lavoro Asset Compute.

+ [Utilizzare lo strumento di sviluppo Asset Compute](./develop/development-tool.md)

## Test e debug

Scopri come testare i processi di lavoro Asset Compute personalizzati per avere la certezza che funzionino e che il debug di tali processi venga eseguito per comprendere e risolvere i problemi relativi all’esecuzione del codice personalizzato.

### Eseguire un test su un processo di lavoro

Asset Compute fornisce un framework di test per la creazione di suite di test per i processi di lavoro, rendendo semplice la definizione di test che garantiscono un comportamento corretto.

+ [Eseguire un test su un processo di lavoro](./test-debug/test.md)

### Debug di un processo di lavoro

I processi di lavoro di Asset Compute forniscono diversi livelli di debug, dall’output tradizionale `console.log(..)` alle integrazioni con __VS Code__ e __wskdebug__, consentendo agli sviluppatori di analizzare il codice del processo di lavoro durante l’esecuzione in tempo reale.

+ [Debug di un processo di lavoro](./test-debug/debug.md)

## Distribuzione

Scopri come integrare i processi di lavoro Asset Compute personalizzati con AEM as a Cloud Service, distribuendoli prima in Adobe I/O Runtime e richiamandoli dall’istanza di authoring di AEM as a Cloud Service tramite i Profili elaborazione di AEM Assets.

### Distribuzione in Adobe I/O Runtime

I processi di lavoro di Asset Compute devono essere distribuiti in Adobe I/O Runtime per poter essere utilizzati con AEM as a Cloud Service.

+ [Utilizzare i Profili elaborazione](./deploy/runtime.md)

### Integrare i processi di lavoro tramite i Profili elaborazione AEM

Una volta distribuiti in Adobe I/O Runtime, i processi di lavoro Asset Compute possono essere registrati in AEM as a Cloud Service tramite i [Profili elaborazione delle risorse](../../assets/configuring/processing-profiles.md). I Profili elaborazione vengono, a loro volta, applicati alle cartelle di risorse che si applicano alle risorse in esse contenute.

+ [Integrare con i Profili elaborazione di AEM](./deploy/processing-profiles.md)

## Avanzato

Questi tutorial in forma ridotta affrontano casi d’uso più avanzati partendo dall’apprendimento di base definito nei capitoli precedenti.

+ [Sviluppare un processo di lavoro metadati di Asset Compute](./advanced/metadata.md) in grado di riscrivere i metadati in

## Codebase su Github

Il codebase del tutorial è disponibile su Github all’indirizzo:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ ramo principale

Il codice sorgente non contiene i file `.env` o `config.json` richiesti. Questi devono essere aggiunti e configurati utilizzando le informazioni sugli [account e servizi](#accounts-and-services).

## Risorse aggiuntive

Di seguito sono riportate diverse risorse di Adobe che forniscono ulteriori informazioni, API e SDK utili per lo sviluppo dei processi di lavoro Asset Compute.

### Documentazione

+ [Documentazione del servizio Asset Compute](https://experienceleague.adobe.com/it/docs/asset-compute/using/extend/understand-extensibility)
+ [File readme dello strumento di sviluppo Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [Processi di lavoro Asset Compute di esempio](https://github.com/adobe/asset-compute-example-workers)

### API e SDK

+ [SDK di Asset Compute](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Libreria cloud wrapper Blobstore Adobe](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Libreria nuovi tentativi recupero nodo Adobe](https://github.com/adobe/node-fetch-retry)
+ [Processi di lavoro Asset Compute di esempio](https://github.com/adobe/asset-compute-example-workers)
