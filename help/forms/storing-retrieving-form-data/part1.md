---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL
description: Esercitazione con più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Configura origine dati

Esistono diversi modi con cui AEM l&#39;integrazione con il database esterno. Una delle pratiche più comuni e standard dell&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource pool di Apache Sling Connection tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i driver [](https://mvnrepository.com/artifact/mysql/mysql-connector-java) My SQL appropriati in AEM.
Quindi impostare le proprietà DataSource del pool di connessioni Sling. Queste proprietà sono specifiche del database. La schermata seguente mostra le impostazioni utilizzate per questa esercitazione. Lo schema del database viene fornito come parte di questa esercitazione.
![data-source](assets/data-source.png)

Nel database è presente una tabella denominata formdata con le 3 colonne come mostrato nella schermata sottostante![data-base](assets/data-base-tables.PNG)
