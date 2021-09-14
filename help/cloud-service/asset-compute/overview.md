---
title: Estensibilità dei microservizi di Asset compute per AEM come Cloud Service
description: Questa esercitazione illustra la creazione di un semplice processo di lavoro di Asset compute che crea un rendering della risorsa ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il processo di lavoro sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un processo di lavoro Asset compute personalizzato da utilizzare con AEM come Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1026'
ht-degree: 0%

---

# Estensibilità dei microservizi di Asset compute

AEM come microservizi di Asset compute di Cloud Service supportano lo sviluppo e la distribuzione di processi di lavoro personalizzati che vengono utilizzati per leggere e manipolare i dati binari delle risorse memorizzate in AEM, più comunemente, per creare rappresentazioni personalizzate delle risorse.

Mentre in AEM 6.x i processi di flusso di lavoro AEM personalizzati sono stati utilizzati per leggere, trasformare e riscrivere le rappresentazioni delle risorse, in AEM come lavoratori di Asset compute Cloud Service soddisfano questa esigenza.

## Cosa farai

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Questa esercitazione illustra la creazione di un semplice processo di lavoro di Asset compute che crea un rendering della risorsa ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il processo di lavoro sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un processo di lavoro Asset compute personalizzato da utilizzare con AEM come Cloud Service.

### Obiettivi {#objective}

1. Fornire e configurare gli account e i servizi necessari per creare e distribuire un lavoratore Asset compute
1. Creare e configurare un progetto di Asset compute
1. Sviluppa un processo di lavoro Asset compute che genera un rendering personalizzato
1. Creazione di test per e apprendimento di come eseguire il debug del processo di lavoro Asset compute personalizzato
1. Distribuire il processo di lavoro Asset compute e integrarlo AEM come servizio Author di Cloud Service tramite Profili elaborazione

## Configurazione

Scopri come prepararsi in modo appropriato all’estensione dei processi di lavoro di Asset compute e capire quali servizi e account devono essere forniti e configurati e quali software devono essere installati localmente per lo sviluppo.

### Provisioning di account e servizi{#accounts-and-services}

I seguenti account e servizi richiedono il provisioning e l&#39;accesso a per completare l&#39;esercitazione, AEM come ambiente di sviluppo di Cloud Service o programma sandbox, l&#39;accesso ad Adobe Project Firefly e Microsoft Azure Blob Storage.

+ [Prestazione di account e servizi](./set-up/accounts-and-services.md)

### Ambiente di sviluppo locale

Lo sviluppo locale di progetti di Asset compute richiede uno specifico set di strumenti per sviluppatori, diverso dallo sviluppo AEM tradizionale, tra cui: Microsoft Visual Studio Code, Docker Desktop, Node.js e il supporto di moduli npm.

