---
title: Memorizzazione e recupero dei dati dei moduli con allegati dal database MySQL
description: Esercitazione in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli con gli allegati
feature: Moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 4%

---


# Memorizzazione e recupero dei dati dei moduli adattivi con 2FA

Questa esercitazione illustra i passaggi necessari per salvare e recuperare i dati dei moduli adattivi con gli allegati che utilizzano 2FA. Questa esercitazione utilizzava il database MySQL per memorizzare i dati del modulo adattivo. Il database desiderato può essere utilizzato per memorizzare i dati finché sono stati implementati i driver specifici del database in AEM. Ad alto livello, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* Utilizzare l’API GuideBridge per accedere ai dati del modulo adattivo

* Effettua una chiamata POST a un servlet. Questo servlet memorizza i dati nel database e gli allegati del modulo nell&#39;archivio CRX. I dati memorizzati nel database sono associati a un GUID.

* Per compilare il modulo adattivo con i dati memorizzati, è necessario recuperare i dati associati al GUID e compilare il modulo adattivo utilizzando il metodo **request.setAttribute**.

## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Prerequisiti

Il pubblico di questo contenuto deve avere esperienza nelle seguenti aree:

* Modulo adattivo
* Modello dati modulo
* Servizi/componenti OSGi
* Librerie client AEM
