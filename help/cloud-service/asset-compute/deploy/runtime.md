---
title: Distribuire Asset Compute worker in Adobe I/O Runtime per l'utilizzo con AEM as a Cloud Service
description: I progetti Asset Compute e i relativi processi di lavoro devono essere distribuiti in Adobe I/O Runtime per poter essere utilizzati da AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Implementare in Adobe I/O Runtime

I progetti Asset Compute e i relativi processi di lavoro devono essere distribuiti in Adobe I/O Runtime tramite Adobe I/O CLI, affinché possa essere utilizzato da AEM as a Cloud Service.

Durante la distribuzione in Adobe I/O Runtime per l’utilizzo da parte dei servizi AEM as a Cloud Service Author sono necessarie solo due variabili di ambiente:

+ `AIO_runtime_namespace` punti a cui distribuire App Builder Workspace
+ `AIO_runtime_auth` sono le credenziali di autenticazione dell&#39;area di lavoro App Builder

Le altre variabili standard definite nel file `.env` vengono fornite in modo implicito da AEM as a Cloud Service quando richiama il processo di lavoro Asset Compute.

## Area di lavoro sviluppo

Poiché il progetto è stato generato utilizzando `aio app init` utilizzando l&#39;area di lavoro `Development`, `AIO_runtime_namespace` viene impostato automaticamente su `81368-wkndaemassetcompute-development` con `AIO_runtime_auth` corrispondente nel file `.env` locale.  Se nella directory utilizzata per il comando di distribuzione è presente un file `.env`, vengono utilizzati i relativi valori, a meno che non vengano sostituiti tramite un&#39;esportazione della variabile a livello di sistema operativo, ovvero nel modo in cui vengono indirizzate le aree di lavoro [stage e produzione](#stage-and-production).

![distribuzione app aio tramite variabili .env](./assets/runtime/development__aio.png)

Per distribuire nell&#39;area di lavoro, definire nel file `.env` dei progetti:

1. Apri la riga di comando nella directory principale del progetto di Asset compute
1. Esegui il comando `aio app deploy`
1. Eseguire il comando `aio app get-url` per ottenere l&#39;URL del processo di lavoro da utilizzare nel profilo di elaborazione di AEM as a Cloud Service per fare riferimento a questo processo di lavoro Asset compute personalizzato. Se il progetto contiene più lavoratori, vengono elencati gli URL discreti per ciascun lavoratore.

Se gli ambienti di sviluppo locali e gli ambienti di sviluppo AEM as a Cloud Service utilizzano distribuzioni Asset Compute separate, le distribuzioni in AEM as a Cloud Service Dev possono essere gestite nello stesso modo delle [distribuzioni di staging e produzione](#stage-and-production).

## Aree di lavoro di staging e produzione{#stage-and-production}

La distribuzione nelle aree di lavoro di staging e produzione viene in genere eseguita dal sistema CI/CD scelto. Il progetto di Asset compute deve essere distribuito in modo discreto in ogni Workspace (Stage e poi Produzione).

L&#39;impostazione delle variabili di ambiente vere sostituisce i valori per le variabili con lo stesso nome in `.env`.

![distribuzione app aio tramite variabili di esportazione](./assets/runtime/stage__export-and-aio.png)

L’approccio generale, in genere automatizzato da un sistema CI/CD, per la distribuzione negli ambienti di staging e produzione è il seguente:

1. Verificare che il modulo npm CLI Adobe I/O [e il plug-in Asset Compute](../set-up/development-environment.md#aio) siano installati
1. Consulta il progetto di Asset compute da distribuire da Git
1. Imposta le variabili di ambiente con i valori corrispondenti all’area di lavoro di destinazione (Stage o Produzione)
   + Le due variabili richieste sono `AIO_runtime_namespace` e `AIO_runtime_auth` e vengono ottenute per area di lavoro in Adobe I/O Developer Console tramite la funzione __Scarica tutto__ di Workspace.

![Adobe Developer Console - Spazio dei nomi e autenticazione runtime AIO](./assets/runtime/stage-auth-namespace.png)

I valori di queste chiavi possono essere impostati eseguendo i comandi di esportazione dalla riga di comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se i processi di lavoro Asset compute richiedono altre variabili, ad esempio l’archiviazione cloud, queste devono essere esportate anche come variabili di ambiente.

1. Una volta impostate tutte le variabili di ambiente per la distribuzione nell’area di lavoro di destinazione, esegui il comando deploy:
   + `aio app deploy`
1. Gli URL del lavoratore a cui fa riferimento il profilo di elaborazione di AEM as a Cloud Service sono disponibili anche tramite:
   + `aio app get-url`.

Se la versione del progetto di Asset compute cambia, anche gli URL del lavoratore cambiano in modo da riflettere la nuova versione e l’URL dovrà essere aggiornato nei Profili di elaborazione.

## Provisioning API di Workspace{#workspace-api-provisioning}

Quando [il progetto App Builder nell&#39;Adobe I/O](../set-up/app-builder.md) è stato configurato per supportare lo sviluppo locale, è stata creata una nuova area di lavoro Sviluppo e sono state aggiunte __API Asset compute, Eventi di I/O__ e __API Gestione eventi I/O__.

Le API __Asset Compute, I/O Events__ e __I/O Events Management__ vengono aggiunte in modo esplicito solo alle aree di lavoro utilizzate per lo sviluppo locale. Le aree di lavoro che si integrano (esclusivamente) con gli ambienti AEM as a Cloud Service __non__ richiedono l&#39;aggiunta esplicita di queste API, in quanto sono rese naturalmente disponibili per AEM as a Cloud Service.
