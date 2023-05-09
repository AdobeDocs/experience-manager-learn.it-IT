---
title: Configura origine dati AEM
description: Configurare l'origine dati con backup MySQL per memorizzare e recuperare i dati del modulo
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 5%

---

# Configurare l’origine dati

Esistono diversi modi con cui AEM l’integrazione con un database esterno. Uno dei modi più comuni per integrare un database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite le [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire il [Driver MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Crea un’origine dati in pool di connessione Apache Sling e fornisci le proprietà specificate nella schermata sottostante. Lo schema di database viene fornito come parte di questa esercitazione risorse.

![sorgente dati](assets/data-source.PNG)

Nel database è presente una tabella denominata formdata con le 3 colonne mostrate nella schermata sottostante.

![base dati](assets/data-base.PNG)


>[!NOTE]
>Assicurati di assegnare un nome alla tua origine dati **aemformstutorial**. Il codice di esempio utilizza il nome per connettersi al database.

| Nome proprietà | Valore |
| ------------------------|--------------------------------------- |
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Risorse

Il file SQL per creare lo schema può essere [scaricato da qui](assets/sign-multiple-forms.sql). Per creare lo schema e la tabella, è necessario importare il file utilizzando Workbench di MySql.

## Passaggi successivi

[Crea un servizio OSGi per memorizzare e recuperare i dati nel database](./create-osgi-service.md)
