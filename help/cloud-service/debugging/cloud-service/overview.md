---
title: Debug AEM come Cloud Service
description: su un’infrastruttura cloud self-service, scalabile e scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari facet di AEM come Cloud Service, dalla generazione e distribuzione fino a ottenere i dettagli dell’esecuzione delle applicazioni AEM.
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Sviluppo
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 2%

---


# Debug AEM come Cloud Service

AEM come Cloud Service è il modo nativo del cloud di sfruttare le applicazioni AEM. AEM come Cloud Service funziona su un&#39;infrastruttura cloud self-service, scalabile e scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari aspetti di AEM come Cloud Service, dalla generazione e distribuzione fino a ottenere i dettagli dell&#39;esecuzione AEM applicazioni.

## Registri

I registri forniscono dettagli sul funzionamento dell’applicazione in AEM as a Cloud Service, nonché informazioni sui problemi relativi alle implementazioni.

[Eseguire il debug AEM come Cloud Service utilizzando i registri](./logs.md)

## Generazione e distribuzione

Le pipeline di Adobe Cloud Manager distribuiscono AEM’applicazione attraverso una serie di passaggi per determinare la qualità del codice e la vitalità quando vengono implementate in AEM come Cloud Service. Ognuno dei passaggi può causare un errore, rendendo importante comprendere come eseguire il debug delle build per determinare la causa principale di e come risolvere eventuali errori.

[Debug di AEM come build e implementazione di Cloud Service](./build-and-deployment.md)

## Console per sviluppatori

Developer Console fornisce una serie di informazioni e introspezioni in ambienti AEM come ambienti di Cloud Service utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona all’interno di AEM come Cloud Service.

[Debug di AEM come Cloud Service con Developer Console](./developer-console.md)

## CRXDE Lite

CRXDE Lite è uno strumento classico ma potente per il debug AEM come ambienti di sviluppo di Cloud Service. CRXDE Lite fornisce una suite di funzionalità che facilita il debug dall’ispezione di tutte le risorse e proprietà, dalla manipolazione delle parti modificabili del JCR, dall’analisi delle autorizzazioni e dalla valutazione delle query.

[Debug di AEM come Cloud Service con CRXDE Lite](./crxde-lite.md)
