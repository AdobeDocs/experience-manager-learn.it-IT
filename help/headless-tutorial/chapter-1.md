---
title: Capitolo 1 - Configurazione e download delle esercitazioni
seo-title: Guida introduttiva a AEM Content Services - Capitolo 1 - Configurazione delle esercitazioni
description: Capitolo 1 dell'AEM senza titolo l'impostazione della linea di base per l'istanza AEM per l'esercitazione.
seo-description: Capitolo 1 dell'AEM senza titolo l'impostazione della linea di base per l'istanza AEM per l'esercitazione.
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---


# Impostazione esercitazione

È sempre consigliabile utilizzare la versione più recente dei componenti core AEM e AEM WCM.

* AEM 6.5 o versione successiva
* AEM WCM Core Components 2.4.0 o versioni successive
   * Incluso nel pacchetto [WKND Mobile AEM contenuto dell&#39;applicazione:](#wknd-mobile-application-packages)

Prima di avviare questa esercitazione, assicurarsi che le seguenti istanze AEM siano [installate ed eseguite sul computer](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)locale:

* **AEM Author** sulla **porta 4502**
* **AEM Publish** sulla **porta 4503**

## Pacchetti di applicazioni mobili WKND{#wknd-mobile-application-packages}

Installate i seguenti pacchetti di AEM contenuti su AEM Author **e AEM Publish, utilizzando** [!DNL AEM Package Manager].

* [ui.apps: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy per AEM componenti core WCM
   * [!DNL WKND Mobile] CSS delle pagine AEM Content Services (per stile minore)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Struttura del sito
   * [!DNL WKND Mobile] Struttura delle cartelle DAM
   * [!DNL WKND Mobile] risorse immagine

Nel [capitolo 7](./chapter-7.md) eseguiremo l&#39;app mobile [!DNL WKND Mobile] Android utilizzando [Android Studio](https://developer.android.com/studio) e l&#39;APK fornito (pacchetto applicazione Android):

* [[!DNL Android Mobile App: GitHub > Risorse > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capitolo AEM pacchetti di contenuti

Questo set di pacchetti di contenuti crea il contenuto e la configurazione descritti nel capitolo associato e in tutti i capitoli precedenti. Questi pacchetti sono facoltativi ma possono accelerare la creazione di contenuto.

* [Capitolo 2 Contenuto: GitHub > Risorse > com.adobe.aem.guide.wknd-mobile.content.Chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 3 Contenuto: GitHub > Risorse > com.adobe.aem.guide.wknd-mobile.content.Chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 4 Contenuto: GitHub > Risorse > com.adobe.aem.guide.wknd-mobile.content.Chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 5 Contenuto: GitHub > Risorse > com.adobe.aem.guide.wknd-mobile.content.Chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Codice origine

Il codice sorgente sia per il progetto AEM che per il progetto [!DNL Android Mobile App] sono disponibili nel [[!DNL AEM Guide - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Il codice sorgente non deve essere creato o modificato per questa esercitazione, ma viene fornito per consentire la trasparenza completa nella creazione di tutti gli aspetti dell&#39;esercitazione.

Se si verifica un problema con l&#39;esercitazione o il codice, lasciare un problema [](https://github.com/adobe/aem-guides-wknd-mobile/issues)GitHub.

## Passa alla fine

Per terminare l&#39;esercitazione, il pacchetto di contenuti [com.adobe.aem.guide.wknd-mobile.content.Chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) può essere installato su **** AEM Author e AEM Publish. Tenete presente che il contenuto e la configurazione non saranno visualizzati come pubblicati in AEM Author, tuttavia a causa della distribuzione manuale, tutti i contenuti e la configurazione richiesti saranno disponibili in AEM Publish, consentendo [!DNL WKND Mobile App] all&#39;utente di accedere ai contenuti.


## Passaggio successivo

* [Capitolo 2 - Definizione dei modelli di frammento di contenuto evento](./chapter-2.md)
