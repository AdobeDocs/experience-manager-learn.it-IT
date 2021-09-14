---
title: Configurare un ambiente di sviluppo locale per l’estensibilità di Asset compute
description: Lo sviluppo di processi di lavoro Asset compute, che sono applicazioni JavaScript Node.js, richiede strumenti di sviluppo specifici diversi dallo sviluppo AEM tradizionale, che vanno da Node.js e vari moduli npm a Docker Desktop e Microsoft Visual Studio Code.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 0%

---

# Configurare l&#39;ambiente di sviluppo locale

I progetti di Asset compute di Adobe non possono essere integrati con il runtime AEM locale fornito dall’SDK di AEM e vengono sviluppati utilizzando la propria catena di strumenti, separata da quella richiesta dalle applicazioni AEM basate sull’archetipo di progetto Maven AEM.

Per estendere i microservizi Asset compute, è necessario installare i seguenti strumenti nel computer sviluppatore locale.

## Istruzioni di configurazione abbreviate

Di seguito sono riportate le istruzioni per l&#39;impostazione di una cresta. I dettagli relativi a questi strumenti di sviluppo sono descritti nelle sezioni discrete di seguito.

1. [Installare Docker ](https://www.docker.com/products/docker-desktop) Desktop e estrarre le immagini Docker richieste:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installazione del codice di Visual Studio](https://code.visualstudio.com/download)
1. [Installa Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installa i moduli npm richiesti e i plug-in Adobe I/O CLI dalla riga di comando:

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Per ulteriori informazioni sulle istruzioni di installazione abbreviate, consulta le sezioni seguenti.

## Installazione del codice di Visual Studio{#vscode}

[Il codice ](https://code.visualstudio.com/download) di Microsoft Visual Studio viene utilizzato per lo sviluppo e il debug dei processi di lavoro di Asset compute. Mentre è possibile utilizzare altri sistemi IDE [compatibili con JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) per sviluppare il processo di lavoro, solo Visual Studio Code può essere integrato nel processo di lavoro di Asset compute [debug](../test-debug/debug.md).

Questa esercitazione presuppone l&#39;utilizzo del codice di Visual Studio in quanto fornisce la migliore esperienza di sviluppo per estendere l&#39;Asset compute.

## Installa Docker Desktop{#docker}

Scarica e installa l&#39;ultimo [Docker Desktop](https://www.docker.com/products/docker-desktop) stabile, in quanto è necessario per [testare](../test-debug/test.md) e [eseguire il debug](../test-debug/debug.md) Asset compute a livello locale.

Dopo aver installato Docker Desktop, avviarlo e installare le seguenti immagini Docker dalla riga di comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Gli sviluppatori di computer Windows devono assicurarsi che utilizzino contenitori Linux per le immagini di cui sopra. I passaggi per passare ai contenitori Linux sono descritti nella documentazione [Docker for Windows](https://docs.docker.com/docker-for-windows/).

## Installa Node.js (e npm){#node-js}

I processi di lavoro di Asset compute sono basati su [Node.js](https://nodejs.org/) e quindi richiedono a Node.js 10+ (e npm) di svilupparsi e generare.

+ [Installa Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) nello stesso modo dello sviluppo AEM tradizionale.

## Installare Adobe I/O CLI{#aio}

[Installa l’Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli), o  ____ aiois un modulo npm riga di comando (CLI) che facilita l’utilizzo e l’interazione con le tecnologie Adobe I/O e viene utilizzato per generare e sviluppare processi di lavoro Asset compute personalizzati a livello locale.

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_È richiesta la versione 7.1.0 di Adobe I/O CLI. Al momento non sono supportate versioni successive di Adobe I/O CLI._


## Installare il plug-in Adobe I/O CLI Asset compute{#aio-asset-compute}

Il plug-in Asset compute [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installa wskdebug{#wskdebug}

Scarica e installa il modulo npm [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) per facilitare il debug locale dei processi di lavoro di Asset compute.

_Per il funzionamento di  [](#wskdebug) wskdebug è necessario Visual Studio Code 1.48.x+._

```
$ npm install -g @openwhisk/wskdebug
```

## Installa ngrok{#ngrok}

Scarica e installa il modulo [ngrok](https://www.npmjs.com/package/ngrok) npm, che fornisce l&#39;accesso pubblico al computer di sviluppo locale, per facilitare il debug locale dei processi di lavoro di Asset compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
