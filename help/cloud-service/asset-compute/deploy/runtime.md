---
title: Distribuire i processi di lavoro di Asset compute in Adobe I/O Runtime per l’utilizzo con AEM as a Cloud Service
description: I progetti di Asset compute e i relativi processi di lavoro devono essere implementati in Adobe I/O Runtime per essere utilizzati da AEM as a Cloud Service.
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
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Distribuzione in Adobe I/O Runtime

I progetti di Asset compute e i relativi processi di lavoro devono essere implementati in Adobe I/O Runtime tramite l’interfaccia CLI di Adobe I/O, in modo che AEM as a Cloud Service possa essere utilizzato.

Quando si distribuisce in Adobe I/O Runtime per l’utilizzo AEM servizi Author as a Cloud Service, sono necessarie solo due variabili di ambiente:

+ `AIO_runtime_namespace` indirizza l’area di lavoro di App Builder a
+ `AIO_runtime_auth` sono le credenziali di autenticazione dell’area di lavoro di App Builder

Le altre variabili standard definite nel `.env` Il file viene fornito in modo implicito da AEM as a Cloud Service quando richiama il processo di lavoro di Asset compute.

## Area di lavoro di sviluppo

Poiché questo progetto è stato generato con `aio app init` utilizzando `Development` area di lavoro, `AIO_runtime_namespace` è automaticamente impostato su `81368-wkndaemassetcompute-development` con la corrispondenza `AIO_runtime_auth` nel nostro locale `.env` file.  Se `.env` il file esiste nella directory utilizzata per emettere il comando di distribuzione, i relativi valori vengono utilizzati, a meno che non vengano sostituiti tramite un&#39;esportazione di variabili a livello di sistema operativo, che è il modo in cui [stadio e produzione](#stage-and-production) le aree di lavoro sono oggetto di targeting.

![Distribuzione di app aio tramite variabili .env](./assets/runtime/development__aio.png)

Per distribuire nell’area di lavoro definita nei progetti `.env` file:

1. Apri la riga di comando nella directory principale del progetto di Asset compute
1. Esegui il comando `aio app deploy`
1. Esegui il comando `aio app get-url` per ottenere l’URL del lavoratore da utilizzare nel AEM profilo di elaborazione as a Cloud Service per fare riferimento a questo processo di lavoro Asset compute personalizzato. Se il progetto contiene più processi di lavoro, vengono elencati gli URL discreti per ciascun processo di lavoro.

Se gli ambienti di sviluppo locali e AEM sviluppo as a Cloud Service utilizzano implementazioni di Asset compute separate, le distribuzioni a sviluppo as a Cloud Service possono essere gestite nello stesso modo di [Implementazioni di stage e produzione](#stage-and-production).

## Aree di lavoro Stage e Produzione{#stage-and-production}

La distribuzione nelle aree di lavoro Stage e Production viene in genere eseguita dal sistema CI/CD desiderato. Il progetto di Asset compute deve essere distribuito in modo discreto in ogni area di lavoro (Stage e quindi Produzione).

L&#39;impostazione di variabili di ambiente reali sostituisce i valori per le variabili con lo stesso nome in `.env`.

![Distribuzione di app aio tramite variabili di esportazione](./assets/runtime/stage__export-and-aio.png)

L’approccio generale, generalmente automatizzato da un sistema CI/CD, per l’implementazione in ambienti Stage e Production è il seguente:

1. Assicurati che [Modulo npm Adobe I/O CLI e plug-in Asset compute](../set-up/development-environment.md#aio) sono installati
1. Consulta il progetto di Asset compute da distribuire da Git
1. Imposta le variabili di ambiente con i valori corrispondenti all’area di lavoro di destinazione (Stage o Produzione)
   + Le due variabili richieste sono `AIO_runtime_namespace` e `AIO_runtime_auth` e sono ottenuti per area di lavoro in Adobe I/O Developer Console tramite l’ __Scarica tutto__ funzionalità.

![Console per sviluppatori Adobe - Spazio dei nomi e autenticazione runtime AIO](./assets/runtime/stage-auth-namespace.png)

I valori di queste chiavi possono essere impostati emettendo comandi di esportazione dalla riga di comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se i processi di lavoro di Asset compute richiedono altre variabili, ad esempio l’archiviazione cloud, queste devono essere esportate anche come variabili di ambiente.

1. Una volta impostate tutte le variabili di ambiente per l’area di lavoro di destinazione a cui distribuire, esegui il comando di distribuzione:
   + `aio app deploy`
1. Gli URL di lavoro a cui fa riferimento il profilo di elaborazione as a Cloud Service AEM sono disponibili anche tramite:
   + `aio app get-url`.

Se la versione del progetto di Asset compute cambia anche gli URL del processo di lavoro per riflettere la nuova versione, e l’URL dovrà essere aggiornato nei Profili di elaborazione.

## Provisioning API di Workspace{#workspace-api-provisioning}

Quando [configurazione del progetto App Builder in Adobe I/O](../set-up/app-builder.md) per supportare lo sviluppo locale, è stata creata una nuova area di lavoro Sviluppo e __asset compute, eventi I/O__ e __API di gestione eventi I/O__ sono stati aggiunti a esso.

La __asset compute, eventi I/O__ e __API di gestione eventi I/O__ Le API vengono aggiunte solo esplicitamente alle aree di lavoro utilizzate per lo sviluppo locale. Le aree di lavoro che si integrano (esclusivamente) con AEM ambienti as a Cloud Service __not__ è necessario che queste API siano aggiunte esplicitamente in quanto le API sono rese naturalmente disponibili per AEM as a Cloud Service.
