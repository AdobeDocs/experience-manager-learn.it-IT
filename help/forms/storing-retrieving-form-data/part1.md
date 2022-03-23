---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL - Configura origine dati
description: Esercitazione in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
version: 6.3,6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 1%

---

# Configura origine dati

Esistono diversi modi con cui AEM abilitare l’integrazione con il database esterno. Una delle pratiche più comuni e standard dell&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire il [Driver MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Crea un’origine dati in pool di connessione Apache Sling e fornisci le proprietà specificate nella schermata sottostante. Lo schema di database viene fornito come parte di questa esercitazione risorse.

![sorgente dati](assets/save-continue.PNG)

Nel database è presente una tabella denominata formdata con le 3 colonne mostrate nella schermata sottostante.

![base dati](assets/data-base-tables.PNG)

Il file sql per creare lo schema può essere [scaricato da qui](assets/form-data-db.sql). Per creare lo schema e la tabella, è necessario importare il file utilizzando Workbench di MySql.

>[!NOTE]
>Assicurati di assegnare un nome alla tua origine dati **SaveAndContinue**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
| ------------------------|---------------------------------------|
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |
