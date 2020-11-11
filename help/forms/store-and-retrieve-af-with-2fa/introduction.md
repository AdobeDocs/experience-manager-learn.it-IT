---
title: Memorizzazione e recupero dei dati del modulo con allegati dal database MySQL
description: Esercitazione con più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli con gli allegati
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---


# Memorizzazione e recupero di dati per moduli adattivi con 2FA

Questa esercitazione illustra i passaggi necessari per salvare e recuperare i dati dei moduli adattivi con gli allegati che utilizzano 2FA. Questa esercitazione ha utilizzato il database MySQL per memorizzare i dati del modulo adattivo. È possibile utilizzare il database di vostra scelta per memorizzare i dati finché i driver specifici del database sono stati distribuiti in AEM. A un livello elevato, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* Utilizzare l&#39;API GuideBridge per accedere ai dati del modulo adattivo

* Effettuare una chiamata POST a un servlet. Questo servlet memorizza i dati nel database e gli allegati del modulo nell&#39;archivio CRX. I dati memorizzati nel database sono associati a un GUID.

* Per compilare il modulo adattivo con i dati memorizzati, è necessario recuperare i dati associati al GUID e compilare il modulo adattivo utilizzando il metodo **request.setAttribute** .

## Dimostrazione del caso di utilizzo

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Prerequisiti

L&#39;audience di questo contenuto deve avere qualche esperienza nelle seguenti aree:

* Modulo adattivo
* Modello dati modulo
* Servizi/componenti OSGi
* AEM librerie client
