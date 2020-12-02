---
title: Configurare un ambiente di sviluppo locale per  estensibilità del Asset compute
description: Lo sviluppo  Asset compute di lavoro, che sono applicazioni JavaScript Node.js, richiede strumenti di sviluppo specifici che differiscono da quelli tradizionali, che vanno da Node.js e vari moduli npm a Docker Desktop e Microsoft Visual Studio Code.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# Configurare l&#39;ambiente di sviluppo locale

 progetti di Adobe  Asset compute non possono essere integrati con il runtime AEM locale fornito dall’SDK AEM e vengono sviluppati utilizzando una propria catena di strumenti, separata da quella richiesta da AEM applicazioni basate sul tipo di progetto AEM Maven.

Per estendere  microservizi di Asset compute, nel computer sviluppatore locale devono essere installati i seguenti strumenti.

## Istruzioni di configurazione abbreviate

Di seguito sono riportate le istruzioni per l&#39;impostazione di una cresta. I dettagli relativi a questi strumenti di sviluppo sono descritti nelle sezioni distinte di seguito.

1. [Installare Docker ](https://www.docker.com/products/docker-desktop) Desktop e estrarre le immagini Docker richieste:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Installazione del codice di Visual Studio](https://code.visualstudio.com/download)
1. [Installa Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installate i moduli npm e  plug-in Adobe I/O CLI dalla riga di comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Per ulteriori informazioni sulle istruzioni di installazione abbreviate, consulta le sezioni riportate di seguito.

## Installazione di Visual Studio Code{#vscode}

[Microsoft Visual Studio ](https://code.visualstudio.com/download) Codeis utilizzato per lo sviluppo e il debug  Asset compute di lavoro. Mentre è possibile utilizzare altri sistemi IDE [JavaScript compatibili con IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) per sviluppare il lavoratore, solo Visual Studio Code può essere integrato in [debug](../test-debug/debug.md)  Asset compute.

_Visual Studio Code 1.48.x+ è richiesto per il funzionamento di  [](#wskdebug) wskdebugging._

Questa esercitazione si basa sull&#39;utilizzo di Visual Studio Code, in quanto offre la migliore esperienza di sviluppo per estendere  Asset compute.

## Installazione di Docker Desktop{#docker}

Scaricate e installate la versione più recente, stabile [Docker Desktop](https://www.docker.com/products/docker-desktop), in quanto è necessaria per [testare](../test-debug/test.md) e [debug](../test-debug/debug.md) progetti  Asset compute localmente.

Dopo aver installato Docker Desktop, avviarlo e installare le seguenti immagini Docker dalla riga di comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Gli sviluppatori su computer Windows devono assicurarsi che utilizzino contenitori Linux per le immagini sopra riportate. I passaggi per passare ai contenitori Linux sono descritti nella documentazione [Docker for Windows](https://docs.docker.com/docker-for-windows/).

## Installazione di Node.js (e npm){#node-js}

 lavoratori di Asset compute sono basati su [Node.js](https://nodejs.org/) e pertanto richiedono Node.js 10+ (e npm) per lo sviluppo e la creazione.

+ [Installate Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) nello stesso modo in cui per lo sviluppo AEM tradizionale.

## Installazione  Adobe I/O CLI{#aio}

[Installate  CLI](../../local-development-environment/development-tools.md#aio-cli) di Adobe I/O, o  ____ aiois un modulo CLI (Command-Line) che facilita l&#39;uso e l&#39;interazione con  tecnologie Adobe I/O e che viene utilizzato per generare e sviluppare  lavoratori Asset compute personalizzati a livello locale.

```
$ npm install -g @adobe/aio-cli
```

## Installare il plug-in  Adobe I/O CLI  Asset compute{#aio-asset-compute}

[ Adobe I/O CLI  plug-in di Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installazione di wskdebug{#wskdebug}

Scaricate e installate il modulo npm [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) per facilitare il debug locale dei lavoratori  Asset compute.

_Visual Studio Code 1.48.x+ è richiesto per il funzionamento di  [](#wskdebug) wskdebugging._

```
$ npm install -g @openwhisk/wskdebug
```

## Installazione di ngrok{#ngrok}

Scaricate e installate il modulo [ngrok](https://www.npmjs.com/package/ngrok) npm, che fornisce l&#39;accesso pubblico al computer di sviluppo locale, per facilitare il debug locale dei  Asset compute di lavoro.

```
$ npm install -g ngrok --unsafe-perm=true
```
