---
title: Debug di AEM con il Browser dell’archivio
description: Il Browser dell’archivio è un potente strumento che fornisce visibilità all’archivio dati sottostante dell’AEM, consentendo un facile debug dell’ambiente as a Cloud Service dell’AEM.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 310
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Debug di AEM as a Cloud Service con il Browser dell’archivio

Il Browser dell’archivio è un potente strumento che fornisce visibilità all’archivio dati sottostante dell’AEM, consentendo un facile debug dell’ambiente as a Cloud Service dell’AEM. Il Browser dell’archivio supporta una visualizzazione in sola lettura delle risorse e delle proprietà di AEM in Produzione, Stage e Sviluppo, nonché nei servizi Author, Publish e Preview.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

Il browser dell’archivio è __SOLO__ disponibile in ambienti AEM as a Cloud Service (usare [CRXDE Liti](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) per eseguire il debug dell’SDK AEM locale).

## Accesso al browser dell’archivio

Per accedere al Browser dell’archivio in AEM as a Cloud Service:

1. Assicurati che l’utente abbia [l&#39;accesso richiesto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Accedi a [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Seleziona il programma contenente l’ambiente AEM as a Cloud Service di cui eseguire il debug
1. Apri [Console per sviluppatori](./developer-console.md) corrispondente all&#39;ambiente AEM as a Cloud Service per il debug
1. Seleziona la __Browser dell’archivio__ scheda
1. Seleziona il livello di servizio AEM da sfogliare
   + Tutti gli autori
   + Tutti gli editori
   + Tutte le anteprime
1. Seleziona __Apri Browser Archivio__

Il Browser dell’archivio si apre per il livello di servizio selezionato (Autore, Pubblica o Anteprima) in modalità di sola lettura, visualizzando le risorse e le proprietà a cui l’utente ha accesso.

## Accesso a pubblicazione e anteprima

Per impostazione predefinita, l’accesso a Pubblica o Anteprima è limitato, riducendo le risorse disponibili nel Browser dell’archivio. [Per visualizzare tutte le risorse in Pubblica (o Anteprima), aggiungi gli utenti al ruolo Amministratori Pubblica (o Anteprima).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
