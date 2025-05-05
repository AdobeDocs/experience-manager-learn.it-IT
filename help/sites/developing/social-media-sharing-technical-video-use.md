---
title: Utilizzo della condivisione sui social media in AEM Sites
description: Esplora la configurazione e l’utilizzo del componente Condivisione social media.
feature: Core Components
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 2%

---

# Utilizzo della condivisione sui social media {#using-social-media-sharing-in-aem-sites}

Esplora la configurazione e l’utilizzo del componente Condivisione social media.

>[!VIDEO](https://video.tv.adobe.com/v/329908?quality=12&learn=on&captions=ita)

Questo video illustra le seguenti funzionalità del componente Condivisione social media (parte di [Componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)) tramite il sito Web di esempio [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - Aggiunta e configurazione del componente Condivisione social media
* 1:00 - Condivisione con Facebook
* 3:10 - Condivisione con Pinterest
* 6:25 - Utilizzo del componente Condivisione social media in una pagina di prodotto

## Configurazione esternalizzazione {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[L&#39;esternalizzatore di AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/externalizer.html) deve essere impostato sia su AEM Author che su AEM Publish, per mappare la modalità di esecuzione di pubblicazione al dominio accessibile al pubblico utilizzato per accedere a AEM Publish.

In questo video, `/etc/hosts` viene utilizzato per falsificare *www.example.com* per risolvere in localhost e viene utilizzata una [configurazione di base di AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=it) per consentire a www.example.com di avviare AEM Publish.

## Materiali di supporto {#supporting-materials}

* [Scarica i componenti core di AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Scarica We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installazione di Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=it)
