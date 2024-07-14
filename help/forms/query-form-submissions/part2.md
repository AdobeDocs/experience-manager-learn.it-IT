---
title: Query dell’invio del modulo
description: Tutorial in più parti che illustra i passaggi necessari per eseguire query sugli invii di moduli memorizzati nel portale di Azure
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 1%

---

# Creare un invio personalizzato

È stato scritto un gestore di invio personalizzato per gestire l’invio del modulo. Ad alto livello, il gestore di invio personalizzato esegue le seguenti operazioni

* Estrae il nome del modulo inviato.
* Estrae i dati inviati. I dati inviati di un modulo basato su un componente core sono sempre in formato JSON.
* Estrae e memorizza gli allegati del modulo nel portale di Azure. Aggiorna i dati JSON inviati con l’URL dell’allegato.
* Crea tag di indice BLOB: trova l&#39;elenco dei campi ricercabili per il modulo e il valore corrispondente dai dati inviati.
* Associa i tag di indice BLOB ai dati inviati e li memorizza nel portale di Azure.

La schermata seguente mostra i tag di indice BLOB nel portale di Azure

![blob-index-tags](assets/blob-index-tags.png)

Il codice di invio personalizzato si trova in **_StoreFormDataWithBlobIndexTagsInAzure_** e il codice per l&#39;archiviazione e il recupero dei dati da Azure si trova nel componente **_SaveAndFetchFromAzure_**

## Passaggi successivi

[Creare un’interfaccia query](./part3.md)
