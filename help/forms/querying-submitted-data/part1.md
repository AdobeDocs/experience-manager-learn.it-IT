---
title: AEM Forms con schema JSON e dati[Parte 1]
description: Tutorial in più parti per illustrare i passaggi necessari per creare un modulo adattivo con schema JSON e interrogare i dati inviati.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 70
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Creare un modulo adattivo basato sullo schema JSON


Con la versione 6.3 di AEM Forms è stata introdotta la possibilità di creare Forms adattivo basato sullo schema JSON. I dettagli sulla creazione di Forms adattivo con schema JSON sono illustrati in dettaglio in questo [articolo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

Dopo aver creato un modulo adattivo basato sullo schema JSON, il passaggio successivo consiste nell’archiviare i dati inviati nel database. A questo scopo utilizzeremo il nuovo tipo di dati JSON introdotto da vari fornitori di database. Ai fini del presente articolo utilizzeremo il database MySql 8 per memorizzare i dati inviati.

Database MySql 8 utilizzato per questo articolo. MySQL ha introdotto un nuovo tipo di dati denominato [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). In questo modo è più semplice archiviare ed eseguire query sugli oggetti JSON. I dati inviati vengono memorizzati in una colonna di tipo JSON nel database.

La schermata seguente mostra i dati del modulo inviati memorizzati nel tipo di dati JSON. La colonna &quot;formdata&quot; è di tipo JSON. Il nome del modulo associato ai dati è stato inoltre memorizzato nella colonna nomemodulo

>[!NOTE]
>
>Assicurati che il file dello schema JSON sia denominato in modo appropriato. Ad esempio, deve essere denominato nel seguente formato &lt;name>schema.json. Pertanto, il file dello schema può essere mortgage.schema.json o credit.schema.json.


![datastore](assets/datastored.gif)


[Schemi JSON di esempio che possono essere utilizzati per creare Forms adattivo.](assets/samplejsonschemas.zip). Scarica e decomprimi il file zip per ottenere gli schemi JSON
