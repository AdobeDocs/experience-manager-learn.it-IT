---
title: Comprendere Adobe Cloud Manager
description: Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente di gestire, esaminare e gestire facilmente gli ambienti AEM.
sub-product: cloud manager, foundation
feature: gasdotti, programmi, progetti, porte di qualità, relazioni
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 4%

---


# Comprendere Adobe Cloud Manager

Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente di gestire, esaminare e gestire facilmente gli ambienti AEM.

## Panoramica di Cloud Manager

Questa serie video illustra le funzioni chiave di Cloud Manager per AEM, tra cui:

* [Programmi](#programs)
* [Ambienti](#environments)
* [Rapporti](#reports)
* [Pipeline di produzione CI/CD](#cicd-production-pipeline)
* [Pipeline di non produzione CI/CD](#cicd-non-production-pipeline)
* [Attività](#activity)

Per una panoramica completa, consulta la [Guida utente di Cloud Manager](https://docs.adobe.com/content/help/it/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html).

## Programmi {#programs}

[I ](https://docs.adobe.com/content/help/it-IT/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) programmi di Cloud Manager rappresentano set di ambienti AEM che supportano set logici di iniziative aziendali, in genere corrispondenti a un contratto a livello di servizio (SLA, Service Level Agreement) acquistato. Ad esempio, un programma può rappresentare le risorse AEM per supportare i siti Web pubblici globali, mentre un altro programma rappresenta una DAM centrale interna.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Ambienti {#environments}

[Gli ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) ambienti Cloud Manager sono composti di istanze di AEM Author, AEM Publish e Dispatcher. Diversi ambienti supportano ruoli e possono essere coinvolti utilizzando diverse pipeline CI/CD (descritte di seguito). Gli ambienti di Cloud Manager dispongono in genere di un ambiente di produzione e di un ambiente stage.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Rapporti {#reports}

[I ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) rapporti di Cloud Manager forniscono una visualizzazione degli ambienti del programma e delle istanze di AEM tramite un set di grafici che generano rapporti e tengono traccia di diverse metriche per ogni istanza di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Pipeline di produzione CI/CD {#cicd-production-pipeline}

*[Utilizza la pipeline CI/CD nella serie video ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) di Adobe Cloud Manager per approfondire l’esecuzione della pipeline di produzione, ad esempio l’esplorazione delle implementazioni non riuscite e riuscite.*

>[!NOTE]
>
> In questi video, i tempi di creazione, test e distribuzione sono stati accelerati per ridurre il tempo del video. In genere, un’esecuzione completa della pipeline richiede 45 minuti o più (incluso il test di prestazioni obbligatorio di 30 minuti), a seconda delle dimensioni del progetto, del numero di istanze AEM e dei processi UAT.

### Configurazione

La configurazione [CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) definisce il trigger che avvierà la pipeline, i parametri che controllano la distribuzione di produzione e i parametri del test delle prestazioni.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Esecuzione della pipeline

La [pipeline di produzione CI/CD](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) viene utilizzata per generare e distribuire il codice attraverso Stage nell&#39;ambiente di produzione, riducendo il tempo al valore.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Pipeline di non produzione CI/CD {#cicd-non-production-pipeline}

[Le ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) pipeline CI/CD non di produzione sono suddivise in due categorie: pipeline di qualità del codice e pipeline di distribuzione. La qualità del codice pipeline tutto il codice da un ramo Git alla creazione ed è valutata in base alla scansione della qualità del codice di Cloud Manager. Le pipeline di distribuzione supportano la distribuzione automatica del codice dall’archivio Git a qualsiasi ambiente non di produzione, il che significa qualsiasi ambiente AEM predisposto che non sia Stage o Production.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Attività {#activity}

Cloud Manager fornisce una visualizzazione consolidata dell’attività di un programma, elencando tutte le esecuzioni di pipeline CI/CD, sia di produzione che di non produzione, consentendo la visibilità delle attività passate e presenti e i dettagli di qualsiasi attività possono essere rivisti.

Cloud Manager si integra anche a livello di utente con [Adobe Experience Cloud Notifications](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html), fornendo una visualizzazione onnipresente degli eventi e delle azioni di interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
