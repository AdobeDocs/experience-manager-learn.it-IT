---
title: Filigrane in  AEM Assets
description: AEM come funzionalità di Cloud Service  filigrana, le rappresentazioni personalizzate delle immagini possono essere filigrane utilizzando qualsiasi immagine PNG.
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# Filigrane

AEM come funzionalità di Cloud Service  filigrana, le rappresentazioni personalizzate delle immagini possono essere filigrane utilizzando qualsiasi immagine PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configurazione OSGi

Il seguente stub di configurazione OSGi può essere aggiornato e aggiunto al progetto secondario `ui.config` del progetto AEM.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
