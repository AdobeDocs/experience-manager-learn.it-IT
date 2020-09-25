---
title: Debug AEM come Cloud Service
description: sull'infrastruttura cloud self-service, scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari facet di AEM come un Cloud Service, dalla creazione e distribuzione fino a ottenere i dettagli delle applicazioni AEM in esecuzione.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
translation-type: tm+mt
source-git-commit: debb65a218ae0b9e5105c3f63018358902330b34
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---


# Debug AEM come Cloud Service

AEM come Cloud Service è il modo nativo di sfruttare le applicazioni AEM. AEM come Cloud Service si basa su un&#39;infrastruttura cloud self-service, scalabile e scalabile, che richiede AEM sviluppatori di comprendere come comprendere e correggere vari aspetti di AEM come Cloud Service, dalla creazione e implementazione fino a ottenere i dettagli di applicazioni AEM in esecuzione.

## Registri

I registri forniscono dettagli sul funzionamento dell’applicazione in AEM come Cloud Service, nonché informazioni sui problemi relativi alle distribuzioni.

[Debug di AEM come Cloud Service utilizzando i registri](./logs.md)

## Creazione e implementazione

 pipeline di Adobe Cloud Manager distribuisce AEM applicazione attraverso una serie di passaggi per determinare la qualità del codice e la vitalità del codice quando viene distribuito in AEM come Cloud Service. Ciascuno dei passaggi potrebbe causare un errore, rendendo importante comprendere come eseguire il debug delle build per determinare la causa principale di e come risolvere eventuali errori.

[Debug AEM come build e implementazione di Cloud Service](./build-and-deployment.md)

## Console per sviluppatori

La console Sviluppatore offre una serie di informazioni e introspezione in AEM come ambienti di Cloud Service utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona all’interno AEM come Cloud Service.

[Debug di AEM come Cloud Service con Developer Console](./developer-console.md)

## CRXDE Lite

CRXDE Lite è uno strumento classico ma potente per il debug AEM come ambienti di sviluppo Cloud Service. CRXDE Lite fornisce una suite di funzionalità che facilita il debug dall&#39;ispezione di tutte le risorse e proprietà, dalla manipolazione delle parti modificabili del JCR, dall&#39;analisi delle autorizzazioni e dalla valutazione delle query.

[Debug AEM come Cloud Service con CRXDE Lite](./crxde-lite.md)
