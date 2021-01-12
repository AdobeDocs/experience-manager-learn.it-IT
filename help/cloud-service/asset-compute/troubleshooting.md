---
title: Risoluzione dei problemi  estensibilità del Asset compute per  AEM Assets
description: Di seguito è riportato un indice di problemi ed errori comuni, insieme alle risoluzioni, che potrebbero verificarsi durante lo sviluppo e la distribuzione di Asset compute  personalizzati per  AEM Assets.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 649d971ecaa67c0d1dd2636f3c212bfee3d13561
workflow-type: tm+mt
source-wordcount: '1241'
ht-degree: 0%

---


# Risoluzione dei problemi &#39;estensibilità del Asset compute

Di seguito è riportato un indice di problemi ed errori comuni, insieme alle risoluzioni, che potrebbero verificarsi durante lo sviluppo e la distribuzione di Asset compute  personalizzati per  AEM Assets.

## Sviluppa{#develop}

### La rappresentazione viene restituita parzialmente disegnata/danneggiata{#rendition-returned-partially-drawn-or-corrupt}

+ __Errore__: Il rendering viene eseguito in modo incompleto (quando un&#39;immagine) o è danneggiato e non può essere aperto.

   ![La rappresentazione viene restituita parzialmente disegnata](./assets/troubleshooting/develop__await.png)

+ __Causa__: La  `renditionCallback` funzione del lavoratore esiste prima che la rappresentazione possa essere scritta completamente in  `rendition.path`.
+ __Risoluzione__: Rivedete il codice del lavoratore personalizzato e assicuratevi che tutte le chiamate asincrone siano sincronizzate utilizzando  `await`.

## Strumento di sviluppo{#development-tool}

### File Console.json mancante nel progetto  Asset compute{#missing-console-json}

+ __Errore:__ Errore: File richiesti mancanti alla convalida (.../node_module/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) in corrispondenza dell’impostazione asincronaAssetCompute (.../node_module/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:AA)
+ __Causa:__ Il  `console.json` file non è presente nella directory principale del progetto di Asset compute
+ __Risoluzione:__ Scaricare un nuovo  `console.json` modulo dal progetto Adobe I/O
   1. In console.adobe.io, apri il progetto Adobe I/O  il progetto del Asset compute  è configurato per l’utilizzo
   1. Toccate il pulsante __Scarica__ in alto a destra
   1. Salvate il file scaricato nella directory principale del progetto di Asset compute  utilizzando il nome file `console.json`

### Rientro YAML errato in manifest.yml{#incorrect-yaml-indentation}

+ __Errore:__ YAMLException: rientro errato di una voce di mappatura alla riga X, colonna Y:(via standard out dal  `aio app run` comando)
+ __Causa: i file__ Yaml sono sensibili alla spaziatura bianca. È probabile che il rientro non sia corretto.
+ __Risoluzione:__ rivedete  `manifest.yml` e accertatevi che tutti i rientri siano corretti.

### il limite memorySize è impostato su low{#memorysize-limit-is-set-too-low}

+ __Errore:OpenWhiskError per il server di sviluppo__  locale: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Restituito HTTP 400 (Richiesta non valida) —> &quot;Il contenuto della richiesta non è corretto:requisito non riuscito: memoria 64 MB inferiore alla soglia consentita di 134217728 B&quot;
+ __Causa:__ Un  `memorySize` limite per il lavoratore nel campo  `manifest.yml` è stato impostato al di sotto della soglia minima consentita, come segnalato dal messaggio di errore in byte.
+ __Risoluzione:__  rivedete i  `memorySize` limiti  `manifest.yml` e accertatevi che siano tutti grandi oltre la soglia minima consentita.

### Impossibile avviare lo strumento di sviluppo a causa di private.key{#missing-private-key} mancante

+ __Errore:__ Errore Dev ServerLocale: File obbligatori mancanti in validatePrivateKeyFile.... (tramite uscita standard da `aio app run` comando)
+ __Causa:__ il  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore nel  `.env` file non indica  `private.key` o non  `private.key` è leggibile dall&#39;utente corrente.
+ __Risoluzione:__ Esaminare il  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore nel  `.env` file e assicurarsi che contenga il percorso completo e assoluto del file  `private.key` nel file system.

### Il menu a discesa dei file di origine non è corretto{#source-files-dropdown-incorrect}

 Strumento di sviluppo Asset compute può immettere uno stato in cui vengono estratti dati non aggiornati ed è più evidente nel menu a discesa __File di origine__ che visualizza elementi non corretti.

+ __Errore: nel menu a discesa del file__ di origine vengono visualizzati elementi non corretti.
+ __Causa:lo stato del browser__ non aggiornato nella cache causa il
+ __Risoluzione:__ Nel browser cancellare completamente lo &quot;stato applicazione&quot; della scheda del browser, la cache del browser, l&#39;archivio locale e il lavoratore del servizio.

### Parametro query devToolToken mancante o non valido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Errore:__  notifica &quot;Non autorizzata&quot; nello strumento di sviluppo  Asset compute
+ __Causa:__ `devToolToken` mancante o non valido
+ __Risoluzione:__ chiudere la finestra del browser dello strumento di sviluppo del Asset compute , terminare eventuali processi dello strumento di sviluppo in esecuzione avviati tramite il  `aio app run` comando e riavviare lo strumento di sviluppo (utilizzando  `aio app run`).

### Impossibile rimuovere i file di origine{#unable-to-remove-source-files}

+ __Errore:__ non è possibile rimuovere i file sorgente aggiunti dall&#39;interfaccia utente Strumenti di sviluppo
+ __Causa:__ Questa funzionalità non è stata implementata
+ __Risoluzione:__ accedi al provider di archiviazione cloud utilizzando le credenziali definite in  `.env`. Individuare il contenitore utilizzato dagli strumenti di sviluppo (specificato anche in `.env`), accedere alla cartella __source__ ed eliminare le immagini sorgente. È possibile che sia necessario eseguire i passaggi descritti nel menu a discesa [File di origine non corretti](#source-files-dropdown-incorrect) se i file di origine eliminati continuano a essere visualizzati nel menu a discesa in quanto possono essere memorizzati nella cache localmente nello &quot;stato dell&#39;applicazione&quot; degli strumenti di sviluppo.

   ![Archiviazione BLOB di Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Prova{#test}

### Nessuna rappresentazione generata durante l&#39;esecuzione del test{#test-no-rendition-generated}

+ __Errore:__ Errore: Nessuna rappresentazione generata.
+ __Causa:__ il lavoratore non è riuscito a generare una rappresentazione a causa di un errore imprevisto, ad esempio un errore di sintassi JavaScript.
+ __Risoluzione:__ rivedere l&#39;esecuzione del test  `test.log` in  `/build/test-results/test-worker/test.log`. Individuare la sezione in questo file corrispondente al test case non riuscito e verificare la presenza di errori.

   ![Risoluzione dei problemi - Nessuna rappresentazione generata](./assets/troubleshooting/test__no-rendition-generated.png)

### Il test genera una rappresentazione non corretta che causa il fallimento del test{#tests-generates-incorrect-rendition}

+ __Errore:__ Errore: La rappresentazione &#39;rendition.xxx&#39; non è come previsto.
+ __Causa:__ il lavoratore genera una rappresentazione che non era uguale a quella  `rendition.<extension>` fornita nel test case.
   + Se il file previsto `rendition.<extension>` non viene creato esattamente nello stesso modo del rendering generato localmente nel caso di test, il test potrebbe non riuscire in quanto potrebbero esserci differenze nei bit. Ad esempio, se il lavoratore del Asset compute  modifica il contrasto utilizzando le API e il risultato previsto viene creato regolando il contrasto in Adobe Photoshop CC, i file potrebbero avere lo stesso aspetto, ma variazioni minori nei bit potrebbero essere diverse.
+ __Risoluzione:__ esaminare l&#39;output della rappresentazione dal test andando al  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`file di rappresentazione previsto nel caso di prova e confrontarlo con quello previsto. Per creare una risorsa prevista esatta, effettuate le seguenti operazioni:
   + Utilizzare lo strumento di sviluppo per generare una rappresentazione, convalidarla come corretta e utilizzarla come file di rappresentazione previsto
   + In alternativa, convalidate il file generato dal test su `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, convalidatelo e utilizzatelo come file di rappresentazione previsto

## Debug

### Il debugger non si collega{#debugger-does-not-attach}

+ __Errore__: Errore durante l&#39;elaborazione dell&#39;avvio: Errore: Impossibile connettersi alla destinazione di debug in...
+ __Causa__: Docker Desktop non è in esecuzione sul sistema locale. Verifica questo problema esaminando la console di debug del codice VS (Visualizza > Console di debug), confermando che l&#39;errore viene segnalato.
+ __Risoluzione__: Avviate  [Docker Desktop e confermate che le immagini Docker necessarie siano installate](./set-up/development-environment.md#docker).

### Punti di interruzione non in pausa{#breakpoints-no-pausing}

+ __Errore__: Quando si esegue il lavoratore del Asset compute  dallo strumento di sviluppo debug, VS Code non si ferma ai punti di interruzione.

#### Debugger codice VS non collegato{#vs-code-debugger-not-attached}

+ __Causa:__ Il debugger del codice VS è stato arrestato/disconnesso.
+ __Risoluzione:__ Riavviare il debugger del codice VS e verificare che sia collegato guardando la console Output di debug del codice VS (Visualizza > Console di debug)

#### Debug del codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del lavoro{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ Il debugger del codice VS non si è collegato prima di toccare  ____ Runin Development Tool.
+ __Risoluzione:__ verificare che il debugger sia collegato rivedendo la console Debug del codice VS (Visualizza > Console di debug), quindi eseguire nuovamente il lavoratore del Asset compute  dallo strumento di sviluppo.

### Timeout del lavoro durante il debug{#worker-times-out-while-debugging}

+ __Errore__: Report della console di debug: &quot;L&#39;azione verrà timeout in -XXX millisecondi&quot; o  [ Asset compute Strumento di sviluppo ](./develop/development-tool.md) esegue l&#39;anteprima delle rappresentazioni in modo indefinito o
+ __Causa__: Il timeout del lavoratore come definito nel file  [manifest.](./develop/manifest.md) ymlis è stato superato durante il debug.
+ __Risoluzione__: Aumenta temporaneamente il timeout del lavoratore nel file  [manifest.](./develop/manifest.md) ymlor accelera le attività di debug.

### Impossibile terminare il processo di debug{#cannot-terminate-debugger-process}

+ __Errore__:  `Ctrl-C` nella riga di comando non interrompe il processo di debugger (`npx adobe-asset-compute devtool`).
+ __Causa__: Un bug nella  `@adobe/aio-cli-plugin-asset-compute` versione 1.3.x  `Ctrl-C` non viene riconosciuto come comando terminante.
+ __Risoluzione__: Aggiornamento  `@adobe/aio-cli-plugin-asset-compute` alla versione 1.4.1+

   ```
   $ aio update
   ```

   ![Risoluzione dei problemi - aggiornamento aio](./assets/troubleshooting/debug__terminate.png)

## Implementa{#deploy}

### Rendering personalizzato mancante dalla risorsa in AEM{#custom-rendition-missing-from-asset}

+ __Errore: elaborazione delle risorse__ nuove e rielaborate completata, ma manca la rappresentazione personalizzata

#### Profilo di elaborazione non applicato alla cartella antenata

+ __Causa:__ la risorsa non esiste in una cartella con il profilo di elaborazione che utilizza il lavoratore personalizzato
+ __Risoluzione:__ applica il profilo di elaborazione a una cartella antenata della risorsa

#### Profilo di elaborazione sostituito dal profilo di elaborazione inferiore

+ __Causa:__ la risorsa esiste sotto una cartella a cui è applicato il profilo di elaborazione del lavoratore personalizzato, ma tra tale cartella e la risorsa è stato applicato un altro profilo di elaborazione che non utilizza il lavoratore cliente.
+ __Risoluzione:__ combinare o altrimenti riconciliare i due profili di elaborazione e rimuovere il profilo di elaborazione intermedio

### Elaborazione risorsa non riuscita in AEM{#asset-processing-fails}

+ __Errore: icona Elaborazione__ risorsa non riuscita visualizzata sulla risorsa
+ __Causa:__ Si è verificato un errore nell&#39;esecuzione del lavoratore personalizzato
+ __Risoluzione:__ seguite le istruzioni sul  [debug ](./test-debug/debug.md#aio-app-logs) delle attività Adobe I/O Runtime  `aio app logs`.


