---
title: Creare il modulo iniziale per attivare il processo
description: Crea un modulo iniziale per attivare la notifica e-mail per avviare il processo di firma.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 9%

---

# Crea modulo iniziale

Il modulo iniziale (Modulo di rifinanziamento) viene utilizzato per la firma di più moduli attivando il flusso di lavoro di AEM **Firma più Forms**. È possibile immettere i valori desiderati, ma assicurarsi che i campi riportati di seguito vengano aggiunti al modulo.

| Tipo campo | Nome | Scopo | Nascosto | Valore predefinito |
|--- |--- |---|--- |--- |
| Campo testo | firmato | Per indicare lo stato di firma | Y | N |
| Campo testo | GUID | Per identificare in modo univoco il modulo | Y | 3889 |
| Campo testo | customerName | Per acquisire il nome del cliente | N | |
| Campo testo | customerEmail | E-mail del cliente per inviare la notifica | N | |
| Casella di controllo | formsToSign | Gli elementi identificano i moduli nel pacchetto | N | |

È necessario configurare il modulo iniziale per attivare un flusso di lavoro AEM denominato **signmultipleforms**
Verificare che il percorso del file di dati sia impostato su **Data.xml**. Questo è molto importante, poiché il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

## Risorse

Il modulo iniziale (modulo di rifinanziamento) può essere [scaricato da qui](assets/refinance-form.zip)

## Passaggi successivi

[Creare moduli da utilizzare per la firma](./create-forms-for-signing.md)
