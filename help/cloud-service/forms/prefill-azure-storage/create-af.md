---
title: Memorizzare i dati del modulo adattivo nell’archiviazione Azure
description: Creare e configurare un modulo adattivo per archiviare i dati nell’archiviazione di Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
exl-id: 0b543c6b-9cfd-4fac-b8d0-33153c036f4b
duration: 83
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 2%

---

# Tutti gli elementi insieme

Ora disponiamo di tutte le configurazioni/integrazioni richieste per il caso d’uso. L’ultimo passaggio consiste nel creare un modulo adattivo basato sul modello di dati del modulo supportato dall’archiviazione di Azure.

Crea e adatta un modulo e accertati che sia basato sul modello di modulo adattivo corretto. In questo modo il codice personalizzato associato al modello verrà eseguito ogni volta che viene eseguito il rendering di un modulo adattivo.

## Modulo adattivo di esempio

Nel modulo sono stati aggiunti 2 campi nascosti

* ID BLOB: questo campo viene compilato con un GUID quando viene inizializzato. Il valore di questo campo viene utilizzato come blobid per identificare in modo univoco l’archiviazione blob dei dati del modulo. Questo blobid viene utilizzato per identificare i dati del modulo.
* ID BLOB restituito: questo campo viene popolato con il risultato della chiamata del servizio per archiviare i dati nell’archiviazione di Azure. Questo valore sarà lo stesso dell’ID BLOB del passaggio precedente.

Il modulo include le seguenti regole business

* Il pulsante Salva modulo viene visualizzato quando l&#39;utente immette l&#39;indirizzo di posta elettronica. Facendo clic sul pulsante Salva modulo, i dati del modulo vengono memorizzati nell’archiviazione di Azure utilizzando l’operazione di richiamo del servizio del modello dati del modulo.
* Il BlobID restituito dal servizio di richiamo viene memorizzato nel campo ID BLOB. Quando questo valore viene modificato, viene inviato un messaggio di posta elettronica al richiedente utilizzando SendGrid. L’e-mail conterrà il collegamento per aprire il modulo parzialmente completato identificato dall’ID BLOB.
* Quando i dati vengono archiviati correttamente in Azure Storage, viene visualizzato un testo di conferma

### Passaggi successivi

[Distribuire le risorse di esempio](./deploy-sample-assets.md)
