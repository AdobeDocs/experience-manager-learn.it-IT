---
title: Scrivere il documento di payload nel file system
description: Passaggio del processo personalizzato per aggiungere al file system il documento di scrittura che si trova sotto la cartella payload
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Estrai nodo dal file xml dei dati inviati

Questo passaggio del processo personalizzato consiste nel creare un nuovo documento xml estraendo un nodo da un altro documento xml. Per generare pdf è necessario utilizzare questa opzione quando si desidera unire i dati inviati con il modello xdp. Ad esempio, quando si invia un modulo adattivo, i dati da unire al modello xdp si trovano all&#39;interno dell&#39;elemento dati. In questo caso è necessario creare un altro documento xml estraendo l’elemento dati appropriato.

La schermata seguente mostra gli argomenti che è necessario passare al passaggio del processo personalizzato
![fase del processo](assets/create-xml-process-step.png)
I seguenti parametri:
* Data.xml - Il file xml dal quale si desidera estrarre il nodo
* datatomerge.xml - Il nuovo xml creato con il nodo estratto
* /afData/afUnboundData/data - Il nodo da estrarre


La schermata seguente mostra il datamerge.xml creato nella cartella payload
![create-xml](assets/create-xml.png)

[Il bundle personalizzato può essere scaricato da qui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)