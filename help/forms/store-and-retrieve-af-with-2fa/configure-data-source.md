---
title: Configura origine dati
description: Crea origine dati puntando al database MySQL
feature: Moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 2%

---


# Configura origine dati

AEM consente l’integrazione con database esterno in diversi modi. Una delle pratiche più comuni e standard dell&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passo è quello di scaricare e distribuire i [driver MySQL ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati ad AEM.
Quindi, imposta le proprietà DataSource in pool di connessione Sling specifiche del database. La schermata seguente mostra le impostazioni utilizzate per questa esercitazione. Lo schema di database viene fornito come parte di questa esercitazione risorse.

![sorgente dati](assets/data-source.JPG)


* Classe del driver JDBC: `com.mysql.cj.jdbc.Driver`
* URI di connessione JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Assicurati di denominare la tua origine dati `StoreAndRetrieveAfData` in quanto questo è il nome utilizzato nel servizio OSGi.


## Crea database


Per questo caso d&#39;uso è stato utilizzato il seguente database. Il database ha una tabella denominata `formdatawithattachments` con le 4 colonne come mostrato nella schermata sottostante.
![base dati](assets/table-schema.JPG)

* La colonna **afdata** conterrà i dati del modulo adattivo.
* La colonna **attachmentInfo** contiene le informazioni sugli allegati del modulo.
* Le colonne **phoneNumber** contengono il numero di cellulare della persona che compila il modulo.

Crea il database importando lo [schema del database](assets/data-base-schema.sql)
utilizzo di Workbench MySQL.

## Crea modello dati modulo

Crea il modello dati del modulo e lo basa sull’origine dati creata nel passaggio precedente.
Configura il servizio **get** di questo modello di dati del modulo come mostrato nella schermata sottostante.
Assicurati di non restituire array nel servizio **get**.

Questo servizio **get** viene utilizzato per recuperare il numero di telefono associato all&#39;ID applicazione.

![get-service](assets/get-service.JPG)

Questo modello di dati del modulo verrà quindi utilizzato in **MyAccountForm** per recuperare il numero di telefono associato all&#39;ID applicazione.
