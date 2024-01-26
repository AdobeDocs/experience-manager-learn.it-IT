---
title: Introduzione all'archiviazione e al recupero di dati da database MySQL
description: Tutorial in più parti per illustrare i passaggi necessari per l’archiviazione e il recupero dei dati del modulo
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 244
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Memorizzazione e recupero di dati di moduli adattivi dal database MySQL

Questo tutorial illustra i passaggi necessari per salvare e recuperare i dati dei moduli adattivi dal database. Questa esercitazione utilizzava il database MySQL per memorizzare i dati di Moduli adattivi. Il database desiderato può essere utilizzato per memorizzare i dati, purché siano stati distribuiti driver specifici del database in AEM. A un livello avanzato, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* Utilizzare l’API GuideBridge per accedere ai dati del modulo adattivo

* Effettuare una chiamata POST a un servlet. Questo servlet memorizza i dati nel database. I dati archiviati sono associati a un GUID

* Per compilare il modulo adattivo con i dati memorizzati, è necessario recuperare i dati associati al GUID e popolare il modulo adattivo utilizzando **request.setAttribute** metodo.

## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


