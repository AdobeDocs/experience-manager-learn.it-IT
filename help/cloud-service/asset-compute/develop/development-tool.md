---
title: Strumento di sviluppo Asset Compute
description: Lo strumento di sviluppo Asset Compute è un web harness locale che consente agli sviluppatori di configurare ed eseguire i processi di lavoro Asset Computer a livello locale, al di fuori del contesto di AEM SDK, rispetto alle risorse Asset Compute in Adobe I/O Runtime.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Strumento di sviluppo Asset Compute

Lo strumento di sviluppo Asset Compute è un web harness locale che consente agli sviluppatori di configurare ed eseguire i processi di lavoro Asset Computer a livello locale, al di fuori del contesto di AEM SDK, rispetto alle risorse Asset Compute in Adobe I/O Runtime.

## Eseguire lo strumento di sviluppo Asset Compute

Lo strumento di sviluppo Asset Compute può essere eseguito dalla directory principale del progetto Asset Compute tramite il comando del terminale:

```
$ aio app run
```

Verrà avviato lo strumento di sviluppo all&#39;indirizzo __http://localhost:9000__ e verrà aperto automaticamente in una finestra del browser. Per eseguire lo strumento di sviluppo, è necessario specificare [un devToolToken valido generato automaticamente tramite un parametro di query](#troubleshooting__devtooltoken).

## Interfaccia degli strumenti di sviluppo di Asset Compute{#interface}

![Strumento di sviluppo Asset Compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __File Source:__ La selezione del file di origine viene utilizzata per:
   + È stato selezionato il binario della risorsa che funge da binario `source` passato al processo di lavoro di Asset Compute
   + Carica file di origine
1. __Definizione profili Asset Compute:__ Definisce il processo di lavoro Asset Compute da eseguire, inclusi i parametri: incluso l&#39;endpoint URL del processo di lavoro, il nome della rappresentazione risultante ed eventuali parametri
1. __Esegui:__ Il pulsante Esegui esegue il profilo Asset Compute definito nell&#39;editor dei profili di configurazione di Asset Compute
1. __Interrompi:__ Il pulsante Interrompi annulla un&#39;esecuzione avviata dal pulsante Esegui
1. __Richiesta/risposta:__ fornisce la richiesta HTTP e la risposta a/da Asset Compute worker in esecuzione in Adobe I/O Runtime. Può essere utile per il debug
1. __Registri di attivazione:__ i registri che descrivono l&#39;esecuzione del processo di lavoro Asset Compute, insieme a eventuali errori. Queste informazioni sono disponibili anche nel formato standard `aio app run`
1. __Rappresentazioni:__ visualizza tutte le rappresentazioni generate dall&#39;esecuzione del processo di lavoro Asset Compute
1. __parametro query devToolToken:__ Il token dello strumento di sviluppo Asset Compute richiede la presenza di un parametro query `devToolToken` valido. Questo token viene generato automaticamente ogni volta che viene generato un nuovo strumento di sviluppo

### Esegui un processo di lavoro personalizzato

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through dell&#39;esecuzione di un lavoro Asset Compute nello strumento di sviluppo (nessun audio)_

1. Verificare che lo strumento di sviluppo Asset Compute sia avviato dalla directory principale del progetto utilizzando il comando `aio app run`.
1. Nello strumento di sviluppo Asset Compute, carica o seleziona un [file immagine di esempio](../assets/samples/sample-file.jpg)
   + Verifica che il file sia selezionato nel menu a discesa __File Source__
1. Rivedi l&#39;area di testo __Definizione profilo Asset Compute__
   + La chiave `worker` definisce l&#39;URL del processo di lavoro Asset Compute distribuito
   + La chiave `name` definisce il nome della rappresentazione da generare
   + Altri valori chiave possono essere forniti in questo oggetto JSON e sono disponibili nel processo di lavoro sotto l&#39;oggetto `rendition.instructions`
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

1. Tocca il pulsante __Esegui__
1. La __sezione Rappresentazioni__ verrà compilata con un segnaposto per le rappresentazioni
1. Al termine del processo di lavoro, nel segnaposto della rappresentazione verrà visualizzata la rappresentazione generata

Se si apportano modifiche al codice di lavoro mentre Development Tool è in esecuzione, le modifiche verranno &quot;implementate a caldo&quot;. La &quot;distribuzione a caldo&quot; richiede diversi secondi, quindi consenti il completamento della distribuzione prima di rieseguire il processo di lavoro da Strumento di sviluppo.

## Risoluzione dei problemi

+ [Rientro YAML non corretto](../troubleshooting.md#incorrect-yaml-indentation)
+ [Il limite memorySize è impostato su un valore troppo basso](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Impossibile avviare lo strumento di sviluppo. Private.key mancante](../troubleshooting.md#missing-private-key)
+ [Menu a discesa dei file di Source non corretto](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parametro query devToolToken mancante o non valido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Impossibile rimuovere i file di origine](../troubleshooting.md#unable-to-remove-source-files)
+ [La rappresentazione ha restituito un disegno parziale/danneggiato](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
