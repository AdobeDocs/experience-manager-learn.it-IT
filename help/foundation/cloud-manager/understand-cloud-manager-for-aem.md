---
title: Informazioni su Adobe Cloud Manager
description: Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente una gestione semplice, introduttiva e self-service degli ambienti AEM.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Developer
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1011
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 16%

---

# Informazioni su Adobe Cloud Manager

Adobe Cloud Manager fornisce una soluzione semplice ma solida che consente una gestione semplice, introduttiva e self-service degli ambienti AEM.

## Panoramica di Cloud Manager

Questa serie di video esplora le funzioni chiave di Cloud Manager per AEM, tra cui:

* [Programmi](#programs)
* [Ambienti](#environments)
* [Rapporti](#reports)
* [Pipeline CI/CD di produzione](#cicd-production-pipeline)
* [Pipeline CI/CD non di produzione](#cicd-non-production-pipeline)
* [Attività](#activity)

Per una panoramica completa, consultare la [Guida utente di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=it).

## Programmi {#programs}

[I programmi Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html?lang=it) rappresentano insiemi di ambienti AEM che supportano set logici di iniziative aziendali, in genere corrispondenti a un Service level agreement (SLA) acquistato. Ad esempio, un programma può rappresentare le risorse AEM per supportare i siti Web pubblici globali, mentre un altro programma può rappresentare un DAM centrale interno.

>[!VIDEO](https://video.tv.adobe.com/v/328126?captions=ita&quality=12&learn=on)

## Ambienti {#environments}

[Gli ambienti Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html?lang=it) sono composti da istanze AEM Author, AEM Publish e Dispatcher. Diversi ambienti supportano i ruoli e possono essere attivati utilizzando diverse pipeline CI/CD (descritte di seguito). In genere, gli ambienti Cloud Manager dispongono di un ambiente Produzione e di un ambiente Stage.

>[!VIDEO](https://video.tv.adobe.com/v/328127?captions=ita&quality=12&learn=on)

## Rapporti {#reports}

I [report Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=it) forniscono una visualizzazione degli ambienti del programma e delle istanze AEM tramite un set di grafici che generano report e tengono traccia di varie metriche per ogni istanza AEM.

>[!VIDEO](https://video.tv.adobe.com/v/328128?captions=ita&quality=12&learn=on)

## Pipeline CI/CD di produzione {#cicd-production-pipeline}

*[Utilizzare la pipeline CI/CD nella serie video di Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) fornisce informazioni approfondite sull&#39;esecuzione della pipeline di produzione, inclusa l&#39;esplorazione delle distribuzioni non riuscite e riuscite.*

>[!NOTE]
>
> In tutti questi video, i tempi di build, test e implementazione sono stati velocizzati per ridurre il tempo dedicato al video. Un’esecuzione completa della pipeline richiede in genere 45 minuti o più (incluso il test delle prestazioni obbligatorio di 30 minuti), a seconda delle dimensioni del progetto, del numero di istanze di AEM e dei processi UAT.

### Configurazione

La configurazione della [pipeline CI/CD di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=it) definisce il trigger che avvia la pipeline e i parametri che controllano la distribuzione di produzione e i parametri di test delle prestazioni.

>[!VIDEO](https://video.tv.adobe.com/v/328575?captions=ita&quality=12&learn=on)

### Esecuzione delle pipeline

La pipeline di produzione [CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=it) viene utilizzata per generare e distribuire il codice nell&#39;ambiente di produzione tramite Stage, riducendo il time-to-value.

>[!VIDEO](https://video.tv.adobe.com/v/328576?captions=ita&quality=12&learn=on)

## Pipeline CI/CD non di produzione {#cicd-non-production-pipeline}

[Le pipeline CI/CD non di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=it) sono suddivise in due categorie: pipeline di qualità del codice e pipeline di distribuzione. Le pipeline di qualità del codice instradano tutto il codice da un ramo Git per la generazione e la valutazione in base alla scansione della qualità del codice di Cloud Manager. Le pipeline di implementazione supportano l’implementazione automatica del codice dall’archivio Git in qualsiasi ambiente non di produzione, ovvero qualsiasi ambiente con provisioning AEM che non sia di staging o di produzione.

>[!VIDEO](https://video.tv.adobe.com/v/328577?captions=ita&quality=12&learn=on)

## Attività {#activity}

Cloud Manager fornisce una vista consolidata dell’attività di un programma, elencando tutte le esecuzioni della pipeline CI/CD, sia di produzione che non di produzione, consentendo la visibilità delle attività passate e presenti e i dettagli di qualsiasi attività possono essere rivisti.

Cloud Manager si integra inoltre a livello di singolo utente con [Notifiche Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html?lang=it), fornendo una visualizzazione onnipresente degli eventi e delle azioni di interesse.

>[!VIDEO](https://video.tv.adobe.com/v/328129?captions=ita&quality=12&learn=on)
