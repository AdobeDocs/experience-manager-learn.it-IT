---
title: Configura origine dati
description: Crea origine dati puntando al database MySQL
feature: Moduli adattivi
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 2%

---


# Configura origine dati

Esistono diversi modi con cui AEM abilitare l’integrazione con il database esterno. Una delle pratiche più comuni e standard dell&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i [driver MySQL ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati da AEM.
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
