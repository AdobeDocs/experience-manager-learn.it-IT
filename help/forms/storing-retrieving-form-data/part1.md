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
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 1%

---

# Configura origine dati

Esistono diversi modi con cui AEM l&#39;integrazione con il database esterno. Una delle procedure più comuni e standard per l&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessioni Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i driver [MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati in AEM.
Create Apache Sling Connection Pooled DataSource e fornite le proprietà come specificato nella schermata sottostante. Lo schema del database viene fornito come parte di questa esercitazione.

![data-source](assets/save-continue.PNG)

Nel database è presente una tabella denominata formdata con le 3 colonne come mostrato nella schermata sottostante.

![banca dati](assets/data-base-tables.PNG)

Il file sql per creare lo schema può essere [scaricato da qui](assets/form-data-db.sql). È necessario importare il file utilizzando WorkbenchSql per creare lo schema e la tabella.

>[!NOTE]
>Assicurarsi di assegnare un nome all&#39;origine dati **SaveAndContinue**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
------------------------|---------------------------------------
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


