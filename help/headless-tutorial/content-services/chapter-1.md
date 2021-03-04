---
title: Capitolo 1 - Configurazione e download delle esercitazioni - Content Services
seo-title: Guida introduttiva di AEM Content Services - Capitolo 1 - Configurazione delle esercitazioni
description: Capitolo 1 dell’esercitazione AEM Headless sulla configurazione della linea di base per l’istanza AEM per l’esercitazione.
seo-description: Capitolo 1 dell’esercitazione AEM Headless sulla configurazione della linea di base per l’istanza AEM per l’esercitazione.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# Configurazione delle esercitazioni

Si consiglia sempre l’ultima versione dei componenti core di AEM e AEM WCM.

* AEM 6.5 o versione successiva
* Componenti core di AEM WCM 2.4.0 o versione successiva
   * Incluso nel [pacchetto di contenuti dell&#39;applicazione AEM mobile WKND sotto](#wknd-mobile-application-packages)

Prima di avviare questa esercitazione, assicurati che le seguenti istanze AEM siano [installate ed eseguite sul computer locale](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM** Authoron  **porta 4502**
* **AEM** Porta di pubblicazione  **4503**

## Pacchetti di applicazioni mobili WKND{#wknd-mobile-application-packages}

Installa i seguenti pacchetti di contenuti AEM su **sia** AEM Author che AEM Publish, utilizzando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy per componenti core AEM WCM
   * [!DNL WKND Mobile] CSS delle pagine di AEM Content Services (per stile minore)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Struttura del sito
   * [!DNL WKND Mobile] Struttura della cartella DAM
   * [!DNL WKND Mobile] risorse immagine

In [Capitolo 7](./chapter-7.md) eseguiremo l&#39;app mobile [!DNL WKND Mobile] Android utilizzando [Android Studio](https://developer.android.com/studio) e l&#39;APK fornito (pacchetto di applicazione Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capitolo Pacchetti di contenuti AEM

Questo insieme di pacchetti di contenuti crea il contenuto e la configurazione descritti nel capitolo associato e in tutti i capitoli precedenti. Questi pacchetti sono facoltativi ma possono accelerare la creazione di contenuti.

* [Capitolo 2 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 3 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 4 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capitolo 5 Contenuto: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Codice sorgente

Il codice sorgente sia per il progetto AEM che per il [!DNL Android Mobile App] sono disponibili in [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Il codice sorgente non deve essere generato o modificato per questa esercitazione, ma è fornito per consentire la completa trasparenza nella creazione di tutti gli aspetti dell’esercitazione.

Se trovi un problema con l&#39;esercitazione o il codice, lascia un [problema GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Passa alla fine

Per passare alla fine dell&#39;esercitazione, il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.capitolo-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) può essere installato su **sia** AEM Author che AEM Publish. Tieni presente che il contenuto e la configurazione non verranno visualizzati come pubblicati in AEM Author, tuttavia a causa della distribuzione manuale, tutti i contenuti e la configurazione richiesti saranno disponibili in AEM Publish e il contenuto potrà essere accessibile a [!DNL WKND Mobile App] .


## Passaggio successivo

* [Capitolo 2 - Definizione dei modelli di frammento di contenuto dell’evento](./chapter-2.md)
