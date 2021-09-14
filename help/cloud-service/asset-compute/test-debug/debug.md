---
title: Debug di un processo di lavoro Asset compute
description: Il debug dei processi di lavoro di Asset compute può essere eseguito in diversi modi, dalle semplici istruzioni di log di debug, al codice VS allegato come debugger remoto, all’estrazione dei registri per le attivazioni in Adobe I/O Runtime iniziate da AEM come Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Debug di un processo di lavoro Asset compute

Il debug dei processi di lavoro di Asset compute può essere eseguito in diversi modi, dalle semplici istruzioni di log di debug, al codice VS allegato come debugger remoto, all’estrazione dei registri per le attivazioni in Adobe I/O Runtime iniziate da AEM come Cloud Service.

## Registrazione

La forma più semplice di debugging dei processi di lavoro Asset compute utilizza istruzioni `console.log(..)` tradizionali nel codice del processo di lavoro. L&#39;oggetto JavaScript `console` è un oggetto globale implicito, pertanto non è necessario importarlo o richiederlo, in quanto è sempre presente in tutti i contesti.

Queste istruzioni di registro sono disponibili per la revisione in modo diverso in base alla modalità di esecuzione del processo di lavoro Asset compute:

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

Il modulo npm [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) supporta l&#39;associazione di un debugger ai processi di lavoro di Asset compute, inclusa la possibilità di impostare punti di interruzione nel codice VS e di passare attraverso il codice.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Click-through di debugging di un processo di lavoro Asset compute utilizzando wskdebug (nessun audio)_

1. Assicurati che i moduli [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) npm siano installati
1. Assicurati che [Docker Desktop e le immagini Docker di supporto](../set-up/development-environment.md#docker) siano installate ed in esecuzione
1. Chiudi tutte le istanze in esecuzione attive dello strumento di sviluppo.
1. Distribuisci il codice più recente utilizzando `aio app deploy` e registra il nome dell&#39;azione distribuita (nome compreso tra `[...]`). Verrà utilizzato per aggiornare il `launch.json` nel passaggio 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Avvia una nuova istanza di Asset compute Development Tool utilizzando il comando `npx adobe-asset-compute devtool`
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
1. Apri `/actions/worker/index.js` e tocca a sinistra dei numeri di riga per aggiungere punti di interruzione 1. Passare alla finestra del browser Web dello strumento di sviluppo Asset compute aperta nel passaggio 6
1. Toccare il pulsante __Esegui__ per eseguire il processo di lavoro
1. Torna a Codice VS, a `/actions/worker/index.js` e passa attraverso il codice
1. Per uscire dallo strumento di sviluppo debug, tocca `Ctrl-C` nel terminale che eseguiva il comando `npx adobe-asset-compute devtool` al punto 6

## Accesso ai registri da Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service sfrutta i lavoratori Asset compute tramite ](../deploy/processing-profiles.md) Profili di elaborazione richiamandoli direttamente in Adobe I/O Runtime. Poiché queste chiamate non richiedono lo sviluppo locale, non è possibile eseguire il debug delle loro esecuzioni utilizzando strumenti locali come Asset compute Development Tool o wskdebug. È invece possibile utilizzare Adobe I/O CLI per recuperare i registri dal processo di lavoro eseguito in una particolare area di lavoro in Adobe I/O Runtime.

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
