---
title: Risoluzione dei problemi di estensibilità di Asset Compute per AEM Assets
description: Di seguito è riportato un indice dei problemi e degli errori comuni, insieme alle risoluzioni, che possono essere riscontrati durante lo sviluppo e la distribuzione di processi di lavoro Asset Compute personalizzati per AEM Assets.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 0%

---

# Risolvere i problemi relativi all’estensibilità di Asset Compute

Di seguito è riportato un indice dei problemi e degli errori comuni, insieme alle risoluzioni, che possono essere riscontrati durante lo sviluppo e la distribuzione di processi di lavoro Asset Compute personalizzati per AEM Assets.

## Sviluppa{#develop}

### La rappresentazione è parzialmente disegnata/danneggiata{#rendition-returned-partially-drawn-or-corrupt}

+ __Errore__: il rendering della rappresentazione non viene eseguito completamente (quando un&#39;immagine) o è danneggiato e non può essere aperto.

  ![Rendering restituito parzialmente disegnato](./assets/troubleshooting/develop__await.png)

+ __Causa__: la funzione `renditionCallback` del processo di lavoro verrà chiusa prima che il rendering possa essere scritto completamente in `rendition.path`.
+ __Risoluzione__: rivedere il codice del processo di lavoro personalizzato e assicurarsi che tutte le chiamate asincrone siano sincronizzate utilizzando `await`.

## Strumento di sviluppo{#development-tool}

### File Console.json mancante nel progetto Asset Compute{#missing-console-json}

+ __Errore:__ Errore: file richiesti mancanti alla convalida (`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`) in setupAssetCompute asincrono (`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`)
+ __Causa:__ file `console.json` mancante nella radice del progetto Asset Compute
+ __Risoluzione:__ Scarica un nuovo `console.json` dal tuo progetto Adobe I/O
   1. In console.adobe.io, apri il progetto Adobe I/O per il quale il progetto Asset Compute è configurato
   1. Tocca il pulsante __Scarica__ in alto a destra
   1. Salva il file scaricato nella directory principale del progetto Asset Compute utilizzando il nome file `console.json`

### Rientro YAML non corretto in manifest.yml{#incorrect-yaml-indentation}

+ __Errore:__ YAMLException: rientro non valido di una voce di mapping alla riga X, colonna Y:(tramite l&#39;uscita standard dal comando `aio app run`)
+ __Causa:__ i file Yaml sono sensibili agli spazi vuoti, è probabile che il rientro non sia corretto.
+ __Risoluzione:__ Rivedi `manifest.yml` e assicurati che tutti i rientri siano corretti.

### Il limite memorySize è impostato su un valore troppo basso{#memorysize-limit-is-set-too-low}

+ __Errore:__ OpenWhiskError del server di sviluppo locale: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true ha restituito HTTP 400 (richiesta non valida) —> &quot;Il contenuto della richiesta non era corretto:requisito non riuscito: memoria inferiore di 64 MB alla soglia consentita di 134217728 B&quot;
+ __Causa:__ Un limite `memorySize` per il processo di lavoro in `manifest.yml` è stato impostato al di sotto della soglia minima consentita, come riportato dal messaggio di errore in byte.
+ __Risoluzione:__ Rivedi i limiti di `memorySize` in `manifest.yml` e assicurati che siano tutti grandi rispetto alla soglia minima consentita.

### Impossibile avviare lo strumento di sviluppo. Private.key mancante{#missing-private-key}

+ __Errore:__ Errore server di sviluppo locale: file richiesti mancanti in validatePrivateKeyFile.... (tramite uscita standard dal comando `aio app run`)
+ __Causa:__ Il valore `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` nel file `.env` non punta a `private.key` o `private.key` non è leggibile dall&#39;utente corrente.
+ __Risoluzione:__ Esaminare il valore `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` nel file `.env` e assicurarsi che contenga il percorso completo e assoluto di `private.key` nel file system.

### Menu a discesa dei file di Source non corretto{#source-files-dropdown-incorrect}

Lo strumento di sviluppo Asset Compute potrebbe entrare in uno stato in cui vengono estratti dati non aggiornati ed è più evidente nel menu a discesa __Source file__ che mostra elementi non corretti.

+ __Errore:__ nel menu a discesa del file Source vengono visualizzati elementi non corretti.
+ __Causa:__ lo stato non aggiornato del browser nella cache causa la
+ __Risoluzione:__ Nel browser cancellare completamente lo &quot;stato dell&#39;applicazione&quot; della scheda del browser, la cache del browser, l&#39;archiviazione locale e il processo di lavoro del servizio.

### Parametro query devToolToken mancante o non valido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Errore:__ notifica &quot;Non autorizzato&quot; nello strumento di sviluppo Asset Compute
+ __Causa:__ `devToolToken` mancante o non valido
+ __Risoluzione:__ Chiudere la finestra del browser Strumenti di sviluppo di Asset Compute, terminare tutti i processi dello strumento di sviluppo in esecuzione avviati tramite il comando `aio app run` e riavviare lo strumento di sviluppo (utilizzando `aio app run`).

### Impossibile rimuovere i file di origine{#unable-to-remove-source-files}

+ __Errore:__ Non è possibile rimuovere i file di origine aggiunti dall&#39;interfaccia utente degli strumenti di sviluppo
+ __Causa:__ questa funzionalità non è stata implementata
+ __Risoluzione:__ Accedi al provider di archiviazione cloud utilizzando le credenziali definite in `.env`. Individuare il contenitore utilizzato dagli strumenti di sviluppo (specificato anche in `.env`), passare alla cartella __source__ ed eliminare le immagini di origine. Potrebbe essere necessario eseguire i passaggi descritti nel menu a discesa [File Source non corretti](#source-files-dropdown-incorrect) se i file di origine eliminati continuano a essere visualizzati nel menu a discesa in quanto potrebbero essere memorizzati nella cache locale nello &quot;stato applicazione&quot; degli strumenti di sviluppo.

  ![Archiviazione BLOB di Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Test{#test}

### Nessuna rappresentazione generata durante l’esecuzione del test{#test-no-rendition-generated}

+ __Errore:__ Errore: nessuna rappresentazione generata.
+ __Causa:__ Il processo di lavoro non è riuscito a generare una rappresentazione a causa di un errore imprevisto, ad esempio un errore di sintassi JavaScript.
+ __Risoluzione:__ Rivedi `test.log` dell&#39;esecuzione del test alle `/build/test-results/test-worker/test.log`. Individua la sezione in questo file corrispondente al test case non riuscito e individua eventuali errori.

  ![Risoluzione dei problemi - Nessuna rappresentazione generata](./assets/troubleshooting/test__no-rendition-generated.png)

### Il test genera una rappresentazione errata causando un errore del test{#tests-generates-incorrect-rendition}

+ __Errore:__ Errore: la rappresentazione &#39;rendition.xxx&#39; non è quella prevista.
+ __Causa:__ Il processo di lavoro ha generato una rappresentazione diversa da quella fornita per `rendition.<extension>` nel test case.
   + Se il file `rendition.<extension>` previsto non viene creato esattamente nello stesso modo della rappresentazione generata localmente nel test case, il test potrebbe non riuscire in quanto ci potrebbe essere una qualche differenza nei bit. Ad esempio, se il processo di lavoro di Asset Compute modifica il contrasto utilizzando le API e il risultato atteso viene creato regolando il contrasto in Adobe Photoshop CC, i file possono apparire gli stessi, ma le variazioni minori nei bit possono essere diverse.
+ __Risoluzione:__ esaminare l&#39;output del rendering dal test passando a `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` e confrontarlo con il file di rendering previsto nel test case. Per creare una risorsa prevista esatta:
   + Utilizza lo strumento di sviluppo per generare una rappresentazione, convalidarla correttamente e utilizzarla come file di rappresentazione previsto
   + In alternativa, convalidare il file generato da test in `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, verificarne la correttezza e utilizzarlo come file di rendering previsto

## Debug

### Il debugger non si collega{#debugger-does-not-attach}

+ __Errore__: errore durante l&#39;elaborazione del lancio. Errore: impossibile connettersi alla destinazione di debug in...
+ __Causa__: Docker Desktop non è in esecuzione nel sistema locale. Verifica questo fatto esaminando la console di debug del codice VS (Visualizza > Console di debug), e conferma che questo errore sia segnalato.
+ __Risoluzione__: avviare [Docker Desktop e verificare che le immagini Docker richieste siano installate](./set-up/development-environment.md#docker).

### Punti di interruzione non in pausa{#breakpoints-no-pausing}

+ __Errore__: quando si esegue il processo di lavoro Asset Compute dallo strumento di sviluppo con debug, il codice VS non viene messo in pausa nei punti di interruzione.

#### Debugger codice VS non collegato{#vs-code-debugger-not-attached}

+ __Causa:__ Il debugger del codice di Visual Studio è stato arrestato/disconnesso.
+ __Risoluzione:__ riavvia il debugger del codice VS e verifica che sia collegato guardando la console di output di debug del codice VS (Visualizza > Console di debug)

#### Debugger del codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del processo di lavoro{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ il debugger del codice di Visual Studio non è stato collegato prima di toccare __Esegui__ nello strumento di sviluppo.
+ __Risoluzione:__ Verificare che il debugger sia collegato esaminando la console di debug di VS Code (Visualizza > Console di debug), quindi eseguire nuovamente Asset Compute worker da Development Tool.

### Timeout del processo di lavoro durante il debug{#worker-times-out-while-debugging}

+ __Errore__: nella console di debug viene segnalato che &quot;l&#39;azione verrà interrotta tra -XXX millisecondi&quot; oppure che l&#39;anteprima della copia trasformata ](./develop/development-tool.md) di [Asset Compute Development Tool si estende per un tempo indefinito oppure
+ __Causa__: il timeout del processo di lavoro definito in [manifest.yml](./develop/manifest.md) è stato superato durante il debug.
+ __Risoluzione__: aumenta temporaneamente il timeout del lavoratore in [manifest.yml](./develop/manifest.md) o accelera le attività di debug.

### Impossibile terminare il processo del debugger{#cannot-terminate-debugger-process}

+ __Errore__: `Ctrl-C` nella riga di comando non termina il processo del debugger (`npx adobe-asset-compute devtool`).
+ __Causa__: un bug in `@adobe/aio-cli-plugin-asset-compute` 1.3.x causa il mancato riconoscimento di `Ctrl-C` come comando di terminazione.
+ __Risoluzione__: aggiornamento di `@adobe/aio-cli-plugin-asset-compute` alla versione 1.4.1+

  ```
  $ aio update
  ```

  ![Risoluzione dei problemi - Aggiornamento aio](./assets/troubleshooting/debug__terminate.png)

## Distribuzione{#deploy}

### Rappresentazione personalizzata mancante nella risorsa in AEM{#custom-rendition-missing-from-asset}

+ __Errore:__ le risorse nuove ed elaborate nuovamente sono state elaborate correttamente, ma manca la rappresentazione personalizzata

#### Profilo di elaborazione non applicato alla cartella predecessore

+ __Causa:__ la risorsa non esiste in una cartella con il profilo di elaborazione che utilizza il processo di lavoro personalizzato
+ __Risoluzione:__ Applica il profilo di elaborazione a una cartella predecessore della risorsa

#### Profilo di elaborazione sostituito da Profilo di elaborazione inferiore

+ __Causa:__ la risorsa si trova in una cartella alla quale è stato applicato il profilo di elaborazione del processo di lavoro personalizzato. Tra la cartella e la risorsa è stato tuttavia applicato un profilo di elaborazione diverso che non utilizza il processo di lavoro del cliente.
+ __Risoluzione:__ combinare o altrimenti riconciliare i due profili di elaborazione e rimuovere il profilo di elaborazione intermedio

### L’elaborazione delle risorse in AEM non riesce{#asset-processing-fails}

+ __Errore:__ visualizzazione del badge Elaborazione risorsa non riuscita sulla risorsa
+ __Causa:__ Errore durante l&#39;esecuzione del processo di lavoro personalizzato
+ __Risoluzione:__ Segui le istruzioni su [debug delle attivazioni di Adobe I/O Runtime](./test-debug/debug.md#aio-app-logs) tramite `aio app logs`.
