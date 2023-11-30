---
title: Risolvere i problemi relativi all’estensibilità degli Asset compute per AEM Assets
description: Di seguito è riportato un indice dei problemi e degli errori più comuni, insieme alle relative risoluzioni, che possono verificarsi durante lo sviluppo e la distribuzione di processi di lavoro di Asset compute personalizzati per AEM Assets.
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# Risoluzione dei problemi di estensibilità degli Asset compute

Di seguito è riportato un indice dei problemi e degli errori più comuni, insieme alle relative risoluzioni, che possono verificarsi durante lo sviluppo e la distribuzione di processi di lavoro di Asset compute personalizzati per AEM Assets.

## Sviluppa{#develop}

### La rappresentazione è parzialmente disegnata/danneggiata{#rendition-returned-partially-drawn-or-corrupt}

+ __Errore__: la rappresentazione viene riprodotta in modo incompleto (quando un’immagine) o è danneggiata e non può essere aperta.

  ![La rappresentazione viene restituita parzialmente disegnata](./assets/troubleshooting/develop__await.png)

+ __Causa__: del lavoratore `renditionCallback` è in corso l&#39;uscita dalla funzione prima che la rappresentazione possa essere completamente scritta in `rendition.path`.
+ __Risoluzione__: rivedi il codice di lavoro personalizzato e assicurati che tutte le chiamate asincrone siano rese sincrone utilizzando `await`.

## Strumento di sviluppo{#development-tool}

### File Console.json mancante nel progetto Asset compute{#missing-console-json}

+ __Errore:__ Errore: file richiesti mancanti al momento della convalida (.../node_module/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) in corrispondenza di setupAssetCompute asincrono (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:AA)
+ __Causa:__ Il `console.json` file mancante nella directory principale del progetto Asset compute
+ __Risoluzione:__ Scarica un nuovo `console.json` crea il tuo progetto di Adobe I/O
   1. In console.adobe.io, apri il progetto Adobe I/O per il quale il progetto Asset compute è configurato
   1. Tocca il __Scarica__ pulsante in alto a destra
   1. Salva il file scaricato nella directory principale del progetto di Asset compute utilizzando il nome file `console.json`

### Rientro YAML non corretto in manifest.yml{#incorrect-yaml-indentation}

+ __Errore:__ YAMLException: rientro non valido di una voce di mappatura alla riga X, colonna Y:(tramite standard out da `aio app run` comando)
+ __Causa:__ I file Yaml sono sensibili allo spazio vuoto, è probabile che il rientro non sia corretto.
+ __Risoluzione:__ Rivedi il `manifest.yml` e assicurarsi che il rientro sia corretto.

### Il limite memorySize è impostato su un valore troppo basso{#memorysize-limit-is-set-too-low}

+ __Errore:__  OpenWhiskError del server di sviluppo locale: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true ha restituito HTTP 400 (richiesta non valida) —> &quot;Il contenuto della richiesta non era corretto:requisito non riuscito: memoria inferiore di 64 MB alla soglia consentita di 134217728 B&quot;
+ __Causa:__ A `memorySize` limite per il lavoratore nel `manifest.yml` è stato impostato al di sotto della soglia minima consentita come segnalato dal messaggio di errore in byte.
+ __Risoluzione:__  Rivedi `memorySize` limiti nella `manifest.yml` e garantire che siano tutti di dimensioni superiori alla soglia minima consentita.

### Impossibile avviare lo strumento di sviluppo. Private.key mancante{#missing-private-key}

+ __Errore:__ Errore del server di sviluppo locale: file richiesti mancanti in validatePrivateKeyFile.... (tramite uscita standard da `aio app run` comando)
+ __Causa:__ Il `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore in `.env` file, non punta a `private.key` o `private.key` non è leggibile dall&#39;utente corrente.
+ __Risoluzione:__ Rivedi `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore in `.env` e assicurarsi che contenga il percorso completo e assoluto del file `private.key` sul file system.

### Menu a discesa dei file di origine non corretto{#source-files-dropdown-incorrect}

Lo strumento di sviluppo Asset compute può entrare in uno stato in cui richiama dati non aggiornati ed è più evidente nel __File di origine__ menu a discesa che mostra gli elementi non corretti.

