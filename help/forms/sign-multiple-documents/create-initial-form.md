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
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 7%

---

# Crea modulo iniziale

Il modulo iniziale (Modulo di rifinanziamento) viene utilizzato per la firma di più moduli attivando il flusso di lavoro AEM **Firma più Forms**. È possibile immettere i valori desiderati, ma assicurarsi che i campi riportati di seguito vengano aggiunti al modulo.

| Tipo di campo | Nome | Scopo | Nascosto | Valore predefinito |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | firmato | Per indicare lo stato di firma | Y | N |
| TextField | GUID | Per identificare in modo univoco il modulo | Y | 3889 |
| TextField | customerName | Per acquisire il nome del cliente | N |
| TextField | customerEmail | E-mail del cliente per inviare la notifica | N |
| Casella di controllo | formsToSign | Gli elementi identificano i moduli nel pacchetto | N |

È necessario configurare il modulo iniziale per attivare un flusso di lavoro AEM denominato **signmultipleforms**
Verificare che il percorso del file di dati sia impostato su **Data.xml**. Questo è molto importante, poiché il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

## Risorse

Il modulo iniziale (modulo di rifinanziamento) può essere [scaricato da qui](assets/refinance-form.zip)

## Passaggi successivi

[Creare moduli da utilizzare per la firma](./create-forms-for-signing.md)
