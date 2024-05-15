---
title: Debug di AEM as a Cloud Service
description: su un’infrastruttura cloud self-service, scalabile, che rende necessario che gli sviluppatori AEM comprendano come comprendere ed eseguire il debug di vari aspetti di AEM as a Cloud Service, dalla creazione e distribuzione fino a ottenere i dettagli sull’esecuzione delle applicazioni AEM.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# Debug di AEM as a Cloud Service

AEM as a Cloud Service è la modalità nativa per il cloud di sfruttare le applicazioni AEM. AEM as a Cloud Service viene eseguito su un&#39;infrastruttura cloud self-service, scalabile, che richiede agli sviluppatori AEM di comprendere come comprendere ed eseguire il debug di vari aspetti di AEM as a Cloud Service, dalla generazione e distribuzione fino a ottenere i dettagli sull&#39;esecuzione delle applicazioni AEM.

## Registri

I registri forniscono informazioni dettagliate sul funzionamento dell’applicazione in AEM as a Cloud Service, nonché informazioni approfondite sui problemi relativi alle distribuzioni.

[Debug di AEM as a Cloud Service tramite i registri](./logs.md)

## Generazione e implementazione

Le pipeline di Adobe Cloud Manager distribuiscono l’applicazione AEM tramite una serie di passaggi per determinare la qualità e la fattibilità del codice quando vengono distribuite in AEM as a Cloud Service. Ciascuno dei passaggi può causare errori, rendendo importante comprendere come eseguire il debug delle build per determinare la causa principale di e come risolvere eventuali errori.

[Debug della build e della distribuzione as a Cloud Service dell’AEM](./build-and-deployment.md)

## Console di sviluppo

La Developer Console fornisce una serie di informazioni e introspezioni in ambienti AEM as a Cloud Service che sono utili per comprendere come l’applicazione viene riconosciuta da e funziona all’interno di AEM as a Cloud Service.

[Debug di AEM as a Cloud Service con Developer Console](./developer-console.md)

## Browser dell’archivio

Il Browser dell’archivio è un potente strumento che fornisce visibilità all’archivio dati sottostante dell’AEM, consentendo un facile debug dell’ambiente as a Cloud Service dell’AEM. Il Browser dell’archivio supporta una visualizzazione in sola lettura delle risorse e delle proprietà di AEM in Produzione, Stage e Sviluppo, nonché nei servizi Author, Publish e Preview.

[Debug di AEM as a Cloud Service con il Browser dell’archivio](./repository-browser.md)
