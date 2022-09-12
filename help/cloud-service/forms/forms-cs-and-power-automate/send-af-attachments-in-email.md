---
title: Inviare allegati modulo in un messaggio di posta elettronica
description: Estrarre e inviare gli allegati dei moduli inviati in un messaggio di posta elettronica utilizzando il flusso di lavoro automatizzato dell'alimentazione
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: aea43a705b3959f8be26238d32b816b3953e153e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Estrarre gli allegati del modulo dai dati del modulo inviati

Estrarre gli allegati del modulo e inviare gli allegati in un messaggio di posta elettronica in Power automatizza il flusso di lavoro.
Il video seguente illustra i passaggi necessari per la creazione di allegati dai dati inviati.
>[!VIDEO](https://video.tv.adobe.com/v/3409017/?quality=12&learn=on)

Di seguito è riportato lo schema dell&#39;oggetto allegato da utilizzare nel passaggio dello schema Parse JSON

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
