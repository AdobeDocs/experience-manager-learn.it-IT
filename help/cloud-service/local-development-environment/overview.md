---
title: Ambiente di sviluppo locale per AEM as a Cloud Service
description: Panoramica dell’ambiente di sviluppo locale di Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 15%

---

# Configurazione dell’ambiente di sviluppo locale {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Panoramica"
>abstract="La configurazione dell’ambiente di sviluppo locale per AEM as a Cloud Service include gli strumenti di sviluppo necessari per sviluppare, generare ed elaborare progetti AEM, nonché runtime locali per consentire agli sviluppatori di convalidare rapidamente le nuove funzioni localmente prima di distribuirle in AEM as a Cloud Service tramite Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=it" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=it" text="Nozioni di base sullo sviluppo"

Questo tutorial illustra come configurare un ambiente di sviluppo locale per Adobe Experience Manager (AEM) utilizzando l’SDK as a Cloud Service per l’AEM. Sono inclusi gli strumenti di sviluppo necessari per sviluppare, generare e compilare progetti AEM, nonché i tempi di esecuzione locali che consentono agli sviluppatori di convalidare rapidamente le nuove funzioni a livello locale prima di distribuirle a AEM as a Cloud Service tramite Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![Stack di tecnologie per l&#39;ambiente di sviluppo locale as a Cloud Service AEM](./assets/overview/aem-sdk-technology-stack.png)

L’ambiente di sviluppo locale per l’AEM può essere suddiviso in tre gruppi logici:

+ Il __Progetto AEM__ contiene il codice personalizzato, la configurazione e il contenuto dell’applicazione AEM personalizzata.
+ Il __Runtime AEM locale__ che esegue localmente una versione locale dei servizi Author e Publish dell’AEM.
+ Il __Runtime Dispatcher locale__ che esegue una versione locale di Apache HTTP Web Server e Dispatcher.

Questo tutorial illustra come installare e configurare gli elementi evidenziati nel diagramma precedente, fornendo un ambiente di sviluppo locale stabile per lo sviluppo dell’AEM.

## Organizzazione del file system

Questo tutorial ha stabilito la posizione degli artefatti dell’SDK as a Cloud Service per l’AEM e il codice del progetto AEM come segue:

+ `~/aem-sdk` è una cartella organizzativa contenente i vari strumenti forniti dall’SDK as a Cloud Service dell’AEM
+ `~/aem-sdk/author` contiene il servizio di authoring AEM
+ `~/aem-sdk/publish` contiene il servizio di pubblicazione AEM
+ `~/aem-sdk/dispatcher` contiene gli strumenti di Dispatcher
+ `~/code/<project name>` contiene il codice sorgente personalizzato del progetto AEM

Tieni presente che `~` è una scorciatoia per la directory dell&#39;utente. In Windows, equivale a `%HOMEPATH%`;

## Strumenti di sviluppo per progetti AEM

Il progetto AEM è la base di codice personalizzata contenente il codice, la configurazione e il contenuto distribuiti tramite Cloud Manager in AEM as a Cloud Service. La struttura di progetto di base viene generata tramite [Progetto AEM Archetipo Maven](https://github.com/adobe/aem-project-archetype).

Questa sezione del tutorial mostra come:

+ Installa [!DNL Java]
+ Installa [!DNL Node.js] (e npm)
+ Installa [!DNL Maven]
+ Installa [!DNL Git]

[Impostare gli strumenti di sviluppo per i progetti AEM](./development-tools.md)

## Runtime AEM locale

L’SDK as a Cloud Service dell’AEM fornisce [!DNL QuickStart Jar] che esegue una versione locale dell’AEM. Il [!DNL QuickStart Jar] può essere utilizzato per eseguire il servizio di authoring AEM o il servizio di pubblicazione AEM a livello locale. Tieni presente che mentre [!DNL QuickStart Jar] offre un’esperienza di sviluppo locale; non tutte le funzioni disponibili in AEM as a Cloud Service sono incluse [!DNL QuickStart Jar].

Questa sezione del tutorial mostra come:

+ Installa [!DNL Java]
+ Scaricare l’SDK dell’AEM
+ Esegui il [!DNL AEM Author Service]
+ Esegui il [!DNL AEM Publish Service]

[Configurare il runtime AEM locale](./aem-runtime.md)

## Locale [!DNL Dispatcher] Runtime

Gli strumenti di Dispatcher dell’SDK as a Cloud Service dell’AEM forniscono tutto ciò che è necessario per configurare l’interfaccia [!DNL Dispatcher] runtime. [!DNL Dispatcher] Gli strumenti sono [!DNL Docker]basato su e fornisce strumenti per riga di comando per eseguire la trascrizione [!DNL Apache HTTP] Server web e [!DNL Dispatcher] file di configurazione in formati compatibili e distribuirli in [!DNL Dispatcher] in esecuzione in [!DNL Docker] contenitore.

Questa sezione del tutorial mostra come:

+ Scaricare l’SDK dell’AEM
+ Installa [!DNL Dispatcher] Strumenti
+ Esegui il comando locale [!DNL Dispatcher] runtime

[Configurare il Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)
