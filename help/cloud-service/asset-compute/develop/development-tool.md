---
title: Strumento di sviluppo Asset compute
description: Lo strumento di sviluppo di Asset compute è un cablaggio web locale che consente agli sviluppatori di configurare ed eseguire localmente i processi di lavoro di Asset Computer, al di fuori del contesto dell’SDK di AEM rispetto alle risorse di Asset compute in Adobe I/O Runtime.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# Strumento di sviluppo Asset compute

Lo strumento di sviluppo di Asset compute è un cablaggio web locale che consente agli sviluppatori di configurare ed eseguire localmente i processi di lavoro di Asset Computer, al di fuori del contesto dell’SDK di AEM rispetto alle risorse di Asset compute in Adobe I/O Runtime.

## Esegui lo strumento di sviluppo Asset compute

Lo strumento di sviluppo Asset compute può essere eseguito dalla radice del progetto di Asset compute tramite il comando del terminale:

```
$ aio app run
```

Verrà avviato lo strumento di sviluppo in __http://localhost:9000__, e lo apre automaticamente in una finestra del browser. Per l&#39;esecuzione dello strumento di sviluppo, [devToolToken valido e generato automaticamente deve essere fornito tramite un parametro di query](#troubleshooting__devtooltoken).

## Interfaccia degli strumenti di sviluppo di Asset compute{#interface}

![Strumento di sviluppo Asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __File di origine:__ La selezione del file di origine viene utilizzata per:
   + Selezionato il binario della risorsa che agisce come `source` passato al lavoratore Asset compute
   + Caricare file di origine
1. __Definizione del profilo di Asset compute:__ Definisce il processo di lavoro Asset compute da eseguire, inclusi i parametri: inclusi il punto finale dell’URL del processo di lavoro, il nome della rappresentazione risultante ed eventuali parametri
1. __Esegui:__ Il pulsante Esegui esegue il profilo di Asset compute come definito nell’editor del profilo di configurazione di Asset compute
1. __Interrompi:__ Il pulsante Interrompi annulla un&#39;esecuzione avviata toccando il pulsante Esegui
1. __Richiesta/risposta:__ Fornisce la richiesta e la risposta HTTP a/dal processo di lavoro Asset compute in esecuzione in Adobe I/O Runtime. Può essere utile per il debug
1. __Registri di attivazione:__ I registri che descrivono l’esecuzione del processo di lavoro di Asset compute, insieme a eventuali errori. Queste informazioni sono disponibili anche nella `aio app run` uscita standard
1. __Rappresentazioni:__ Visualizza tutte le rappresentazioni generate dall&#39;esecuzione del processo di lavoro Asset compute
1. __Parametro di query devToolToken:__ Il token dello strumento di sviluppo di Asset compute richiede un valore valido `devToolToken` parametro di query presente. Questo token viene generato automaticamente ogni volta che viene generato un nuovo strumento di sviluppo

### Eseguire un processo di lavoro personalizzato

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through dell&#39;esecuzione di un lavoro Asset compute nello strumento di sviluppo (nessun audio)_

1. Assicurati che lo strumento di sviluppo Asset compute sia avviato dalla directory principale del progetto utilizzando `aio app run` comando.
1. Nello strumento di sviluppo di Asset compute, carica o seleziona un [file immagine di esempio](../assets/samples/sample-file.jpg)
   + Assicurati che il file sia selezionato nella __File di origine__ menu a discesa
1. Consulta la sezione __asset compute di definizione del profilo__ area di testo
   + La `worker` definisce l’URL del processo di lavoro Asset compute distribuito
   + La `name` key definisce il nome del rendering da generare
   + Altre chiavi/valori possono essere forniti in questo oggetto JSON e sono disponibili nel processo di lavoro sotto `rendition.instructions` oggetto
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

1. Tocca __Esegui__ pulsante
1. La __Sezione Rappresentazioni__ verrà popolato con un titolare di luogo di rendering
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
