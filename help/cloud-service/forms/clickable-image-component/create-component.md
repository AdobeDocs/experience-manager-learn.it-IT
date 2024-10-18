---
title: Creazione del componente immagine cliccabile
description: Creazione del componente immagine cliccabile in AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
exl-id: b635f171-775d-480e-bf7a-c92ab4af0aee
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 1%

---

# Crea componente

Questo articolo presuppone che tu abbia esperienza nello sviluppo per AEM Forms CS. Si presume inoltre che sia stato creato un progetto di archetipo AEM Forms.

Apri il progetto AEM Forms in IntelliJ o in un altro IDE a tua scelta. Crea un nuovo nodo denominato svg in

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent`` è l&#39;appId fornito durante la creazione del progetto Maven. Questo appId potrebbe essere diverso nel tuo ambiente.


## Crea file .content.xml

Crea un file denominato .content.xml sotto il nodo svg. Aggiungi il seguente contenuto al file appena creato. È possibile modificare le proprietà jcr:description, jcr:title e componentGroup in base alle proprie esigenze.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## Creare svg.html

Crea un file denominato svg.html. Il file eseguirà il rendering del SVG della mappa USA.Copiare il contenuto di [svg.html](assets/svg.html) nel file appena creato. Quello che hai copiato è la mappa SVG di USA. Salva il file.

## Distribuire il progetto

Distribuisci il progetto nell’istanza cloud locale pronta per testare il componente.

Per distribuire il progetto, è necessario passare alla cartella principale del progetto nella finestra del prompt dei comandi ed eseguire il comando seguente.

```
mvn clean install -PautoInstallSinglePackage
```

Il progetto verrà implementato nell’istanza AEM Forms locale e il componente sarà disponibile per essere incluso nel modulo adattivo

![mappa-usa](./assets/usa-map.png)
