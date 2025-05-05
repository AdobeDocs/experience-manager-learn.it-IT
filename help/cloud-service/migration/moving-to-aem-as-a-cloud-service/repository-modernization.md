---
title: Modernizzazione dell’archivio
description: Scopri la modernizzazione dell’archivio, i contenuti mutabili e immutabili, la struttura del pacchetto e lo strumento CLI del modernizzatore dell’archivio.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
duration: 1305
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Modernizzazione dell’archivio

Scopri la modernizzazione dell’archivio, i contenuti mutabili e immutabili, la struttura del pacchetto e lo strumento CLI del modernizzatore dell’archivio.

>[!VIDEO](https://video.tv.adobe.com/v/336958?quality=12&learn=on)

## Strumento Repository Modernizer

![Modernizzatore archivio](./assets/repository-modernizer.png)

Come parte del refactoring della base di codice, utilizza lo strumento [Repository Modernizer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html?lang=it) per ristrutturare una base di codice 6.x in una struttura più moderna.

## Attività chiave

* Utilizza lo strumento [Adobe I/O Repository Modernizer](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) per ristrutturare un progetto in modo che corrisponda alla struttura prevista di un progetto AEM as a Cloud Service.
* Regola e correggi manualmente eventuali errori di generazione nella base di codice aggiornata.
* Configura un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it) e distribuisci la base di codice aggiornata. Eseguire un&#39;iterazione finché il progetto non si trova in uno stato stabile.
* Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a convalidarla.
