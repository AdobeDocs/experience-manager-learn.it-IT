---
title: Debug di AEM con il Browser dell’archivio
description: Il Browser dell’archivio è un potente strumento che fornisce visibilità all’archivio dati sottostante dell’AEM, consentendo un facile debug dell’ambiente AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Debug di AEM as a Cloud Service con il Browser dell’archivio

Il Browser dell’archivio è un potente strumento che fornisce visibilità all’archivio dati sottostante dell’AEM, consentendo un facile debug dell’ambiente AEM as a Cloud Service. Il Browser dell’archivio supporta una visualizzazione in sola lettura delle risorse e delle proprietà di AEM in Produzione, Stage e Sviluppo, nonché nei servizi Author, Publish e Preview.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

Il Browser dell&#39;archivio è __ONLY__ disponibile negli ambienti AEM as a Cloud Service (utilizza [CRXDE Liti](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) per eseguire il debug dell&#39;SDK AEM locale).

## Accesso al browser dell’archivio

Per accedere al Browser dell’archivio su AEM as a Cloud Service:

1. Assicurati che il tuo utente disponga di [l&#39;accesso richiesto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Accedi a [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Seleziona il programma contenente l’ambiente AEM as a Cloud Service di cui eseguire il debug
1. Apri [Developer Console](./developer-console.md) corrispondente all&#39;ambiente AEM as a Cloud Service di cui eseguire il debug
1. Selezionare la scheda __Browser repository__
1. Seleziona il livello di servizio AEM da sfogliare
   + Tutti gli autori
   + Tutti gli editori
   + Tutte le anteprime
1. Seleziona __Apri Browser Archivio__

Il Browser dell’archivio si apre per il livello di servizio selezionato (Autore, Publish o Anteprima) in modalità di sola lettura, visualizzando le risorse e le proprietà a cui l’utente ha accesso.

## Accesso a Publish e Anteprima

Per impostazione predefinita, l’accesso a Publish o Anteprima è limitato, riducendo le risorse disponibili nel Browser dell’archivio. [Per visualizzare tutte le risorse su Publish (o Anteprima), aggiungere utenti a un ruolo Amministratori Publish (o Anteprima).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
