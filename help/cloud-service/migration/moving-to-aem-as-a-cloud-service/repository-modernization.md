---
title: Modernizzazione archivio
description: Scopri la modernizzazione dell’archivio, il contenuto mutabile e immutabile, la struttura del pacchetto e lo strumento CLI del modernizzatore dell’archivio.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# Modernizzazione archivio

Scopri la modernizzazione dell’archivio, il contenuto mutabile e immutabile, la struttura del pacchetto e lo strumento CLI del modernizzatore dell’archivio.

>[!VIDEO](https://video.tv.adobe.com/v/336958/?quality=12&learn=on)

## Strumento Repository Modernizer

![Modernizzatore dell&#39;archivio](./assets/repository-modernizer.png)

Come parte del refactoring della base di codice, utilizza il [Strumento Repository Modernizer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html) ristrutturare una base di codice 6.x in una struttura più moderna.

### Attività chiave

* Utilizza la [Adobe I/O Repository Modernizer](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) strumento per ristrutturare un progetto in modo che corrisponda alla struttura prevista di un progetto as a Cloud Service AEM.
* Regola e correggi manualmente eventuali errori di compilazione nella base di codice aggiornata.
* Imposta un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e distribuisci la base di codice aggiornata. Iterate fino a quando il progetto non si trova in uno stato stabile.
* Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a eseguire la convalida.
