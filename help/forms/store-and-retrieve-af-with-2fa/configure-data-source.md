---
title: Configurare Data Source
description: Crea origine dati che punta al database MySQL
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 64
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 3%

---

# Configurare Data Source

AEM consente l’integrazione con un database esterno in diversi modi. Una delle procedure più comuni e standard per l&#39;integrazione del database è l&#39;utilizzo delle proprietà di configurazione DataSource in pool di connessione Apache Sling tramite [configMgr](http://localhost:4502/system/console/configMgr).
Il primo passaggio consiste nel scaricare e distribuire i [driver MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) appropriati in AEM.
Quindi, imposta le proprietà dell’origine dati in pool di connessione Sling specifiche per il database. La schermata seguente mostra le impostazioni utilizzate per questa esercitazione. Lo schema di database viene fornito come parte di queste risorse di esercitazione.

>[!NOTE]
>Assicurati di denominare l&#39;origine dati `StoreAndRetrieveAfData` in quanto è il nome utilizzato nel servizio OSGi.


![origine dati](assets/data-source.JPG)

| Nome proprietà | Valore proprietà |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Nome origine dati | `StoreAndRetrieveAfData` |   |
| Classe unità JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |   |
| URI connessione JDBC | `jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&autoReconnect=true` |   |
|                     |                                                                                    |   |


## Crea database


Ai fini di questo caso d’uso è stato utilizzato il seguente database. Il database include una tabella denominata `formdatawithattachments` con le 4 colonne, come illustrato nella schermata seguente.
![database](assets/table-schema.JPG)

* La colonna **afdata** conterrà i dati del modulo adattivo.
* La colonna **attachmentsInfo** conterrà le informazioni sugli allegati del modulo.
* Le colonne **numeroTelefono** conterranno il numero di cellulare della persona che compila il modulo.

Creare il database importando lo [schema di database](assets/data-base-schema.sql)
utilizzo di MySQL Workbench.

## Crea modello dati modulo

Crea il modello dati del modulo e basalo sull’origine dati creata nel passaggio precedente.
Configura il servizio **get** di questo modello dati modulo come mostrato nella schermata seguente.
Assicurarsi di non restituire un array nel servizio **get**.

Questo servizio **get** ha lo scopo di recuperare il numero di telefono associato all&#39;ID applicazione.

![get-service](assets/get-service.JPG)

Questo modello di dati modulo verrà quindi utilizzato in **ModuloAccountPersonale** per recuperare il numero di telefono associato all&#39;ID applicazione.

## Passaggi successivi

[Scrivi il codice per salvare gli allegati del modulo](./store-form-attachments.md)
