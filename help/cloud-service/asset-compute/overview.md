---
title: ' estensibilità dei microservizi Asset compute per AEM come Cloud Service'
description: Questa esercitazione spiega come creare un semplice  Asset compute che crea una rappresentazione di una risorsa ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il lavoratore sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un lavoratore Asset compute  personalizzato da utilizzare con AEM come Cloud Service.
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


# Estensibilità dei microservizi  Asset compute

AEM come Cloud Service  Asset compute microservizi   supportano lo sviluppo e la distribuzione di lavoratori personalizzati che vengono utilizzati per leggere e manipolare i dati binari delle risorse memorizzate in AEM, più comunemente, per creare rappresentazioni di risorse personalizzate.

Mentre in AEM 6.x i processi personalizzati AEM Workflow venivano utilizzati per leggere, trasformare e riscrivere le rappresentazioni delle risorse, AEM come Cloud Service  Asset compute di lavoro soddisfano questa esigenza.

## Azioni

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Questa esercitazione spiega come creare un semplice  Asset compute che crea una rappresentazione di una risorsa ritagliando la risorsa originale in un cerchio e applica contrasto e luminosità configurabili. Sebbene il lavoratore sia di base, questa esercitazione lo utilizza per esplorare la creazione, lo sviluppo e la distribuzione di un lavoratore Asset compute  personalizzato da utilizzare con AEM come Cloud Service.

### Obiettivi {#objective}

1. Fornire e configurare i conti e i servizi necessari per creare e implementare un lavoratore Asset compute 
1. Creazione e configurazione di un progetto di Asset compute 
1. Sviluppare un lavoratore Asset compute  che genera una rappresentazione personalizzata
1. Creazione di test per e apprendimento di come eseguire il debug del lavoratore Asset compute  personalizzato
1. Implementare il lavoratore del Asset compute  e integrarlo AEM come servizio Autore Cloud Service tramite Profili di elaborazione

## Configurare

Scoprite come prepararsi in modo appropriato per l&#39;estensione  Asset compute di lavoro, e capire quali servizi e account devono essere predisposti e configurati e quali software devono essere installati localmente per lo sviluppo.

### Provisioning di account e servizi{#accounts-and-services}

I seguenti account e servizi richiedono il provisioning e l&#39;accesso a per completare l&#39;esercitazione, AEM come un Cloud Service ambiente sviluppatore o programma sandbox, l&#39;accesso a  progetto Adobe Firefly e archiviazione Blob di Microsoft Azure.

+ [Provisioning di account e servizi](./set-up/accounts-and-services.md)

### Ambiente di sviluppo locale

Lo sviluppo locale di progetti  Asset compute richiede un set di strumenti per sviluppatori specifico, diverso da quello tradizionale, che include: Microsoft Visual Studio Code, Docker Desktop, Node.js e moduli npm supportati.

