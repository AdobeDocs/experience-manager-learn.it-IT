---
title: Scrivi il documento di payload nel file system
description: Passaggio di processo personalizzato per aggiungere al file system un documento di scrittura che si trova nella cartella del payload
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Scrivere il documento nel file system

Un caso d’uso comune consiste nella scrittura nel file system dei documenti generati nel flusso di lavoro.
Questo passaggio del processo di flusso di lavoro personalizzato semplifica la scrittura dei documenti del flusso di lavoro nel file system.
Il processo personalizzato accetta i seguenti argomenti separati da virgole

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Il primo argomento è il nome del documento da salvare nel file system. Il secondo argomento è il percorso della cartella in cui si desidera salvare il documento. Ad esempio, nel caso d&#39;uso precedente il documento è scritto in `c:\confirmation\ChangeBeneficiary.pdf`

La schermata seguente mostra gli argomenti da passare al passaggio del processo personalizzato
![write-payload-file-system](assets/write-payload-file-system.png)

[Il bundle personalizzato può essere scaricato da qui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
