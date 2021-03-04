---
title: Debug di AEM as a Cloud Service
description: su un’infrastruttura cloud self-service, scalabile e scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari facet di AEM as a Cloud Service, dalla creazione e distribuzione fino a ottenere i dettagli dell’esecuzione delle applicazioni AEM.
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante, intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 2%

---


# Debug di AEM as a Cloud Service

AEM as a Cloud Service è il modo nativo per il cloud di sfruttare le applicazioni AEM. AEM as a Cloud Service viene eseguito su un’infrastruttura cloud self-service, scalabile e scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari facet di AEM as a Cloud Service, dalla creazione e implementazione fino a ottenere i dettagli dell’esecuzione delle applicazioni AEM.

## Registri

I registri forniscono dettagli sul funzionamento dell’applicazione in AEM as a Cloud Service e informazioni sui problemi relativi alle implementazioni.

[Debug di AEM as a Cloud Service tramite i registri](./logs.md)

## Generazione e distribuzione

Le pipeline di Adobe Cloud Manager distribuiscono l’applicazione AEM attraverso una serie di passaggi per determinare la qualità del codice e la viabilità quando vengono implementate in AEM as a Cloud Service. Ognuno dei passaggi può causare un errore, rendendo importante comprendere come eseguire il debug delle build per determinare la causa principale di e come risolvere eventuali errori.

[Debug della build e implementazione di AEM as a Cloud Service](./build-and-deployment.md)

## Console per sviluppatori

La Console per sviluppatori fornisce una serie di informazioni e introspezioni negli ambienti AEM as a Cloud Service utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona in AEM as a Cloud Service.

[Debug di AEM as a Cloud Service con la Console per sviluppatori](./developer-console.md)

## CRXDE Lite

CRXDE Lite è uno strumento classico ma potente per il debug degli ambienti di sviluppo di AEM as a Cloud Service. CRXDE Lite fornisce una suite di funzionalità che facilita il debug dall&#39;ispezione di tutte le risorse e proprietà, dalla manipolazione delle parti mutabili del JCR, dall&#39;analisi delle autorizzazioni e dalla valutazione delle query.

[Debug di AEM as a Cloud Service con CRXDE Lite](./crxde-lite.md)
