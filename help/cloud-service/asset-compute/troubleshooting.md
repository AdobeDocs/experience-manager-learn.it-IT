---
title: Risoluzione dei problemi di estensibilità del calcolo delle risorse per  AEM Assets
description: Di seguito è riportato un indice di errori e problemi comuni, insieme alle risoluzioni, che potrebbero verificarsi durante lo sviluppo e la distribuzione di Asset Compute Worker personalizzati per  AEM Assets.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# Risoluzione dei problemi di estensibilità del calcolo delle risorse

Di seguito è riportato un indice di errori e problemi comuni, insieme alle risoluzioni, che potrebbero verificarsi durante lo sviluppo e la distribuzione di Asset Compute Worker personalizzati per  AEM Assets.

## Sviluppa{#develop}

### Rendering restituito parzialmente disegnato/danneggiato{#rendition-returned-partially-drawn-or-corrupt}

+ __Errore__: Il rendering viene eseguito in modo incompleto (quando un&#39;immagine) o è danneggiato e non può essere aperto.

   ![La rappresentazione viene restituita parzialmente disegnata](./assets/troubleshooting/develop__await.png)

+ __Causa__: La funzione del `renditionCallback` lavoratore è già attiva prima che sia possibile scrivere completamente la rappresentazione `rendition.path`.
+ __Risoluzione__: Rivedete il codice del lavoratore personalizzato e assicuratevi che tutte le chiamate asincrone siano sincronizzate utilizzando `await`.

## Strumento di sviluppo{#development-tool}

### Rientro YAML errato in manifest.yml{#incorrect-yaml-indentation}

+ __Errore:__ YAMLException: rientro errato di una voce di mappatura alla riga X, colonna Y:(via standard out dal `aio app run` comando)
+ __Causa:__ I file Yaml sono sensibili alla spaziatura bianca. È probabile che il rientro non sia corretto.
+ __Risoluzione:__ Controlla `manifest.yml` e assicurati che tutti i rientri siano corretti.

### il limite memorySize è impostato su low{#memorysize-limit-is-set-too-low}

+ __Errore:__  OpenWhiskError per Dev Server locale: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Restituito HTTP 400 (Richiesta non valida) —> &quot;Il contenuto della richiesta non è corretto:requisito non riuscito: memoria 64 MB inferiore alla soglia consentita di 134217728 B&quot;
+ __Causa:__ Un `memorySize` limite per il lavoratore nel `manifest.yml` campo è stato impostato al di sotto della soglia minima consentita, come segnalato dal messaggio di errore in byte.
+ __Risoluzione:__  Rivedete i `memorySize` limiti in `manifest.yml` e accertatevi che siano tutti grandi oltre la soglia minima consentita.

### Impossibile avviare lo strumento di sviluppo a causa di private.key mancante{#missing-private-key}

+ __Errore:__ Errore Dev Server Locale: File obbligatori mancanti in validatePrivateKeyFile.... (tramite uscita standard dal `aio app run` comando)
+ __Causa:__ Il `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore nel `.env` file non indica `private.key` o non `private.key` è leggibile dall&#39;utente corrente.
+ __Risoluzione:__ Rivedete il `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore nel `.env` file e accertatevi che contenga il percorso completo e assoluto del file system `private.key` .

### File di origine non corretti{#source-files-dropdown-incorrect}

Lo strumento di sviluppo del calcolo delle risorse può inserire uno stato in cui vengono estratti dati non aggiornati, ed è più evidente nel menu a discesa del file ____ di origine che visualizza elementi non corretti.

+ __Errore:__ Il menu a discesa del file di origine visualizza elementi non corretti.
+ __Causa:__ Lo stato del browser memorizzato nella cache non è valido
+ __Risoluzione:__ Nel browser cancellare completamente lo &quot;stato applicazione&quot; della scheda del browser, la cache del browser, l&#39;archiviazione locale e il lavoratore del servizio.

### Parametro di query devToolToken mancante o non valido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Errore:__ Notifica &quot;non autorizzata&quot; nello strumento di sviluppo del calcolo delle risorse
+ __Causa:__ `devToolToken` è mancante o non valido
+ __Risoluzione:__ Chiudete la finestra del browser Strumento di sviluppo di calcolo risorse, interrompete eventuali processi dello strumento di sviluppo in esecuzione avviati tramite il `aio app run` comando e riavviate lo strumento di sviluppo (utilizzando `aio app run`).

### Impossibile rimuovere i file sorgente{#unable-to-remove-source-files}

+ __Errore:__ Non è possibile rimuovere i file sorgente aggiunti dall&#39;interfaccia utente di Strumenti di sviluppo
+ __Causa:__ Questa funzionalità non è stata implementata
+ __Risoluzione:__ Accedi al provider di archiviazione cloud utilizzando le credenziali definite in `.env`. Individuate il contenitore utilizzato da Strumenti di sviluppo (specificato anche in `.env`), individuate la cartella __sorgente__ ed eliminate eventuali immagini sorgente. È possibile che sia necessario eseguire i passaggi descritti nel menu a discesa File [sorgente non corretti](#source-files-dropdown-incorrect) se i file sorgente eliminati continuano a essere visualizzati nel menu a discesa, in quanto possono essere memorizzati nella cache localmente nello &quot;stato applicazione&quot; degli strumenti di sviluppo.

   ![Archiviazione BLOB di Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Prova{#test}

### Nessuna rappresentazione generata durante l&#39;esecuzione del test{#test-no-rendition-generated}

+ __Errore:__ Errore: Nessuna rappresentazione generata.
+ __Causa:__ Il lavoratore non è riuscito a generare una rappresentazione a causa di un errore imprevisto, ad esempio un errore di sintassi JavaScript.
+ __Risoluzione:__ Controllare l&#39;esecuzione del test `test.log` in `/build/test-results/test-worker/test.log`. Individuare la sezione in questo file corrispondente al test case non riuscito e verificare la presenza di errori.

   ![Risoluzione dei problemi - Nessuna rappresentazione generata](./assets/troubleshooting/test__no-rendition-generated.png)

### Il test genera una rappresentazione non corretta che causa il fallimento del test{#tests-generates-incorrect-rendition}

+ __Errore:__ Errore: La rappresentazione &#39;rendition.xxx&#39; non è come previsto.
+ __Causa:__ Il lavoratore genera un rendering che non era uguale a quello `rendition.<extension>` fornito nel caso di prova.
   + Se il `rendition.<extension>` file previsto non viene creato esattamente nello stesso modo del rendering generato localmente nel caso di test, il test potrebbe non riuscire in quanto potrebbero esserci differenze nei bit. Ad esempio, se il lavoratore Asset Compute modifica il contrasto utilizzando le API e il risultato previsto viene creato regolando il contrasto in Adobe Photoshop CC, i file potrebbero apparire identici, ma le variazioni minori nei bit potrebbero essere diverse.
+ __Risoluzione:__ Esaminare l&#39;output della rappresentazione dal test andando al file di rappresentazione previsto `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`e confrontarlo con il file di rappresentazione previsto nel caso di test. Per creare una risorsa prevista esatta, effettuate le seguenti operazioni:
   + Utilizzare lo strumento di sviluppo per generare una rappresentazione, convalidarla come corretta e utilizzarla come file di rappresentazione previsto
   + In alternativa, convalidate il file generato dal test in `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, convalidatelo e utilizzatelo come file di rappresentazione previsto

## Debug


### Il debugger non si collega{#debugger-does-not-attach}

+ __Errore__: Errore durante l&#39;elaborazione dell&#39;avvio: Errore: Impossibile connettersi alla destinazione di debug in...
+ __Causa__: Docker Desktop non è in esecuzione sul sistema locale. Verifica questo problema esaminando la console di debug del codice VS (Visualizza > Console di debug), confermando che l&#39;errore viene segnalato.
+ __Risoluzione__: Avviate [Docker Desktop e confermate che le immagini Docker necessarie siano installate](./set-up/development-environment.md#docker).

### Punti di interruzione non in pausa{#breakpoints-no-pausing}

+ __Errore__: Quando si esegue il lavoratore Asset Compute dallo strumento di sviluppo debug, VS Code non si ferma ai punti di interruzione.

#### Debug codice VS non collegato{#vs-code-debugger-not-attached}

+ __Causa:__ Il debugger del codice VS è stato arrestato/disconnesso.
+ __Risoluzione:__ Riavviare il debugger del codice VS e verificare che sia collegato guardando la console Output di debug del codice VS (Visualizza > Console di debug)

#### Debugger codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del lavoro{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ Il debugger del codice VS non si è collegato prima di toccare __Run__ in Development Tool.
+ __Risoluzione:__ Verificare che il debugger sia collegato rivedendo la console Debug del codice VS (Visualizza > Console di debug), quindi eseguire nuovamente il lavoratore di calcolo risorse dallo strumento di sviluppo.

### Timeout del lavoro durante il debug{#worker-times-out-while-debugging}

+ __Errore__: Report della console di debug: &quot;L&#39;azione verrà timeout in -XXX millisecondi&quot; o l&#39;anteprima del rendering dello strumento di sviluppo del calcolo delle [risorse](./develop/development-tool.md) vieneeseguita a tempo indeterminato o
+ __Causa__: Il timeout del lavoratore come definito nel file [manifest.yml](./develop/manifest.md) viene superato durante il debug.
+ __Risoluzione__: Aumenta temporaneamente il timeout del lavoratore nel file [manifest.yml](./develop/manifest.md) o accelera le attività di debug.

### Impossibile terminare il processo di debug{#cannot-terminate-debugger-process}

+ __Errore__: `Ctrl-C` nella riga di comando non interrompe il processo di debugger (`npx adobe-asset-compute devtool`).
+ __Causa__: Un bug nella `@adobe/aio-cli-plugin-asset-compute` versione 1.3.x `Ctrl-C` non viene riconosciuto come comando terminante.
+ __Risoluzione__: Aggiornamento `@adobe/aio-cli-plugin-asset-compute` alla versione 1.4.1+

   ```
   $ aio update
   ```

   ![Risoluzione dei problemi - aggiornamento aio](./assets/troubleshooting/debug__terminate.png)

## Implementa{#deploy}

### Rendering personalizzato mancante dalla risorsa in AEM{#custom-rendition-missing-from-asset}

+ __Errore:__ Elaborazione delle risorse nuove e rielaborate completata, ma manca la rappresentazione personalizzata

#### Profilo di elaborazione non applicato alla cartella antenata

+ __Causa:__ La risorsa non esiste in una cartella con il profilo di elaborazione che utilizza il lavoratore personalizzato
+ __Risoluzione:__ Applica il profilo di elaborazione a una cartella antenata della risorsa

#### Profilo di elaborazione sostituito dal profilo di elaborazione inferiore

+ __Causa:__ La risorsa esiste sotto una cartella a cui è applicato il profilo di elaborazione del lavoratore personalizzato, ma tra tale cartella e la risorsa è stato applicato un altro profilo di elaborazione che non utilizza il lavoratore cliente.
+ __Risoluzione:__ Combinare o comunque riconciliare i due profili di elaborazione e rimuovere il profilo di elaborazione intermedio

### Elaborazione risorsa non riuscita in AEM{#asset-processing-fails}

+ __Errore:__ Il contrassegno Elaborazione risorsa non riuscita viene visualizzato sulla risorsa
+ __Causa:__ Errore durante l&#39;esecuzione del lavoratore personalizzato
+ __Risoluzione:__ Seguite le istruzioni sul [debug delle attivazioni](./test-debug/debug.md#aio-app-logs) Adobe I/O Runtime utilizzando `aio app logs`.


