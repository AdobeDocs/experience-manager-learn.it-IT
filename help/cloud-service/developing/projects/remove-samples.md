---
title: Rimozione di campioni da un progetto Maven AEM
description: Scopri come pulire e rimuovere il codice di esempio da un progetto AEM generato dall’Archetipo di progetto AEM.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
source-git-commit: e8b3bcaeee40b4bfd4f967f929ad664e8d168cb0
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 6%

---


# Rimozione di campioni da un progetto Maven AEM

Scopri come pulire e rimuovere il codice di esempio generato da un progetto AEM generato da AEM Project Archetype.

>[!VIDEO](https://video.tv.adobe.com/v/337263/?quality=12&learn=on)


## Riferimenti

+ [Archetipo AEM progetto Maven](https://github.com/adobe/aem-project-archetype)

## Comandi

Per rimuovere i file di esempio generati dal progetto Maven AEM è possibile eseguire i seguenti comandi:

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## Modifiche

Rimuovi `<div class="helloworld" ...></div>` da:

```
ui.frontend/src/main/webpack/static/index.html
```

Rimuovi `<helloworld>` definizione dell&#39;istanza del componente da:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```