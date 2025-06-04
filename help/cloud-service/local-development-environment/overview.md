---
title: Ambiente di sviluppo locale di AEM as a Cloud Service
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
workflow-type: ht
source-wordcount: '530'
ht-degree: 100%

---

# Configurazione dell’ambiente di sviluppo locale {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Panoramica"
>abstract="La configurazione dell’ambiente di sviluppo locale per AEM as a Cloud Service include gli strumenti di sviluppo necessari per sviluppare, generare ed elaborare progetti AEM, nonché runtime locali per consentire agli sviluppatori di convalidare rapidamente le nuove funzioni localmente prima di distribuirle in AEM as a Cloud Service tramite Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=it" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=it" text="Nozioni di base sullo sviluppo"

Questo tutorial illustra i passaggi necessari per la configurazione di un ambiente di sviluppo locale per Adobe Experience Manager (AEM) utilizzando l’SDK di AEM as a Cloud Service. Vengono trattati anche gli strumenti di sviluppo necessari per sviluppare, creare e compilare progetti AEM, nonché runtime locali per consentire agli sviluppatori di convalidare rapidamente le nuove funzioni localmente prima di distribuirle in AEM as a Cloud Service tramite Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/36501?quality=12&learn=on&captions=ita)

![Stack di tecnologie per l’ambiente di sviluppo locale di AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

L’ambiente di sviluppo locale di AEM può essere suddiviso in tre gruppi logici:

+ Il __progetto AEM__ contiene il codice personalizzato, la configurazione e il contenuto che costituiscono l’applicazione AEM personalizzata.
+ Il __Runtime AEM locale__ che esegue una versione locale dei servizi di authoring e pubblicazione AEM localmente.
+ Il __Runtime Dispatcher locale__ che esegue una versione locale del server web Apache HTTP e Dispatcher.

Questo tutorial descrive come installare e configurare gli elementi evidenziati nel diagramma precedente, fornendo un ambiente di sviluppo locale stabile per lo sviluppo AEM.

## Organizzazione del file system

Per questo tutorial la posizione degli artefatti dell’SDK di AEM as a Cloud Service e il codice del progetto AEM è definita come segue:

+ `~/aem-sdk` è una cartella organizzativa contenente i vari strumenti forniti da AEM as a Cloud Service SDK
+ `~/aem-sdk/author` contiene il servizio AEM Author
+ `~/aem-sdk/publish` contiene il servizio AEM Publish
+ `~/aem-sdk/dispatcher` contiene gli strumenti di Dispatcher
+ `~/code/<project name>` contiene il codice sorgente del progetto AEM personalizzato

Tieni presente che `~` è un’abbreviazione della directory dell’utente. In Windows, equivale a `%HOMEPATH%`;

## Strumenti di sviluppo per progetti AEM

Il progetto AEM è la base di codice personalizzata contenente il codice, la configurazione e il contenuto che vengono distribuiti tramite Cloud Manager in AEM as a Cloud Service. La struttura del progetto della linea di base viene generata tramite un [archetipo di progetto Maven di AEM](https://github.com/adobe/aem-project-archetype).

Questa sezione del tutorial mostra come:

+ Installare [!DNL Java]
+ Installare [!DNL Node.js] (e npm)
+ Installare [!DNL Maven]
+ Installare [!DNL Git]

[Configurare gli strumenti di sviluppo per i progetti AEM](./development-tools.md)

## Runtime AEM locale

L’SDK di AEM as a Cloud Service fornisce un [!DNL QuickStart Jar] che esegue una versione locale di AEM. [!DNL QuickStart Jar] può essere utilizzato per eseguire localmente il servizio AEM Author o il servizio AEM Publish. Tieni presente che sebbene [!DNL QuickStart Jar] fornisca un’esperienza di sviluppo locale, non tutte le funzioni disponibili in AEM as a Cloud Service sono incluse in [!DNL QuickStart Jar].

Questa sezione del tutorial mostra come:

+ Installare [!DNL Java]
+ Scaricare l’SDK di AEM
+ Eseguire [!DNL AEM Author Service]
+ Eseguire [!DNL AEM Publish Service]

[Configurare il Runtime di AEM locale](./aem-runtime.md)

## Runtime [!DNL Dispatcher] locale

Gli strumenti Dispatcher dell’SDK di AEM as a Cloud Service forniscono tutto il necessario per configurare il runtime [!DNL Dispatcher] locale. Gli strumenti di [!DNL Dispatcher] sono basati su [!DNL Docker] e forniscono strumenti per riga di comando per eseguire il transpiling del server web [!DNL Apache HTTP] e dei file di configurazione di [!DNL Dispatcher] in formati compatibili e distribuirli in [!DNL Dispatcher] in esecuzione nel contenitore [!DNL Docker].

Questa sezione del tutorial mostra come:

+ Scaricare l’SDK di AEM
+ Installare gli strumenti di [!DNL Dispatcher]
+ Esegui il runtime [!DNL Dispatcher] locale

[Configurare il runtime  [!DNL Dispatcher]  locale](./dispatcher-tools.md)
