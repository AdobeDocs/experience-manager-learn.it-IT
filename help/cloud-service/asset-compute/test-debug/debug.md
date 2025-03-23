---
title: Eseguire il debug di un processo di lavoro Asset Compute
description: 'È possibile eseguire il debug dei processi di lavoro di Asset Compute in diversi modi: tramite semplici istruzioni di registro di debug, l’associazione di VS Code come debugger remoto e l’estrazione di registri per le attivazioni in Adobe I/O Runtime avviate da AEM as a Cloud Service.'
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 229
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Eseguire il debug di un processo di lavoro Asset Compute

È possibile eseguire il debug dei processi di lavoro di Asset Compute in diversi modi: tramite semplici istruzioni di registro di debug, l’associazione di VS Code come debugger remoto e l’estrazione di registri per le attivazioni in Adobe I/O Runtime avviate da AEM as a Cloud Service.

## Registrazione

La forma più semplice di debug dei processi di lavoro Asset Compute utilizza le istruzioni `console.log(..)` tradizionali nel codice del processo di lavoro. L&#39;oggetto JavaScript `console` è un oggetto globale implicito, pertanto non è necessario importarlo o richiederlo, in quanto è sempre presente in tutti i contesti.

Queste istruzioni di registro sono disponibili per la revisione in modo diverso in base alla modalità di esecuzione del processo di lavoro di Asset Compute:

+ Da `aio app run`, i registri vengono stampati in uscita standard e i [registri di attivazione dello strumento di sviluppo](../develop/development-tool.md)
  ![esecuzione app console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Da `aio app test`, i registri vengono stampati in `/build/test-results/test-worker/test.log`
  ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Utilizzando `wskdebug`, le istruzioni di registro vengono stampate nella console di debug del codice VS (Visualizza > Console di debug), in uscita standard
  ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Utilizzando `aio app logs`, le istruzioni di registro vengono stampate nell&#39;output del registro di attivazione

## Debugging remoto tramite il debugger collegato

>[!WARNING]
>
>Utilizzare Microsoft Visual Studio Code 1.48.0 o versione successiva per la compatibilità con wskdebug

Il modulo [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm supporta il collegamento di un debugger ai processi di lavoro di Asset Compute, inclusa la possibilità di impostare punti di interruzione nel codice VS e di scorrere il codice.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Click-through del debug di un processo di lavoro Asset Compute tramite wskdebug (nessun audio)_

1. Assicurarsi che siano installati [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) moduli npm
1. Verificare che [Docker Desktop e le immagini Docker di supporto](../set-up/development-environment.md#docker) siano installati e in esecuzione
1. Chiude tutte le istanze in esecuzione attive dello strumento di sviluppo.
1. Distribuire il codice più recente utilizzando `aio app deploy` e registrare il nome dell&#39;azione distribuita (nome compreso tra `[...]`). Utilizzato per aggiornare `launch.json` nel passaggio 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Avvia una nuova istanza di Asset Compute Development Tool con il comando `npx adobe-asset-compute devtool`
1. In Codice VS, tocca l’icona Debug nella navigazione a sinistra
   + Se richiesto, toccare __crea un file launch.json > Node.js__ per creare un nuovo file `launch.json`.
   + Altrimenti, tocca l&#39;icona __Ingranaggio__ a destra del menu a discesa __Avvia programma__ per aprire `launch.json` esistente nell&#39;editor.
1. Aggiungi la seguente configurazione di oggetto JSON all&#39;array `configurations`:

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
1. Tocca il pulsante verde __Esegui__ a sinistra del menu a discesa __wskdebug__
1. Apri `/actions/worker/index.js` e tocca a sinistra dei numeri di riga per aggiungere i punti di interruzione 1. Passa alla finestra del browser Web Asset Compute Development Tool aperta nel passaggio 6.
1. Tocca il pulsante __Esegui__ per eseguire il processo di lavoro
1. Torna a Codice VS, a `/actions/worker/index.js` e scorri il codice
1. Per uscire dallo strumento di sviluppo con debug, toccare `Ctrl-C` nel terminale che ha eseguito il comando `npx adobe-asset-compute devtool` nel passaggio 6

## Accesso ai registri da Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service sfrutta i processi di lavoro di Asset Compute tramite Profili elaborazione](../deploy/processing-profiles.md) richiamandoli direttamente in Adobe I/O Runtime. Poiché queste chiamate non coinvolgono lo sviluppo locale, non è possibile eseguire il debug delle relative esecuzioni utilizzando strumenti locali come Asset Compute Development Tool o wskdebug. È invece possibile utilizzare Adobe I/O CLI per recuperare i registri dal processo di lavoro eseguito in una particolare area di lavoro in Adobe I/O Runtime.

1. Verificare che le [variabili di ambiente specifiche dell&#39;area di lavoro](../deploy/runtime.md) siano impostate tramite `AIO_runtime_namespace` e `AIO_runtime_auth`, in base all&#39;area di lavoro che richiede il debug.
1. Dalla riga di comando, eseguire `aio app logs`
   + Se il traffico dell&#39;area di lavoro è intenso, espandere il numero di registri di attivazione tramite il flag `--limit`:
     `$ aio app logs --limit=25`
1. I registri di attivazioni più recenti (fino a `--limit`) forniti vengono restituiti come output del comando per la revisione.

   ![registri app aio](./assets/debug/aio-app-logs.png)

## Risoluzione dei problemi

+ [Il debugger non si collega](../troubleshooting.md#debugger-does-not-attach)
+ [Punti di interruzione non in pausa](../troubleshooting.md#breakpoints-no-pausing)
+ [Debugger codice VS non collegato](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Debugger del codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del processo di lavoro](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Timeout del processo di lavoro durante il debug](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossibile terminare il processo del debugger](../troubleshooting.md#cannot-terminate-debugger-process)
