---
title: Test di un lavoratore Asset compute
description: Il progetto di Asset compute definisce un pattern per creare ed eseguire facilmente test di operai Asset compute.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 0%

---

# Test di un lavoratore Asset compute

Il progetto di Asset compute definisce un pattern per creare ed eseguire facilmente [test di Asset compute worker](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomia di un test operaio

I test dei lavoratori Asset compute vengono suddivisi in suite di test e all’interno di ogni suite di test, uno o più casi di test che affermano una condizione da sottoporre a test.

La struttura dei test in un progetto di Asset compute è la seguente:

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
   + File di origine da testare (l&#39;estensione può essere qualsiasi cosa tranne `.link`)
   + Obbligatorio
+ `rendition.<extension>`
   + Rendering previsto
   + Obbligatorio, ad eccezione del test di errore
+ `params.json`
   + Istruzioni JSON per il rendering singolo
   + Facoltativo
+ `validate`
   + Uno script che ottiene il percorso previsto ed effettivo del file di rendering come argomenti e deve restituire il codice di uscita 0 se il risultato è positivo, o un codice di uscita diverso da zero se la convalida o il confronto non è riuscito.
   + Facoltativo, impostazioni predefinite per il comando `diff`
   + Utilizzare uno script shell che racchiude un comando di esecuzione docker per utilizzare diversi strumenti di convalida
+ `mock-<host-name>.json`
   + Risposte HTTP formattate JSON per [schermare le chiamate al servizio esterno](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Facoltativo, utilizzato solo se il codice del processo di lavoro rende proprie le richieste HTTP.

## Scrittura di un caso di test

Questo caso di test afferma l&#39;input parametrizzato (`params.json`) per il file di input (`file.jpg`) genera il rendering PNG previsto (`rendition.png`).

1. Elimina innanzitutto il caso di test generato automaticamente `simple-worker` in `/test/asset-compute/simple-worker` perché non è valido, in quanto il nostro lavoro non copia più semplicemente l&#39;origine nel rendering.
1. Crea una nuova cartella di test case in `/test/asset-compute/worker/success-parameterized` per verificare l’esecuzione corretta del processo di lavoro che genera un rendering PNG.
1. Nella cartella `success-parameterized` , aggiungi il file di input di prova [a2/> per questo test case e denominalo `file.jpg`.](./assets/test/success-parameterized/file.jpg)
1. Nella cartella `success-parameterized` , aggiungi un nuovo file denominato `params.json` che definisce i parametri di input del processo di lavoro:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Si tratta degli stessi chiave/valori passati nella definizione del profilo Asset compute dello strumento di sviluppo ](../develop/development-tool.md), meno la chiave `worker`.[

1. Aggiungi il file di rendering [previsto](./assets/test/success-parameterized/rendition.png) a questo test case e denominalo `rendition.png`. Questo file rappresenta l&#39;output previsto del processo di lavoro per l&#39;input specificato `file.jpg`.
1. Dalla riga di comando, esegui i test della directory principale del progetto eseguendo `aio app test`
   + Assicurati che [Docker Desktop](../set-up/development-environment.md#docker) e che le immagini Docker di supporto siano installate e avviate
   + Termina qualsiasi istanza dello strumento di sviluppo in esecuzione

![Test - Completato  ](./assets/test/success-parameterized/result.png)

## Scrittura di un caso di test di verifica dell&#39;errore

Questo test case verifica che il processo di lavoro generi l&#39;errore appropriato quando il parametro `contrast` è impostato su un valore non valido.

1. Crea una nuova cartella di test case in `/test/asset-compute/worker/error-contrast` per testare un&#39;esecuzione di errore del processo di lavoro a causa di un valore di parametro `contrast` non valido.
1. Nella cartella `error-contrast` , aggiungi il file di input di prova [a2/> per questo test case e denominalo `file.jpg`. ](./assets/test/error-contrast/file.jpg) Il contenuto di questo file è irrilevante per questo test, deve solo esistere per superare il controllo &quot;Sorgente danneggiata&quot;, per raggiungere i `rendition.instructions` controlli di validità, che questo test case convalida.
1. Nella cartella `error-contrast` , aggiungi un nuovo file denominato `params.json` che definisce i parametri di input del processo di lavoro con il contenuto:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Imposta i parametri `contrast` su `10`, un valore non valido, in quanto il contrasto deve essere compreso tra -1 e 1, per generare un valore `RenditionInstructionsError`.
   + Asserisci che l’errore appropriato venga generato nei test impostando la chiave `errorReason` sul &quot;motivo&quot; associato all’errore previsto. Questo parametro di contrasto non valido genera l&#39; [errore personalizzato](../develop/worker.md#errors), `RenditionInstructionsError`, quindi imposta l&#39; `errorReason` sul motivo dell&#39;errore o `rendition_instructions_error` per asserire che viene generato.

1. Poiché non deve essere generato alcun rendering durante un&#39;esecuzione errata, non è necessario alcun file `rendition.<extension>`.
1. Esegui la suite di test dalla radice del progetto eseguendo il comando `aio app test`
   + Assicurati che [Docker Desktop](../set-up/development-environment.md#docker) e che le immagini Docker di supporto siano installate e avviate
   + Termina qualsiasi istanza dello strumento di sviluppo in esecuzione

![Test - Contrasto errore](./assets/test/error-contrast/result.png)

## Casi di test su Github

Gli ultimi casi di test sono disponibili su Github al seguente indirizzo:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Risoluzione dei problemi

+ [Nessun rendering generato durante l’esecuzione del test](../troubleshooting.md#test-no-rendition-generated)
+ [Il test genera rendering non corretto](../troubleshooting.md#tests-generates-incorrect-rendition)
