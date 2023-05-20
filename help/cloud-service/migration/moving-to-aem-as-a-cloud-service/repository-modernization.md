---
title: Modernizzazione dell’archivio
description: Scopri la modernizzazione dell’archivio, i contenuti mutabili e immutabili, la struttura del pacchetto e lo strumento CLI del modernizzatore dell’archivio.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 5%

---

# Modernizzazione dell’archivio

Scopri la modernizzazione dell’archivio, i contenuti mutabili e immutabili, la struttura del pacchetto e lo strumento CLI del modernizzatore dell’archivio.

>[!VIDEO](https://video.tv.adobe.com/v/336958?quality=12&learn=on)

## Strumento Repository Modernizer

![Modernizzatore dell&#39;archivio](./assets/repository-modernizer.png)

Come parte del refactoring della base di codice, utilizza [Strumento Repository Modernizer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html) per ristrutturare una base di codice 6.x in una struttura più moderna.

## Attività chiave

* Utilizza il [Adobe I/O Repository Modernizer](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) strumento per ristrutturare un progetto in modo che corrisponda alla struttura prevista di un progetto as a Cloud Service dell’AEM.
* Regola e correggi manualmente eventuali errori di generazione nella base di codice aggiornata.
* Configurare un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it) e implementa la base di codice aggiornata. Eseguire un&#39;iterazione finché il progetto non si trova in uno stato stabile.
* Distribuisci la base di codice aggiornata in un ambiente di sviluppo as a Cloud Service per AEM e continua a convalidarla.
