---
title: Capitolo 1 - Configurazione e download dei tutorial - Content Services
description: Il capitolo 1 del tutorial AEM headless illustra la configurazione di base dell’istanza AEM per il tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 1%

---

# Configurazione esercitazione

Si consiglia sempre di utilizzare la versione più recente dei componenti core WCM dell’AEM e dell’AEM.

* AEM 6.5 o versione successiva
* Componenti core WCM AEM 2.4.0 o versione successiva
   * Incluso nel pacchetto di contenuti dell&#39;applicazione mobile AEM [WKND di seguito](#wknd-mobile-application-packages)

Prima di avviare questo tutorial, assicurati che le seguenti istanze AEM siano [installate e in esecuzione nel computer locale](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **Autore AEM** su **porta 4502**
* **Publish AEM** sulla **porta 4503**

## Pacchetti applicazioni mobili WKND{#wknd-mobile-application-packages}

Installa i seguenti pacchetti di contenuti AEM su **entrambi** AEM Author e AEM Publish, utilizzando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * Componente proxy [!DNL WKND Mobile] per componenti core WCM AEM
   * CSS [!DNL WKND Mobile] pagine AEM Content Services (per stili secondari)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] struttura sito
   * [!DNL WKND Mobile] struttura cartelle DAM
   * [!DNL WKND Mobile] risorse immagine

Nel [Capitolo 7](./chapter-7.md) eseguiremo l&#39;app mobile di Android [!DNL WKND Mobile] utilizzando [Android Studio](https://developer.android.com/studio) e il pacchetto di applicazione APK (Android Application Package) fornito:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capitolo Pacchetti di contenuti AEM

Questo set di pacchetti di contenuti crea il contenuto e la configurazione descritti nel capitolo associato e in tutti i capitoli precedenti. Questi pacchetti sono facoltativi ma possono accelerare la creazione dei contenuti.

* [Capitolo 2 Contenuto: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 3 Contenuto: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 4 Contenuto: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 5 Contenuto: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Codice sorgente

Il codice sorgente sia per il progetto AEM che per [!DNL Android Mobile App] è disponibile in [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Il codice sorgente non deve essere creato o modificato per questa esercitazione, ma viene fornito per consentire una completa trasparenza nella creazione di tutti gli aspetti dell’esercitazione.

Se riscontri un problema con l&#39;esercitazione o con il codice, lascia un [problema GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Passa alla fine

Per passare alla fine dell&#39;esercitazione, è possibile installare il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) in **entrambi** i Publish AEM Author e AEM. Si noti che il contenuto e la configurazione non verranno visualizzati come pubblicati in AEM Author. Tuttavia, a causa della distribuzione manuale, tutto il contenuto e la configurazione necessari sono disponibili in AEM Publish, consentendo a [!DNL WKND Mobile App] di accedere al contenuto.


## Passaggio successivo

* [Capitolo 2 - Definizione dei modelli per frammenti di contenuto dell’evento](./chapter-2.md)
