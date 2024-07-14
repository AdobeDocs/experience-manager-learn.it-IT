---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL - Configurare Data Source
description: Tutorial in più parti per illustrare i passaggi necessari per l’archiviazione e il recupero dei dati del modulo
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 36
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 1%

---

# Configurare Data Source

Esistono molti modi in cui l’AEM consente l’integrazione con il database esterno. Una delle procedure più comuni e standard per l&#39;integrazione del database consiste nell&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i [driver MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati in AEM.
Crea un’origine dati in pool di connessione Apache Sling e fornisci le proprietà come specificato nella schermata seguente. Lo schema di database viene fornito come parte di queste risorse di esercitazione.

![origine dati](assets/save-continue.PNG)

Database dispone di una tabella denominata formdata con le tre colonne, come illustrato nella schermata seguente.

![database](assets/data-base-tables.PNG)

Il file SQL per creare lo schema può essere [scaricato da qui](assets/form-data-db.sql). Per creare lo schema e la tabella, è necessario importare il file utilizzando MySql Workbench.

>[!NOTE]
>Assicurati di denominare l&#39;origine dati **SaveAndContinue**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
| ------------------------|---------------------------------------|
| Nome origine dati | `SaveAndContinue` |
| Classe driver JDBC | `com.mysql.cj.jdbc.Driver` |
| URI connessione JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |
