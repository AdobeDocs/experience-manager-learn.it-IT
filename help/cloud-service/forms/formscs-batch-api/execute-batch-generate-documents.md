---
title: Eseguire la configurazione batch
description: Avviare il processo di generazione del documento eseguendo il batch
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# Esegui configurazione batch

Per eseguire il batch, effettua una richiesta POST all’API seguente

```xml
<baseURL>/confi/<configName>/execution
```

Questa API prevede un oggetto json vuoto come parametro nel corpo della richiesta.
Questa API restituisce un URL univoco nell&#39;intestazione di risposta identificata dalla chiave **location**.
Una richiesta GET a questo URL univoco ti dirà lo stato dell’esecuzione batch

Il video seguente illustra l’attivazione della configurazione batch

>[!VIDEO](https://video.tv.adobe.com/v/343707?quality=12&learn=on&captions=ita)
