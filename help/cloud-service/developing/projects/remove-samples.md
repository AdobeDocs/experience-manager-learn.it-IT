---
title: Rimozione di campioni da un progetto AEM Maven
description: Scopri come pulire e rimuovere il codice di esempio da un progetto AEM generato dall’archetipo del progetto AEM.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 3%

---

# Rimozione di campioni da un progetto AEM Maven

Scopri come pulire e rimuovere il codice di esempio generato da un progetto AEM generato dall’archetipo del progetto AEM.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Riferimenti

+ [Archetipo progetto AEM Maven](https://github.com/adobe/aem-project-archetype)

## Comandi

Per rimuovere i file di esempio generati dal progetto Maven AEM, è possibile eseguire i seguenti comandi:

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

Rimuovi il `<div class="helloworld" ...></div>` da:

```
ui.frontend/src/main/webpack/static/index.html
```

Rimuovi il `<helloworld>` definizione dell’istanza del componente da:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
