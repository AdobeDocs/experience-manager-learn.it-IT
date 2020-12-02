---
title: 'Test di un lavoratore Asset compute '
description: Il progetto di Asset compute  definisce un pattern per la creazione e l'esecuzione di test dei lavoratori  Asset compute.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 0%

---


# Test di un lavoratore Asset compute 

Il progetto di Asset compute  definisce un pattern per la creazione e l&#39;esecuzione di [test di operai  Asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## Anatomia di un test operaio

 test dei lavoratori Asset compute vengono suddivisi in suite di test, e all&#39;interno di ciascuna suite di test, uno o più casi di test che affermano una condizione da sottoporre a test.

La struttura dei test in un progetto  Asset compute è la seguente:

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

Ogni cast di test può contenere i file seguenti:

+ `file.<extension>`
   + File di origine da sottoporre a test (l&#39;estensione può essere qualsiasi cosa tranne `.link`)
   + Obbligatorio
+ `rendition.<extension>`
   + Rendering previsto
   + Obbligatorio, ad eccezione del test degli errori
+ `params.json`
   + Istruzioni JSON per la rappresentazione singola
   + Facoltativo
+ `validate`
   + Uno script che ottiene i percorsi dei file di rappresentazione previsti ed effettivi come argomenti e deve restituire il codice di uscita 0 se il risultato è positivo, oppure un codice di uscita diverso da zero se la convalida o il confronto non riesce.
   + Facoltativo, impostazione predefinita per il comando `diff`
   + Utilizzare uno script shell che racchiude un comando di esecuzione docker per utilizzare diversi strumenti di convalida
+ `mock-<host-name>.json`
   + Risposte HTTP formattate JSON per [schermare le chiamate al servizio esterno](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Facoltativo, utilizzato solo se il codice del lavoro rende proprie le richieste HTTP

## Scrittura di un test case

Questo test case afferma l&#39;input con parametri (`params.json`) per il file di input (`file.jpg`) genera la rappresentazione PNG prevista (`rendition.png`).

1. Eliminate innanzitutto il caso di test generato automaticamente in `simple-worker` perché non è valido, in quanto il lavoratore non copia più semplicemente l&#39;origine nella rappresentazione.`/test/asset-compute/simple-worker`
1. Create una nuova cartella di test case in `/test/asset-compute/worker/success-parameterized` per verificare l&#39;esecuzione corretta del lavoratore che genera una rappresentazione PNG.
1. Nella cartella `success-parameterized`, aggiungere il file di input [test](./assets/test/success-parameterized/file.jpg) per questo test case e denominarlo `file.jpg`.
1. Nella cartella `success-parameterized`, aggiungere un nuovo file denominato `params.json` che definisce i parametri di input del lavoratore:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   Si tratta degli stessi valori chiave/chiave passati nella definizione del profilo di Asset compute  dello strumento di sviluppo [, meno il tasto ](../develop/development-tool.md).`worker`
1. Aggiungete il file di rendering previsto [](./assets/test/success-parameterized/rendition.png) a questo caso di test e denominatelo `rendition.png`. Questo file rappresenta l&#39;output previsto del lavoratore per l&#39;input `file.jpg` specificato.
1. Dalla riga di comando, eseguire i test della radice del progetto eseguendo `aio app test`
   + Verificare che [Docker Desktop](../set-up/development-environment.md#docker) e che le immagini Docker supportate siano installate e avviate
   + Termina tutte le istanze dello strumento di sviluppo in esecuzione

![Test - Successo  ](./assets/test/success-parameterized/result.png)

## Scrittura di un caso di verifica dell&#39;errore

Questo test case verifica in modo che il lavoratore generi l&#39;errore appropriato quando il parametro `contrast` è impostato su un valore non valido.

1. Create una nuova cartella di test case in `/test/asset-compute/worker/error-contrast` per verificare l&#39;esecuzione di un errore del lavoratore a causa di un valore di parametro `contrast` non valido.
1. Nella cartella `error-contrast`, aggiungere il file di input [test](./assets/test/error-contrast/file.jpg) per questo test case e denominarlo `file.jpg`. Il contenuto di questo file è irrilevante per questo test, deve solo esistere per superare il controllo &quot;Sorgente danneggiata&quot;, per raggiungere i `rendition.instructions` controlli di validità, che questo test case convalida.
1. Nella cartella `error-contrast`, aggiungere un nuovo file denominato `params.json` che definisce i parametri di input del lavoratore con il contenuto:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Impostate i parametri `contrast` su `10`, un valore non valido, in quanto il contrasto deve essere compreso tra -1 e 1, per generare un `RenditionInstructionsError`.
   + Asserire che l&#39;errore appropriato venga restituito nei test impostando la chiave `errorReason` sul &quot;motivo&quot; associato all&#39;errore previsto. Questo parametro di contrasto non valido genera l&#39;errore [personalizzato](../develop/worker.md#errors), `RenditionInstructionsError`, pertanto imposta l&#39;errore `errorReason` sul motivo oppure`rendition_instructions_error` per asserire che viene generato.

1. Poiché non è necessario generare alcuna rappresentazione durante un&#39;esecuzione non è necessario alcun file `rendition.<extension>`.
1. Eseguire la suite di test dalla radice del progetto eseguendo il comando `aio app test`
   + Verificare che [Docker Desktop](../set-up/development-environment.md#docker) e che le immagini Docker supportate siano installate e avviate
   + Termina tutte le istanze dello strumento di sviluppo in esecuzione

![Test - Contrasto errore](./assets/test/error-contrast/result.png)

## Casi di test su Github

I casi finali di prova sono disponibili su Github al seguente indirizzo:

+ [aem-guide-wknd-asset-compute/test/asset-computing/Worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Risoluzione dei problemi

+ [Nessuna rappresentazione generata durante l&#39;esecuzione del test](../troubleshooting.md#test-no-rendition-generated)
+ [Test genera rendering non corretto](../troubleshooting.md#tests-generates-incorrect-rendition)
