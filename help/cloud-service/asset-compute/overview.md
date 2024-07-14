---
title: Estensibilità dei microservizi Asset compute per AEM as a Cloud Service
description: Questo tutorial illustra la creazione di un semplice processo di lavoro in Asset compute che crea il rendering di una risorsa ritagliando la risorsa originale in un cerchio e applicando contrasto e luminosità configurabili. Sebbene il worker sia di base, questo tutorial lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un worker Asset Compute personalizzato da utilizzare con AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Estensibilità dei microservizi Asset compute

I microservizi di Asset compute dell’AEM come Cloud Service supportano lo sviluppo e l’implementazione di processi di lavoro personalizzati che vengono utilizzati per leggere e manipolare i dati binari delle risorse memorizzate nell’AEM, in genere per creare rappresentazioni personalizzate delle risorse.

Mentre in AEM 6.x venivano utilizzati processi di flusso di lavoro AEM personalizzati per leggere, trasformare e riscrivere le rappresentazioni delle risorse, in AEM as a Cloud Service gli Asset Compute worker soddisfano questa necessità.

## Come procedere

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Questo tutorial illustra la creazione di un semplice processo di lavoro in Asset compute che crea il rendering di una risorsa ritagliando la risorsa originale in un cerchio e applicando contrasto e luminosità configurabili. Sebbene il worker sia di base, questo tutorial lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un worker Asset Compute personalizzato da utilizzare con AEM as a Cloud Service.

### Obiettivi {#objective}

1. Fornire e configurare gli account e i servizi necessari per creare e distribuire un lavoratore Asset compute
1. Creazione e configurazione di un progetto Asset Compute
1. Sviluppa un processo di lavoro Asset compute che genera una rappresentazione personalizzata
1. Scrivi i test per e scopri come eseguire il debug di un processo di lavoro Asset Compute personalizzato
1. Distribuire il processo di lavoro Asset Compute e integrarlo con il servizio AEM as a Cloud Service Author tramite Profili di elaborazione

## Configurazione

Scopri come prepararti in modo appropriato all’estensione dei processi di lavoro per gli Asset compute e quali servizi e account devono essere predisposti e configurati e quale software deve essere installato in locale per lo sviluppo.

### Provisioning di account e servizi{#accounts-and-services}

I seguenti account e servizi richiedono il provisioning e l’accesso a per completare l’esercitazione, l’ambiente di sviluppo AEM as a Cloud Service o il programma sandbox, e accedere all’archiviazione BLOB di App Builder e Microsoft Azure.

+ [Fornire account e servizi](./set-up/accounts-and-services.md)

### Ambiente di sviluppo locale

Lo sviluppo locale di progetti Asset Compute richiede un set di strumenti specifico per gli sviluppatori, diverso dallo sviluppo AEM tradizionale, tra cui: Microsoft Visual Studio Code, Docker Desktop, Node.js e il supporto di moduli npm.

+ [Configurare l’ambiente di sviluppo locale](./set-up/development-environment.md)

### App Builder

I progetti di Asset compute sono progetti App Builder appositamente definiti e, per configurarli e distribuirli, hanno bisogno dell’accesso ad App Builder in Adobe Developer Console.

+ [Configurare App Builder](./set-up/app-builder.md)

## Sviluppa

Scopri come creare e configurare un progetto Asset Compute e quindi sviluppare un processo di lavoro personalizzato che generi una rappresentazione personalizzata delle risorse.

### Crea un nuovo progetto Asset Compute

I progetti di Asset compute, che contengono uno o più processi di lavoro di Asset compute, vengono generati utilizzando l&#39;interfaccia CLI interattiva di Adobe I/O. I progetti di Asset compute sono progetti App Builder appositamente strutturati, che a loro volta sono progetti Node.js.

+ [Crea un nuovo progetto Asset Compute](./develop/project.md)

### Configurare le variabili di ambiente

Le variabili di ambiente vengono mantenute nel file `.env` per lo sviluppo locale e vengono utilizzate per fornire le credenziali di Adobe I/O e dell&#39;archiviazione cloud richieste per lo sviluppo locale.

+ [Configurare le variabili di ambiente](./develop/environment-variables.md)

### Configurare manifest.yml

I progetti di Asset compute contengono manifesti che definiscono tutti i processi di lavoro di Asset compute all’interno del progetto, nonché le risorse disponibili quando vengono distribuiti in Adobe I/O Runtime per l’esecuzione.

