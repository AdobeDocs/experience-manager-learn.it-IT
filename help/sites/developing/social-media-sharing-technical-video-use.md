---
title: Utilizzo della condivisione per social media in  AEM Sites
description: Scoprite come impostare e usare il componente Condivisione social media.
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 7%

---


# Utilizzo della condivisione social media {#using-social-media-sharing-in-aem-sites}

Scoprite come impostare e usare il componente Condivisione social media.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Questo video esplora le seguenti funzionalità del componente Condivisione social media (parte di [AEM Componenti principali](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html)) utilizzando il sito Web di esempio [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - Aggiunta e configurazione del componente Condivisione social media
* 1:00 - Condivisione su Facebook
* 3:10 - Condivisione su Pinterest
* 6:25 - Utilizzo del componente Condivisione social media in una pagina di prodotto

## Impostazione di Externalizer {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM&#39;](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) esternalizzatore deve essere configurato sia su AEM Author che su AEM Publish, per mappare la modalità di pubblicazione sul dominio accessibile al pubblico utilizzato per accedere a AEM Publish.

In questo video utilizziamo `/etc/hosts` per individuare *www.example.com* localhost e per utilizzare una [configurazione di base del dispatcher AEM](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) per consentire a www.example.com di iniziare a pubblicare AEM.

## Materiali di supporto {#supporting-materials}

* [Download dei componenti core AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Download We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installazione di Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
