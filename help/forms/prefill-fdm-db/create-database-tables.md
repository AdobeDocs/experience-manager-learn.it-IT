---
title: Creazione di tabelle di database
description: Crea database da utilizzare per il modello dati del modulo
feature: moduli adattivi
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---


# Creare tabelle di database

Il modello dati del modulo può essere basato su origini RDBMS, RESTfull, SOAP o OData. Questo corso si concentra sulla pre-archiviazione di Moduli adattivi utilizzando il modello dati del modulo supportato dall&#39;origine dati RDBMS. Ai fini di questa esercitazione è stato utilizzato il database MYSQL. Abbiamo creato le due tabelle seguenti per illustrare il caso d’uso

* **** newhiretable - Questa tabella memorizza le informazioni necessarie

   ![newhire](assets/newhire-table.png)


* **** fruibile - Consente di archiviare i nuovi beneficiari

   ![beneficiari](assets/beneficiaries-table.png)

È possibile importare il file [sql](assets/db-schema.sql) utilizzando MySQL workbench per creare tabelle con alcuni dati di esempio.