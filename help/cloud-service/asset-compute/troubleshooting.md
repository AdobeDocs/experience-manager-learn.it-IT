---
title: Risolvere i problemi di estensibilità di Asset compute per AEM Assets
description: Di seguito è riportato un indice dei problemi e degli errori comuni, insieme alle risoluzioni, che possono essere riscontrati durante lo sviluppo e la distribuzione di processi di lavoro Asset compute personalizzati per AEM Assets.
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# Risolvere i problemi relativi all’estensibilità di Asset compute

Di seguito è riportato un indice dei problemi e degli errori comuni, insieme alle risoluzioni, che possono essere riscontrati durante lo sviluppo e la distribuzione di processi di lavoro Asset compute personalizzati per AEM Assets.

## Sviluppa{#develop}

### Rendering restituito parzialmente disegnato/danneggiato{#rendition-returned-partially-drawn-or-corrupt}

+ __Errore__: Il rendering viene eseguito in modo incompleto (quando un’immagine) o è danneggiato e non può essere aperto.

   ![Rendering restituito parzialmente disegnato](./assets/troubleshooting/develop__await.png)

+ __Causa__: La  `renditionCallback` funzione del lavoratore è in uscita prima che il rendering possa essere scritto completamente in  `rendition.path`.
+ __Risoluzione__: Rivedi il codice personalizzato del processo di lavoro e assicurati che tutte le chiamate asincrone siano rese sincrone utilizzando  `await`.

## Strumento di sviluppo{#development-tool}

### File Console.json mancante dal progetto di Asset compute{#missing-console-json}

+ __Errore:__ Errore: File obbligatori mancanti al momento della convalida (.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.:XX:jsYY) in async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.:XX:jsYY)
+ __Causa:__ il  `console.json` file è mancante dalla radice del progetto di Asset compute
+ __Risoluzione:__ Scaricare un nuovo  `console.json` modulo dal progetto di Adobe I/O
   1. In console.adobe.io, apri il progetto Adobe I/O configurato per l’utilizzo del progetto Asset compute.
   1. Tocca il pulsante __Scarica__ in alto a destra
   1. Salva il file scaricato nella directory principale del progetto di Asset compute utilizzando il nome file `console.json`

### rientro YAML errato in manifest.yml{#incorrect-yaml-indentation}

+ __Errore:__ YAMLException: rientro errato di una voce di mappatura alla riga X, colonna Y: (tramite standard fuori dal  `aio app run` comando)
+ __Causa:__ i file Yaml sono sensibili a spazi bianchi, è probabile che il rientro non sia corretto.
+ __Risoluzione:__ rivedi  `manifest.yml` e assicurati che tutti i rientri siano corretti.

### il limite memorySize è impostato troppo basso{#memorysize-limit-is-set-too-low}

+ __Errore:__  OpenWhiskError per server di sviluppo locale: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Restituiva HTTP 400 (Bad Request) —> &quot;Il contenuto della richiesta era malformato:required non riuscito: memoria 64 MB inferiore alla soglia consentita di 134217728 B&quot;
+ __Causa:__ un  `memorySize` limite per il lavoratore nel  `manifest.yml` è stato impostato al di sotto della soglia minima consentita, come riportato dal messaggio di errore in byte.
+ __Risoluzione:__  esamina i  `memorySize` limiti in  `manifest.yml` e assicurati che siano tutti grandi della soglia minima consentita.

### Impossibile avviare lo strumento di sviluppo a causa della mancanza di private.key{#missing-private-key}

+ __Errore:__ Errore del server di sviluppo locale: File obbligatori mancanti in validatePrivateKeyFile.... (tramite uscita standard dal comando `aio app run` )
+ __Causa:__ il  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore nel  `.env` file non indica  `private.key` o non  `private.key` è leggibile dall&#39;utente corrente.
+ __Risoluzione:__ rivedi il  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valore nel  `.env` file e assicurati che contenga il percorso completo e assoluto del  `private.key` sul tuo file system.

### Elenco a discesa dei file di origine errato{#source-files-dropdown-incorrect}

