---
title: Ricerca e indicizzazione in AEM as a Cloud Service
description: Scopri AEM indici di ricerca di as a Cloud Service, come convertire le definizioni AEM 6 e come distribuire gli indici.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Ricerca e indicizzazione

Scopri AEM indici di ricerca di as a Cloud Service, come convertire AEM 6 definizioni di indice in modo AEM compatibile as a Cloud Service e come distribuire gli indici in AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Strumento Convertitore indice

![Strumento Convertitore indice](./assets/index-converter.png)

Come parte del refactoring della base di codice, utilizza il [Strumento di conversione indice](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) per convertire le definizioni personalizzate dell&#39;indice Oak in definizioni di indici compatibili as a Cloud Service.

### Attività chiave

* Utilizza la [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) strumento per migrare i flussi di lavoro per l’elaborazione delle risorse e utilizzare i microservizi Asset compute.
* Imposta un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e distribuire gli indici personalizzati. Assicurati che gli indici aggiornati siano aggiornati.
* Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a eseguire la convalida.
* Se modifichi un indice preconfigurato **SEMPRE** copia la definizione di indice più recente da un ambiente as a Cloud Service AEM in esecuzione nella versione più recente. Modifica la definizione dell&#39;indice copiata in base alle tue esigenze.