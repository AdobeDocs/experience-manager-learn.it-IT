---
title: Comprendere Adobe Cloud Manager
description: Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente di gestire facilmente gli ambienti AEM, introdurre e self-service.
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
ht-degree: 3%

---

# Comprendere Adobe Cloud Manager

Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente di gestire facilmente gli ambienti AEM, introdurre e self-service.

## Panoramica di Cloud Manager

Questa serie video illustra le funzioni chiave di Cloud Manager per AEM tra cui:

* [Programmi](#programs)
* [Ambienti](#environments)
* [Rapporti](#reports)
* [Pipeline di produzione CI/CD](#cicd-production-pipeline)
* [Pipeline di non produzione CI/CD](#cicd-non-production-pipeline)
* [Attività](#activity)

Per una panoramica completa, consulta la sezione [Guida utente di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=it).

## Programmi {#programs}

[Programmi di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) rappresentano insiemi di ambienti AEM che supportano set logici di iniziative aziendali, in genere corrispondenti a un contratto a livello di servizio (SLA, Service Level Agreement) acquistato. Ad esempio, un programma può rappresentare le risorse AEM per supportare i siti Web pubblici globali, mentre un altro programma rappresenta una DAM centrale interna.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Ambienti {#environments}

[Ambienti Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) sono composti da istanze di AEM Author, AEM Publish e Dispatcher. Diversi ambienti supportano ruoli e possono essere coinvolti utilizzando diverse pipeline CI/CD (descritte di seguito). Gli ambienti di Cloud Manager dispongono in genere di un ambiente di produzione e di un ambiente stage.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Rapporti {#reports}

[Report di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) fornisce una visualizzazione degli ambienti e delle istanze AEM del programma tramite un set di grafici che generano rapporti e tengono traccia di varie metriche per ogni istanza AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## Pipeline di produzione CI/CD {#cicd-production-pipeline}

*[Utilizzare la pipeline CI/CD in Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) le serie video forniscono informazioni approfondite sull’esecuzione della pipeline di produzione, inclusa l’esplorazione delle implementazioni non riuscite e di successo.*

>[!NOTE]
>
> In questi video, i tempi di creazione, test e distribuzione sono stati accelerati per ridurre il tempo del video. In genere, un’esecuzione completa della pipeline richiede 45 minuti o più (incluso il test di prestazioni obbligatorio di 30 minuti), a seconda della dimensione del progetto, del numero di istanze AEM e dei processi UAT.

### Configurazione

La [Pipeline di produzione CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) La configurazione definisce il trigger che avvia la pipeline e i parametri che controllano la distribuzione di produzione e i parametri del test delle prestazioni.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Esecuzione delle pipeline

La [Pipeline di produzione CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) viene utilizzato per generare e distribuire il codice attraverso Stage nell&#39;ambiente di produzione, riducendo il tempo a valore.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## Pipeline di non produzione CI/CD {#cicd-non-production-pipeline}

[pipeline non di produzione CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) sono suddivise in due categorie: pipeline di qualità del codice e pipeline di distribuzione. La qualità del codice pipeline tutto il codice da un ramo Git alla creazione ed è valutata in base alla scansione della qualità del codice di Cloud Manager. Le pipeline di distribuzione supportano la distribuzione automatica del codice dall’archivio Git a qualsiasi ambiente non di produzione, il che significa che qualsiasi ambiente AEM predisposto che non sia Stage o Production.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Attività {#activity}

Cloud Manager fornisce una visualizzazione consolidata dell’attività di un programma, elencando tutte le esecuzioni di pipeline CI/CD, sia di produzione che di non produzione, consentendo la visibilità delle attività passate e presenti e i dettagli di qualsiasi attività possono essere rivisti.

Cloud Manager si integra anche a livello di utente con [Notifiche Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), fornendo una visione onnipresente degli eventi e delle azioni di interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
