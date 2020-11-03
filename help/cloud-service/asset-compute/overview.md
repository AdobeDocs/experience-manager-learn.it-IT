---
title: Ottimizzazione dei microservizi per AEM come Cloud Service
description: Questa esercitazione spiega come creare un semplice Asset Compute Worker che crea una rappresentazione di risorsa ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il lavoratore sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un lavoratore di calcolo risorse personalizzato da utilizzare con AEM come Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 0%

---


# Estensibilità dei microservizi di calcolo delle risorse

AEM come Cloud Service  i microservizi Asset Compute supportano lo sviluppo e la distribuzione di lavoratori personalizzati che vengono utilizzati per leggere e manipolare i dati binari delle risorse memorizzate in AEM, più comunemente, per creare rappresentazioni delle risorse personalizzate.

Mentre in AEM 6.x i processi personalizzati AEM Flusso di lavoro sono stati utilizzati per leggere, trasformare e riscrivere le rappresentazioni delle risorse, in AEM come Cloud Service Asset Compute Workers soddisfare questa esigenza.

## Azioni

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Questa esercitazione spiega come creare un semplice Asset Compute Worker che crea una rappresentazione di risorsa ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il lavoratore sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un lavoratore di calcolo risorse personalizzato da utilizzare con AEM come Cloud Service.

### Obiettivi {#objective}

1. Provisioning e configurazione degli account e dei servizi necessari per creare e distribuire un lavoratore di elaborazione risorse
1. Creazione e configurazione di un progetto Asset Compute
1. Sviluppare un lavoratore di calcolo risorse che genera una rappresentazione personalizzata
1. Creazione di test per il lavoratore di calcolo risorse personalizzato e istruzioni per il debug
1. Implementare il lavoratore di elaborazione risorse e integrarlo AEM come servizio di creazione Cloud Service tramite profili di elaborazione

## Configurare

Scoprite come prepararsi in modo appropriato per l&#39;estensione di Asset Compute Workers, e capire quali servizi e account devono essere predisposti e configurati e quali software devono essere installati localmente per lo sviluppo.

### Provisioning di account e servizi{#accounts-and-services}

I seguenti account e servizi richiedono il provisioning e l&#39;accesso a per completare l&#39;esercitazione, AEM come un Cloud Service ambiente sviluppatore o programma sandbox, l&#39;accesso a  progetto Adobe Firefly e archiviazione Blob di Microsoft Azure.

+ [Provisioning di account e servizi](./set-up/accounts-and-services.md)

### Ambiente di sviluppo locale

Lo sviluppo locale di progetti Asset Compute richiede uno specifico set di strumenti per sviluppatori, diverso dallo sviluppo AEM tradizionale, che include: Microsoft Visual Studio Code, Docker Desktop, Node.js e moduli npm supportati.

