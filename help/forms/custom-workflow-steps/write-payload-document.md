---
title: Scrivere il documento di payload nel file system
description: Passaggio del processo personalizzato per aggiungere al file system il documento di scrittura che si trova sotto la cartella payload
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Scrivere il documento nel file system

Un caso d’uso comune è quello di scrivere nel file system i documenti generati nel flusso di lavoro.
Questa fase del processo del flusso di lavoro personalizzato semplifica la scrittura dei documenti del flusso di lavoro nel file system.
Il processo personalizzato accetta i seguenti argomenti separati da virgole

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Il primo argomento è il nome del documento che si desidera salvare nel file system. Il secondo argomento è il percorso della cartella in cui si desidera salvare il documento. Ad esempio, nel caso d’uso precedente, il documento verrà scritto su c:\confirmation\ChangeBeneficiary.pdf

La schermata seguente mostra gli argomenti che è necessario passare al passaggio del processo personalizzato
![write-payload-file-system](assets/write-payload-file-system.png)

[Il bundle personalizzato può essere scaricato da qui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)