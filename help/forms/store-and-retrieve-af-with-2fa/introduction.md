---
title: Memorizzazione e recupero dei dati del modulo con allegati dal database MySQL
description: Tutorial in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati del modulo con allegati
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 148
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# Memorizzazione e recupero di dati di moduli adattivi con 2FA

Questo tutorial illustra i passaggi necessari per salvare e recuperare i dati del modulo adattivo con allegati utilizzando 2FA. Questa esercitazione utilizzava il database MySQL per memorizzare i dati di Moduli adattivi. Il database scelto può essere utilizzato per memorizzare i dati purché siano stati distribuiti driver specifici del database in AEM. A un livello avanzato, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* Utilizzare l’API GuideBridge per accedere ai dati del modulo adattivo

* Effettuare una chiamata POST a un servlet. Questo servlet memorizza i dati nel database e gli allegati del modulo nel repository di CRX. I dati memorizzati nel database sono associati a un GUID.

* Per compilare il modulo adattivo con i dati memorizzati, è necessario recuperare i dati associati al GUID e popolare il modulo adattivo utilizzando il metodo **request.setAttribute**.

## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Prerequisiti

Il pubblico di questo contenuto deve avere un’esperienza nelle seguenti aree:

* Modulo adattivo
* Modello dati modulo
* Servizi/componenti OSGi
* Librerie client di AEM


## Passaggi successivi

[Configurazione di Data Source](./configure-data-source.md)