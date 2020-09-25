---
title: ' AEM Forms con schema e dati JSON[Parte 1]'
seo-title: ' AEM Forms con schema e dati JSON[Part1]'
description: Esercitazione con più parti per illustrare i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
seo-description: Esercitazione con più parti per illustrare i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Creare un modulo adattivo basato sullo schema JSON


La possibilità di creare un Forms adattivo basato sullo schema JSON è stata introdotta con  versione AEM Forms 6.3. I dettagli sulla creazione di Forms adattivo con lo schema JSON sono illustrati in dettaglio in questo [articolo](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html).

Dopo aver creato un modulo adattivo basato sullo schema JSON, il passaggio successivo consiste nell&#39;archiviare i dati inviati nel database. A questo scopo utilizzeremo il nuovo tipo di dati JSON introdotto da diversi fornitori di database. Ai fini di questo articolo, verrà utilizzato il database MySql 8 per memorizzare i dati inviati.

Database MySql 8 utilizzato per questo articolo. MySQL ha introdotto un nuovo tipo di dati denominato [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Questo semplifica la memorizzazione e la query degli oggetti JSON. I dati inviati verranno memorizzati in una colonna di tipo JSON nel nostro database.

La schermata seguente mostra i dati del modulo inviati memorizzati nel tipo di dati JSON. La colonna &quot;formdata&quot; è di tipo JSON. Abbiamo anche memorizzato il nome del modulo associato ai dati nel nome del modulo della colonna

>[!NOTE]
>
>Assicurarsi che il file di schema json sia denominato in modo appropriato. Ad esempio, deve essere denominato nel seguente formato &lt;nome>schema.json. Pertanto, il file dello schema può essere mutuo.schema.json o credit.schema.json.


![datastored](assets/datastored.gif)


[Schemi JSON di esempio che possono essere utilizzati per creare Forms adattivo.](assets/samplejsonschemas.zip). Scaricate e decomprimete il file zip per ottenere gli schemi JSON

