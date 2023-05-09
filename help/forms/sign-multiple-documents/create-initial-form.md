---
title: Creare il modulo iniziale per attivare il processo
description: Creare un modulo iniziale per attivare la notifica via e-mail e avviare il processo di firma.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# Crea modulo iniziale

Il modulo iniziale (Modulo di rifinanziamento) viene utilizzato per la firma di più moduli attivando la **Sign Multiple Forms** AEM flusso di lavoro. È possibile immettere i valori desiderati, ma accertarsi che al modulo siano aggiunti i campi seguenti.

| Tipo di campo | Nome | Scopo | Nascosto | Valore predefinito |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| CampoTesto | firmato | Per indicare lo stato della firma | Y | N |
| CampoTesto | guid | Per identificare il modulo in modo univoco | Y | 3889 |
| CampoTesto | customerName | Per acquisire il nome del cliente | N |
| CampoTesto | customerEmail | Indirizzo e-mail del cliente per inviare una notifica | N |
| Casella di controllo | formsToSign | Gli elementi identificano i moduli nel pacchetto | N |

È necessario configurare il modulo iniziale per attivare un flusso di lavoro AEM denominato **signmultiplo**
Assicurati che il Percorso file dati sia impostato su **Data.xml**. Questo è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

## Risorse

Il modulo iniziale (Modulo di rifinanziamento) può essere [scaricato da qui](assets/refinance-form.zip)

## Passaggi successivi

[Creazione di moduli da utilizzare per la firma](./create-forms-for-signing.md)