+ __Errore:__ Nel menu a discesa del file di origine vengono visualizzati elementi non corretti.
+ __Causa:__ Lo stato obsoleto del browser nella cache causa
+ __Risoluzione:__ Nel browser, cancella completamente lo &quot;stato dell’applicazione&quot; della scheda del browser, la cache del browser, l’archiviazione locale e il service worker.

### Parametro query devToolToken mancante o non valido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Errore:__ Notifica &quot;Non autorizzata&quot; nello strumento di sviluppo Asset compute
+ __Causa:__ `devToolToken` è mancante o non valido
+ __Risoluzione:__ Chiudi la finestra del browser Strumento di sviluppo di Asset compute, interrompi tutti i processi dello strumento di sviluppo in esecuzione avviati tramite `aio app run` e riavvia Strumento di sviluppo (utilizzando `aio app run`).

### Impossibile rimuovere i file di origine{#unable-to-remove-source-files}

+ __Errore:__ Non è possibile rimuovere i file di origine aggiunti dall’interfaccia utente degli strumenti di sviluppo
+ __Causa:__ Questa funzionalità non è stata implementata
+ __Risoluzione:__ Accedi al provider di archiviazione cloud utilizzando le credenziali definite in `.env`. Individua il contenitore utilizzato dagli strumenti di sviluppo (specificati anche in `.env`), accedi a __sorgente__ ed eliminare le immagini di origine. Potrebbe essere necessario eseguire i passaggi descritti in [Menu a discesa dei file di origine non corretto](#source-files-dropdown-incorrect) se i file di origine eliminati continuano a essere visualizzati nel menu a discesa in quanto possono essere memorizzati nella cache locale nello &quot;stato dell’applicazione&quot; degli strumenti di sviluppo.

  ![Archiviazione BLOB di Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Prova{#test}

### Nessuna rappresentazione generata durante l’esecuzione del test{#test-no-rendition-generated}

+ __Errore:__ Errore: nessuna rappresentazione generata.
+ __Causa:__ Il processo di lavoro non è riuscito a generare una rappresentazione a causa di un errore imprevisto, ad esempio un errore di sintassi JavaScript.
+ __Risoluzione:__ Rivedi il di esecuzione del test `test.log` a `/build/test-results/test-worker/test.log`. Individua la sezione in questo file corrispondente al test case non riuscito e individua eventuali errori.

  ![Risoluzione dei problemi - Nessuna rappresentazione generata](./assets/troubleshooting/test__no-rendition-generated.png)

### Il test genera una rappresentazione errata causando un errore del test{#tests-generates-incorrect-rendition}

+ __Errore:__ Errore: &#39;rendition.xxx&#39; della rappresentazione non conforme al previsto.
+ __Causa:__ Il processo di lavoro ha generato una rappresentazione diversa da quella `rendition.<extension>` fornito nel test case.
   + Se il valore previsto `rendition.<extension>` Il file non viene creato esattamente nello stesso modo della rappresentazione generata localmente nel caso di test, il test potrebbe non riuscire in quanto ci potrebbero essere alcune differenze nei bit. Ad esempio, se il processo di lavoro Asset compute modifica il contrasto utilizzando le API e il risultato atteso viene creato regolando il contrasto in Adobe Photoshop CC, i file possono apparire gli stessi, ma le variazioni minori nei bit possono essere diverse.
+ __Risoluzione:__ Rivedi l’output della rappresentazione dal test passando a `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`e confrontarlo con il file di rappresentazione previsto nel test case. Per creare una risorsa prevista esatta:
   + Utilizza lo strumento di sviluppo per generare una rappresentazione, convalidarla correttamente e utilizzarla come file di rappresentazione previsto
   + In alternativa, convalida il file generato dal test in `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, verificare che sia corretto e utilizzarlo come file di rappresentazione previsto

## Debug

### Il debugger non si collega{#debugger-does-not-attach}

+ __Errore__: errore durante l’elaborazione del lancio. Errore: impossibile connettersi alla destinazione di debug in...
+ __Causa__: Docker Desktop non è in esecuzione nel sistema locale. Verifica questo fatto esaminando la console di debug del codice VS (Visualizza > Console di debug), e conferma che questo errore sia segnalato.
+ __Risoluzione__: Avvio [Docker Desktop e conferma l&#39;installazione delle immagini Docker richieste](./set-up/development-environment.md#docker).

### Punti di interruzione non in pausa{#breakpoints-no-pausing}

+ __Errore__: quando si esegue il processo di lavoro Asset compute dallo strumento di sviluppo con debug, VS Code non viene messo in pausa nei punti di interruzione.

#### Debugger codice VS non collegato{#vs-code-debugger-not-attached}

+ __Causa:__ Il debugger del codice VS è stato arrestato/disconnesso.
+ __Risoluzione:__ Riavvia il debugger del codice VS e verifica che sia collegato guardando la console Output debug codice VS (Visualizza > Console di debug)

#### Debugger del codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del processo di lavoro{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ Il debugger del codice VS non è stato collegato prima del tocco __Esegui__ nello strumento di sviluppo.
+ __Risoluzione:__ Assicurati che il debugger sia stato allegato esaminando la Console di debug di VS Code (Visualizza > Console di debug), quindi esegui nuovamente il processo di lavoro Asset compute da Strumento di sviluppo.

### Timeout del processo di lavoro durante il debug{#worker-times-out-while-debugging}

+ __Errore__: la console di debug segnala &quot;L’azione si interrompe in -XXX millisecondi&quot; oppure [Strumenti per lo sviluppo Asset compute](./develop/development-tool.md) l’anteprima della rappresentazione ruota a tempo indeterminato o
+ __Causa__: timeout del lavoratore come definito nella [manifest.yml](./develop/manifest.md) viene superato durante il debug.
+ __Risoluzione__: aumenta temporaneamente il timeout del lavoratore in [manifest.yml](./develop/manifest.md) o accelerare le attività di debug.

### Impossibile terminare il processo del debugger{#cannot-terminate-debugger-process}

+ __Errore__: `Ctrl-C` nella riga di comando non termina il processo di debugger (`npx adobe-asset-compute devtool`).
+ __Causa__: bug in `@adobe/aio-cli-plugin-asset-compute` 1.3.x, risultati in `Ctrl-C` non riconosciuto come comando di terminazione.
+ __Risoluzione__: Aggiornamento `@adobe/aio-cli-plugin-asset-compute` alla versione 1.4.1+

  ```
  $ aio update
  ```

  ![Risoluzione dei problemi - Aggiornamento aio](./assets/troubleshooting/debug__terminate.png)

## Distribuzione{#deploy}

### Rappresentazione personalizzata mancante dalla risorsa in AEM{#custom-rendition-missing-from-asset}

+ __Errore:__ Le risorse nuove e rielaborate vengono elaborate correttamente, ma manca la rappresentazione personalizzata

#### Profilo di elaborazione non applicato alla cartella predecessore

+ __Causa:__ La risorsa non esiste in una cartella con il profilo di elaborazione che utilizza il processo di lavoro personalizzato
+ __Risoluzione:__ Applicare il profilo di elaborazione a una cartella precedente della risorsa

#### Profilo di elaborazione sostituito da Profilo di elaborazione inferiore

+ __Causa:__ La risorsa si trova sotto una cartella a cui è stato applicato il profilo di elaborazione del processo di lavoro personalizzato. Tuttavia, tra tale cartella e la risorsa è stato applicato un profilo di elaborazione diverso che non utilizza il processo di lavoro del cliente.
+ __Risoluzione:__ Combinare o riconciliare in altro modo i due profili di elaborazione e rimuovere il profilo di elaborazione intermedio

### Elaborazione delle risorse non riuscita in AEM{#asset-processing-fails}

+ __Errore:__ Elaborazione risorsa Badge non riuscito visualizzato sulla risorsa
+ __Causa:__ Si è verificato un errore nell&#39;esecuzione del processo di lavoro personalizzato
+ __Risoluzione:__ Segui le istruzioni su [debug delle attivazioni di Adobe I/O Runtime](./test-debug/debug.md#aio-app-logs) utilizzo `aio app logs`.
