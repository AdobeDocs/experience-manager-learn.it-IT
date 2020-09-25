---
title: Creazione di tabelle di database
description: Creare un database da utilizzare per il modello dati del modulo
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# Crea tabelle di database

Il modello dati modulo può essere basato su origini RDBMS, RESTfull, SOAP o OData. Questo corso è incentrato sulla precompilazione del modulo adattivo tramite il modello dati del modulo supportato dall&#39;origine dati RDBMS. Per questa esercitazione è stato utilizzato il database MYSQL. Sono state create le due tabelle seguenti per illustrare il caso di utilizzo

* **nuova** tabella - Questa tabella memorizza le informazioni sul nuovo

   ![newhire](assets/newhire-table.png)


* **tabella dei beneficiari** - Questo memorizza i nuovi beneficiari

   ![Beneficiari](assets/beneficiaries-table.png)

È possibile importare il file [](assets/db-schema.sql) SQL utilizzando Workbench MySQL per creare tabelle con alcuni dati di esempio.