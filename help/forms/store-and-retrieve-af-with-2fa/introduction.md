---
title: Memorizzazione e recupero dei dati dei moduli con allegati dal database MySQL
description: Esercitazione in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli con gli allegati
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 1%

---

# Memorizzazione e recupero dei dati dei moduli adattivi con 2FA

Questa esercitazione illustra i passaggi necessari per salvare e recuperare i dati dei moduli adattivi con gli allegati che utilizzano 2FA. Questa esercitazione utilizzava il database MySQL per memorizzare i dati del modulo adattivo. Il database desiderato può essere utilizzato per memorizzare i dati finché sono stati implementati i driver specifici del database in AEM. Ad alto livello, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* Utilizzare l’API GuideBridge per accedere ai dati del modulo adattivo

* Effettua una chiamata POST a un servlet. Questo servlet memorizza i dati nel database e gli allegati del modulo nell&#39;archivio CRX. I dati memorizzati nel database sono associati a un GUID.

* Per compilare il modulo adattivo con i dati memorizzati, è necessario recuperare i dati associati al GUID e compilare il modulo adattivo utilizzando la variabile **request.setAttribute** metodo .

## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Prerequisiti

Il pubblico di questo contenuto deve avere esperienza nelle seguenti aree:

* Modulo adattivo
* Modello dati modulo
* Servizi/componenti OSGi
* Librerie client AEM
