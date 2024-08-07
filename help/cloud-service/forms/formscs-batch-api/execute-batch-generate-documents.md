---
title: Eseguire la configurazione batch
description: Avviare il processo di generazione del documento eseguendo il batch
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Esegui configurazione batch

Per eseguire il batch, effettua una richiesta POST alla seguente API

```xml
<baseURL>/confi/<configName>/execution
```

Questa API prevede un oggetto json vuoto come parametro nel corpo della richiesta.
Questa API restituisce un URL univoco nell&#39;intestazione di risposta identificata dalla chiave **location**.
Una richiesta di GET a questo URL univoco ti dirà lo stato dell’esecuzione batch

Il video seguente illustra l’attivazione della configurazione batch

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
