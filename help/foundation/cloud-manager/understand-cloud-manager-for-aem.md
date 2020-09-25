---
title: Comprendere  Adobe Cloud Manager
description: ' Adobe Cloud Manager offre una soluzione semplice ma affidabile che consente di gestire facilmente gli ambienti AEM, di effettuare analisi e di self-service.'
sub-product: cloud manager, fondazione
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 4%

---


# Comprendere  Adobe Cloud Manager

 Adobe Cloud Manager offre una soluzione semplice ma affidabile che consente di gestire facilmente gli ambienti AEM, di effettuare analisi e di self-service.

## Panoramica di Cloud Manager

Questa serie video esplora le funzionalità chiave di Cloud Manager per AEM, tra cui:

* [Programmi](#programs)
* [Ambienti](#environments)
* [Rapporti](#reports)
* [Tubazione di produzione CI/CD](#cicd-production-pipeline)
* [Tubi di non produzione CI/CD](#cicd-non-production-pipeline)
* [Attività](#activity)

Per una panoramica completa, consulta la Guida [utente di](https://docs.adobe.com/content/help/it-IT/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)Cloud Manager.

## Programmi {#programs}

[I programmi](https://docs.adobe.com/content/help/it-IT/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) di Cloud Manager rappresentano insiemi di ambienti AEM che supportano insiemi logici di iniziative aziendali, in genere corrispondenti a un contratto di servizio (SLA) acquistato. Ad esempio, un programma può rappresentare le risorse AEM per supportare i siti Web pubblici globali, mentre un altro programma rappresenta una DAM centrale interna.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Ambienti {#environments}

[Gli ambienti](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) di Cloud Manager sono composti di istanze di AEM Author, AEM Publish e Dispatcher. Diversi ambienti supportano i ruoli e possono essere coinvolti utilizzando diverse tubazioni CI/CD (descritte di seguito). Gli ambienti di Cloud Manager hanno in genere un ambiente di produzione e un ambiente Stage.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Rapporti {#reports}

[I report](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) di Cloud Manager forniscono una visualizzazione degli ambienti e delle istanze AEM del programma tramite un set di grafici che eseguono il report e tengono traccia di una serie di metriche per ogni istanza AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD Production Pipeline {#cicd-production-pipeline}

*[L&#39;utilizzo della pipeline CI/CD nella  serie video di Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)fornisce un&#39;analisi approfondita dell&#39;esecuzione della pipeline di produzione, compresa l&#39;esplorazione delle implementazioni non riuscite e riuscite.*

>[!NOTE]
>
> In tutti questi video, i tempi di creazione, test e implementazione sono stati accelerati per ridurre il tempo del video. Un&#39;esecuzione completa della pipeline richiede in genere 45 minuti o più (compreso il test obbligatorio delle prestazioni di 30 minuti), a seconda delle dimensioni del progetto, del numero di istanze AEM e dei processi UAT.

### Configurazione

La configurazione [CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) definisce il trigger che avvierà la tubazione, i parametri che controllano la distribuzione di produzione e i parametri di test delle prestazioni.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Esecuzione pipeline

La pipeline di produzione [CI/CD](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) viene utilizzata per creare e distribuire il codice attraverso Stage all&#39;ambiente di produzione, riducendo il tempo a valore.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Tubi di non produzione CI/CD {#cicd-non-production-pipeline}

[I gasdotti](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) CI/CD non di produzione sono suddivisi in due categorie, i gasdotti Code Quality e i gasdotti di distribuzione. La qualità del codice distribuisce tutto il codice da un ramo Git per creare e per essere valutato in base alla scansione della qualità del codice di Cloud Manager. Le pipeline di distribuzione supportano la distribuzione automatizzata del codice dall&#39;archivio Git a qualsiasi ambiente non di produzione, ovvero qualsiasi ambiente AEM fornito che non sia Stage o Production.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Attività {#activity}

Cloud Manager offre una vista consolidata sull&#39;attività di un programma, in cui sono elencate tutte le esecuzioni di pipeline CI/CD, sia di produzione che di non produzione, consentendo la visibilità nell&#39;attività passata e presente, e i dettagli di qualsiasi attività possono essere rivisti.

Cloud Manager si integra inoltre a livello di utente con le notifiche [](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)Adobe Experience Cloud, fornendo una visualizzazione onnipresente degli eventi e delle azioni di interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
