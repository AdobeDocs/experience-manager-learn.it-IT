---
title: Creare il modulo iniziale per attivare il processo
description: Creare un modulo iniziale per attivare la notifica via e-mail e avviare il processo di firma.
feature: Moduli adattivi
version: 6.4,6.5
topic: Sviluppo
role: Business Practitioner
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 7%

---


# Crea modulo iniziale

Il modulo iniziale (Modulo di rifinanziamento) viene utilizzato per la firma di più moduli attivando il flusso di lavoro **Sign Multiple Forms** AEM . È possibile immettere i valori desiderati, ma accertarsi che al modulo siano aggiunti i campi seguenti.

| Tipo di campo | Nome | Scopo | Nascosto | Valore predefinito |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| CampoTesto | firmato | Per indicare lo stato della firma | Y | N |
| CampoTesto | guid | Per identificare il modulo in modo univoco | Y | 3889 |
| CampoTesto | customerName | Per acquisire il nome del cliente | N |
| CampoTesto | customerEmail | Indirizzo e-mail del cliente per inviare una notifica | N |
| Casella di controllo | formsToSign | Gli elementi identificano i moduli nel pacchetto | N |

Il modulo iniziale deve essere configurato per attivare un flusso di lavoro AEM denominato **signmultipleforms**
Assicurati che il Percorso file dati sia impostato su **Data.xml**. Questo è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

## Assets

Il modulo iniziale (Modulo di rifinanziamento) può essere [scaricato da qui](assets/refinance-form.zip)





