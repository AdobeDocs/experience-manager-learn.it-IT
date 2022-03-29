---
title: Debug AEM as a Cloud Service
description: su un’infrastruttura cloud self-service, scalabile e scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari facet di AEM as a Cloud Service, dalla creazione e distribuzione fino a ottenere i dettagli dell’esecuzione delle applicazioni AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Debug AEM as a Cloud Service

AEM as a Cloud Service è il modo nativo del cloud di sfruttare le applicazioni AEM. AEM as a Cloud Service viene eseguito su un’infrastruttura cloud self-service, scalabile e scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari facet di AEM as a Cloud Service, dalla generazione e distribuzione fino a ottenere i dettagli dell’esecuzione delle applicazioni AEM.

## Registri

I registri forniscono dettagli sul funzionamento dell’applicazione in AEM as a Cloud Service, nonché informazioni sui problemi relativi alle distribuzioni.

[Debug AEM as a Cloud Service tramite i registri](./logs.md)

## Generazione e distribuzione

Le pipeline di Adobe Cloud Manager distribuiscono AEM’applicazione attraverso una serie di passaggi per determinare la qualità del codice e la vitalità quando vengono distribuite a AEM as a Cloud Service. Ognuno dei passaggi può causare un errore, rendendo importante comprendere come eseguire il debug delle build per determinare la causa principale di e come risolvere eventuali errori.

[Debug AEM build e distribuzione as a Cloud Service](./build-and-deployment.md)

## Console per sviluppatori

Developer Console fornisce una serie di informazioni e introspezioni in ambienti as a Cloud Service AEM utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona all’interno AEM as a Cloud Service.

[Debug AEM as a Cloud Service con Developer Console](./developer-console.md)

## Browser del repository

Repository Browser è un potente strumento che fornisce visibilità AEM&#39;archivio dati sottostante, consentendo un facile debug dell&#39;ambiente as a Cloud Service AEM. Il browser Repository supporta una visualizzazione in sola lettura delle risorse e delle proprietà di AEM in Produzione, Stage e Sviluppo, nonché dei servizi Author, Publish e Preview.

[Debug AEM as a Cloud Service con il browser del repository](./repository-browser.md)
