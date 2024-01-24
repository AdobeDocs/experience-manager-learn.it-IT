---
title: Utilizzo della condivisione sui social media in AEM Sites
description: Esplora la configurazione e l’utilizzo del componente Condivisione social media.
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 518
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 2%

---

# Utilizzo della condivisione sui social media {#using-social-media-sharing-in-aem-sites}

Esplora la configurazione e l’utilizzo del componente Condivisione social media.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Questo video illustra le seguenti funzioni del componente Condivisione social media (parte di [Componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)) utilizzando [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) sito web di esempio.

* 0:00 - Aggiunta e configurazione del componente Condivisione social media
* 1:00 - Condivisione con Facebook
* 3:10 - Condivisione con Pinterest
* 6:25 - Utilizzo del componente Condivisione social media in una pagina di prodotto

## Configurazione esternalizzazione {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[Esternalizzatore AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) deve essere impostato sia su AEM Author che su AEM Publish, per mappare la modalità di esecuzione di pubblicazione sul dominio accessibile al pubblico utilizzato per accedere a AEM Publish.

In questo video utilizziamo `/etc/hosts` da spoofare *www.example.com* per risolvere in localhost e utilizzare un [configurazione di base del Dispatcher AEM](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) per consentire a www.example.com di eseguire la pubblicazione iniziale dell’AEM.

## Materiali di supporto {#supporting-materials}

* [Scaricare i componenti core dell’AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Scarica We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installazione di Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
