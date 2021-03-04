---
title: Strumento di sviluppo Asset Compute
description: Asset Compute Development Tool è un cablaggio web locale che consente agli sviluppatori di configurare ed eseguire in locale i processi di lavoro Asset Computer, al di fuori del contesto dell’SDK AEM, in base alle risorse Asset Compute in Adobe I/O Runtime.
feature: Microservizi Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrazioni, Sviluppo
role: Developer (Sviluppatore)
level: Intermedio, esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---


# Strumento di sviluppo Asset Compute

Asset Compute Development Tool è un cablaggio web locale che consente agli sviluppatori di configurare ed eseguire in locale i processi di lavoro Asset Computer, al di fuori del contesto dell’SDK AEM, in base alle risorse Asset Compute in Adobe I/O Runtime.

## Esegui lo strumento di sviluppo Asset Compute

Lo strumento di sviluppo di Asset Compute può essere eseguito dalla directory principale del progetto Asset Compute tramite il comando del terminale:

```
$ aio app run
```

Verrà avviato lo strumento di sviluppo all&#39;indirizzo __http://localhost:9000__ e lo verrà automaticamente aperto in una finestra del browser. Affinché lo strumento di sviluppo possa essere eseguito, è necessario fornire [un devToolToken valido e generato automaticamente tramite un parametro di query](#troubleshooting__devtooltoken).

## Interfaccia degli strumenti di sviluppo di Asset Compute{#interface}

![Strumento di sviluppo Asset Compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __File di origine:__ la selezione del file di origine viene utilizzata per:
   + Selezionato il binario della risorsa che sarà il binario `source` passato al processo di lavoro Asset Compute
   + Caricare file di origine
1. __Definizione dei profili Asset Compute:__ definisce il processo di lavoro Asset Compute da eseguire, inclusi i parametri: inclusi il punto finale dell’URL del processo di lavoro, il nome della rappresentazione risultante ed eventuali parametri
1. __Esegui:__ il pulsante Esegui esegue il profilo Asset Compute definito nell’editor dei profili di configurazione di Asset Compute
1. __Interrompi:__ il pulsante Interrompi annulla un&#39;esecuzione iniziata toccando il pulsante Esegui
1. __Richiesta/risposta:__ fornisce la richiesta e la risposta HTTP a/dal processo di lavoro Asset Compute in esecuzione in Adobe I/O Runtime. Può essere utile per il debug
1. __Registri di attivazione:__ i registri che descrivono l’esecuzione del processo di lavoro Asset Compute, insieme a eventuali errori. Queste informazioni sono disponibili anche nell’ uscita standard `aio app run`
1. __Rappresentazioni:__ visualizza tutte le rappresentazioni generate dall’esecuzione del processo di lavoro Asset Compute
1. __Parametro di query devToolToken:__ il token dello strumento di sviluppo di Asset Compute richiede un parametro di  `devToolToken` query valido per essere presente. Questo token viene generato automaticamente ogni volta che viene generato un nuovo strumento di sviluppo

### Eseguire un processo di lavoro personalizzato

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through dell’esecuzione di un lavoro Asset Compute nello strumento di sviluppo (nessun audio)_

1. Assicurati che Asset Compute Development Tool sia avviato dalla directory principale del progetto utilizzando il comando `aio app run` .
1. Nello strumento di sviluppo di Asset Compute, carica o seleziona un [file immagine di esempio](../assets/samples/sample-file.jpg)
   + Assicurati che il file sia selezionato nel menu a discesa __File di origine__
1. Rivedi l’area di testo __Definizione profilo Asset Compute__
   + La chiave `worker` definisce l’URL del processo di lavoro Asset Compute implementato
   + La chiave `name` definisce il nome del rendering da generare
   + È possibile fornire altri valori/chiave in questo oggetto JSON e sarà disponibile nel processo di lavoro sotto l&#39;oggetto `rendition.instructions`
      + Facoltativamente aggiungere valori per `size`, `contrast` e `brightness`:

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

1. Tocca il pulsante __Esegui__
1. La sezione __Rappresentazioni__ verrà compilata con un segnaposto per il rendering
1. Al termine del processo di lavoro, il segnaposto del rendering visualizza il rendering generato

Apportare modifiche al codice del lavoratore durante l&#39;esecuzione dello strumento di sviluppo renderà &quot;hot deploy&quot; le modifiche. La &quot;distribuzione rapida&quot; richiede diversi secondi, quindi consenti al processo di distribuzione di completare prima di eseguire nuovamente il processo di lavoro dallo strumento di sviluppo.

## Risoluzione dei problemi

+ [rientro YAML errato](../troubleshooting.md#incorrect-yaml-indentation)
+ [il limite memorySize è impostato troppo basso](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Impossibile avviare lo strumento di sviluppo a causa della mancanza di private.key](../troubleshooting.md#missing-private-key)
+ [Elenco a discesa dei file di origine errato](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parametro di query devToolToken mancante o non valido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Impossibile rimuovere i file di origine](../troubleshooting.md#unable-to-remove-source-files)
+ [Rendering restituito parzialmente disegnato/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
