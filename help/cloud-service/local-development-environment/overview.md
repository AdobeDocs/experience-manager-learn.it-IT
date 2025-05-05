---
title: Ambiente di sviluppo locale per AEM as a Cloud Service
description: Panoramica dell’ambiente di sviluppo locale di Adobe Experience Manager (AEM).
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 12%

---

# Configurazione dell’ambiente di sviluppo locale {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Panoramica"
>abstract="La configurazione dell’ambiente di sviluppo locale per AEM as a Cloud Service include gli strumenti di sviluppo necessari per sviluppare, generare ed elaborare progetti AEM, nonché runtime locali per consentire agli sviluppatori di convalidare rapidamente le nuove funzioni localmente prima di distribuirle in AEM as a Cloud Service tramite Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=it" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=it" text="Nozioni di base sullo sviluppo"

Questo tutorial illustra come configurare un ambiente di sviluppo locale per Adobe Experience Manager (AEM) utilizzando AEM as a Cloud Service SDK. Sono inclusi gli strumenti di sviluppo necessari per sviluppare, generare e compilare progetti AEM, nonché i tempi di esecuzione locali, che consentono agli sviluppatori di convalidare rapidamente le nuove funzioni a livello locale prima di distribuirle ad AEM as a Cloud Service tramite Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/36501?quality=12&learn=on&captions=ita)

![Stack di tecnologie per l&#39;ambiente di sviluppo locale AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

L’ambiente di sviluppo locale per AEM può essere suddiviso in tre gruppi logici:

+ Il __progetto AEM__ contiene il codice personalizzato, la configurazione e il contenuto dell&#39;applicazione AEM personalizzata.
+ __Local AEM Runtime__ che esegue una versione locale dei servizi AEM Author e Publish localmente.
+ __Local Dispatcher Runtime__ che esegue una versione locale di Apache HTTP Web Server e Dispatcher.

Questo tutorial illustra come installare e configurare gli elementi evidenziati nel diagramma precedente, fornendo un ambiente di sviluppo locale stabile per lo sviluppo AEM.

## Organizzazione del file system

Questo tutorial ha stabilito la posizione degli artefatti SDK di AEM as a Cloud Service e il codice del progetto AEM come segue:

+ `~/aem-sdk` è una cartella organizzativa contenente i vari strumenti forniti da AEM as a Cloud Service SDK
+ `~/aem-sdk/author` contiene il servizio AEM Author
+ `~/aem-sdk/publish` contiene il servizio di pubblicazione AEM
+ `~/aem-sdk/dispatcher` contiene gli strumenti di Dispatcher
+ `~/code/<project name>` contiene il codice sorgente del progetto AEM personalizzato

`~` è una scorciatoia per la directory dell&#39;utente. In Windows, equivale a `%HOMEPATH%`;

## Strumenti di sviluppo per progetti AEM

Il progetto AEM è la base di codice personalizzata contenente il codice, la configurazione e il contenuto distribuiti tramite Cloud Manager in AEM as a Cloud Service. La struttura del progetto di base viene generata tramite [Archetipo Maven progetto AEM](https://github.com/adobe/aem-project-archetype).

Questa sezione del tutorial mostra come:

+ Installa [!DNL Java]
+ Installa [!DNL Node.js] (e npm)
+ Installa [!DNL Maven]
+ Installa [!DNL Git]

[Impostare strumenti di sviluppo per progetti AEM](./development-tools.md)

## Runtime AEM locale

AEM as a Cloud Service SDK fornisce un [!DNL QuickStart Jar] che esegue una versione locale di AEM. [!DNL QuickStart Jar] può essere utilizzato per eseguire il servizio AEM Author o AEM Publish localmente. Sebbene [!DNL QuickStart Jar] fornisca un&#39;esperienza di sviluppo locale, non tutte le funzionalità disponibili in AEM as a Cloud Service sono incluse in [!DNL QuickStart Jar].

Questa sezione del tutorial mostra come:

+ Installa [!DNL Java]
+ Scarica AEM SDK
+ Esegui [!DNL AEM Author Service]
+ Esegui [!DNL AEM Publish Service]

[Configurare il runtime locale di AEM](./aem-runtime.md)

## Runtime [!DNL Dispatcher] locale

Gli strumenti Dispatcher di AEM as a Cloud Service SDK forniscono tutto il necessario per configurare il runtime [!DNL Dispatcher] locale. Gli strumenti di [!DNL Dispatcher] sono basati su [!DNL Docker] e forniscono strumenti per riga di comando per eseguire il transpile del server Web [!DNL Apache HTTP] e dei file di configurazione [!DNL Dispatcher] in formati compatibili e distribuirli in [!DNL Dispatcher] in esecuzione nel contenitore [!DNL Docker].

Questa sezione del tutorial mostra come:

+ Scarica AEM SDK
+ Installa strumenti [!DNL Dispatcher]
+ Esegui il runtime [!DNL Dispatcher] locale

[Configura il runtime locale [!DNL Dispatcher] ](./dispatcher-tools.md)
