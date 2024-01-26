---
title: Capitolo 1 - Configurazione e download dei tutorial - Content Services
description: Il capitolo 1 del tutorial AEM headless illustra la configurazione di base dell’istanza AEM per il tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 110
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Configurazione esercitazione

Si consiglia sempre di utilizzare la versione più recente dei componenti core WCM dell’AEM e dell’AEM.

* AEM 6.5 o versione successiva
* Componenti core WCM AEM 2.4.0 o versione successiva
   * Incluso nel [Pacchetto di contenuti per applicazioni AEM per dispositivi mobili WKND di seguito](#wknd-mobile-application-packages)

Prima di iniziare questo tutorial, assicurati che le seguenti istanze di AEM siano [installato ed in esecuzione sul computer locale](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **Autore AEM** il **porta 4502**
* **Pubblicazione AEM** il **porta 4503**

## Pacchetti applicazioni mobili WKND{#wknd-mobile-application-packages}

Installa i seguenti pacchetti di contenuti AEM su **entrambi** Autore AEM e pubblicazione AEM, utilizzando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy per componenti core WCM AEM
   * [!DNL WKND Mobile] CSS delle pagine AEM Content Services (per stili secondari)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Struttura del sito
   * [!DNL WKND Mobile] Struttura delle cartelle DAM
   * [!DNL WKND Mobile] risorse immagine

In entrata [Capitolo 7](./chapter-7.md) eseguiremo il [!DNL WKND Mobile] App mobile Android tramite [Android Studio](https://developer.android.com/studio) e l’APK (Android Application Package) fornito:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capitolo Pacchetti di contenuti AEM

Questo set di pacchetti di contenuti crea il contenuto e la configurazione descritti nel capitolo associato e in tutti i capitoli precedenti. Questi pacchetti sono facoltativi ma possono accelerare la creazione dei contenuti.

* [Capitolo 2 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 3 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 4 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 5 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Codice sorgente

Il codice sorgente sia per il progetto AEM che per [!DNL Android Mobile App] sono disponibili sul [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Il codice sorgente non deve essere creato o modificato per questa esercitazione, ma viene fornito per consentire una completa trasparenza nella creazione di tutti gli aspetti dell’esercitazione.

Se riscontri un problema con l’esercitazione o con il codice, lascia un [Problema GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Passa alla fine

Per passare alla fine dell’esercitazione, il [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) il pacchetto di contenuti può essere installato su **entrambi** Autore AEM e pubblicazione AEM. Tieni presente che il contenuto e la configurazione non verranno visualizzati come pubblicati in AEM Author; tuttavia, a causa della distribuzione manuale, tutto il contenuto e la configurazione necessari sono disponibili in AEM Publish e consentono [!DNL WKND Mobile App] per accedere al contenuto.


## Passaggio successivo

* [Capitolo 2 - Definizione dei modelli per frammenti di contenuto dell’evento](./chapter-2.md)
