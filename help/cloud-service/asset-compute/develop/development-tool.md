---
title: Strumento di sviluppo del calcolo delle risorse
description: Asset Compute Development Tool è un cablaggio Web locale che consente agli sviluppatori di configurare ed eseguire i lavoratori Risorse per computer localmente, al di fuori del contesto dell’SDK AEM rispetto alle risorse Asset Compute di Adobe I/O Runtime.
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


# Strumento di sviluppo del calcolo delle risorse

Asset Compute Development Tool è un cablaggio Web locale che consente agli sviluppatori di configurare ed eseguire i lavoratori Risorse per computer localmente, al di fuori del contesto dell’SDK AEM rispetto alle risorse Asset Compute di Adobe I/O Runtime.

## Eseguire lo strumento di sviluppo del calcolo delle risorse

Lo strumento di sviluppo del calcolo delle risorse può essere eseguito dalla radice del progetto Asset Compute tramite il comando terminale:

```
$ aio app run
```

Questo attiverà lo strumento di sviluppo all&#39;indirizzo __http://localhost:9000__ e lo aprirà automaticamente in una finestra del browser. Affinché lo strumento di sviluppo possa essere eseguito, è necessario fornire [un devToolToken generato automaticamente e valido tramite un parametro](#troubleshooting__devtooltoken)di query.

## Comprendere l’interfaccia Strumenti di sviluppo di calcolo risorse{#interface}

![Strumento di sviluppo del calcolo delle risorse](./assets/development-tool/asset-compute-dev-tool.png)

1. __File di origine:__ La selezione del file di origine viene utilizzata per:
   + Selezionato il binario della risorsa che sarà il `source` binario passato al lavoratore Asset Compute
   + Caricare i file sorgente
1. __Definizione dei profili di calcolo delle risorse:__ Definisce il lavoratore Asset Compute da eseguire, compresi i parametri: incluso il punto finale dell&#39;URL del lavoratore, il nome della rappresentazione risultante ed eventuali parametri
1. __Esegui:__ Il pulsante Esegui esegue il profilo di calcolo delle risorse come definito nell’editor del profilo di configurazione di Asset Compute
1. __Interrompi:__ Il pulsante Interrompi annulla un&#39;esecuzione avviata toccando il pulsante Esegui
1. __Richiesta/Risposta:__ Fornisce la richiesta HTTP e la risposta a/da Asset Compute Worker in esecuzione in Adobe I/O Runtime. Può essere utile per il debug
1. __Registri di attivazione:__ Registri che descrivono l&#39;esecuzione del lavoratore di calcolo delle risorse, con eventuali errori. Queste informazioni sono disponibili anche nella `aio app run` versione standard
1. __Rappresentazioni:__ Visualizza tutte le rappresentazioni generate dall&#39;esecuzione del lavoratore Asset Compute
1. __parametro di query devToolToken:__ Il token dello strumento di sviluppo di elaborazione risorse richiede la presenza di un parametro di `devToolToken` query valido. Questo token viene generato automaticamente ogni volta che viene visualizzato un nuovo strumento di sviluppo

### Eseguire un lavoratore personalizzato

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through dell’esecuzione di un lavoro di elaborazione risorse in Strumento di sviluppo (nessun audio)_

1. Assicurarsi che Asset Compute Development Tool sia avviato dalla directory principale del progetto utilizzando il `aio app run` comando.
1. Nello strumento di sviluppo del calcolo delle risorse, caricate o selezionate un file immagine [di esempio](../assets/samples/sample-file.jpg)
   + Verificare che il file sia selezionato nel menu a discesa del file ____ di origine
1. Esaminare l&#39;area di testo della definizione __del profilo di calcolo della__ risorsa
   + La `worker` chiave definisce l&#39;URL del lavoratore Asset Compute distribuito
   + La `name` chiave definisce il nome della rappresentazione da generare
   + In questo oggetto JSON possono essere fornite altre chiavi/valori e saranno disponibili nel lavoratore sotto l&#39; `rendition.instructions` oggetto
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

1. Tap the __Run__ button
1. La sezione ____ Rappresentazioni verrà compilata con un segnaposto per la rappresentazione
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
