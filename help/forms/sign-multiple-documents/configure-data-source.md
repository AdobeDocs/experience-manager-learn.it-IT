---
title: Configura origine dati AEM
description: Configurare l'origine dati basata su MySQL per la memorizzazione e il recupero dei dati del modulo
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 2%

---

# Configurare l&#39;origine dati

Esistono diversi modi per AEM consentire l&#39;integrazione con un database esterno. Uno dei modi più comuni per integrare un database è l&#39;utilizzo delle proprietà di configurazione DataSource pool di connessioni Apache Sling attraverso la [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i driver [MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati in AEM.
Create Apache Sling Connection Pooled DataSource e fornite le proprietà come specificato nella schermata sottostante. Lo schema del database viene fornito come parte di questa esercitazione.

![data-source](assets/data-source.PNG)

Nel database è presente una tabella denominata formdata con le 3 colonne come mostrato nella schermata sottostante.

![banca dati](assets/data-base.PNG)


>[!NOTE]
>Assicurarsi di assegnare un nome all&#39;origine dati **aemformstutorial**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
------------------------|---------------------------------------
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Il file sql per creare lo schema può essere [scaricato da qui](assets/sign-multiple-forms.sql). È necessario importare il file utilizzando WorkbenchSql per creare lo schema e la tabella.


