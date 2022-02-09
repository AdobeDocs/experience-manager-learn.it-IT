---
title: Esegui la configurazione batch
description: Avviare il processo di generazione del documento eseguendo il batch
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Esegui configurazione batch

Per eseguire il batch, effettua una richiesta POST alla seguente API

```xml
<baseURL>/confi/<configName>/execution
```

Questa API richiede un oggetto json vuoto come parametro nel corpo della richiesta.
Questa API restituisce un URL univoco nell’intestazione della risposta identificata da **posizione** chiave.
Una richiesta di GET a questo URL univoco ti dirà lo stato dell’esecuzione batch

Il video seguente illustra l’attivazione della configurazione batch

>[!VIDEO](https://video.tv.adobe.com/v/340242/?quality=12&learn=on)
