---
title: Distribuzione di processi di lavoro di Asset compute in Adobe I/O Runtime da utilizzare con AEM come Cloud Service
description: I progetti di Asset compute e i relativi processi di lavoro devono essere implementati in Adobe I/O Runtime per essere utilizzati da AEM come Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---

# Distribuzione in Adobe I/O Runtime

I progetti di Asset compute e i relativi processi di lavoro devono essere implementati in Adobe I/O Runtime tramite l’interfaccia CLI di Adobe I/O, che deve essere utilizzata da AEM come Cloud Service.

Durante la distribuzione in Adobe I/O Runtime per l’utilizzo da AEM as a Cloud Service Author services sono richieste solo due variabili di ambiente:

+ `AIO_runtime_namespace` indica all’area di lavoro Firefly del progetto Adobe di distribuire
+ `AIO_runtime_auth` sono le credenziali di autenticazione dell’area di lavoro Adobe Project Firefly

Le altre variabili standard definite nel file `.env` vengono fornite in modo implicito da AEM come Cloud Service quando richiama il processo di lavoro Asset compute.

## Area di lavoro di sviluppo

Poiché questo progetto è stato generato utilizzando `aio app init` utilizzando l&#39;area di lavoro `Development`, `AIO_runtime_namespace` viene automaticamente impostato su `81368-wkndaemassetcompute-development` con la corrispondenza `AIO_runtime_auth` nel file locale `.env`.  Se un file `.env` esiste nella directory utilizzata per emettere il comando di distribuzione, i relativi valori vengono utilizzati, a meno che non vengano sostituiti tramite un&#39;esportazione di variabili a livello di sistema operativo, ovvero come vengono eseguite le operazioni di targeting per le aree di lavoro [stage e produzione](#stage-and-production).

![Distribuzione di app aio tramite variabili .env](./assets/runtime/development__aio.png)

Per distribuire nell’area di lavoro, definisci nel file dei progetti `.env` :

1. Apri la riga di comando nella directory principale del progetto di Asset compute
1. Esegui il comando `aio app deploy`
1. Esegui il comando `aio app get-url` per ottenere l&#39;URL del processo di lavoro da utilizzare nel AEM come profilo di elaborazione del Cloud Service per fare riferimento a questo processo di lavoro Asset compute personalizzato. Se il progetto contiene più processi di lavoro, vengono elencati gli URL discreti per ciascun processo di lavoro.

Se gli ambienti di sviluppo locale e di AEM come ambienti di sviluppo di Cloud Service utilizzano implementazioni di Asset compute separate, le implementazioni in AEM come sviluppo di Cloud Service possono essere gestite nello stesso modo delle [distribuzioni di stage e produzione](#stage-and-production).

## Aree di lavoro Stage e Produzione{#stage-and-production}

La distribuzione nelle aree di lavoro Stage e Production viene in genere eseguita dal sistema CI/CD desiderato. Il progetto di Asset compute deve essere distribuito in modo discreto in ogni area di lavoro (Stage e quindi Produzione).

L&#39;impostazione di variabili di ambiente vere esclude i valori per le variabili con lo stesso nome in `.env`.

![Distribuzione di app aio tramite variabili di esportazione](./assets/runtime/stage__export-and-aio.png)

L’approccio generale, generalmente automatizzato da un sistema CI/CD, per l’implementazione in ambienti Stage e Production è il seguente:

1. Assicurati che il modulo [Adobe I/O CLI npm e il plug-in Asset compute](../set-up/development-environment.md#aio) siano installati
1. Consulta il progetto di Asset compute da distribuire da Git
1. Imposta le variabili di ambiente con i valori corrispondenti all’area di lavoro di destinazione (Stage o Produzione)
   + Le due variabili richieste sono `AIO_runtime_namespace` e `AIO_runtime_auth` e vengono ottenute per area di lavoro in Adobe I/O Developer Console tramite la funzione __Scarica tutto__ di Workspace.

![Console per sviluppatori Adobe - Spazio dei nomi e autenticazione runtime AIO](./assets/runtime/stage-auth-namespace.png)

I valori di queste chiavi possono essere impostati emettendo comandi di esportazione dalla riga di comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se i processi di lavoro di Asset compute richiedono altre variabili, ad esempio l’archiviazione cloud, queste devono essere esportate anche come variabili di ambiente.

1. Una volta impostate tutte le variabili di ambiente per l’area di lavoro di destinazione a cui distribuire, esegui il comando di distribuzione:
   + `aio app deploy`
1. Gli URL del lavoratore a cui fa riferimento il AEM come profilo di elaborazione del Cloud Service sono disponibili anche tramite:
   + `aio app get-url`.

Se la versione del progetto di Asset compute cambia anche gli URL del processo di lavoro per riflettere la nuova versione, e l’URL dovrà essere aggiornato nei Profili di elaborazione.

## Provisioning API di Workspace{#workspace-api-provisioning}

Quando [si configura il progetto Adobe Project Firefly in Adobe I/O](../set-up/firefly.md) per supportare lo sviluppo locale, viene creata una nuova area di lavoro di sviluppo e sono state aggiunte le API di gestione eventi di __Asset compute, I/O__ e __I/O__.

Le API __Asset compute, I/O Events__ e __I/O Events Management API__ vengono aggiunte esplicitamente solo alle aree di lavoro utilizzate per lo sviluppo locale. Le aree di lavoro che si integrano (esclusivamente) con AEM come ambienti di Cloud Service hanno __non__ bisogno di queste API aggiunte esplicitamente in quanto le API sono rese naturalmente disponibili per AEM come Cloud Service.
