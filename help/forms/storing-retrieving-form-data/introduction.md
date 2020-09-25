---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL
description: Esercitazione con più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 0%

---


# Memorizzazione e recupero di dati modulo adattivo dal database MySQL

Questa esercitazione illustra i passaggi necessari per salvare e recuperare i dati del modulo adattivo dal database. Questa esercitazione ha utilizzato il database MySQL per memorizzare i dati del modulo adattivo. È possibile utilizzare il database di vostra scelta per memorizzare i dati finché i driver specifici del database sono stati distribuiti in AEM. A un livello elevato, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* Utilizzare l&#39;API GuideBridge per accedere ai dati del modulo adattivo

* Effettuare una chiamata POST a un servlet. Questo servlet memorizza i dati nel database. I dati memorizzati sono associati a un GUID

* Per compilare il modulo adattivo con i dati memorizzati, è necessario recuperare i dati associati al GUID e compilare il modulo adattivo utilizzando il metodo **request.setAttribute** .

## Dimostrazione del caso di utilizzo

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
