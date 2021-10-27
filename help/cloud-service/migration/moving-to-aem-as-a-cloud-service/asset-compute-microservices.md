---
title: AEM Assets Microservices e passaggio a AEM as a Cloud Service
description: Scopri in che modo i microservizi di asset compute di AEM Assets as a Cloud Service consentono di generare automaticamente ed efficacemente qualsiasi rendering per le risorse, sostituendo questo ruolo del flusso di lavoro AEM tradizionale.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# AEM Assets Microservices - Passaggio a AEM as a Cloud Service

Scopri in che modo i microservizi di asset compute di AEM Assets as a Cloud Service consentono di generare automaticamente ed efficacemente qualsiasi rendering per le risorse, sostituendo questo ruolo del flusso di lavoro AEM tradizionale.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Strumento di migrazione del flusso di lavoro

![Strumento Asset Workflow Migration (Migrazione flussi di lavoro per risorse) ](./assets/asset-workflow-migration.png)

Come parte del refactoring della base di codice, utilizza il [Strumento Asset Workflow Migration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) per migrare i flussi di lavoro esistenti e utilizzare i microservizi Asset compute in AEM as a Cloud Service.

### Attività chiave

* Utilizza la [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) strumento per migrare i flussi di lavoro per l’elaborazione delle risorse e utilizzare i microservizi Asset compute.
* Imposta un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e distribuire i flussi di lavoro aggiornati. Potrebbe essere necessaria una regolazione manuale per flussi di lavoro complessi.
* Continua a eseguire iterazioni in un ambiente di sviluppo locale utilizzando l’SDK AEM fino a quando il flusso di lavoro aggiornato non corrisponde alla parità delle funzioni.
* Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a eseguire la convalida.

