---
title: Configurare la configurazione dei dati batch
description: Configurare la configurazione dei dati batch
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Creare la configurazione batch

Per utilizzare un’API batch, crea una configurazione batch ed esegui un’esecuzione in base a tale configurazione. Il video seguente mostra una dimostrazione della creazione della configurazione batch utilizzando l’API

>[!NOTE]
>Assicurati che l&#39;utente AEM appartenga a ```forms-users``` per effettuare chiamate API.


>[!VIDEO](https://video.tv.adobe.com/v/340241/?quality=12&learn=on)

## Crea configurazione batch

Di seguito è riportato l’endpoint POST per la creazione della configurazione batch

```xml
<baseURL>/config
```

Di seguito è riportata la configurazione minima da specificare durante la creazione della configurazione batch. Deve essere passato come oggetto JSON nel corpo della richiesta HTTP

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

## Verificare la configurazione batch

Per verificare la corretta creazione della configurazione batch, puoi effettuare una chiamata di richiesta GET al seguente endpoint


```xml
<baseURL>/config/monthlystatements
```

È sufficiente passare un oggetto JSON vuoto nel corpo della richiesta HTTP

