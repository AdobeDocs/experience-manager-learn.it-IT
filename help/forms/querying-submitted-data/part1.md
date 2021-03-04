---
title: Moduli AEM con schema e dati JSON[Parte 1]
seo-title: AEM Forms con schema JSON e dati[Parte1]
description: Esercitazione in più parti per illustrarti i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
seo-description: Esercitazione in più parti per illustrarti i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
feature: Moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 1%

---


# Creare un modulo adattivo basato su uno schema JSON


La possibilità di creare moduli adattivi basati sullo schema JSON è stata introdotta con la versione 6.3 di AEM Forms. I dettagli sulla creazione di moduli adattivi con schema JSON sono descritti in dettaglio in questo [articolo](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html).

Una volta creato un modulo adattivo basato sullo schema JSON, il passaggio successivo consiste nell’archiviare i dati inviati nel database. A questo scopo utilizzeremo il nuovo tipo di dati JSON introdotto da vari fornitori di database. Ai fini di questo articolo utilizzeremo il database MySql 8 per memorizzare i dati inviati.

Database MySql 8 utilizzato per questo articolo. MySQL ha introdotto un nuovo tipo di dati denominato [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Questo semplifica l’archiviazione e la query degli oggetti JSON. I dati inviati verranno memorizzati in una colonna di tipo JSON nel nostro database.

La schermata seguente mostra i dati del modulo inviati memorizzati nel tipo di dati JSON. La colonna &quot;formdata&quot; è di tipo JSON. Abbiamo inoltre memorizzato il nome del modulo associato ai dati nel nome del modulo della colonna

>[!NOTE]
>
>Assicurati che il file di schema json sia denominato in modo appropriato. Ad esempio, deve essere denominato nel seguente formato &lt;name>schema.json. Quindi il file di schema può essere mutuo.schema.json o credit.schema.json.


![datastore](assets/datastored.gif)


[Schemi JSON di esempio che possono essere utilizzati per creare moduli adattivi.](assets/samplejsonschemas.zip). Scarica e decomprimi il file zip per ottenere gli schemi JSON

