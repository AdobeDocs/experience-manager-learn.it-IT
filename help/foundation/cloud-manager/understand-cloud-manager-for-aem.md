---
title: Comprendere Adobe Cloud Manager
description: Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente di gestire facilmente gli ambienti AEM, presentarli e renderli self-service.
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 16%

---

# Comprendere Adobe Cloud Manager

Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente di gestire facilmente gli ambienti AEM, presentarli e renderli self-service.

## Panoramica di Cloud Manager

Questa serie di video esplora le funzioni chiave di Cloud Manager per l’AEM, tra cui:

* [Programmi](#programs)
* [Ambienti](#environments)
* [Rapporti](#reports)
* [Pipeline CI/CD di produzione](#cicd-production-pipeline)
* [Pipeline CI/CD non di produzione](#cicd-non-production-pipeline)
* [Attività](#activity)

Per una panoramica completa, consulta [Guida utente di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=it).

## Programmi {#programs}

[Programmi di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) rappresentano insiemi di ambienti AEM che supportano set logici di iniziative aziendali, in genere corrispondenti a un contratto del livello di servizio (SLA) acquistato. Ad esempio, un programma può rappresentare le risorse dell’AEM per supportare i siti web pubblici globali, mentre un altro programma può rappresentare un DAM centrale interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Ambienti {#environments}

[Ambienti Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) sono composte da istanze AEM Author, AEM Publish e Dispatcher. Diversi ambienti supportano i ruoli e possono essere attivati utilizzando diverse pipeline CI/CD (descritte di seguito). In genere, gli ambienti di Cloud Manager hanno un ambiente Produzione e un ambiente Stage.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Rapporti {#reports}

[Rapporti di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) fornisce una vista degli ambienti del programma e delle istanze AEM attraverso una serie di grafici che riferiscono e tengono traccia di varie metriche per ogni istanza AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## Pipeline CI/CD di produzione {#cicd-production-pipeline}

*[Utilizzare la pipeline CI/CD in Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Le serie video forniscono informazioni approfondite sull’esecuzione della pipeline di produzione, inclusa l’esplorazione delle distribuzioni non riuscite e riuscite.*

>[!NOTE]
>
> In tutti questi video, i tempi di build, test e implementazione sono stati velocizzati per ridurre il tempo dedicato al video. Un’esecuzione completa della pipeline richiede in genere 45 minuti o più (incluso il test delle prestazioni obbligatorio di 30 minuti), a seconda delle dimensioni del progetto, del numero di istanze AEM e dei processi UAT.

### Configurazione

Il [Pipeline CI/CD di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) La configurazione definisce il trigger che avvia la pipeline e i parametri che controllano la distribuzione di produzione e i parametri di test delle prestazioni.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Esecuzione delle pipeline

Il [Pipeline CI/CD di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) viene utilizzato per generare e distribuire il codice nell’ambiente di produzione tramite Stage, riducendo il time-to-value.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## Pipeline CI/CD non di produzione {#cicd-non-production-pipeline}

[Le pipeline CI/CD non di produzione sono suddivise in due categorie: pipeline di qualità del codice e pipeline di implementazione. ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) Le pipeline di qualità del codice instradano tutto il codice da un ramo Git per la generazione e la valutazione in base alla scansione della qualità del codice di Cloud Manager. Le pipeline di implementazione supportano l’implementazione automatica del codice dall’archivio Git in qualsiasi ambiente non di produzione, ovvero qualsiasi ambiente con provisioning AEM che non sia Stage o Production.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Attività {#activity}

Cloud Manager fornisce una vista consolidata dell’attività di un programma, elencando tutte le esecuzioni della pipeline CI/CD, sia di produzione che non di produzione, consentendo la visibilità delle attività passate e presenti e visualizzando tutti i dettagli delle attività.

Cloud Manager si integra anche a livello di utente con [Notifiche Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), che fornisce una visione onnipresente degli eventi e delle azioni di interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
