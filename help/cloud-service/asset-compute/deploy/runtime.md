---
title: Distribuire Asset compute worker in Adobe I/O Runtime per l'utilizzo con AEM as a Cloud Service
description: I progetti Asset compute e i lavoratori in essi contenuti devono essere distribuiti in Adobe I/O Runtime per poter essere utilizzati dagli as a Cloud Service AEM.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Implementare in Adobe I/O Runtime

I progetti Asset compute, e i relativi processi di lavoro, devono essere distribuiti in Adobe I/O Runtime tramite l&#39;interfaccia CLI Adobe I/O per poter essere utilizzati da AEM as a Cloud Service.

Durante la distribuzione in Adobe I/O Runtime per l’utilizzo da parte dei servizi di authoring as a Cloud Service dell’AEM sono necessarie solo due variabili di ambiente:

+ `AIO_runtime_namespace` punta all’area di lavoro di App Builder per la distribuzione a
+ `AIO_runtime_auth` sono le credenziali di autenticazione dell’area di lavoro di App Builder

Le altre variabili standard definite nel `.env` sono forniti implicitamente da AEM as a Cloud Service quando richiama il lavoratore Asset compute.

## Area di lavoro sviluppo

Poiché questo progetto è stato generato con `aio app init` utilizzando `Development` workspace, `AIO_runtime_namespace` è impostato automaticamente su `81368-wkndaemassetcompute-development` con la corrispondenza `AIO_runtime_auth` nel nostro locale `.env` file.  Se un `.env` esiste nella directory utilizzata per emettere il comando deploy, vengono utilizzati i relativi valori, a meno che non vengano sostituiti tramite un&#39;esportazione di variabile a livello di sistema operativo, nel qual modo [fase e produzione](#stage-and-production) le aree di lavoro sono impostate come destinazione.

![distribuzione di app aio tramite variabili .env](./assets/runtime/development__aio.png)

Per distribuire nell’area di lavoro, definisci nei progetti `.env` file:

1. Apri la riga di comando nella directory principale del progetto di Asset compute
1. Esegui il comando `aio app deploy`
1. Esegui il comando `aio app get-url` per ottenere l’URL del lavoratore da utilizzare nel profilo di elaborazione as a Cloud Service dell’AEM per fare riferimento a questo lavoratore Asset compute personalizzato. Se il progetto contiene più lavoratori, vengono elencati gli URL discreti per ciascun lavoratore.

Se gli ambienti di sviluppo locale e gli ambienti di sviluppo as a Cloud Service AEM utilizzano distribuzioni Asset compute separate, le distribuzioni nello sviluppo as a Cloud Service AEM possono essere gestite nello stesso modo in cui [Distribuzioni negli ambienti di staging e produzione](#stage-and-production).

## Aree di lavoro di staging e produzione{#stage-and-production}

La distribuzione nelle aree di lavoro di staging e produzione viene in genere eseguita dal sistema CI/CD scelto. Il progetto di Asset compute deve essere distribuito in ogni area di lavoro (Stage e quindi Produzione) in modo discreto.

L’impostazione di variabili di ambiente vere sostituisce i valori per le variabili con lo stesso nome in `.env`.

![distribuzione di app aio tramite variabili di esportazione](./assets/runtime/stage__export-and-aio.png)

L’approccio generale, in genere automatizzato da un sistema CI/CD, per la distribuzione negli ambienti di staging e produzione è il seguente:

1. Assicurati che [Modulo npm CLI Adobe I/O e plug-in Asset compute](../set-up/development-environment.md#aio) sono installati
1. Consulta il progetto di Asset compute da distribuire da Git
1. Imposta le variabili di ambiente con i valori corrispondenti all’area di lavoro di destinazione (Stage o Produzione)
   + Le due variabili richieste sono `AIO_runtime_namespace` e `AIO_runtime_auth` e sono ottenuti per area di lavoro in Adobe I/O Developer Console tramite il __Scarica tutto__ funzionalità.

![Console Adobe Developer: spazio dei nomi e autenticazione di runtime AIO](./assets/runtime/stage-auth-namespace.png)

I valori di queste chiavi possono essere impostati eseguendo i comandi di esportazione dalla riga di comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se i processi di lavoro Asset compute richiedono altre variabili, ad esempio l’archiviazione cloud, queste devono essere esportate anche come variabili di ambiente.

1. Una volta impostate tutte le variabili di ambiente per la distribuzione nell’area di lavoro di destinazione, esegui il comando deploy:
   + `aio app deploy`
1. Gli URL del lavoratore a cui fa riferimento il profilo di elaborazione as a Cloud Service dell’AEM sono disponibili anche tramite:
   + `aio app get-url`.

Se la versione del progetto di Asset compute cambia, anche gli URL del lavoratore cambiano in modo da riflettere la nuova versione e l’URL dovrà essere aggiornato nei Profili di elaborazione.

## Provisioning API di Workspace{#workspace-api-provisioning}

Quando [configurazione del progetto App Builder in Adobe I/O](../set-up/app-builder.md) per supportare lo sviluppo locale, è stata creata una nuova area di lavoro Sviluppo e __Asset compute, Eventi di I/O__ e __API per la gestione degli eventi di I/O__ sono stati aggiunti.

Il __Asset compute, Eventi di I/O__ e __API per la gestione degli eventi di I/O__ Le API vengono aggiunte in modo esplicito solo alle aree di lavoro utilizzate per lo sviluppo locale. Le aree di lavoro che si integrano (esclusivamente) con gli ambienti as a Cloud Service dell’AEM __non__ devono essere aggiunte esplicitamente in quanto le API sono rese disponibili naturalmente per gli as a Cloud Service AEM.
