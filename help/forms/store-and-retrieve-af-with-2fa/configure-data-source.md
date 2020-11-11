---
title: Configura origine dati
description: Create DataSource puntando al database MySQL
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---


# Configura origine dati

Esistono diversi modi con cui AEM l&#39;integrazione con il database esterno. Una delle pratiche più comuni e standard dell&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource pool di Apache Sling Connection tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i driver [](https://mvnrepository.com/artifact/mysql/mysql-connector-java) MySQL appropriati da AEM.
Quindi impostare le proprietà DataSource del pool di connessioni Sling specifiche per il database. La schermata seguente mostra le impostazioni utilizzate per questa esercitazione. Lo schema del database viene fornito come parte di questa esercitazione.

![data-source](assets/data-source.JPG)


* Classe driver JDBC: `com.mysql.cj.jdbc.Driver`
* URI connessione JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Assicurarsi di assegnare un nome all&#39;origine dati `StoreAndRetrieveAfData` in quanto questo è il nome utilizzato nel servizio OSGi.


## Crea database


Il seguente database è stato utilizzato per questo caso d&#39;uso. Il database ha una tabella chiamata `formdatawithattachments` con le 4 colonne come mostrato nella schermata sottostante.
![banca dati](assets/table-schema.JPG)

* I dati **della colonna** contengono i dati del modulo adattivo.
* La colonna **attachmentsInfo** contiene le informazioni sugli allegati del modulo.
* Le colonne **phoneNumber** contengono il numero di cellulare della persona che compila il modulo.

Creare il database importando lo schema [del](assets/data-base-schema.sql)database tramite WorkbenchSQL.

## Crea modello dati modulo

Creare un modello dati modulo e basarlo sull&#39;origine dati creata nel passaggio precedente.
Configurare il servizio **get** di questo modello di dati del modulo come mostrato nella schermata sottostante.
Assicurarsi di non restituire array nel servizio **get** .

Questo servizio **get** viene utilizzato per recuperare il numero di telefono associato all&#39;ID applicazione.

![get-service](assets/get-service.JPG)

Questo modello di dati del modulo verrà quindi utilizzato in **MyAccountForm** per recuperare il numero di telefono associato all&#39;ID applicazione.
