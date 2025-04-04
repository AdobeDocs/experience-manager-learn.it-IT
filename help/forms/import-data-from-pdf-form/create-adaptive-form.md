---
title: Crea schema
description: Creare uno schema basato sui dati da importare nel modulo adattivo
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# Introduzione

Il primo passaggio consiste nel creare uno schema basato sui dati che verranno utilizzati per compilare il modulo adattivo.

## XFA è basato su uno schema

Utilizza lo schema per creare il modulo adattivo

## XFA non è basato su uno schema

* Apri XDP in AEM Forms Designer.
* Fai clic su File | Proprietà modulo | Anteprima.
* Fai clic su Genera dati di anteprima.
* Fai clic su Genera.
* Specificare un nome di file significativo, ad esempio `form-data.xml`

Puoi utilizzare uno degli strumenti online gratuiti per [generare XSD](https://www.freeformatter.com/xsd-generator.html) dai dati xml generati nel passaggio precedente.

Crea un modulo adattivo basato sullo schema del passaggio precedente.

>[!NOTE]
>Si consiglia sempre di esaminare i dati generati all’invio del modulo adattivo. Questo ti darà una buona idea del formato XML dei dati che devono essere uniti al modulo adattivo.

Dati inviati da un modulo adattivo
![dati inviati](./assets/af-submitted-data.png)

Dati esportati da PDF
![dati-esportati](./assets/exported-data.png)

Dai dati esportati, dovrai estrarre il nodo **_topmostSubform_** con i namespace appropriati conservati per unire correttamente i dati con il modulo adattivo.

## Passaggi successivi

[Crea servizio OSGi](./create-osgi-service.md)
