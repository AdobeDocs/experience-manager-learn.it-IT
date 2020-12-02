---
title: 'Eseguire il debug di un Asset compute '
description: ' Asset compute di lavoro possono essere sottoposti a debug in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino a richiamare i log per le attivazioni in Adobe I/O Runtime iniziate da AEM come Cloud Service.'
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Eseguire il debug di un Asset compute 

 Asset compute di lavoro possono essere sottoposti a debug in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino a richiamare i log per le attivazioni in Adobe I/O Runtime iniziate da AEM come Cloud Service.

## Registrazione

La forma più semplice di debugging  Asset compute worker utilizza le istruzioni `console.log(..)` tradizionali nel codice del lavoratore. L&#39;oggetto JavaScript `console` è un oggetto globale implicito, pertanto non è necessario importarlo o richiederlo, in quanto è sempre presente in tutti i contesti.

Queste istruzioni di registro sono disponibili per la revisione in modo diverso in base alla modalità di esecuzione del lavoratore del Asset compute :

+ Da `aio app run`, i file di registro vengono stampati in uscita standard e i registri di attivazione [dello strumento di sviluppo](../develop/development-tool.md)
   ![console di esecuzione app aio.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Da `aio app test`, i file di registro vengono stampati su `/build/test-results/test-worker/test.log`
   ![console di test app aio.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Utilizzando `wskdebug`, le istruzioni del registro vengono stampate nella console di debug del codice VS (Visualizza > Console di debug), con uscita standard
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Utilizzando `aio app logs`, le istruzioni di registro vengono stampate nell&#39;output del registro di attivazione

## Debug remoto tramite debugger collegato

>[!WARNING]
>
>Utilizzare Microsoft Visual Studio Code 1.48.0 o versione successiva per compatibilità con wskdebug

Il modulo [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm supporta l&#39;associazione di un debugger a  operatori di Asset compute, inclusa la possibilità di impostare punti di interruzione nel codice VS e di scorrere il codice.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Fare clic per eseguire il debug di un Asset compute  che utilizza wskdebug (nessun audio)_

1. Assicurarsi che i moduli npm [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) siano installati
1. Assicurarsi che [Docker Desktop e le immagini Docker di supporto](../set-up/development-environment.md#docker) siano installate ed in esecuzione
1. Chiude tutte le istanze in esecuzione attive dello strumento di sviluppo.
1. Distribuire il codice più recente utilizzando `aio app deploy` e registrare il nome dell&#39;azione distribuita (nome tra `[...]`). Questo verrà utilizzato per aggiornare il `launch.json` nel passaggio 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Avviare una nuova istanza  Strumento di sviluppo Asset compute utilizzando il comando `npx adobe-asset-compute devtool`
1. In Codice VS, toccate l&#39;icona Debug nella barra di navigazione a sinistra
   + Se richiesto, toccate __create un file launch.json > Node.js__ per creare un nuovo file `launch.json`.
   + In caso contrario, toccate l&#39;icona __Gear__ a destra del menu a discesa __Launch Program__ per aprire la `launch.json` esistente nell&#39;editor.
1. Aggiungere la seguente configurazione di oggetto JSON all&#39;array `configurations`:

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

1. Selezionare il nuovo __wskdebug__ dal menu a discesa
1. Toccate il pulsante verde __Esegui__ a sinistra del menu a discesa __wskdebug__
1. Aprire `/actions/worker/index.js` e toccare a sinistra dei numeri di riga per aggiungere punti di interruzione 1. Passare alla finestra del browser Web  Asset compute Development Tool aperta nel passaggio 6
1. Toccate il pulsante __Esegui__ per eseguire il lavoro
1. Torna a Codice VS, a `/actions/worker/index.js` e passa attraverso il codice
1. Per uscire dallo strumento di sviluppo debug, toccare `Ctrl-C` nel terminale che ha eseguito il comando `npx adobe-asset-compute devtool` al punto 6

## Accesso ai registri da Adobe I/O Runtime{#aio-app-logs}

[AEM come Cloud Service sfrutta  lavoratori Asset compute tramite ](../deploy/processing-profiles.md) Profili di elaborazione richiamandoli direttamente in Adobe I/O Runtime. Poiché queste chiamate non richiedono lo sviluppo locale, non è possibile eseguire il debug delle loro esecuzioni utilizzando strumenti locali come  Strumento di sviluppo Asset compute o wskdebug. Al contrario, l&#39;interfaccia CLI di Adobe I/O  può essere utilizzata per recuperare i registri dal lavoro eseguito in una particolare area di lavoro in Adobe I/O Runtime.

1. Verificare che le variabili di ambiente specifiche per l&#39;area di lavoro [siano impostate tramite `AIO_runtime_namespace` e `AIO_runtime_auth`, in base all&#39;area di lavoro che richiede il debug.](../deploy/runtime.md)
1. Dalla riga di comando, eseguire `aio app logs`
   + Se l&#39;area di lavoro presenta un traffico pesante, espandete il numero di registri di attivazione tramite il flag `--limit`:
      `$ aio app logs --limit=25`
1. I log delle attivazioni più recenti (fino a `--limit`) forniti verranno restituiti come output del comando per la revisione.

   ![log delle app](./assets/debug/aio-app-logs.png)

## Risoluzione dei problemi

+ [Il debugger non si collega](../troubleshooting.md#debugger-does-not-attach)
+ [Punti di interruzione non in pausa](../troubleshooting.md#breakpoints-no-pausing)
+ [Debug codice VS non collegato](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Debugger codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del lavoro](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Timeout del lavoro durante il debug](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossibile terminare il processo di debug](../troubleshooting.md#cannot-terminate-debugger-process)
