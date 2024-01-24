---
title: Crea componente kit di benvenuto
description: Crea una pagina dei siti AEM con collegamenti per scaricare risorse in base ai dati del modulo inviato.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 66496f0e-c121-4b6d-b371-084393ece3ca
duration: 23
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '74'
ht-degree: 0%

---

# Componente kit di benvenuto

È stato creato un componente pagina per elencare le risorse della pagina che possono essere scaricate dall’utente finale. I percorsi delle singole risorse vengono salvati in una proprietà denominata **percorsi**. I dati del modulo inviati determinano le risorse da includere.

Il codice che segue riporta le risorse presenti nella pagina:

```html
   <p class="cmp-press-kit__press-kit-size">
        Welcome kit contains ${pressKit.assets.size} assets.
    </p>
<ul class="cmp-press-kit__asset-list" data-sly-list.asset="${pressKit.assets}">
    <li class="cmp-press-kit__asset-item">
        <div class="cmp-press-kit__asset " >
            <div class="cmp-press-kit__asset-content">
                <img class="cmp-press-kit__asset-image" src="${asset.path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png" alt="${asset.name}"/>
                <p class="cmp-press-kit__asset-title">${asset.title}</p>
            </div>
            <div class="cmp-press-kit__asset-actions">
                <a class="cmp-press-kit__asset-download-button" href="${asset.path}">Download</a>
            </div>
        </div>
    </li>
</ul>
</sly>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!ready}"></sly>
```
