---
title: Ambiente di sviluppo locale per AEM come Cloud Service
description: Panoramica dell'ambiente di sviluppo locale di Adobe Experience Manager (AEM).
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---


# Ambiente di sviluppo locale impostato

Questa esercitazione spiega come impostare un ambiente di sviluppo locale per Adobe Experience Manager (AEM) mediante l’AEM come Cloud Service SDK. Sono inclusi gli strumenti di sviluppo necessari per sviluppare, creare e compilare progetti AEM, nonché i tempi di esecuzione locali, che consentono agli sviluppatori di convalidare rapidamente le nuove funzionalità localmente prima di distribuirle AEM come Cloud Service tramite  Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM come Cloud Service di sviluppo locale](./assets/overview/aem-sdk-technology-stack.png)

L&#39;ambiente di sviluppo locale per AEM può essere suddiviso in tre gruppi logici:

+ Il __AEM Project__ contiene il codice, la configurazione e il contenuto personalizzati dell&#39;applicazione AEM personalizzata.
+ Runtime __AEM__ locale che esegue localmente una versione locale dei servizi AEM Author e Publish.
+ Il runtime __del dispatcher__ locale che esegue una versione locale del server Web Apache HTTP e del dispatcher.

Questa esercitazione spiega come installare e impostare gli elementi evidenziati nel diagramma precedente, fornendo un ambiente di sviluppo locale stabile per lo sviluppo AEM.

## Organizzazione del file system

Questa esercitazione ha stabilito la posizione del AEM come artefatti SDK di Cloud Service e AEM codice progetto come segue:

+ `~/aem-sdk` è una cartella organizzativa contenente i vari strumenti forniti dal AEM come SDK di Cloud Service
+ `~/aem-sdk/author` contiene AEM Author Service
+ `~/aem-sdk/publish` contiene AEM Publish Service
+ `~/aem-sdk/dispatcher` contiene gli strumenti Dispatcher
+ `~/code/<project name>` contiene il codice sorgente AEM progetto personalizzato

Nota `~` abbreviata per la directory dell&#39;utente. In Windows, questo è l&#39;equivalente di `%HOMEPATH%`;

## Strumenti di sviluppo per progetti AEM

Il progetto AEM è la base di codice personalizzata contenente il codice, la configurazione e il contenuto distribuito tramite Cloud Manager per AEM come Cloud Service. La struttura del progetto di base viene generata tramite il [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype).

Questa sezione dell&#39;esercitazione mostra come:

+ Installare la versione [!DNL Java]
+ Installazione [!DNL Node.js] (e npm)
+ Installare la versione [!DNL Maven]
+ Installare la versione [!DNL Git]

[Imposta strumenti di sviluppo per AEM progetti](./development-tools.md)

## Runtime AEM locale

L’AEM come SDK per Cloud Service fornisce una versione [!DNL QuickStart Jar] che esegue una versione locale di AEM. Può [!DNL QuickStart Jar] essere utilizzato per eseguire localmente AEM Author Service o AEM Publish Service. Mentre [!DNL QuickStart Jar] fornisce un&#39;esperienza di sviluppo locale, non tutte le funzioni disponibili in AEM come Cloud Service sono incluse nel [!DNL QuickStart Jar].

Questa sezione dell&#39;esercitazione mostra come:

+ Installare la versione [!DNL Java]
+ Download dell’SDK AEM
+ Eseguire il [!DNL AEM Author Service]
+ Eseguire il [!DNL AEM Publish Service]

[Configurare il runtime AEM locale](./aem-runtime.md)

## Runtime [!DNL Dispatcher] locale

AEM come strumenti Dispatcher dell&#39;SDK di Cloud Service, fornisce tutto il necessario per configurare il [!DNL Dispatcher] runtime locale. [!DNL Dispatcher] Gli strumenti sono [!DNL Docker]basati su e forniscono strumenti della riga di comando per trasferire [!DNL Apache HTTP] Web Server e file di configurazione in formati compatibili e distribuirli [!DNL Dispatcher] in esecuzione nel [!DNL Dispatcher] [!DNL Docker] contenitore.

Questa sezione dell&#39;esercitazione mostra come:

+ Download dell’SDK AEM
+ Installare [!DNL Dispatcher] gli strumenti
+ Eseguire il runtime locale [!DNL Dispatcher]

[Configurare [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)
