---
title: Debug di AEM as a Cloud Service
description: su un’infrastruttura cloud scalabile e self-service, che che consente agli sviluppatori AEM di eseguire il debug di vari aspetti di AEM as a Cloud Service, dalla creazione all’implementazione, e di ottenere dettagli sulle applicazioni AEM in esecuzione.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '314'
ht-degree: 100%

---

# Debug di AEM as a Cloud Service

AEM as a Cloud Service è la soluzione nativa per il cloud che consente di sfruttare le applicazioni AEM. AEM as a Cloud Service viene eseguito su un’infrastruttura cloud scalabile e self-service, che consente agli sviluppatori AEM di eseguire il debug di vari aspetti di AEM as a Cloud Service, dalla creazione all’implementazione, e di ottenere dettagli sulle applicazioni AEM in esecuzione.

## Registri

I registri forniscono dettagli sul funzionamento dell’applicazione in AEM as a Cloud Service, nonché informazioni sui problemi relativi alle implementazioni.

[Debug di AEM as a Cloud Service tramite i registri](./logs.md)

## Creazione e distribuzione

Le pipeline di Adobe Cloud Manager distribuiscono l’applicazione AEM attraverso una serie di passaggi per determinare la qualità e la fattibilità del codice in vista della distribuzione in AEM as a Cloud Service. In ciascuno dei passaggi si possono verificare errori, è quindi importante comprendere come eseguire il debug delle build per determinare la causa principale e come risolvere eventuali errori.

[Debug di creazione e distribuzione di AEM as a Cloud Service](./build-and-deployment.md)

## Developer Console

La Developer Console fornisce una serie di informazioni e introspezioni sugli ambienti AEM as a Cloud Service utili per comprendere come l’applicazione viene riconosciuta da, e funziona in AEM as a Cloud Service.

[Debug di AEM as a Cloud Service con Developer Console](./developer-console.md)

## Browser dell’archivio

Il browser dell’archivio è un potente strumento che fornisce visibilità all’archivio dati sottostante di AEM, consentendo un debug semplice dell’ambiente AEM as a Cloud Service. Il browser dell’archivio supporta una visualizzazione di sola lettura delle risorse e delle proprietà di AEM in Produzione, Staging e Sviluppo, nonché dei servizi Author, Publish e Preview.

[Debug di AEM as a Cloud Service con il browser dell’archivio](./repository-browser.md)
