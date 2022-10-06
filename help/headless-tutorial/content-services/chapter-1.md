---
title: Capitolo 1 - Configurazione e download delle esercitazioni - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: Capitolo 1 dell’esercitazione AEM Headless la configurazione della linea di base per l’istanza AEM per l’esercitazione.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 1%

---

# Configurazione delle esercitazioni

Si consiglia sempre di utilizzare AEM e AEM versione più recente dei componenti core WCM.

* AEM 6.5 o versione successiva
* AEM i componenti core WCM 2.4.0 o versioni successive
   * Incluso nel [Pacchetto di contenuti dell&#39;applicazione di AEM mobile WKND qui sotto](#wknd-mobile-application-packages)

Prima di avviare questa esercitazione, assicurati che le seguenti istanze di AEM siano [installato ed in esecuzione nel computer locale](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM Author** su **porta 4502**
* **Pubblicazione AEM** su **porta 4503**

## Pacchetti di applicazioni mobili WKND{#wknd-mobile-application-packages}

Installa i seguenti pacchetti di contenuti AEM su **entrambi** AEM Author e AEM Publish, utilizzando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy per AEM componenti core WCM
   * [!DNL WKND Mobile] CSS delle pagine di AEM Content Services (per stile minore)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Struttura sito
   * [!DNL WKND Mobile] Struttura della cartella DAM
   * [!DNL WKND Mobile] risorse immagine

In [Capitolo 7](./chapter-7.md) eseguiremo il [!DNL WKND Mobile] App mobile Android che utilizza [Android Studio](https://developer.android.com/studio) e l&#39;APK fornito (pacchetto di applicazione Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capitolo AEM pacchetti di contenuti

Questo insieme di pacchetti di contenuti crea il contenuto e la configurazione descritti nel capitolo associato e in tutti i capitoli precedenti. Questi pacchetti sono facoltativi ma possono accelerare la creazione di contenuti.

* [Capitolo 2 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 3 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 4 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 5 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Codice sorgente

Il codice sorgente del progetto AEM e del [!DNL Android Mobile App] sono disponibili nella [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Il codice sorgente non deve essere generato o modificato per questa esercitazione, ma è fornito per consentire la completa trasparenza nella creazione di tutti gli aspetti dell’esercitazione.

Se trovi un problema con l&#39;esercitazione o il codice, lascia un [Problema GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Passa alla fine

Per passare alla fine dell’esercitazione, l’ [com.adobe.aem.guides.wknd-mobile.content.capitolo-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) il pacchetto di contenuti può essere installato su **entrambi** AEM Author e AEM Publish. Tieni presente che il contenuto e la configurazione non verranno visualizzati come pubblicati in AEM Author, tuttavia a causa della distribuzione manuale, su AEM Publish sono disponibili tutti i contenuti e la configurazione necessari per consentire l’ [!DNL WKND Mobile App] per accedere al contenuto.


## Passaggio successivo

* [Capitolo 2 - Definizione dei modelli di frammento di contenuto dell’evento](./chapter-2.md)
