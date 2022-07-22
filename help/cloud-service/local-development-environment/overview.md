---
title: Ambiente di sviluppo locale per AEM as a Cloud Service
description: Panoramica dell’ambiente di sviluppo locale di Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 2%

---

# Configurazione ambiente di sviluppo locale {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Panoramica"
>abstract="L’impostazione dell’ambiente di sviluppo locale per AEM as a Cloud Service include gli strumenti di sviluppo necessari per sviluppare, generare e compilare AEM progetti, nonché i tempi di esecuzione locali, consentendo agli sviluppatori di convalidare rapidamente le nuove funzionalità localmente prima di distribuirle per AEM as a Cloud Service tramite Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=it" text="Linee guida per lo sviluppo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Nozioni di base sullo sviluppo"

Questa esercitazione descrive come configurare un ambiente di sviluppo locale per Adobe Experience Manager (AEM) utilizzando l’SDK as a Cloud Service AEM. Sono inclusi gli strumenti di sviluppo necessari per sviluppare, generare e compilare progetti AEM, nonché i tempi di esecuzione locali, che consentono agli sviluppatori di convalidare rapidamente le nuove funzionalità localmente prima di distribuirle in AEM as a Cloud Service tramite Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM Stack di tecnologia per l&#39;ambiente di sviluppo locale as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

L’ambiente di sviluppo locale per AEM può essere suddiviso in tre gruppi logici:

+ La __Progetto AEM__ contiene il codice personalizzato, la configurazione e il contenuto dell&#39;applicazione AEM personalizzata.
+ La __Runtime AEM locale__ esegue localmente una versione locale dei servizi Author e Publish di AEM.
+ La __Runtime del Dispatcher locale__ esegue una versione locale di Apache HTTP Web Server e Dispatcher.

Questa esercitazione spiega come installare e impostare gli elementi evidenziati nel diagramma precedente, fornendo un ambiente di sviluppo locale stabile per lo sviluppo AEM.

## Organizzazione del file system

Questa esercitazione ha stabilito la posizione degli artefatti SDK AEM as a Cloud Service e il codice AEM progetto come segue:

+ `~/aem-sdk` è una cartella organizzativa contenente i vari strumenti forniti dall’SDK as a Cloud Service AEM
+ `~/aem-sdk/author` contiene AEM Author Service
+ `~/aem-sdk/publish` contiene AEM Publish Service
+ `~/aem-sdk/dispatcher` contiene gli strumenti di Dispatcher
+ `~/code/<project name>` contiene il codice sorgente AEM progetto personalizzato

Tieni presente che `~` è abbreviato per la directory dell&#39;utente. In Windows, è l&#39;equivalente di `%HOMEPATH%`;

## Strumenti di sviluppo per progetti AEM

Il progetto AEM è la base di codice personalizzata contenente il codice, la configurazione e il contenuto distribuito tramite Cloud Manager per AEM as a Cloud Service. La struttura del progetto di base viene generata tramite [Archetipo AEM progetto Maven](https://github.com/adobe/aem-project-archetype).

Questa sezione dell’esercitazione mostra come:

+ Installare la versione [!DNL Java]
+ Installa [!DNL Node.js] (e npm)
+ Installare la versione [!DNL Maven]
+ Installare la versione [!DNL Git]

[Impostare strumenti di sviluppo per progetti AEM](./development-tools.md)

## Runtime AEM locale

L’SDK AEM as a Cloud Service fornisce un [!DNL QuickStart Jar] esegue una versione locale di AEM. La [!DNL QuickStart Jar] può essere utilizzato per eseguire localmente AEM Author Service o AEM Publish Service. Tieni presente che mentre [!DNL QuickStart Jar] fornisce un’esperienza di sviluppo locale; non tutte le funzioni disponibili in AEM as a Cloud Service sono incluse nel [!DNL QuickStart Jar].

Questa sezione dell’esercitazione mostra come:

+ Installare la versione [!DNL Java]
+ Scaricare l&#39;SDK AEM
+ Esegui il [!DNL AEM Author Service]
+ Esegui il [!DNL AEM Publish Service]

[Configurare il runtime di AEM locale](./aem-runtime.md)

## Locale [!DNL Dispatcher] Runtime

AEM strumenti Dispatcher dell’SDK as a Cloud Service fornisce tutto il necessario per configurare la [!DNL Dispatcher] runtime. [!DNL Dispatcher] Strumenti [!DNL Docker]Basato su e fornisce strumenti a riga di comando per il transpile [!DNL Apache HTTP] Server web e [!DNL Dispatcher] configurare i file in formati compatibili e distribuirli in [!DNL Dispatcher] in esecuzione [!DNL Docker] contenitore.

Questa sezione dell’esercitazione mostra come:

+ Scaricare l&#39;SDK AEM
+ Installa [!DNL Dispatcher] Strumenti
+ Esegui il locale [!DNL Dispatcher] runtime

[Imposta locale [!DNL Dispatcher] Runtime](./dispatcher-tools.md)
