---
title: Utilizzo della condivisione di social media in AEM Sites
description: Esplora la configurazione e l'utilizzo del componente Condivisione social media.
feature: Componenti core
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: Gestione dei contenuti
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 8%

---


# Utilizzo della condivisione social media {#using-social-media-sharing-in-aem-sites}

Esplora la configurazione e l&#39;utilizzo del componente Condivisione social media.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Questo video esplora le seguenti funzionalità del componente Condivisione social media (parte di [AEM Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)) utilizzando il sito Web di esempio [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) .

* 0:00 - Aggiunta e configurazione del componente Condivisione social media
* 1:00 - Condivisione su Facebook
* 3:10 - Condivisione su Pinterest
* 6:25 - Utilizzo del componente Condivisione social media in una pagina del prodotto

## Configurazione dell&#39;esternalizzatore {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) esternalizzatore deve essere impostato sia su AEM Author che su AEM Publish, per mappare la modalità di pubblicazione runmode al dominio accessibile al pubblico utilizzato per accedere a AEM Publish.

In questo video utilizziamo `/etc/hosts` per eseguire lo spoof *www.example.com* per risolvere il problema localhost e utilizziamo una [configurazione di base AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) per consentire a www.example.com di affrontare AEM Publish.

## Materiali di supporto {#supporting-materials}

* [Download dei componenti core AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Scarica We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installazione di Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
