---
title: Eseguire il debug di un processo di lavoro Asset compute
description: I processi di lavoro Asset compute possono essere sottoposti a debug in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino all’estrazione dei registri per le attivazioni in Adobe I/O Runtime avviate dall’AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 268
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Eseguire il debug di un processo di lavoro Asset compute

I processi di lavoro Asset compute possono essere sottoposti a debug in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino all’estrazione dei registri per le attivazioni in Adobe I/O Runtime avviate dall’AEM as a Cloud Service.

## Registrazione

La forma più semplice di debug dei processi di lavoro Asset compute utilizza `console.log(..)` istruzioni nel codice del lavoratore. Il `console` L&#39;oggetto JavaScript è un oggetto globale implicito, pertanto non è necessario importarlo o richiederlo, in quanto è sempre presente in tutti i contesti.

Queste istruzioni di registro sono disponibili per la revisione in modo diverso in base alla modalità di esecuzione del lavoratore Asset compute:

+ Da `aio app run`, registra la stampa in formato standard e [Strumenti di sviluppo](../develop/development-tool.md) Registri di attivazione
  ![aio app esegui console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Da `aio app test`, registra la stampa su `/build/test-results/test-worker/test.log`
  ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Utilizzo di `wskdebug`, registra le istruzioni per la stampa nella console di debug del codice VS (Visualizza > Console di debug), uscita standard
  ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Utilizzo di `aio app logs`, le istruzioni di registro vengono stampate nell&#39;output del registro di attivazione

## Debugging remoto tramite il debugger collegato

>[!WARNING]
>
>Utilizzare Microsoft Visual Studio Code 1.48.0 o versione successiva per la compatibilità con wskdebug

Il [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm, supporta l’associazione di un debugger ai processi di lavoro Asset compute, inclusa la possibilità di impostare punti di interruzione nel codice VS e di scorrere il codice.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Click-through del debug di un processo di lavoro Asset compute tramite wskdebug (nessun audio)_

1. Assicurare [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) i moduli npm sono installati
1. Assicurare [Docker Desktop e le immagini Docker di supporto](../set-up/development-environment.md#docker) sono installati e in esecuzione
1. Chiude tutte le istanze in esecuzione attive dello strumento di sviluppo.
1. Distribuisci il codice più recente tramite `aio app deploy`  e registra il nome dell’azione implementata (nome compreso tra `[...]`). Utilizzato per aggiornare `launch.json` al punto 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Avvia una nuova istanza di Asset compute Development Tool utilizzando il comando `npx adobe-asset-compute devtool`
1. In Codice VS, tocca l’icona Debug nella navigazione a sinistra
   + Se richiesto, tocca __crea un file launch.json > Node.js__ per creare un nuovo `launch.json` file.
   + Altrimenti, tocca il __Ingranaggio__ a destra del __Avvia programma__ menu a discesa per aprire il file `launch.json` nell’editor.
1. Aggiungi la seguente configurazione di oggetto JSON a `configurations` array:

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
1. Tocca il verde __Esegui__ a sinistra di __wskdebug__ elenco a discesa
1. Apri `/actions/worker/index.js` e tocca a sinistra dei numeri di riga per aggiungere i punti di interruzione 1. Passa alla finestra del browser Web dello strumento di sviluppo Asset compute aperta nel passaggio 6
1. Tocca il __Esegui__ per eseguire il processo di lavoro
1. Torna a Codice VS, per `/actions/worker/index.js` e scorri il codice
1. Per uscire dallo strumento di sviluppo con debug, tocca `Ctrl-C` nel terminale in esecuzione `npx adobe-asset-compute devtool` comando nel passaggio 6

## Accesso ai registri da Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service sfrutta i lavoratori Asset compute tramite Profili di elaborazione](../deploy/processing-profiles.md) richiamandoli direttamente in Adobe I/O Runtime. Poiché queste chiamate non coinvolgono lo sviluppo locale, non è possibile eseguire il debug delle relative esecuzioni utilizzando strumenti locali come Asset compute Development Tool o wskdebug. È invece possibile utilizzare l’interfaccia della riga di comando Adobe I/O per recuperare i registri dal processo di lavoro eseguito in una particolare area di lavoro in Adobe I/O Runtime.

1. Assicurati che [variabili di ambiente specifiche per l’area di lavoro](../deploy/runtime.md) sono impostati tramite `AIO_runtime_namespace` e `AIO_runtime_auth`, in base all&#39;area di lavoro che richiede il debug.
1. Dalla riga di comando, esegui `aio app logs`
   + Se il traffico dell’area di lavoro è pesante, espandi il numero di registri di attivazione tramite `--limit` contrassegno:
     `$ aio app logs --limit=25`
1. La più recente (fino al fornito `--limit`) i registri di attivazione vengono restituiti come output del comando per la revisione.

   ![registri app aio](./assets/debug/aio-app-logs.png)

## Risoluzione dei problemi

+ [Il debugger non si collega](../troubleshooting.md#debugger-does-not-attach)
+ [Punti di interruzione non in pausa](../troubleshooting.md#breakpoints-no-pausing)
+ [Debugger codice VS non collegato](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Debugger del codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del processo di lavoro](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Timeout del processo di lavoro durante il debug](../troubleshooting.md#worker-times-out-while-debugging)
+ [Impossibile terminare il processo del debugger](../troubleshooting.md#cannot-terminate-debugger-process)