+ [Configurare manifest.yml](./develop/manifest.md)

### Sviluppa un lavoratore

Lo sviluppo di un processo di lavoro Asset Compute è il nucleo dell’estensione dei microservizi Asset Compute, in quanto il processo di lavoro contiene il codice personalizzato che genera, o orchestra, la generazione del rendering della risorsa risultante.

+ [Sviluppa un lavoratore Asset compute](./develop/worker.md)

### Utilizzare lo strumento di sviluppo Asset compute

Lo strumento per lo sviluppo Asset compute fornisce un Web harness locale per la distribuzione, l&#39;esecuzione e l&#39;anteprima delle rappresentazioni generate dai lavoratori, supportando lo sviluppo rapido e iterativo dei lavoratori Asset compute.

+ [Utilizzare lo strumento di sviluppo Asset compute](./develop/development-tool.md)

## Test e debug

Scopri come verificare che i processi di lavoro personalizzati per gli Asset compute siano sicuri del loro funzionamento ed eseguire il debug dei processi di lavoro per gli Asset compute per comprendere e risolvere eventuali problemi relativi all’esecuzione del codice personalizzato.

### Eseguire il test di un lavoratore

L’Asset compute fornisce un framework di test per la creazione di suite di test per i lavoratori, rendendo semplice la definizione di test che garantiscono un comportamento corretto.

+ [Eseguire il test di un lavoratore](./test-debug/test.md)

### Debug di un processo di lavoro

I processi di lavoro Asset Compute forniscono diversi livelli di debug dall&#39;output `console.log(..)` tradizionale alle integrazioni con __VS Code__ e __wskdebug__, consentendo agli sviluppatori di analizzare il codice del processo di lavoro durante l&#39;esecuzione in tempo reale.

+ [Debug di un processo di lavoro](./test-debug/debug.md)

## Distribuzione

Scopri come integrare i processi di lavoro Asset Compute personalizzati con AEM as a Cloud Service, distribuendoli prima in Adobe I/O Runtime e richiamandoli dall’istanza di authoring di AEM as a Cloud Service tramite i Profili di elaborazione di AEM Assets.

### Implementare in Adobe I/O Runtime

Per poter essere utilizzati con AEM as a Cloud Service, i processi di lavoro Asset Compute devono essere implementati in Adobe I/O Runtime.

+ [Utilizzo dei profili di elaborazione](./deploy/runtime.md)

### Integrare i lavoratori tramite i profili di elaborazione AEM

Una volta implementati in Adobe I/O Runtime, i processi di lavoro Asset Compute possono essere registrati in AEM as a Cloud Service tramite [Profili elaborazione Assets](../../assets/configuring/processing-profiles.md). I Profili di elaborazione vengono, a loro volta, applicati alle cartelle di risorse che si applicano alle risorse in essi contenute.

+ [Integrare con i profili di elaborazione dell’AEM](./deploy/processing-profiles.md)

## Avanzate 

Queste esercitazioni abbreviate affrontano casi d’uso più avanzati sulla base degli insegnamenti fondamentali stabiliti nei capitoli precedenti.

+ [Sviluppa un processo di lavoro metadati di Asset Compute](./advanced/metadata.md) in grado di riscrivere i metadati in

## Codebase su Github

La base di codice del tutorial è disponibile su Github all’indirizzo:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ ramo principale

Il codice sorgente non contiene i file `.env` o `config.json` richiesti. Questi devono essere aggiunti e configurati utilizzando le informazioni [account e servizi](#accounts-and-services).

## Risorse aggiuntive

Di seguito sono riportate varie risorse di Adobe che forniscono ulteriori informazioni e utili API e SDK per lo sviluppo di lavoratori Asset compute.

### Documentazione

+ [Documentazione del servizio Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [File readme dello strumento di sviluppo Asset compute](https://github.com/adobe/asset-compute-devtool)
+ [Asset compute di lavoratori](https://github.com/adobe/asset-compute-example-workers)

### API e SDK

+ [SDK Asset compute](https://github.com/adobe/asset-compute-sdk)
   + [Commoni Asset compute](https://github.com/adobe/asset-compute-commons)
   + [Asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Libreria Wrapper Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Libreria tentativi recupero nodo di Adobe](https://github.com/adobe/node-fetch-retry)
+ [Asset compute di lavoratori](https://github.com/adobe/asset-compute-example-workers)
