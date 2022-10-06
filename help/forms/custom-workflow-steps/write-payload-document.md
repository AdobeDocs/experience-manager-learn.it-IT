---
title: Scrivere il documento di payload nel file system
description: Passaggio del processo personalizzato per aggiungere al file system il documento di scrittura che si trova sotto la cartella payload
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Scrivere il documento nel file system

Un caso d’uso comune è quello di scrivere nel file system i documenti generati nel flusso di lavoro.
Questa fase del processo del flusso di lavoro personalizzato semplifica la scrittura dei documenti del flusso di lavoro nel file system.
Il processo personalizzato accetta i seguenti argomenti separati da virgole

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Il primo argomento è il nome del documento che si desidera salvare nel file system. Il secondo argomento è il percorso della cartella in cui si desidera salvare il documento. Ad esempio, nel caso d’uso precedente, il documento è scritto in `c:\confirmation\ChangeBeneficiary.pdf`

La schermata seguente mostra gli argomenti che è necessario passare al passaggio del processo personalizzato
![write-payload-file-system](assets/write-payload-file-system.png)

[Il bundle personalizzato può essere scaricato da qui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
