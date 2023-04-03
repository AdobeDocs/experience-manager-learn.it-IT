---
title: Debug AEM con il browser del repository
description: Repository Browser è un potente strumento che fornisce visibilità AEM'archivio dati sottostante, consentendo un facile debug dell'ambiente as a Cloud Service AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Debug AEM as a Cloud Service con il browser del repository

Repository Browser è un potente strumento che fornisce visibilità AEM&#39;archivio dati sottostante, consentendo un facile debug dell&#39;ambiente as a Cloud Service AEM. Il browser Repository supporta una visualizzazione in sola lettura delle risorse e delle proprietà di AEM in Produzione, Stage e Sviluppo, nonché dei servizi Author, Publish e Preview.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

Browser del repository __SOLO__ disponibile in ambienti AEM as a Cloud Service (utilizza [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) per eseguire il debug dell&#39;SDK AEM locale).

## Accesso al browser del repository

Per accedere al browser del repository su AEM as a Cloud Service:

1. Assicurati che l&#39;utente abbia [l&#39;accesso richiesto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Accedi a [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Selezionare il programma contenente l&#39;ambiente as a Cloud Service AEM cui eseguire il debug
1. Apri [Console per sviluppatori](./developer-console.md) corrispondente all&#39;ambiente as a Cloud Service AEM di cui eseguire il debug
1. Seleziona la __Browser del repository__ scheda
1. Selezionare il livello di servizio AEM per sfogliare
   + Tutti gli autori
   + Tutti gli editori
   + Tutte le anteprime
1. Seleziona __Apri browser Repository__

Il browser Repository si apre per il livello di servizio selezionato (Autore, Pubblica o Anteprima) in modalità di sola lettura, visualizzando le risorse e le proprietà a cui l’utente ha accesso.

## Accesso a Publish e Preview

Per impostazione predefinita, l’accesso a Pubblica o Anteprima è limitato, riducendo le risorse disponibili nel browser dell’archivio. [Per visualizzare tutte le risorse in Pubblica (o Anteprima), aggiungi gli utenti a un ruolo Pubblica (o Anteprima) Amministratori .](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
