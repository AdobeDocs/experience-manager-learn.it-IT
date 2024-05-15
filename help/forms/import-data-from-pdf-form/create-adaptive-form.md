---
title: Crea schema
description: Creare uno schema basato sui dati da importare nel modulo adattivo
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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
* Fornisci un nome file significativo, ad esempio `form-data.xml`

È possibile utilizzare uno qualsiasi degli strumenti online gratuiti per [genera XSD](https://www.freeformatter.com/xsd-generator.html) dai dati xml generati nel passaggio precedente.

Crea un modulo adattivo basato sullo schema del passaggio precedente.

>[!NOTE]
>Si consiglia sempre di esaminare i dati generati all’invio del modulo adattivo. Questo ti darà una buona idea del formato XML dei dati che devono essere uniti al modulo adattivo.

Dati inviati da un modulo adattivo
![submit-data](./assets/af-submitted-data.png)

Dati esportati dal PDF
![export-data](./assets/exported-data.png)

Estrarre i dati esportati dai **_topSubform_** viene mantenuto un nodo con i namespace appropriati per unire correttamente i dati con il modulo adattivo.

## Passaggi successivi

[Crea servizio OSGi](./create-osgi-service.md)
