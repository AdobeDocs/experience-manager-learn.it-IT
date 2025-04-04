---
title: Configurare AEM Data Source
description: Configura origine dati supportata MySQL per archiviare e recuperare i dati del modulo
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 39
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 3%

---

# Configurare l’origine dati

AEM consente l’integrazione con un database esterno in diversi modi. Uno dei modi più comuni per integrare un database consiste nell&#39;utilizzare le proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i [driver MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati in AEM.
Crea un’origine dati in pool di connessione Apache Sling e fornisci le proprietà come specificato nella schermata seguente. Lo schema di database viene fornito come parte di queste risorse di esercitazione.

![origine dati](assets/data-source.PNG)

Database dispone di una tabella denominata formdata con le tre colonne, come illustrato nella schermata seguente.

![database](assets/data-base.PNG)


>[!NOTE]
>Assicurati di denominare l&#39;origine dati **aemformstutorial**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
| ------------------------|--------------------------------------- |
| Nome origine dati | `SaveAndContinue` |
| Classe driver JDBC | `com.mysql.cj.jdbc.Driver` |
| URI connessione JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |

## Risorse

Il file SQL per creare lo schema può essere [scaricato da qui](assets/sign-multiple-forms.sql). Per creare lo schema e la tabella, è necessario importare il file utilizzando MySql Workbench.

## Passaggi successivi

[Creare un servizio OSGi per archiviare e recuperare dati nel database](./create-osgi-service.md)
