---
title: Debug di un processo di lavoro di Asset Compute
description: Il debug dei processi di lavoro di Asset Compute può essere eseguito in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino all’estrazione dei registri per le attivazioni in Adobe I/O Runtime avviati da AEM as a Cloud Service.
feature: Microservizi Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrazioni, Sviluppo
role: Developer (Sviluppatore)
level: Intermedio, esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 0%

---


# Debug di un processo di lavoro di Asset Compute

Il debug dei processi di lavoro di Asset Compute può essere eseguito in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino all’estrazione dei registri per le attivazioni in Adobe I/O Runtime avviati da AEM as a Cloud Service.

## Registrazione

La forma più semplice di debugging dei processi di lavoro Asset Compute utilizza istruzioni `console.log(..)` tradizionali nel codice del processo di lavoro. L&#39;oggetto JavaScript `console` è un oggetto globale implicito, pertanto non è necessario importarlo o richiederlo, in quanto è sempre presente in tutti i contesti.

Queste istruzioni di registro sono disponibili per la revisione in modo diverso in base alla modalità di esecuzione del processo di lavoro Asset Compute:

+ Da `aio app run`, i registri stampano su uscita standard e i registri di attivazione [dello strumento di sviluppo](../develop/development-tool.md)
   ![esegui console.log(..)](./assets/debug/console-log__aio-app-run.png)
+ Da `aio app test`, i registri stampano su `/build/test-results/test-worker/test.log`
   ![console.log di test app aio(..)](./assets/debug/console-log__aio-app-test.png)
+ Utilizzando `wskdebug`, le istruzioni dei registri vengono stampate nella console di debug del codice VS (Visualizza > Console di debug), uscita standard
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Utilizzando `aio app logs`, le istruzioni di registro vengono stampate nell&#39;output del registro di attivazione

## Debug remoto tramite debugger collegato

>[!WARNING]
>
>Utilizzare Microsoft Visual Studio Code 1.48.0 o versione successiva per la compatibilità con wskdebug

Il modulo npm [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) supporta l’associazione di un debugger ai processi di lavoro di Asset Compute, inclusa la possibilità di impostare punti di interruzione nel codice VS e di scorrere il codice.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Click-through del debug di un processo di lavoro Asset Compute utilizzando wskdebug (nessun audio)_

1. Assicurati che i moduli [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) npm siano installati
1. Assicurati che [Docker Desktop e le immagini Docker di supporto](../set-up/development-environment.md#docker) siano installate ed in esecuzione
1. Chiudi tutte le istanze in esecuzione attive dello strumento di sviluppo.
1. Distribuisci il codice più recente utilizzando `aio app deploy` e registra il nome dell&#39;azione distribuita (nome compreso tra `[...]`). Verrà utilizzato per aggiornare il `launch.json` nel passaggio 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Avvia una nuova istanza di Asset Compute Development Tool utilizzando il comando `npx adobe-asset-compute devtool`
1. In Codice VS, tocca l’icona Debug nella navigazione a sinistra
   + Se richiesto, tocca __crea un file launch.json > Node.js__ per creare un nuovo file `launch.json`.
   + In caso contrario, tocca l’icona __Gear__ a destra del menu a discesa __Avvia programma__ per aprire il `launch.json` esistente nell’editor.
1. Aggiungi la seguente configurazione di oggetto JSON all’array `configurations` :

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. Seleziona il nuovo __wskdebug__ dal menu a discesa
1. Toccare il pulsante verde __Esegui__ a sinistra del menu a discesa __wskdebug__
1. Apri `/actions/worker/index.js` e tocca a sinistra dei numeri di riga per aggiungere punti di interruzione 1. Passare alla finestra del browser dello strumento di sviluppo Asset Compute aperta nel passaggio 6
1. Toccare il pulsante __Esegui__ per eseguire il processo di lavoro
1. Torna a Codice VS, a `/actions/worker/index.js` e passa attraverso il codice
1. Per uscire dallo strumento di sviluppo debug, tocca `Ctrl-C` nel terminale che eseguiva il comando `npx adobe-asset-compute devtool` al punto 6

## Accesso ai registri da Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service sfrutta i processi di lavoro Asset Compute tramite ](../deploy/processing-profiles.md) Profili di elaborazione richiamandoli direttamente in Adobe I/O Runtime. Poiché queste chiamate non richiedono lo sviluppo locale, non è possibile eseguire il debug delle relative esecuzioni utilizzando strumenti locali come Asset Compute Development Tool o wskdebug. È invece possibile utilizzare Adobe I/O CLI per recuperare i registri dal processo di lavoro eseguito in una particolare area di lavoro in Adobe I/O Runtime.

1. Assicurati che le [variabili di ambiente specifiche per l&#39;area di lavoro](../deploy/runtime.md) siano impostate tramite `AIO_runtime_namespace` e `AIO_runtime_auth`, in base all&#39;area di lavoro che richiede il debug.
1. Dalla riga di comando, esegui `aio app logs`
   + Se l&#39;area di lavoro sta attraversando un traffico pesante, espandi il numero di registri di attivazione tramite il flag `--limit`:
      `$ aio app logs --limit=25`
1. I registri di attivazione più recenti (fino a `--limit`) forniti verranno restituiti come output del comando per la revisione.

   ![registri app aio](./assets/debug/aio-app-logs.png)

## Risoluzione dei problemi

+ [Debugger non si allega](../troubleshooting.md#debugger-does-not-attach)
+ [Punti di interruzione non in pausa](../troubleshooting.md#breakpoints-no-pausing)
+ [Debugger del codice VS non collegato](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Debugger del codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del processo di lavoro](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Timeout del processo di lavoro durante il debug](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossibile terminare il processo di debug](../troubleshooting.md#cannot-terminate-debugger-process)
