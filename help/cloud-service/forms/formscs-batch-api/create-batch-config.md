---
title: Configurare la configurazione dei dati batch
description: Configurare la configurazione dei dati batch
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 233
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Crea configurazione batch

Per utilizzare un’API batch, crea una configurazione batch ed esegui un’esecuzione basata su tale configurazione. Il video seguente mostra una dimostrazione della creazione di una configurazione batch utilizzando l’API

>[!NOTE]
>Assicurarsi che l&#39;utente AEM appartenga al gruppo ```forms-users``` per effettuare chiamate API.


>[!VIDEO](https://video.tv.adobe.com/v/343700?quality=12&learn=on&captions=ita)

## Crea configurazione batch

Di seguito è riportato l&#39;endpoint POST per la creazione della configurazione batch.

```xml
<baseURL>/config
```

Di seguito è riportata la configurazione minima da specificare durante la creazione della configurazione batch. Questo deve essere passato come oggetto JSON nel corpo della richiesta HTTP

```
{
    "configName": "monthlystatements",
    "dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
    "outputTypes": [
        "PDF"
    ],
    "template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## Verifica configurazione batch

Per verificare la corretta creazione della configurazione batch, puoi effettuare una chiamata di richiesta GET al seguente endpoint


```xml
<baseURL>/config/monthlystatements
```

Devi solo trasmettere un oggetto JSON vuoto nel corpo della richiesta HTTP
