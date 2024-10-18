---
title: Inviare allegati modulo tramite posta elettronica
description: Estrarre e inviare gli allegati del modulo inviato in un messaggio di posta elettronica utilizzando il flusso di lavoro Power Automate
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="Cloud Service AEM Forms" before-title="false"
source-git-commit: ed64dd303a384d48f76c9b8e8e925f5d3b8f3247
workflow-type: tm+mt
source-wordcount: '76'
ht-degree: 0%

---

# Estrarre gli allegati dai dati modulo inviati

Estrarre gli allegati del modulo e inviarli tramite posta elettronica in un flusso di lavoro Power Automate.
Il video seguente illustra i passaggi necessari per creare allegati dai dati inviati.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

Di seguito è riportato lo schema dell’oggetto allegato da utilizzare nel passaggio Analizza schema JSON.

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
