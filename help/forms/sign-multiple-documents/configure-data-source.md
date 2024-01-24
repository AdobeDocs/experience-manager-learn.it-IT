---
title: Configurare l’origine dati AEM
description: Configura origine dati supportata MySQL per archiviare e recuperare i dati del modulo
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 47
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 3%

---

# Configurare l’origine dati

Esistono molti modi in cui l’AEM consente l’integrazione con un database esterno. Uno dei modi più comuni per integrare un database è utilizzare le proprietà di configurazione dell’origine dati in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e implementare la [Driver MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) nell&#39;AEM.
Crea un’origine dati in pool di connessione Apache Sling e fornisci le proprietà come specificato nella schermata seguente. Lo schema di database viene fornito come parte di queste risorse di esercitazione.

![data-source](assets/data-source.PNG)

Database dispone di una tabella denominata formdata con le tre colonne, come illustrato nella schermata seguente.

![database](assets/data-base.PNG)


>[!NOTE]
>Assegna un nome all’origine dati **emformstutoriale**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
| ------------------------|--------------------------------------- |
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Risorse

Il file SQL per creare lo schema può essere [scaricato da qui](assets/sign-multiple-forms.sql). Per creare lo schema e la tabella, è necessario importare il file utilizzando MySql Workbench.

## Passaggi successivi

[Creare un servizio OSGi per archiviare e recuperare dati nel database](./create-osgi-service.md)
