---
title: Memorizzazione e recupero dei dati dei moduli dal database MySQL
description: Esercitazione in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
feature: Moduli adattivi
type: Tutorial
version: 6.3,6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---


# Memorizzazione e recupero dei dati dei moduli adattivi dal database MySQL

Questa esercitazione illustra i passaggi necessari per salvare e recuperare i dati del modulo adattivo dal database. Questa esercitazione utilizzava il database MySQL per memorizzare i dati del modulo adattivo. Il database desiderato può essere utilizzato per memorizzare i dati finché sono stati implementati i driver specifici del database in AEM. Ad alto livello, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* Utilizzare l’API GuideBridge per accedere ai dati del modulo adattivo

* Effettua una chiamata POST a un servlet. Questo servlet memorizza i dati nel database. I dati memorizzati sono associati a un GUID

* Per compilare il modulo adattivo con i dati memorizzati, è necessario recuperare i dati associati al GUID e compilare il modulo adattivo utilizzando il metodo **request.setAttribute**.

## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
