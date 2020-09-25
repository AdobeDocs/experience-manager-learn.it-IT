---
title: Debug di un lavoratore di calcolo delle risorse
description: I lavoratori di elaborazione risorse possono essere sottoposti a debugging in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino a richiamare i registri per le attivazioni in Adobe I/O Runtime iniziate da AEM come Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---


# Debug di un lavoratore di calcolo delle risorse

I lavoratori di elaborazione risorse possono essere sottoposti a debugging in diversi modi, dalle semplici istruzioni di registro di debug, al codice VS allegato come debugger remoto, fino a richiamare i registri per le attivazioni in Adobe I/O Runtime iniziate da AEM come Cloud Service.

## Registrazione

Il modulo più semplice di debug dei lavoratori di elaborazione delle risorse utilizza `console.log(..)` le istruzioni tradizionali nel codice del lavoratore. L&#39;oggetto `console` JavaScript è un oggetto globale implicito, pertanto non è necessario importarlo o richiederlo, in quanto è sempre presente in tutti i contesti.

Le seguenti istruzioni di registro sono disponibili per la revisione in modo diverso in base alle modalità di esecuzione del lavoratore Asset Compute:

+ Da `aio app run`, i registri vengono stampati in uscita standard e i registri di attivazione dello strumento di [sviluppo](../develop/development-tool.md)
   ![console di esecuzione app aio.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Da, `aio app test`i file di registro stampano su `/build/test-results/test-worker/test.log`
   ![console di test app aio.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Utilizzando, `wskdebug`le istruzioni di registro vengono stampate nella console Debug codice VS (Visualizza > Console di debug), in modalità standard
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Utilizzando `aio app logs`, le istruzioni di registro vengono stampate nell&#39;output del registro di attivazione

## Debug remoto tramite debugger collegato

>[!WARNING]
>
>Utilizzare Microsoft Visual Studio Code 1.48.0 o versione successiva per compatibilità con wskdebug

Il modulo npm [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) supporta l&#39;associazione di un debugger ai lavoratori di calcolo delle risorse, inclusa la possibilità di impostare punti di interruzione nel codice VS e di scorrere il codice.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)
_Click-through del debug di un lavoratore di elaborazione risorse con wskdebug (nessun audio)_

1. Verificare che i moduli [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) npm siano installati
1. Assicurarsi che [Docker Desktop e le immagini](../set-up/development-environment.md#docker) Docker di supporto siano installate ed in esecuzione
1. Chiude tutte le istanze in esecuzione attive dello strumento di sviluppo.
1. Distribuisci il codice più recente utilizzando `aio app deploy` e registra il nome dell’azione distribuita (nome compreso tra il `[...]`). Questo verrà utilizzato per aggiornare il `launch.json` punto 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Avvio di una nuova istanza dello strumento di calcolo risorse tramite il comando `npx adobe-asset-compute devtool`
1. In Codice VS, toccate l&#39;icona Debug nella barra di navigazione a sinistra
   + Se richiesto, toccate __create un file launch.json > Node.js__ per creare un nuovo `launch.json` file.
   + In caso contrario, toccate l&#39;icona __ingranaggio__ a destra del menu a discesa Programma __di__ avvio per aprire l&#39;esistente `launch.json` nell&#39;editor.
1. Aggiungere la seguente configurazione di oggetto JSON all&#39; `configurations` array:

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute application's version
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

1. Selezionate il nuovo __wskdebug__ dal menu a discesa
1. Toccate il pulsante verde __Esegui__ a sinistra del menu a discesa __wskdebug__
1. Aprite `/actions/worker/index.js` e toccate a sinistra dei numeri di riga per aggiungere punti di interruzione 1. Passare alla finestra del browser dello strumento di elaborazione risorse, aperta al punto 6
1. Toccate il pulsante __Esegui__ per eseguire il lavoratore
1. Torna a Codice VS, `/actions/worker/index.js` e passa attraverso il codice
1. Per uscire dallo strumento di sviluppo debug, toccate `Ctrl-C` nel terminale che ha eseguito il `npx adobe-asset-compute devtool` comando al punto 6

## Accesso ai registri da Adobe I/O Runtime{#aio-app-logs}

[AEM come Cloud Service sfrutta i lavoratori di elaborazione delle risorse tramite i profili](../deploy/processing-profiles.md) di elaborazione richiamandoli direttamente in Adobe I/O Runtime. Poiché queste chiamate non richiedono lo sviluppo locale, non è possibile eseguire il debug delle loro esecuzioni utilizzando strumenti locali come Asset Compute Development Tool o wskdebug. Al contrario, l&#39;interfaccia CLI I/O  Adobe può essere utilizzata per recuperare i log dal lavoro eseguito in una particolare area di lavoro in Adobe I/O Runtime.

1. Verificare che le variabili [di ambiente specifiche per l’](../deploy/runtime.md) area di lavoro siano impostate tramite `AIO_runtime_namespace` e `AIO_runtime_auth`, in base all’area di lavoro che richiede il debug.
1. Dalla riga di comando, esegui `aio app logs`
   + Se l’area di lavoro presenta un traffico pesante, espandete il numero di registri di attivazione tramite il `--limit` flag:
      `$ aio app logs --limit=25`
1. I log delle attivazioni più recenti (fino a `--limit`quello fornito) verranno restituiti come output del comando per la revisione.

   ![log delle app](./assets/debug/aio-app-logs.png)

## Risoluzione dei problemi

### Il debugger non si collega

+ __Errore__: Errore durante l&#39;elaborazione dell&#39;avvio: Errore: Impossibile connettersi alla destinazione di debug in...
+ __Causa__: Docker Desktop non è in esecuzione sul sistema locale. Verifica questo problema esaminando la console di debug del codice VS (Visualizza > Console di debug), confermando che l&#39;errore viene segnalato.
+ __Risoluzione__: Avviate [Docker Desktop e confermate che le immagini Docker necessarie siano installate](../set-up/development-environment.md#docker).

### Punti di interruzione non in pausa

+ __Errore__: Quando si esegue il lavoratore Asset Compute dallo strumento di sviluppo debug, VS Code non si ferma ai punti di interruzione.

#### Il debugger del codice VS non è collegato

+ __Causa:__ Il debugger del codice VS è stato arrestato/disconnesso.
+ __Risoluzione:__ Riavviare il debugger del codice VS e verificare che sia collegato guardando la console Output di debug del codice VS (Visualizza > Console di debug)

#### Debugger codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del lavoro

+ __Causa:__ Il debugger del codice VS non si è collegato prima di toccare __Run__ in Development Tool.
+ __Risoluzione:__ Verificare che il debugger sia collegato rivedendo la console Debug del codice VS (Visualizza > Console di debug), quindi eseguire nuovamente il lavoratore di calcolo risorse dallo strumento di sviluppo.

### Timeout del lavoro durante il debug

+ __Errore__: Report della console di debug: &quot;L&#39;azione verrà timeout in -XXX millisecondi&quot; o l&#39;anteprima del rendering dello strumento di sviluppo del calcolo delle [risorse](../develop/development-tool.md) vieneeseguita a tempo indeterminato o
+ __Causa__: Il timeout del lavoratore come definito nel file [manifest.yml](../develop/manifest.md) viene superato durante il debug.
+ __Risoluzione__: Aumenta temporaneamente il timeout del lavoratore nel file [manifest.yml](../develop/manifest.md) o accelera le attività di debug.

### Impossibile terminare il processo di debug

+ __Errore__: `Ctrl-C` nella riga di comando non interrompe il processo di debugger (`npx adobe-asset-compute devtool`).
+ __Causa__: Un bug nella `@adobe/aio-cli-plugin-asset-compute` versione 1.3.x `Ctrl-C` non viene riconosciuto come comando terminante.
+ __Risoluzione__: Aggiornamento `@adobe/aio-cli-plugin-asset-compute` alla versione 1.4.1+

   ```
   $ aio update
   ```

   ![Risoluzione dei problemi - aggiornamento aio](./assets/debug/troubleshooting__terminate.png)