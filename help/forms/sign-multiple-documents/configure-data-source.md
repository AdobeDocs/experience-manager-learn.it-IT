---
title: Configura origine dati AEM
description: Configurare l'origine dati con backup MySQL per memorizzare e recuperare i dati del modulo
feature: Moduli adattivi
topic: Sviluppo
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 3%

---

# Configurare l’origine dati

Esistono diversi modi con cui AEM l’integrazione con un database esterno. Uno dei modi più comuni per integrare un database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i [driver MySql ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati in AEM.
Crea un’origine dati in pool di connessione Apache Sling e fornisci le proprietà specificate nella schermata sottostante. Lo schema di database viene fornito come parte di questa esercitazione risorse.

![sorgente dati](assets/data-source.PNG)

Nel database è presente una tabella denominata formdata con le 3 colonne mostrate nella schermata sottostante.

![base dati](assets/data-base.PNG)


>[!NOTE]
>Assicurati di denominare la tua origine dati **aemformstutorial**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
| ------------------------|--------------------------------------- |
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Il file SQL per creare lo schema può essere [scaricato da qui](assets/sign-multiple-forms.sql). Per creare lo schema e la tabella, è necessario importare il file utilizzando Workbench di MySql.
