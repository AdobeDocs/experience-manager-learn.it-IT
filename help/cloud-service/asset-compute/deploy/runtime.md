---
title: Implementare  Asset compute di lavoro in Adobe I/O Runtime per l'uso con AEM come Cloud Service
description: ' progetti di Asset compute, e i relativi lavoratori, devono essere distribuiti in Adobe I/O Runtime per essere utilizzati come Cloud Service da AEM. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---


# Distribuisci in Adobe I/O Runtime

 progetti di Asset compute, e i relativi lavoratori, devono essere distribuiti in Adobe I/O Runtime tramite l&#39;interfaccia CLI  Adobe I/O, in modo che AEM possa essere utilizzato come Cloud Service.

Quando si distribuisce in Adobe I/O Runtime per l&#39;uso da parte di AEM come servizi Cloud Service Author, sono necessarie solo due variabili di ambiente:

+ `AIO_runtime_namespace` indica all&#39;area di lavoro del progetto del Adobe  di distribuire
+ `AIO_runtime_auth` sono le credenziali di autenticazione dell&#39;area di lavoro Project Firefly del  Adobe

Le altre variabili standard definite nel file `.env` vengono implicitamente fornite da AEM come Cloud Service quando richiama il  Asset compute.

## Area di lavoro Sviluppo

Poiché il progetto è stato generato utilizzando `aio app init` utilizzando l&#39;area di lavoro `Development`, `AIO_runtime_namespace` viene automaticamente impostato su `81368-wkndaemassetcompute-development` con la corrispondenza `AIO_runtime_auth` nel file `.env` locale.  Se un file `.env` esiste nella directory utilizzata per emettere il comando di distribuzione, i relativi valori vengono utilizzati, a meno che non vengano sostituiti tramite un&#39;esportazione di variabile a livello di sistema operativo, ovvero come vengono indirizzati [stage e produzione](#stage-and-production) aree di lavoro.

![distribuzione delle app aio utilizzando le variabili .env](./assets/runtime/development__aio.png)

Per distribuire nell&#39;area di lavoro, definire nel file dei progetti `.env`:

1. Aprire la riga di comando nella directory principale del progetto di Asset compute 
1. Esegui il comando `aio app deploy`
1. Eseguite il comando `aio app get-url` per ottenere l&#39;URL del lavoratore da utilizzare nell&#39;AEM come profilo di elaborazione del Cloud Service per fare riferimento a questo Asset compute di lavoro  personalizzato. Se il progetto contiene più lavoratori, vengono elencati gli URL discreti per ciascun lavoratore.

Se lo sviluppo locale e le AEM come ambienti di sviluppo Cloud Service utilizzano distribuzioni di Asset compute  separate, le distribuzioni a AEM come sviluppatore Cloud Service possono essere gestite nello stesso modo delle [distribuzioni di fasi e produzione](#stage-and-production).

## Aree di lavoro Fase e Produzione{#stage-and-production}

L&#39;implementazione nelle aree di lavoro di Stage e Produzione viene in genere effettuata dal sistema CI/CD desiderato. Il progetto del Asset compute  deve essere distribuito in ogni area di lavoro (fase e produzione) in modo discreto.

L&#39;impostazione di variabili di ambiente vere sostituisce i valori per le variabili con lo stesso nome in `.env`.

![distribuzione delle app aio utilizzando le variabili di esportazione](./assets/runtime/stage__export-and-aio.png)

L&#39;approccio generale, generalmente automatizzato da un sistema CI/CD, per l&#39;implementazione in ambienti Stage e Production è:

1. Assicurarsi che il [ modulo npm di Adobe I/O CLI e  plug-in di Asset compute](../set-up/development-environment.md#aio) siano installati
1. Verificare il progetto di Asset compute  da distribuire da Git
1. Impostare le variabili di ambiente con i valori corrispondenti all&#39;area di lavoro di destinazione (Fase o Produzione)
   + Le due variabili richieste sono `AIO_runtime_namespace` e `AIO_runtime_auth` e sono ottenute per area di lavoro in  Adobe I/O Developer Console tramite la funzione __Scarica tutto__ di Workspace.

![ console per sviluppatori di Adobe - Spazio dei nomi runtime AIO e Auth](./assets/runtime/stage-auth-namespace.png)

I valori di queste chiavi possono essere impostati inviando i comandi di esportazione dalla riga di comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se i dipendenti del Asset compute  richiedono altre variabili, ad esempio l&#39;archiviazione cloud, queste devono essere esportate anche come variabili di ambiente.

1. Una volta impostate tutte le variabili di ambiente per l’area di lavoro di destinazione, eseguite il comando di implementazione:
   + `aio app deploy`
1. Gli URL del lavoratore a cui fa riferimento il AEM come profilo di elaborazione Cloud Service sono disponibili anche tramite:
   + `aio app get-url`.

Se la versione del progetto del Asset compute  modifica anche gli URL del lavoratore in modo da riflettere la nuova versione, e l&#39;URL dovrà essere aggiornato nei profili di elaborazione.

## Provisioning delle API Workspace{#workspace-api-provisioning}

Quando [impostare il progetto  Adobe Firefly in  Adobe I/O](../set-up/firefly.md) per supportare lo sviluppo locale, è stata creata una nuova area di lavoro Sviluppo e sono stati aggiunti __Asset compute, eventi I/O__ e __API di gestione eventi I/O__.

Le API di gestione eventi di I/O __Asset compute, eventi di I/O__ e __I/O__ sono aggiunte esplicitamente solo alle aree di lavoro utilizzate per lo sviluppo locale. Le aree di lavoro che si integrano (esclusivamente) con AEM come ambienti di Cloud Service non necessitano di queste API aggiunte esplicitamente in quanto le API sono rese naturalmente disponibili per AEM come Cloud Service.____