+ [Configurare l&#39;ambiente di sviluppo locale](./set-up/development-environment.md)

###  progetto Adobe

 progetti di Asset compute sono specifici  progetti Firefly di progetto Adobe e, come tale, richiedono l&#39;accesso a  progetto  Firefly nella  console per sviluppatori di Adobe per configurarli e distribuirli.

+ [Imposta  progetto Adobe Firefly](./set-up/firefly.md)

## Sviluppa

Scoprite come creare e configurare un progetto di Asset compute  e quindi sviluppare un lavoratore personalizzato che generi una rappresentazione di risorse personalizzate.

### Creare un nuovo progetto di Asset compute 

 progetti di Asset compute, che contengono uno o più lavoratori  Asset compute, vengono generati utilizzando l&#39;interfaccia CLI  Adobe I/O interattiva.  progetti di Asset compute sono progetti  progetto Firefly appositamente strutturati, che a loro volta sono progetti Node.js.

+ [Creare un nuovo progetto di Asset compute ](./develop/project.md)

### Configurare le variabili di ambiente

Le variabili di ambiente vengono mantenute nel file `.env` per lo sviluppo locale e vengono utilizzate per fornire  credenziali Adobe I/O e credenziali di archiviazione cloud richieste per lo sviluppo locale.

+ [Configurare le variabili di ambiente](./develop/environment-variables.md)

### Configurare manifest.yml

 progetti di Asset compute contengono manifesti che definiscono tutti i lavoratori  Asset compute contenuti nel progetto, nonché quali risorse sono disponibili quando vengono distribuiti ad Adobe I/O Runtime per l&#39;esecuzione.

+ [Configurare manifest.yml](./develop/manifest.md)

### Sviluppare un lavoratore

Lo sviluppo di un Asset compute  è il nucleo dell&#39;estensione  microservizi di Asset compute, in quanto il lavoratore contiene il codice personalizzato che genera, o orchestra, la generazione della rappresentazione risorsa risultante.

+ [Sviluppare un Asset compute ](./develop/worker.md)

### Utilizzo dello strumento di sviluppo Asset compute 

 Asset compute Development Tool fornisce un cablaggio Web locale per distribuire, eseguire e visualizzare in anteprima rappresentazioni generate dai lavoratori, a supporto di uno sviluppo rapido e ripetitivo dei lavoratori  Asset compute.

+ [Utilizzo dello strumento di sviluppo Asset compute ](./develop/development-tool.md)

## Test e debug

Scoprite come sottoporre a test i Asset compute di lavoro  personalizzati in modo da essere certi del funzionamento e come eseguire il debug dei lavoratori  Asset compute per comprendere e risolvere i problemi relativi all&#39;esecuzione del codice personalizzato.

### Test di un lavoratore

 Asset compute fornisce un framework di test per la creazione di suite di test per i lavoratori, rendendo più semplice la definizione di test che garantiscano un comportamento corretto.

+ [Test di un lavoratore](./test-debug/test.md)

### Debug di un lavoratore

 operatori di Asset compute forniscono vari livelli di debugging dall&#39;output `console.log(..)` tradizionale, alle integrazioni con __VS Code__ e __wskdebug__, consentendo agli sviluppatori di scorrere il codice di lavoro durante l&#39;esecuzione in tempo reale.

+ [Debug di un lavoratore](./test-debug/debug.md)

## Implementa

Scopri come integrare  Asset compute di lavoro personalizzati con AEM come Cloud Service, prima distribuendoli in Adobe I/O Runtime e poi richiamandoli da AEM come Autore di Cloud Service tramite i profili di elaborazione di AEM Assets.

### Distribuisci in Adobe I/O Runtime

 lavoratori Asset compute devono essere distribuiti in Adobe I/O Runtime per essere utilizzati come Cloud Service con AEM.

+ [Utilizzo dei profili di elaborazione](./deploy/runtime.md)

### Integrare i lavoratori tramite AEM profili di elaborazione

Una volta distribuiti in Adobe I/O Runtime,  lavoratori Asset compute possono essere registrati in AEM come Cloud Service tramite [Profili di elaborazione risorse](../../assets/configuring/processing-profiles.md). I profili di elaborazione vengono, a loro volta, applicati alle cartelle di risorse che si applicano alle risorse in essi contenute.

+ [Integrazione con AEM profili di elaborazione](./deploy/processing-profiles.md)

## Avanzate 

Queste esercitazioni abbreviate consentono di gestire casi di utilizzo più avanzati sulla base delle conoscenze di base stabilite nei capitoli precedenti.

+ [Sviluppare un ](./advanced/metadata.md) workbench metadati Asset compute  in grado di riscrivere i metadati nel pannello

## Codebase su Github

Il codice dell&#39;esercitazione è disponibile su Github all&#39;indirizzo:

+ [filiale master adobe/aem-guide-wknd-asset-computer](https://github.com/adobe/aem-guides-wknd-asset-compute) @

Il codice sorgente non contiene i file `.env` o `config.json` richiesti. Questi devono essere aggiunti e configurati utilizzando le informazioni [account e servizi](#accounts-and-services).

## Altro materiale di riferimento

Di seguito sono riportate diverse risorse  Adobe che forniscono ulteriori informazioni e API e SDK utili per lo sviluppo  lavoratori dei Asset compute.

### Documentazione

+ [ documentazione del servizio Asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [ Strumento di sviluppo Asset compute Leggimi](https://github.com/adobe/asset-compute-devtool)
+ [ Asset compute di lavoro](https://github.com/adobe/asset-compute-example-workers)

### API e SDK

+ [ Asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [ Asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [XMP Asset compute ](https://github.com/adobe/asset-compute-xmp#readme)
+ [ Libreria Wrapper Blobstore Cloud di Adobe](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [ libreria Ritiro nodo di Adobe](https://github.com/adobe/node-fetch-retry)
+ [ Asset compute di lavoro](https://github.com/adobe/asset-compute-example-workers)
