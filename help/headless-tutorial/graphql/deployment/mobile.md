---
title: Implementazioni mobili headless AEM
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# Implementazioni mobili headless AEM

Le implementazioni mobili headless AEM sono app mobili native per iOS, Android, ecc. che consumano e interagiscono con i contenuti in AEM in modo headless.

Le implementazioni per dispositivi mobili richiedono una configurazione minima, in quanto le connessioni HTTP a AEM API headless non vengono avviate nel contesto di un browser.

## Configurazioni di distribuzione

| L’app mobile si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Filtri del Dispatcher](./dispatcher-fitlers.md) | ✘ | ↓ | ↓ |
| [Configurazione CORS](./cors.md) | ✘ | ✘ | ✘ |
| Nome host URL immagine | ↓ | ↓ | ↓ |

## Esempio di app per dispositivi mobili

Adobe fornisce esempi di app mobili iOS e Android.


