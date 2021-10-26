---
title: Configurazione di Dispatcher quando si passa a AEM as a Cloud Service
description: Scopri le modifiche di rilievo apportate AEM Dispatcher per AEM as a Cloud Service, lo strumento di conversione del Dispatcher e come utilizzare l’SDK per strumenti di Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

Scopri AEM Dispatcher per AEM as a Cloud Service, concentrandoti sulle modifiche di rilievo apportate da Dispatcher per AEM 6, sullo strumento di conversione di Dispatcher e su come utilizzare l’SDK degli strumenti di Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Come parte del refactoring della base di codice, utilizza il [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) per reimpostare le configurazioni esistenti del Dispatcher on-premise o Adobe Managed Services su AEM configurazione del Dispatcher compatibile as a Cloud Service.

### Attività chiave

* Utilizza la [Strumento Adobe I/O Dispatcher Converter](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) per migrare una configurazione Dispatcher esistente.
* Fai riferimento al modulo Dispatcher da [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) come best practice.
* [Impostare strumenti Dispatcher locali](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) per convalidare il dispatcher, prima di eseguire il test in un ambiente di Cloud Service.


