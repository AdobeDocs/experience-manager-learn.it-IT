---
title: Configurare un ambiente di sviluppo locale, ad Asset compute l’estensibilità
description: Lo sviluppo di processi di lavoro Asset Compute, che sono applicazioni JavaScript di Node.js, richiede strumenti di sviluppo specifici che differiscono dallo sviluppo AEM tradizionale, che vanno da Node.js e vari moduli npm a Docker Desktop e Microsoft Visual Studio Code.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Configurare l’ambiente di sviluppo locale

I progetti di Asset compute di Adobe non possono essere integrati con il runtime AEM locale fornito dall’SDK dell’AEM e sono sviluppati utilizzando una propria catena di strumenti, separata da quella richiesta dalle applicazioni AEM basate sull’archetipo di progetto Maven dell’AEM.

Per estendere i microservizi Asset Compute, è necessario installare i seguenti strumenti nel computer di sviluppo locale.

## Istruzioni di configurazione abbreviate

Di seguito sono riportate le istruzioni per l&#39;impostazione di una griglia. I dettagli su questi strumenti di sviluppo sono descritti nelle sezioni dedicate di seguito.

1. [Installa Docker Desktop](https://www.docker.com/products/docker-desktop) e richiama le immagini Docker richieste:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installare Visual Studio Code](https://code.visualstudio.com/download)
1. [Installare Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installare i moduli npm richiesti e i plug-in CLI di Adobe I/O dalla riga di comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Per ulteriori informazioni sulle istruzioni di installazione in forma abbreviata, leggere le sezioni seguenti.

## Installare Visual Studio Code{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) è utilizzato per lo sviluppo e il debug di processi di lavoro di Asset compute. Mentre è possibile utilizzare un altro IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) compatibile con JavaScript per sviluppare il processo di lavoro, solo Visual Studio Code può essere integrato nel processo di lavoro Asset compute [debug](../test-debug/debug.md).[

Questo tutorial presuppone l&#39;utilizzo del codice di Visual Studio in quanto fornisce la migliore esperienza di sviluppo per l&#39;estensione di Asset Compute.

## Installa Docker Desktop{#docker}

Scarica e installa l&#39;ultimo [Docker Desktop](https://www.docker.com/products/docker-desktop) stabile, necessario per [testare](../test-debug/test.md) e [debug](../test-debug/debug.md) progetti di Asset compute localmente.

Dopo aver installato Docker Desktop, avviarlo e installare le immagini Docker seguenti dalla riga di comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Gli sviluppatori su computer Windows devono assicurarsi di utilizzare contenitori Linux per le immagini di cui sopra. I passaggi per passare ai contenitori Linux sono descritti nella [documentazione Docker per Windows](https://docs.docker.com/docker-for-windows/).

## Installare Node.js (e npm){#node-js}

I processi di lavoro di Asset compute sono basati su [Node.js](https://nodejs.org/) e pertanto richiedono Node.js 10+ (e npm) per lo sviluppo e la generazione.

+ [Installa Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) nello stesso modo in cui avviene per lo sviluppo AEM tradizionale.

## Installare Adobe I/O CLI{#aio}

[Installare Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli) o __aio__ è un modulo npm per riga di comando (CLI) che facilita l&#39;utilizzo e l&#39;interazione con le tecnologie Adobe I/O e viene utilizzato sia per generare che per sviluppare localmente processi di lavoro Asset Compute personalizzati.

```
$ npm install -g @adobe/aio-cli
```

## Installare il plug-in di Asset compute CLI Adobe I/O{#aio-asset-compute}

Il plug-in di Asset compute CLI [Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installare wskdebug{#wskdebug}

Scarica e installa il modulo di debug [Apache OpenWhisk](https://www.npmjs.com/package/@openwhisk/wskdebug) npm per facilitare il debug locale dei processi di lavoro Asset Compute.

_Per il funzionamento di [wskdebug](#wskdebug) è necessario Visual Studio Code 1.48.x+._

```
$ npm install -g @openwhisk/wskdebug
```

## Installa ngrok{#ngrok}

Scarica e installa il modulo [ngrok](https://www.npmjs.com/package/ngrok) npm, che fornisce accesso pubblico al computer di sviluppo locale, per facilitare il debug locale dei processi di lavoro Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
