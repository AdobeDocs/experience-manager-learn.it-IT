---
title: Eseguire il test di un processo di lavoro Asset compute
description: Il progetto Asset Compute definisce un pattern per la creazione e l’esecuzione di test di lavoratori Asset compute.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 142
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Eseguire il test di un processo di lavoro Asset compute

Il progetto Asset Compute definisce un modello per la creazione e l&#39;esecuzione di [test dei processi di lavoro Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomia di un test di lavoro

I test dei lavoratori Asset compute vengono suddivisi in suite di test e, all’interno di ogni suite di test, uno o più casi di test affermano una condizione da testare.

La struttura dei test in un progetto Asset Compute è la seguente:

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Ogni cast di test può avere i seguenti file:

+ `file.<extension>`
   + File Source da testare (l&#39;estensione può essere qualsiasi cosa tranne `.link`)
   + Obbligatorio
+ `rendition.<extension>`
   + Rendering previsto
   + Obbligatorio, ad eccezione della verifica degli errori
+ `params.json`
   + Istruzioni JSON per la rappresentazione singola
   + Facoltativo
+ `validate`
   + Uno script che ottiene come argomenti i percorsi previsti ed effettivi del file di rendering e deve restituire il codice di uscita 0 se il risultato è ok, o un codice di uscita diverso da zero se la convalida o il confronto non è riuscito.
   + Facoltativo, viene impostato automaticamente sul comando `diff`
   + Utilizza uno script della shell che racchiude un comando di esecuzione docker per l’utilizzo di diversi strumenti di convalida
+ `mock-<host-name>.json`
   + Risposte HTTP in formato JSON per [chiamate al servizio esterno in modalità beffa](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Facoltativo, utilizzato solo se il codice del lavoratore effettua proprie richieste HTTP

## Scrittura di un test case

Questo caso di test afferma l&#39;input con parametri (`params.json`) per il file di input (`file.jpg`) genera la rappresentazione PNG prevista (`rendition.png`).

1. Eliminare prima il caso dei test di `simple-worker` generati automaticamente in `/test/asset-compute/simple-worker` perché non è valido, in quanto il processo di lavoro non copia più semplicemente l&#39;origine nella rappresentazione.
1. Creare una nuova cartella dei test case in `/test/asset-compute/worker/success-parameterized` per verificare la corretta esecuzione del processo di lavoro che genera una rappresentazione PNG.
1. Nella cartella `success-parameterized` aggiungere il [file di input](./assets/test/success-parameterized/file.jpg) di test per questo test case e denominarlo `file.jpg`.
1. Nella cartella `success-parameterized`, aggiungere un nuovo file denominato `params.json` che definisce i parametri di input del processo di lavoro:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Sono gli stessi valori di chiave passati nella definizione del profilo di Asset compute ](../develop/development-tool.md) dello strumento di sviluppo [, meno la chiave `worker`.

1. Aggiungi il [file di rendering](./assets/test/success-parameterized/rendition.png) previsto a questo test case e denominalo `rendition.png`. Questo file rappresenta l&#39;output previsto del processo di lavoro per l&#39;input specificato `file.jpg`.
1. Dalla riga di comando, eseguire i test della directory principale del progetto eseguendo `aio app test`
   + Verificare che [Docker Desktop](../set-up/development-environment.md#docker) e le immagini Docker di supporto siano installati e avviati
   + Termina tutte le istanze dello strumento di sviluppo in esecuzione

![Test - Completato ](./assets/test/success-parameterized/result.png)

## Scrittura di un test case di controllo degli errori

Questo test case verifica che il processo di lavoro generi l&#39;errore appropriato quando il parametro `contrast` è impostato su un valore non valido.

1. Creare una nuova cartella dei test case in `/test/asset-compute/worker/error-contrast` per testare un&#39;esecuzione errata del processo di lavoro a causa di un valore del parametro `contrast` non valido.
1. Nella cartella `error-contrast` aggiungere il [file di input](./assets/test/error-contrast/file.jpg) di test per questo test case e denominarlo `file.jpg`. Il contenuto di questo file non è rilevante per il test. È sufficiente che esista per superare il controllo &quot;Origine danneggiata&quot; per raggiungere i `rendition.instructions` controlli di validità convalidati dal test case.
1. Nella cartella `error-contrast`, aggiungere un nuovo file denominato `params.json` che definisce i parametri di input del processo di lavoro con il contenuto:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Impostare i parametri `contrast` su `10`. Valore non valido. Il contrasto deve essere compreso tra -1 e 1 per generare un `RenditionInstructionsError`.
   + Asserire che l&#39;errore appropriato viene generato nei test impostando la chiave `errorReason` sul &quot;motivo&quot; associato all&#39;errore previsto. Questo parametro di contrasto non valido genera l&#39;[errore personalizzato](../develop/worker.md#errors), `RenditionInstructionsError`, quindi imposta `errorReason` sul motivo dell&#39;errore oppure `rendition_instructions_error` per verificare che sia stato generato.

1. Poiché durante un&#39;esecuzione errata non deve essere generata alcuna rappresentazione, non è necessario alcun file `rendition.<extension>`.
1. Eseguire il gruppo di test dalla radice del progetto eseguendo il comando `aio app test`
   + Verificare che [Docker Desktop](../set-up/development-environment.md#docker) e le immagini Docker di supporto siano installati e avviati
   + Termina tutte le istanze dello strumento di sviluppo in esecuzione

![Test - Contrasto errore](./assets/test/error-contrast/result.png)

## Casi di test su Github

I test case finali sono disponibili su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Risoluzione dei problemi

+ [Nessuna rappresentazione generata durante l’esecuzione del test](../troubleshooting.md#test-no-rendition-generated)
+ [Il test genera una rappresentazione errata](../troubleshooting.md#tests-generates-incorrect-rendition)