Lo strumento di sviluppo di Asset compute può inserire uno stato in cui richiama dati non aggiornati ed è più evidente nel menu a discesa __File di origine__ che visualizza elementi non corretti.

+ __Errore:__ l&#39;elenco a discesa del file di origine visualizza elementi non corretti.
+ __Causa:__ lo stato del browser memorizzato nella cache non è aggiornato
+ __Risoluzione:__ nel browser cancella completamente lo &quot;stato dell&#39;applicazione&quot; della scheda del browser, la cache del browser, lo storage locale e il service worker.

### Parametro di query devToolToken mancante o non valido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Errore:__  notifica &quot;Non autorizzata&quot; nello strumento di sviluppo di Asset compute
+ __Causa:__ `devToolToken` mancante o non valida
+ __Risoluzione:__ chiudi la finestra del browser dello strumento di sviluppo di Asset compute, interrompi tutti i processi dello strumento di sviluppo in esecuzione avviati tramite il  `aio app run` comando e riavvia lo strumento di sviluppo (utilizzando  `aio app run`).

### Impossibile rimuovere i file di origine{#unable-to-remove-source-files}

+ __Errore:__ non è possibile rimuovere i file di origine aggiunti dall&#39;interfaccia utente degli strumenti di sviluppo
+ __Causa:__ funzionalità non implementata
+ __Risoluzione:__ accedi al provider di archiviazione cloud utilizzando le credenziali definite in  `.env`. Individua il contenitore utilizzato dagli strumenti di sviluppo (specificato anche in `.env`), passa alla cartella __source__ ed elimina tutte le immagini sorgente. Potrebbe essere necessario eseguire i passaggi descritti nel menu a discesa [File di origine non corretti](#source-files-dropdown-incorrect) se i file di origine eliminati continuano a essere visualizzati nel menu a discesa in quanto possono essere memorizzati nella cache locale nello &quot;stato dell&#39;applicazione&quot; degli strumenti di sviluppo.

   ![Archiviazione BLOB di Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Prova{#test}

### Nessun rendering generato durante l’esecuzione del test{#test-no-rendition-generated}

+ __Errore:__ errore: Nessun rendering generato.
+ __Causa:__ il processo di lavoro non è riuscito a generare un rendering a causa di un errore imprevisto, ad esempio un errore di sintassi JavaScript.
+ __Risoluzione:__ rivedi l&#39;esecuzione del test  `test.log` in  `/build/test-results/test-worker/test.log`. Individua la sezione in questo file corrispondente al caso di test non riuscito e controlla la presenza di errori.

   ![Risoluzione dei problemi - Nessun rendering generato](./assets/troubleshooting/test__no-rendition-generated.png)

### Il test genera rendering non corretto che causa il mancato funzionamento del test{#tests-generates-incorrect-rendition}

+ __Errore:__ errore: La rappresentazione &#39;rendition.xxx&#39; non è come previsto.
+ __Causa:__ il processo di lavoro ha restituito un rendering diverso da quello  `rendition.<extension>` fornito nel caso di test.
   + Se nel test case il file `rendition.<extension>` previsto non viene creato esattamente come nel rendering generato localmente, il test potrebbe non riuscire in quanto potrebbero esserci alcune differenze nei bit. Ad esempio, se il lavoratore Asset compute modifica il contrasto utilizzando le API e il risultato previsto viene creato regolando il contrasto in Adobe Photoshop CC, i file potrebbero apparire uguali, ma le variazioni minori nei bit potrebbero essere diverse.
+ __Risoluzione:__ esamina l&#39;output di rendering dal test passando a  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` e confrontalo con il file di rendering previsto nel caso di test. Per creare una risorsa prevista esatta:
   + Utilizza lo strumento di sviluppo per generare un rendering, convalidalo e utilizzalo come file di rendering previsto
   + Oppure, convalida il file generato dal test in `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, lo convalida come corretto e lo utilizza come file di rendering previsto

## Debug

### Debugger non si allega{#debugger-does-not-attach}

+ __Errore__: Errore durante l&#39;elaborazione dell&#39;avvio: Errore: Impossibile connettersi alla destinazione di debug in...
+ __Causa__: Docker Desktop non è in esecuzione sul sistema locale. Verifica questo errore esaminando la Console di debug del codice VS (Visualizza > Console di debug), confermando questo errore.
+ __Risoluzione__: Avvia  [Docker Desktop e conferma che le immagini Docker richieste sono installate](./set-up/development-environment.md#docker).

### Punti di interruzione non in pausa{#breakpoints-no-pausing}

+ __Errore__: Quando si esegue il processo di lavoro Asset compute dallo strumento di sviluppo debug, il codice VS non si ferma ai punti di interruzione.

#### Debugger del codice VS non collegato{#vs-code-debugger-not-attached}

+ __Causa:__ il debugger del codice VS è stato arrestato/disconnesso.
+ __Risoluzione:__ Riavvia il debugger del codice VS e verifica che sia collegato guardando la console di output di debug del codice VS (Visualizza > Console di debug)

#### Debugger del codice VS collegato dopo l&#39;avvio dell&#39;esecuzione del processo di lavoro{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ il debugger del codice VS non si è collegato prima di toccare  ____ Esegui strumento di sviluppo.
+ __Risoluzione:__ verificare che il debugger sia stato collegato rivedendo la console di debug del codice VS (Visualizza > Console di debug), quindi eseguire nuovamente il processo di lavoro di Asset compute dallo strumento di sviluppo.

### Timeout del processo di lavoro durante il debug{#worker-times-out-while-debugging}

+ __Errore__: Report della console di debug &quot;L&#39;azione si interrompe in -XXX millisecondi&quot; o l&#39;anteprima dello strumento di sviluppo di  [Asset compute ](./develop/development-tool.md) gira indefinitamente o
+ __Causa__: Il timeout del processo di lavoro definito in  [manifest.](./develop/manifest.md) ymlis è stato superato durante il debug.
+ __Risoluzione__: Aumenta temporaneamente il timeout del lavoratore nelle attività di debug  [manifest.](./develop/manifest.md) ymlor accelerano.

### Impossibile terminare il processo di debug{#cannot-terminate-debugger-process}

+ __Errore__:  `Ctrl-C` nella riga di comando non interrompe il processo di debug (`npx adobe-asset-compute devtool`).
+ __Causa__: Un bug in  `@adobe/aio-cli-plugin-asset-compute` 1.3.x  `Ctrl-C` non viene riconosciuto come comando di terminazione.
+ __Risoluzione__: Aggiornamento  `@adobe/aio-cli-plugin-asset-compute` alla versione 1.4.1+

   ```
   $ aio update
   ```

   ![Risoluzione dei problemi - aggiornamento aio](./assets/troubleshooting/debug__terminate.png)

## Implementa{#deploy}

### Rendering personalizzato mancante dalla risorsa in AEM{#custom-rendition-missing-from-asset}

+ __Errore:__ processo delle risorse nuove ed elaborate nuovamente con successo, ma il rendering personalizzato non è presente

#### Profilo di elaborazione non applicato alla cartella antenata

+ __Causa:__ la risorsa non esiste in una cartella con il profilo di elaborazione che utilizza il processo di lavoro personalizzato
+ __Risoluzione:__ applica il profilo di elaborazione a una cartella precedente della risorsa

#### Profilo di elaborazione sostituito dal profilo di elaborazione inferiore

+ __Causa:__ la risorsa esiste sotto una cartella con il profilo di elaborazione del processo di lavoro personalizzato applicato, ma tra tale cartella e la risorsa è stato applicato un diverso profilo di elaborazione che non utilizza il processo di lavoro del cliente.
+ __Risoluzione:__ combina o in altro modo riconcilia, i due profili di elaborazione e rimuovi il profilo di elaborazione intermedio

### L’elaborazione delle risorse non riesce AEM{#asset-processing-fails}

+ __Errore:__ viene visualizzato il badge Elaborazione risorsa non riuscita sulla risorsa
+ __Causa:__ si è verificato un errore nell&#39;esecuzione del processo di lavoro personalizzato
+ __Risoluzione:__ segui le istruzioni sul  [debug dell’](./test-debug/debug.md#aio-app-logs) attivazione Adobe I/O Runtime utilizzando  `aio app logs`.