+ [Configurare l&#39;ambiente di sviluppo locale](./set-up/development-environment.md)

###  progetto Adobe

I progetti Asset Compute sono progetti  Project Firefly appositamente definiti e, come tali, richiedono l&#39;accesso a  Project Firefly in  Adobe Developer Console per configurarli e distribuirli.

+ [Imposta  progetto Adobe Firefly](./set-up/firefly.md)

## Sviluppa

Scoprite come creare e configurare un progetto Asset Compute e quindi sviluppare un lavoratore personalizzato che generi una rappresentazione di risorse personalizzate.

### Creare un nuovo progetto Asset Compute

I progetti Asset Compute, che contengono uno o più lavoratori Asset Compute, vengono generati utilizzando l&#39;interfaccia CLI I/O Adobe  interattiva. I progetti Asset Compute sono progetti  Project Firefly appositamente strutturati, che a loro volta sono progetti Node.js.

+ [Creare un nuovo progetto Asset Compute](./develop/project.md)

### Configurare le variabili di ambiente

Le variabili di ambiente vengono mantenute nel `.env` file per lo sviluppo locale e vengono utilizzate per fornire  credenziali di I/O Adobe e le credenziali di archiviazione cloud richieste per lo sviluppo locale.

+ [Configurare le variabili di ambiente](./develop/environment-variables.md)

### Configurare manifest.yml

I progetti Asset Compute contengono manifesti che definiscono tutti i lavoratori Asset Compute contenuti nel progetto, nonché le risorse disponibili quando vengono distribuiti ad Adobe I/O Runtime per l&#39;esecuzione.

+ [Configurare manifest.yml](./develop/manifest.md)

### Sviluppare un lavoratore

Lo sviluppo di un lavoratore Asset Compute è il nucleo dell&#39;estensione dei microservizi Asset Compute, in quanto il lavoratore contiene il codice personalizzato che genera, o orchestra, la generazione della rappresentazione risorsa risultante.

+ [Sviluppare un lavoratore di elaborazione risorse](./develop/worker.md)

### Utilizzo dello strumento di sviluppo di calcolo risorse

Lo strumento di sviluppo del calcolo delle risorse fornisce un cablaggio Web locale per distribuire, eseguire e visualizzare in anteprima rappresentazioni generate dai lavoratori, che supporta lo sviluppo rapido e iterativo dei lavoratori di elaborazione delle risorse.

+ [Utilizzo dello strumento di sviluppo di calcolo risorse](./develop/development-tool.md)

## Test e debug

Scoprite come sottoporre a test i lavoratori di calcolo delle risorse personalizzati in modo da essere certi del funzionamento e come eseguire il debug dei lavoratori di elaborazione delle risorse per comprendere e risolvere i problemi relativi all&#39;esecuzione del codice personalizzato.

### Test di un lavoratore

Asset Compute fornisce un framework di test per la creazione di suite di test per i lavoratori, rendendo più semplice la definizione di test che garantiscano un comportamento corretto.

+ [Test di un lavoratore](./test-debug/test.md)

### Debug di un lavoratore

I lavoratori di elaborazione delle risorse forniscono vari livelli di debugging dall&#39; `console.log(..)` output tradizionale alle integrazioni con codice ____ VS e __wskdebug__, consentendo agli sviluppatori di esaminare il codice del lavoro in tempo reale.

+ [Debug di un lavoratore](./test-debug/debug.md)

## Implementa

Scopri come integrare i lavoratori di calcolo delle risorse personalizzati con AEM come Cloud Service, distribuendoli in Adobe I/O Runtime e quindi richiamandoli da AEM come Autore Cloud Service tramite i profili di elaborazione di AEM Assets.

### Distribuisci in Adobe I/O Runtime

I lavoratori di calcolo delle risorse devono essere distribuiti in Adobe I/O Runtime per essere utilizzati con AEM come Cloud Service.

+ [Utilizzo dei profili di elaborazione](./deploy/runtime.md)

### Integrare i lavoratori tramite AEM profili di elaborazione

Una volta distribuiti in Adobe I/O Runtime, i lavoratori di elaborazione delle risorse possono essere registrati in AEM come Cloud Service tramite i profili di elaborazione delle [risorse](../../assets/configuring/processing-profiles.md). I profili di elaborazione vengono, a loro volta, applicati alle cartelle di risorse che si applicano alle risorse in essi contenute.

+ [Integrazione con AEM profili di elaborazione](./deploy/processing-profiles.md)

## Avanzate 

Queste esercitazioni abbreviate consentono di gestire casi di utilizzo più avanzati sulla base delle conoscenze di base stabilite nei capitoli precedenti.

+ [Sviluppare un lavoratore](./advanced/metadata.md) di metadati Asset Compute in grado di scrivere nuovamente i metadati nel pannello

## Codebase su Github

Il codice dell&#39;esercitazione è disponibile su Github all&#39;indirizzo:

+ [adobe/aem-guide-wknd-asset-computer](https://github.com/adobe/aem-guides-wknd-asset-compute) @ ramo principale

Il codice sorgente non contiene i `.env` o `config.json` i file richiesti. Questi devono essere aggiunti e configurati utilizzando [gli account e le informazioni sui servizi](#accounts-and-services) .

## Altro materiale di riferimento

Di seguito sono riportate diverse risorse  Adobe che forniscono ulteriori informazioni e API e SDK utili per lo sviluppo di Asset Compute Worker.

### Documentazione

+ [Documentazione servizio di calcolo risorse](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Leggimi dello strumento di sviluppo di calcolo risorse](https://github.com/adobe/asset-compute-devtool)
+ [Lavoratori esempio di calcolo risorse](https://github.com/adobe/asset-compute-example-workers)

### API e SDK

+ [SDK per elaborazione risorse](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [XMP di calcolo risorse](https://github.com/adobe/asset-compute-xmp#readme)
+ [Libreria Wrapper Blobstore Cloud di Adobe](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [libreria Ritiro nodo di Adobe](https://github.com/adobe/node-fetch-retry)
+ [Lavoratori esempio di calcolo risorse](https://github.com/adobe/asset-compute-example-workers)
