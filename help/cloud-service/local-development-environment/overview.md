---
title: Ambiente di sviluppo locale per AEM come Cloud Service
description: Panoramica dell’ambiente di sviluppo locale di Adobe Experience Manager (AEM).
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 2%

---


# Configurazione ambiente di sviluppo locale

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Panoramica"
>abstract="L’impostazione dell’ambiente di sviluppo locale per AEM come Cloud Service include gli strumenti di sviluppo necessari per sviluppare, generare e compilare progetti AEM, nonché i tempi di esecuzione locali, consentendo agli sviluppatori di convalidare rapidamente le nuove funzionalità localmente prima di distribuirle per AEM come Cloud Service tramite Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Nozioni di base sullo sviluppo"

Questa esercitazione descrive come configurare un ambiente di sviluppo locale per Adobe Experience Manager (AEM) utilizzando l’SDK AEM come Cloud Service. Sono inclusi gli strumenti di sviluppo necessari per sviluppare, generare e compilare progetti AEM, nonché i tempi di esecuzione locali, che consentono agli sviluppatori di convalidare rapidamente le nuove funzionalità localmente prima di distribuirle per AEM come Cloud Service tramite Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM come Cloud Service Local Development Environment Technology Stack](./assets/overview/aem-sdk-technology-stack.png)

L’ambiente di sviluppo locale per AEM può essere suddiviso in tre gruppi logici:

+ Il __AEM Project__ contiene il codice personalizzato, la configurazione e il contenuto dell&#39;applicazione AEM personalizzata.
+ Il __Runtime AEM locale__ esegue localmente una versione locale dei servizi di authoring e pubblicazione di AEM.
+ Il __Runtime del Dispatcher locale__ esegue una versione locale di Apache HTTP Web Server e Dispatcher.

Questa esercitazione spiega come installare e impostare gli elementi evidenziati nel diagramma precedente, fornendo un ambiente di sviluppo locale stabile per lo sviluppo AEM.

## Organizzazione del file system

Questa esercitazione ha stabilito la posizione del AEM come artefatti SDK di Cloud Service e AEM codice del progetto come segue:

+ `~/aem-sdk` è una cartella organizzativa contenente i vari strumenti forniti dall’AEM come SDK di Cloud Service
+ `~/aem-sdk/author` contiene AEM Author Service
+ `~/aem-sdk/publish` contiene AEM Publish Service
+ `~/aem-sdk/dispatcher` contiene gli strumenti di Dispatcher
+ `~/code/<project name>` contiene il codice sorgente AEM progetto personalizzato

Tenere presente che `~` è abbreviato per la directory dell&#39;utente. In Windows, è l’equivalente di `%HOMEPATH%`;

## Strumenti di sviluppo per progetti AEM

Il progetto AEM è la base di codice personalizzata contenente il codice, la configurazione e il contenuto distribuito tramite Cloud Manager per AEM come Cloud Service. La struttura del progetto di base viene generata tramite [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype).

Questa sezione dell’esercitazione mostra come:

+ Installare la versione [!DNL Java]
+ Installa [!DNL Node.js] (e npm)
+ Installare la versione [!DNL Maven]
+ Installare la versione [!DNL Git]

[Impostare strumenti di sviluppo per progetti AEM](./development-tools.md)

## Runtime AEM locale

L&#39;SDK di AEM as a Cloud Service fornisce una [!DNL QuickStart Jar] che esegue una versione locale di AEM. È possibile utilizzare [!DNL QuickStart Jar] per eseguire localmente AEM Author Service o AEM Publish Service. Tieni presente che mentre il [!DNL QuickStart Jar] fornisce un’esperienza di sviluppo locale, non tutte le funzioni disponibili in AEM come Cloud Service sono incluse nel [!DNL QuickStart Jar].

Questa sezione dell’esercitazione mostra come:

+ Installare la versione [!DNL Java]
+ Scaricare l&#39;SDK AEM
+ Esegui il [!DNL AEM Author Service]
+ Esegui il [!DNL AEM Publish Service]

[Configurare il runtime di AEM locale](./aem-runtime.md)

## Runtime locale [!DNL Dispatcher]

AEM strumenti Dispatcher dell’SDK di Cloud Service fornisce tutto il necessario per configurare il runtime locale [!DNL Dispatcher]. [!DNL Dispatcher] Gli strumenti sono  [!DNL Docker]basati su e forniscono strumenti a riga di comando per trasferire i file di  [!DNL Apache HTTP] Web Server e  [!DNL Dispatcher] di configurazione in formati compatibili e distribuirli in  [!DNL Dispatcher] esecuzione nel  [!DNL Docker] contenitore.

Questa sezione dell’esercitazione mostra come:

+ Scaricare l&#39;SDK AEM
+ Installare gli strumenti [!DNL Dispatcher]
+ Esegui il runtime locale [!DNL Dispatcher]

[Configurare il runtime locale [!DNL Dispatcher] Runtime](./dispatcher-tools.md)
