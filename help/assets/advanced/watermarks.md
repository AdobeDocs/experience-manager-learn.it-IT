---
title: Filigrane in AEM Assets
description: La funzione di watermarking AEM del Cloud Service consente di applicare alle rappresentazioni personalizzate delle immagini una filigrana utilizzando qualsiasi immagine PNG.
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---

# Filigrane

La funzione di watermarking AEM del Cloud Service consente di applicare alle rappresentazioni personalizzate delle immagini una filigrana utilizzando qualsiasi immagine PNG.

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
