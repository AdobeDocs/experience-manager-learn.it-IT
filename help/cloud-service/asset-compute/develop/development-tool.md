---
title: Strumento di sviluppo Asset compute
description: Lo strumento di sviluppo Asset compute è un web harness locale che consente agli sviluppatori di configurare ed eseguire i processi di lavoro Asset Computer a livello locale, al di fuori del contesto dell’SDK dell’AEM rispetto alle risorse Asset compute in Adobe I/O Runtime.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 216
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Strumento di sviluppo Asset compute

Lo strumento di sviluppo Asset compute è un web harness locale che consente agli sviluppatori di configurare ed eseguire i processi di lavoro Asset Computer a livello locale, al di fuori del contesto dell’SDK dell’AEM rispetto alle risorse Asset compute in Adobe I/O Runtime.

## Eseguire lo strumento di sviluppo Asset compute

Lo strumento di sviluppo Asset compute può essere eseguito dalla directory principale del progetto Asset compute tramite il comando del terminale:

```
$ aio app run
```

Verrà avviato lo strumento di sviluppo all’indirizzo __http://localhost:9000__ e aprirlo automaticamente in una finestra del browser. Per eseguire lo strumento di sviluppo, [è necessario fornire un devToolToken valido e generato automaticamente tramite un parametro di query](#troubleshooting__devtooltoken).

## Comprendere l’interfaccia degli strumenti di sviluppo Asset compute{#interface}

![Strumento di sviluppo Asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __File di origine:__ La selezione del file di origine viene utilizzata per:
   + È stato selezionato il binario della risorsa che funge da `source` binario passato al lavoratore Asset compute
   + Carica file di origine
1. __Definizione profili di Asset compute:__ Definisce il processo di lavoro Asset compute da eseguire, inclusi i parametri, tra cui l&#39;endpoint URL del processo di lavoro, il nome della rappresentazione risultante ed eventuali parametri
1. __Esegui:__ Il pulsante Esegui esegue il profilo di Asset compute come definito nell’editor del profilo di configurazione dell’Asset compute
1. __Interrompi:__ Il pulsante Interrompi annulla un’esecuzione avviata toccando il pulsante Esegui
1. __Richiesta/Risposta:__ Fornisce la richiesta HTTP e la risposta a/dal processo di lavoro Asset compute in esecuzione in Adobe I/O Runtime. Può essere utile per il debug
1. __Registri di attivazione:__ I registri che descrivono l’esecuzione del lavoratore Asset compute, insieme a eventuali errori. Queste informazioni sono disponibili anche nel `aio app run` uscita standard
1. __Rappresentazioni:__ Visualizza tutte le rappresentazioni generate dall&#39;esecuzione del processo di lavoro Asset compute
1. __Parametro query devToolToken:__ Il token dello strumento di sviluppo Asset compute richiede un `devToolToken` parametro di query da presentare. Questo token viene generato automaticamente ogni volta che viene generato un nuovo strumento di sviluppo

### Esegui un processo di lavoro personalizzato

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through dell&#39;esecuzione di un lavoro di Asset compute nello strumento di sviluppo (nessun audio)_

1. Assicurati che lo strumento di sviluppo Asset compute sia avviato dalla directory principale del progetto utilizzando `aio app run` comando.
1. Nello strumento di sviluppo Asset compute, carica o seleziona un [file immagine di esempio](../assets/samples/sample-file.jpg)
   + Assicurati che il file sia selezionato in __File di origine__ elenco a discesa
1. Rivedi __Definizione profilo Asset compute__ area di testo
   + Il `worker` La chiave definisce l’URL del processo di lavoro Asset compute distribuito
   + Il `name` chiave definisce il nome della rappresentazione da generare
   + Altri valori chiave possono essere forniti in questo oggetto JSON e sono disponibili nel processo di lavoro sotto `rendition.instructions` oggetto
      + Facoltativamente, aggiungi valori per `size`, `contrast` e `brightness`:

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

1. Tocca il __Esegui__ pulsante
1. Il __Sezione Rappresentazioni__ verrà popolato con un segnaposto per la rappresentazione
1. Al termine del processo di lavoro, nel segnaposto della rappresentazione verrà visualizzata la rappresentazione generata

Se si apportano modifiche al codice di lavoro mentre Development Tool è in esecuzione, le modifiche verranno &quot;implementate a caldo&quot;. La &quot;distribuzione a caldo&quot; richiede diversi secondi, quindi consenti il completamento della distribuzione prima di rieseguire il processo di lavoro da Strumento di sviluppo.

## Risoluzione dei problemi

+ [Rientro YAML non corretto](../troubleshooting.md#incorrect-yaml-indentation)
+ [Il limite memorySize è impostato su un valore troppo basso](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Impossibile avviare lo strumento di sviluppo. Private.key mancante](../troubleshooting.md#missing-private-key)
+ [Menu a discesa dei file di origine non corretto](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parametro query devToolToken mancante o non valido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Impossibile rimuovere i file di origine](../troubleshooting.md#unable-to-remove-source-files)
+ [La rappresentazione ha restituito un disegno parziale/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
