---
title: Configurare un ambiente di sviluppo locale per l’estensibilità di elaborazione risorse
description: Lo sviluppo di Asset Compute Workers, che sono applicazioni JavaScript Node.js, richiede strumenti di sviluppo specifici che differiscono da quelli tradizionali AEM sviluppo, che vanno da Node.js e vari moduli npm a Docker Desktop e Microsoft Visual Studio Code.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Configurare l&#39;ambiente di sviluppo locale

 progetti Asset Compute di Adobe non possono essere integrati con il runtime AEM locale fornito dall’SDK AEM e vengono sviluppati utilizzando una propria catena di strumenti, separata da quella richiesta da AEM applicazioni basate sul tipo di progetto AEM Maven.

Per estendere i microservizi Asset Compute, nel computer sviluppatore locale devono essere installati i seguenti strumenti.

## Istruzioni di configurazione abbreviate

Di seguito sono riportate le istruzioni per l&#39;impostazione di una cresta. I dettagli relativi a questi strumenti di sviluppo sono descritti nelle sezioni distinte di seguito.

1. [Installate Docker Desktop](https://www.docker.com/products/docker-desktop) ed estraete le immagini Docker richieste:

   ```
   $ docker pull openwhisk/action-nodejs-v10:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installazione del codice di Visual Studio](https://code.visualstudio.com/download)
1. [Installa Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installate i moduli npm e  plug-in CLI I/O di Adobe richiesti dalla riga di comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

## Installazione del codice di Visual Studio{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) viene utilizzato per lo sviluppo e il debug dei lavoratori di calcolo delle risorse. Mentre per sviluppare il lavoratore è possibile utilizzare un altro IDE [compatibile con](../../local-development-environment/development-tools.md#set-up-the-development-ide) JavaScript, solo Visual Studio Code può essere integrato per [debug](../test-debug/debug.md) Asset Compute.

_Visual Studio Code 1.48.x+ è richiesto per il funzionamento di[wskdebug](#wskdebug)._

Questa esercitazione si basa sull&#39;utilizzo di Visual Studio Code in quanto offre la migliore esperienza di sviluppo per estendere Asset Compute.

## Installare Docker Desktop{#docker}

Scaricate e installate l&#39;ultimo [Docker Desktop](https://www.docker.com/products/docker-desktop)stabile, in quanto questo è richiesto per [testare](../test-debug/test.md) ed eseguire il [debug](../test-debug/debug.md) dei progetti Asset Compute in locale.

Dopo aver installato Docker Desktop, avviarlo e installare le seguenti immagini Docker dalla riga di comando:

```
$ docker pull openwhisk/action-nodejs-v10:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Gli sviluppatori su computer Windows devono assicurarsi che utilizzino contenitori Linux per le immagini sopra riportate. I passaggi per passare ai contenitori Linux sono descritti nella documentazione [](https://docs.docker.com/docker-for-windows/)Docker for Windows.

## Installazione di Node.js (e npm){#node-js}

I lavoratori di elaborazione delle risorse sono basati su [Node.js](https://nodejs.org/)e richiedono pertanto a Node.js 10+ (e npm) di sviluppare e creare.

+ [Installate Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) come per lo sviluppo AEM tradizionale.

## Installazione  CLI I/O Adobe{#aio}

[Installate l&#39;interfaccia CLI](../../local-development-environment/development-tools.md#aio-cli)I/O del Adobe , oppure __aio__ è un modulo npm (Command-Line) della riga di comando che facilita l&#39;uso e l&#39;interazione con  tecnologie I/O del Adobe, ed è utilizzato per generare e sviluppare a livello locale personale di elaborazione delle risorse.

```
$ npm install -g @adobe/aio-cli
```

## Installare il plug-in di elaborazione risorse CLI I/O del Adobe{#aio-asset-compute}

Il plug-in [I/O CLI Asset Compute del Adobe](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installazione di wskdebug{#wskdebug}

Scaricate e installate il modulo npm di debug [](https://www.npmjs.com/package/@openwhisk/wskdebug) Apache OpenWhisk per facilitare il debug locale dei lavoratori di calcolo delle risorse.

_Visual Studio Code 1.48.x+ è richiesto per il funzionamento di[wskdebug](#wskdebug)._

```
$ npm install -g @openwhisk/wskdebug
```

## Installazione di ngrok{#ngrok}

Scaricate e installate il modulo [ngrok](https://www.npmjs.com/package/ngrok) npm, che fornisce l&#39;accesso pubblico al computer di sviluppo locale, per facilitare il debug locale dei lavoratori di elaborazione delle risorse.

```
$ npm install -g ngrok --unsafe-perm=true
```
