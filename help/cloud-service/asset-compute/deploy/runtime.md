---
title: Distribuzione di Asset Compute in Adobe I/O Runtime per l'utilizzo con AEM come Cloud Service
description: 'I progetti Asset Compute e i lavoratori in essi contenuti devono essere distribuiti su Adobe I/O Runtime per essere utilizzati da AEM come Cloud Service. '
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

I progetti Asset Compute e i relativi dipendenti devono essere distribuiti ad Adobe I/O Runtime tramite l&#39;interfaccia CLI I/O del Adobe , in modo da essere utilizzati da AEM come Cloud Service.

Quando si distribuisce in Adobe I/O Runtime per l&#39;uso da parte di AEM come servizi Cloud Service Author, sono necessarie solo due variabili di ambiente:

+ `AIO_runtime_namespace` indica all&#39;area di lavoro del progetto del Adobe  di distribuire
+ `AIO_runtime_auth` sono le credenziali di autenticazione dell&#39;area di lavoro Project Firefly del  Adobe

Le altre variabili standard definite nel `.env` file vengono implicitamente fornite da AEM come Cloud Service quando richiama il lavoratore Asset Compute.

## Area di lavoro Sviluppo

Poiché il progetto è stato generato utilizzando `aio app init` l’ `Development` area di lavoro, `AIO_runtime_namespace` viene impostato automaticamente `81368-wkndaemassetcompute-development` con la corrispondenza `AIO_runtime_auth` nel `.env` file locale.  Se un `.env` file esiste nella directory utilizzata per emettere il comando di distribuzione, i relativi valori vengono utilizzati, a meno che non vengano sostituiti tramite un&#39;esportazione di variabile a livello di sistema operativo, ovvero come vengono indirizzati gli spazi di lavoro [di fase e produzione](#stage-and-production) .

![distribuzione delle app aio utilizzando le variabili .env](./assets/runtime/development__aio.png)

Per distribuire nell&#39;area di lavoro, definire nel `.env` file dei progetti:

1. Aprite la riga di comando nella directory principale del progetto Asset Compute
1. Esegui il comando `aio app deploy`
1. Eseguite il comando `aio app get-url` per ottenere l&#39;URL del lavoratore da utilizzare nell&#39;AEM come profilo di elaborazione Cloud Service per fare riferimento a questo lavoratore personalizzato di elaborazione risorse. Se il progetto contiene più lavoratori, vengono elencati gli URL discreti per ciascun lavoratore.

Se lo sviluppo locale e AEM come ambienti di sviluppo Cloud Service utilizzano distribuzioni separate per il calcolo delle risorse, le distribuzioni a AEM come sviluppatore Cloud Service possono essere gestite nello stesso modo delle distribuzioni [](#stage-and-production)Stage e Produzione.

## Aree di lavoro Fase e Produzione{#stage-and-production}

L&#39;implementazione nelle aree di lavoro di Stage e Produzione viene in genere effettuata dal sistema CI/CD desiderato. Il progetto Asset Compute deve essere distribuito in ogni area di lavoro (fase e produzione) in modo discreto.

L&#39;impostazione di variabili di ambiente vere sostituisce i valori per le variabili con lo stesso nome in `.env`.

![distribuzione delle app aio utilizzando le variabili di esportazione](./assets/runtime/stage__export-and-aio.png)

L&#39;approccio generale, generalmente automatizzato da un sistema CI/CD, per l&#39;implementazione in ambienti Stage e Production è:

1. Verificare che il modulo npm [I/O CLI di Adobe e il plug-in](../set-up/development-environment.md#aio) Asset Compute siano installati
1. Estrarre il progetto Asset Compute da Git
1. Impostare le variabili di ambiente con i valori corrispondenti all&#39;area di lavoro di destinazione (Fase o Produzione)
   + Le due variabili richieste sono `AIO_runtime_namespace` e `AIO_runtime_auth` vengono ottenute per area di lavoro in  Adobe I/O Developer Console tramite la funzione __Scarica tutto__ di Workspace.

![console per sviluppatori di Adobe - Spazio dei nomi runtime AIO e Auth](./assets/runtime/stage-auth-namespace.png)

I valori di queste chiavi possono essere impostati inviando i comandi di esportazione dalla riga di comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se i lavoratori di elaborazione risorse richiedono altre variabili, ad esempio l’archiviazione cloud, queste devono essere esportate anche come variabili di ambiente.

1. Una volta impostate tutte le variabili di ambiente per l’area di lavoro di destinazione, eseguite il comando di implementazione:
   + `aio app deploy`
1. Gli URL del lavoratore a cui fa riferimento il AEM come profilo di elaborazione Cloud Service sono disponibili anche tramite:
   + `aio app get-url`.

Se la versione del progetto Asset Compute modifica anche gli URL del lavoratore in modo da riflettere la nuova versione, e l&#39;URL dovrà essere aggiornato nei profili di elaborazione.

## Provisioning delle API Workspace{#workspace-api-provisioning}

Quando si [configura il progetto Project Firefly del Adobe  in  I/O](../set-up/firefly.md) del Adobe per supportare lo sviluppo locale, è stata creata una nuova area di lavoro Sviluppo e sono state aggiunte le API __di gestione eventi__ Asset Compute, Eventi __I/O e Gestione eventi__ I/O.

Le API __Asset Compute, I/O Events__ e __I/O Events Management API__ vengono aggiunte solo esplicitamente alle aree di lavoro utilizzate per lo sviluppo locale. Le aree di lavoro che si integrano (esclusivamente) con AEM come ambienti di Cloud Service __non__ necessitano di queste API esplicitamente aggiunte, in quanto le API sono rese naturalmente disponibili per AEM come Cloud Service.
