---
title: ' Strumento di sviluppo Asset compute'
description: ' Asset compute Development Tool è un cablaggio Web locale che consente agli sviluppatori di configurare ed eseguire i lavoratori Risorse per computer localmente, al di fuori del contesto dell’SDK AEM rispetto alle risorse Asset compute  in Adobe I/O Runtime.'
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


#  Strumento di sviluppo Asset compute

 Asset compute Development Tool è un cablaggio Web locale che consente agli sviluppatori di configurare ed eseguire i lavoratori Risorse per computer localmente, al di fuori del contesto dell’SDK AEM rispetto alle risorse Asset compute  in Adobe I/O Runtime.

## Eseguire lo strumento di sviluppo del Asset compute 

Lo strumento di sviluppo Asset compute  può essere eseguito dalla radice del progetto di Asset compute  tramite il comando terminale:

```
$ aio app run
```

Verrà avviato lo strumento di sviluppo all&#39;indirizzo __http://localhost:9000__ e verrà automaticamente aperto in una finestra del browser. Affinché lo strumento di sviluppo possa essere eseguito, [è necessario fornire un elemento devToolToken generato automaticamente e valido tramite un parametro di query](#troubleshooting__devtooltoken).

## Informazioni sull&#39;interfaccia  Asset compute Development Tools{#interface}

![ Strumento di sviluppo Asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __File di origine:__ La selezione del file di origine viene utilizzata per:
   + Selezionato il binario della risorsa che sarà il binario `source` passato al lavoratore del Asset compute 
   + Caricare i file sorgente
1. __definizione dei profili di Asset compute:__ Definisce il lavoratore del Asset compute  da eseguire, compresi i parametri: incluso il punto finale dell&#39;URL del lavoratore, il nome della rappresentazione risultante ed eventuali parametri
1. __Esecuzione:__ Il pulsante Esegui esegue il profilo del Asset compute  come definito nell&#39;editor del profilo di configurazione del Asset compute
1. __Interrompi:__ il pulsante Interrompi annulla un&#39;esecuzione avviata toccando il pulsante Esegui
1. __Richiesta/Risposta:__ Fornisce la richiesta HTTP e la risposta a/dal lavoratore del Asset compute  in esecuzione in Adobe I/O Runtime. Può essere utile per il debug
1. __Registri di attivazione:__ i registri che descrivono l&#39;esecuzione del lavoratore del Asset compute , con eventuali errori. Queste informazioni sono disponibili anche nella versione `aio app run` standard
1. __Rappresentazioni:__ visualizza tutte le rappresentazioni generate dall&#39;esecuzione del lavoratore del Asset compute
1. __parametro di query devToolToken:__ il token dello strumento di sviluppo Asset compute  richiede la presenza di un parametro di  `devToolToken` query valido. Questo token viene generato automaticamente ogni volta che viene visualizzato un nuovo strumento di sviluppo

### Eseguire un lavoratore personalizzato

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through dell&#39;esecuzione di un lavoro Asset compute  in Strumento di sviluppo (nessun audio)_

1. Assicurarsi che  Strumento di sviluppo Asset compute sia avviato dalla directory principale del progetto utilizzando il comando `aio app run`.
1. In  Asset compute Development Tool, caricate o selezionate un file di immagine di esempio [](../assets/samples/sample-file.jpg)
   + Assicurarsi che il file sia selezionato nel menu a discesa __File di origine__
1. Rivedete l&#39;area di testo __definizione del profilo di Asset compute__
   + La chiave `worker` definisce l&#39;URL del lavoratore del Asset compute  distribuito
   + La chiave `name` definisce il nome della rappresentazione da generare
   + In questo oggetto JSON è possibile fornire altre chiavi/valori e sarà disponibile nel processo di lavoro sotto l&#39;oggetto `rendition.instructions`
      + È possibile aggiungere valori per `size`, `contrast` e `brightness`:

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. Toccate il pulsante __Esegui__
1. La sezione __Rappresentazioni__ verrà compilata con un segnaposto per la rappresentazione
1. Una volta completato il lavoratore, il segnaposto della rappresentazione visualizzerà la rappresentazione generata

Se si apporta modifiche al codice del lavoratore mentre è in esecuzione lo strumento di sviluppo, le modifiche verranno implementate a caldo. La &quot;distribuzione a caldo&quot; richiede diversi secondi, pertanto l&#39;implementazione può essere completata prima di eseguire nuovamente il lavoratore da Development Tool.

## Risoluzione dei problemi

+ [Rientro YAML errato](../troubleshooting.md#incorrect-yaml-indentation)
+ [il limite memorySize è impostato su low](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Impossibile avviare lo strumento di sviluppo a causa di private.key mancante](../troubleshooting.md#missing-private-key)
+ [File di origine non corretti](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parametro di query devToolToken mancante o non valido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Impossibile rimuovere i file sorgente](../troubleshooting.md#unable-to-remove-source-files)
+ [Rendering restituito parzialmente disegnato/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
