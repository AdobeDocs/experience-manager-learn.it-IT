---
title: Estrarre un nodo dai dati XML inviati
description: Passaggio di processo personalizzato per aggiungere al file system un documento di scrittura che si trova nella cartella del payload
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 53
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Estrarre un nodo dai dati XML inviati

Questo passaggio del processo personalizzato consiste nel creare un nuovo documento XML estraendo un nodo da un altro documento XML. Dovrai utilizzarlo quando desideri unire i dati inviati con un modello xdp per generare un PDF. Ad esempio, quando invii un modulo adattivo, i dati da unire con il modello xdp si trovano all’interno dell’elemento dati. In questo caso, è necessario creare un altro documento xml estraendo l’elemento dati appropriato.

La schermata seguente mostra gli argomenti da passare al passaggio del processo personalizzato
![passaggio del processo](assets/create-xml-process-step.png)
Di seguito sono riportati i parametri
* Data.xml: il file xml da cui estrarre il nodo
* datatomerge.xml: il nuovo xml creato con il nodo estratto
* /afData/afUnboundData/data - Nodo da estrarre


La schermata seguente mostra il file datamerge.xml creato nella cartella del payload
![create-xml](assets/create-xml.png)

[Il bundle personalizzato può essere scaricato da qui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
