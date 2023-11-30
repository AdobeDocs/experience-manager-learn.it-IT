---
title: Creare il modulo iniziale per attivare il processo
description: Crea un modulo iniziale per attivare la notifica e-mail per avviare il processo di firma.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# Crea modulo iniziale

Il modulo iniziale (Modulo di rifinanziamento) viene utilizzato per la firma di più moduli attivando **Firma più Forms** Flusso di lavoro AEM. È possibile immettere i valori desiderati, ma assicurarsi che i campi riportati di seguito vengano aggiunti al modulo.

| Tipo di campo | Nome | Scopo | Nascosto | Valore predefinito |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| CampoTesto | firmato | Per indicare lo stato di firma | Y | N |
| CampoTesto | GUID | Per identificare in modo univoco il modulo | Y | 3889 |
| CampoTesto | customerName | Per acquisire il nome del cliente | N |
| CampoTesto | customerEmail | E-mail del cliente per inviare la notifica | N |
| Casella di controllo | formsToSign | Gli elementi identificano i moduli nel pacchetto | N |

Il modulo iniziale deve essere configurato per attivare un flusso di lavoro AEM denominato **signmultipleforms**
Assicurarsi che il percorso del file di dati sia impostato su **Dati.xml**. Questo è molto importante, poiché il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

## Risorse

Il modulo iniziale (modulo di rifinanziamento) può essere [scaricato da qui](assets/refinance-form.zip)

## Passaggi successivi

[Creare moduli da utilizzare per la firma](./create-forms-for-signing.md)