+ [Configurare l&#39;ambiente di sviluppo locale](./set-up/development-environment.md)

### Progetto Adobe Firefly

I progetti di Asset compute sono progetti Adobe Firefly appositamente definiti e, come tali, richiedono l’accesso ad Adobe Project Firefly in Adobe Developer Console per configurarli e distribuirli.

+ [Imposta progetto Adobe Firefly](./set-up/firefly.md)

## Sviluppa

Scopri come creare e configurare un progetto di Asset compute e quindi sviluppare un processo di lavoro personalizzato per la generazione di un rendering delle risorse personalizzato.

### Crea un nuovo progetto di Asset compute

I progetti di Asset compute, che contengono uno o più processi di lavoro Asset compute, vengono generati utilizzando l’interfaccia CLI interattiva di Adobe I/O. I progetti di Asset compute sono progetti di progetto Adobe Firefly appositamente strutturati, che a loro volta sono progetti Node.js.

+ [Crea un nuovo progetto di Asset compute](./develop/project.md)

### Configurare le variabili di ambiente

Le variabili di ambiente vengono mantenute nel file `.env` per lo sviluppo locale e vengono utilizzate per fornire le credenziali di Adobe I/O e di archiviazione cloud necessarie per lo sviluppo locale.

+ [Configurare le variabili di ambiente](./develop/environment-variables.md)

### Configura il file manifest.yml

I progetti di Asset compute contengono manifesti che definiscono tutti i lavoratori Asset compute contenuti nel progetto, nonché le risorse disponibili quando vengono distribuiti in Adobe I/O Runtime per l’esecuzione.

+ [Configura il file manifest.yml](./develop/manifest.md)

### Sviluppare un processo di lavoro

Lo sviluppo di un processo di lavoro Asset compute è il nucleo dell’estensione dei microservizi Asset compute, in quanto il processo di lavoro contiene il codice personalizzato che genera, o orchestra, la generazione del rendering delle risorse risultanti.

+ [Sviluppare un processo di lavoro Asset compute](./develop/worker.md)

### Utilizzare lo strumento di sviluppo Asset compute

Lo strumento di sviluppo Asset compute fornisce un cablaggio web locale per distribuire, eseguire e visualizzare in anteprima rappresentazioni generate dal processo di lavoro, sostenendo lo sviluppo rapido e iterativo del processo di lavoro Asset compute.

+ [Utilizzare lo strumento di sviluppo Asset compute](./develop/development-tool.md)

## Test e debug

Scopri come testare i processi di lavoro di Asset compute personalizzati in modo che siano sicuri del loro funzionamento e come eseguire il debug dei processi di lavoro di Asset compute per comprendere e risolvere i problemi relativi all’esecuzione del codice personalizzato.

### Test di un processo di lavoro

asset compute fornisce un framework di test per la creazione di suite di test per i lavoratori, rendendo più semplice la definizione di test che garantiscano un comportamento corretto.

+ [Test di un processo di lavoro](./test-debug/test.md)

### Debug di un processo di lavoro

I processi di lavoro di Asset compute forniscono vari livelli di debug dall&#39;output tradizionale `console.log(..)` alle integrazioni con __VS Code__ e __wskdebug__, consentendo agli sviluppatori di passare attraverso il codice di lavoro mentre viene eseguito in tempo reale.

+ [Debug di un processo di lavoro](./test-debug/debug.md)

## Implementa

Scopri come integrare i processi di lavoro di Asset compute personalizzati con AEM come Cloud Service, prima distribuendoli in Adobe I/O Runtime e poi richiamandoli da AEM come autore di Cloud Service tramite i profili di elaborazione di AEM Assets.

### Distribuzione in Adobe I/O Runtime

I lavoratori Asset compute devono essere distribuiti in Adobe I/O Runtime per essere utilizzati con AEM come Cloud Service.

+ [Utilizzo dei profili di elaborazione](./deploy/runtime.md)

### Integrare i processi di lavoro tramite AEM Profili elaborazione

Una volta implementati in Adobe I/O Runtime, i processi di lavoro Asset compute possono essere registrati in AEM come Cloud Service tramite [Profili elaborazione risorse](../../assets/configuring/processing-profiles.md). I profili di elaborazione vengono, a loro volta, applicati alle cartelle di risorse che si applicano alle risorse in esse contenute.

+ [Integrazione con i profili di elaborazione AEM](./deploy/processing-profiles.md)

## Avanzate 

Questi tutorial abbreviati affrontano casi d’uso più avanzati sulla base di insegnamenti fondamentali stabiliti nei capitoli precedenti.

+ [Sviluppa un ](./advanced/metadata.md) workerer di metadati di Asset compute in grado di riscrivere i metadati nel

## Codebase su Github

La base di codice dell’esercitazione è disponibile su Github all’indirizzo:

+ [ramo principale adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

Il codice sorgente non contiene i file `.env` o `config.json` richiesti. Questi devono essere aggiunti e configurati utilizzando le informazioni [account e servizi](#accounts-and-services) .

## Altro materiale di riferimento

Di seguito sono riportate diverse risorse di Adobe che forniscono ulteriori informazioni e API e SDK utili per lo sviluppo di processi di lavoro Asset compute.

### Documentazione

+ [Documentazione del servizio Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [File readme dello strumento di sviluppo di Asset compute](https://github.com/adobe/asset-compute-devtool)
+ [asset compute di lavoratori di esempio](https://github.com/adobe/asset-compute-example-workers)

### API e SDK

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Libreria Wrapper di Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe libreria di tentativi di recupero del nodo](https://github.com/adobe/node-fetch-retry)
+ [asset compute di lavoratori di esempio](https://github.com/adobe/asset-compute-example-workers)
