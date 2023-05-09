---
title: Configura origine dati
description: Crea origine dati puntando al database MySQL
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 3%

---

# Configura origine dati

Esistono diversi modi in cui AEM consente l’integrazione con un database esterno. Una delle pratiche più comuni e standard dell&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire il [Driver MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEM.
Quindi, imposta le proprietà DataSource in pool di connessione Sling specifiche del database. La schermata seguente mostra le impostazioni utilizzate per questa esercitazione. Lo schema di database viene fornito come parte di questa esercitazione risorse.

![sorgente dati](assets/data-source.JPG)


* Classe del driver JDBC: `com.mysql.cj.jdbc.Driver`
* URI di connessione JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Assicurati di assegnare un nome alla tua origine dati `StoreAndRetrieveAfData` poiché è il nome utilizzato nel servizio OSGi.


## Crea database


Per questo caso d&#39;uso è stato utilizzato il seguente database. Nel database è presente una tabella denominata `formdatawithattachments` con le 4 colonne come mostrato nella schermata seguente.
![base dati](assets/table-schema.JPG)

* La colonna **afdata** contiene i dati del modulo adattivo.
* La colonna **attachmentInfo** contiene informazioni sugli allegati del modulo.
* Le colonne **numeroTelefono** conterrà il numero di cellulare della persona che compila il modulo.

Crea il database importando il [schema di database](assets/data-base-schema.sql)
utilizzo di Workbench MySQL.

## Crea modello dati modulo

Creare il modello dati del modulo e basarlo sull’origine dati creata nel passaggio precedente.
Configura le **get** servizio di questo modello di dati del modulo come mostrato nella schermata seguente.
Assicurati di non restituire una matrice nel **get** servizio.

Scopo del presente **get** recupero del numero di telefono associato all&#39;ID applicazione.

![get-service](assets/get-service.JPG)

Questo modello di dati del modulo verrà quindi utilizzato nella **MyAccountForm** per recuperare il numero di telefono associato all&#39;ID applicazione.

## Passaggi successivi

[Scrivere codice per salvare gli allegati del modulo](./store-form-attachments.md)
