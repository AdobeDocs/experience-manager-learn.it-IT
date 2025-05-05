---
title: Configurare un ambiente di sviluppo locale per l’estensibilità di Asset Compute
description: Lo sviluppo dei processi di lavoro di Asset Compute, che sono applicazioni JavaScript Node.js, richiede strumenti di sviluppo specifici diversi dallo sviluppo tradizionale di AEM, che vanno da Node.js e vari moduli npm a Docker Desktop e Microsoft Visual Studio Code.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Configurare l’ambiente di sviluppo locale

I progetti Adobe Asset Compute non possono essere integrati con il runtime AEM locale fornito da AEM SDK e vengono sviluppati utilizzando una propria catena di strumenti, separata da quella richiesta dalle applicazioni AEM basate sull’archetipo del progetto AEM Maven.

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
1. Installa i moduli npm e i plug-in CLI di Adobe I/O richiesti dalla riga di comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Per ulteriori informazioni sulle istruzioni di installazione in forma abbreviata, leggere le sezioni seguenti.

## Installare Visual Studio Code{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) viene utilizzato per lo sviluppo e il debug dei processi di lavoro di Asset Compute. Mentre è possibile utilizzare un altro IDE[&#128279;](../../local-development-environment/development-tools.md#set-up-the-development-ide) compatibile con JavaScript per sviluppare il processo di lavoro, solo Visual Studio Code può essere integrato nel processo di lavoro Asset Compute [debug](../test-debug/debug.md).

Questo tutorial presuppone l&#39;utilizzo del codice di Visual Studio in quanto fornisce la migliore esperienza di sviluppo per estendere Asset Compute.

## Installa Docker Desktop{#docker}

Scarica e installa l&#39;ultimo [Docker Desktop](https://www.docker.com/products/docker-desktop) stabile, necessario per [testare](../test-debug/test.md) e [eseguire il debug](../test-debug/debug.md) dei progetti Asset Compute localmente.

Dopo aver installato Docker Desktop, avviarlo e installare le immagini Docker seguenti dalla riga di comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Gli sviluppatori su computer Windows devono assicurarsi di utilizzare contenitori Linux per le immagini di cui sopra. I passaggi per passare ai contenitori Linux sono descritti nella [documentazione Docker per Windows](https://docs.docker.com/docker-for-windows/).

## Installare Node.js (e npm){#node-js}

I processi di lavoro di Asset Compute sono basati su [Node.js](https://nodejs.org/) e pertanto richiedono Node.js 10+ (e npm) per lo sviluppo e la generazione.

+ [Installa Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) nello stesso modo in cui installa lo sviluppo AEM tradizionale.

## Installare Adobe I/O CLI{#aio}

[Installare Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli) o __aio__ è un modulo npm per riga di comando (CLI) che facilita l&#39;utilizzo e l&#39;interazione con le tecnologie Adobe I/O e viene utilizzato sia per generare che per sviluppare in locale processi di lavoro Asset Compute personalizzati.

```
$ npm install -g @adobe/aio-cli
```

## Installare il plug-in Adobe I/O CLI Asset Compute{#aio-asset-compute}

Plug-in Asset Compute di [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installare wskdebug{#wskdebug}

Scarica e installa il modulo di debug [Apache OpenWhisk](https://www.npmjs.com/package/@openwhisk/wskdebug) npm per facilitare il debug locale dei processi di lavoro di Asset Compute.

_Per il funzionamento di [wskdebug](#wskdebug) è necessario Visual Studio Code 1.48.x+._

```
$ npm install -g @openwhisk/wskdebug
```

## Installa ngrok{#ngrok}

Scarica e installa il modulo [ngrok](https://www.npmjs.com/package/ngrok) npm, che fornisce accesso pubblico al computer di sviluppo locale, per facilitare il debug locale dei processi di lavoro di Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
