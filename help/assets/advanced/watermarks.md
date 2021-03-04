---
title: Filigrane in AEM Assets
description: Le funzionalità di watermarking di AEM as a Cloud Service consentono alle rappresentazioni personalizzate delle immagini di essere filigrate utilizzando qualsiasi immagine PNG.
feature: Microservizi Asset Compute
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Gestione dei contenuti
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

---


# Filigrane

Le funzionalità di watermarking di AEM as a Cloud Service consentono alle rappresentazioni personalizzate delle immagini di essere filigrate utilizzando qualsiasi immagine PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configurazione OSGi

Il seguente stub di configurazione OSGi può essere aggiornato e aggiunto al sottoprogetto del progetto AEM `ui.config` .

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
