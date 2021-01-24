---
title: Creare il modulo iniziale per attivare il processo
description: Creare un modulo iniziale per attivare la notifica e-mail e avviare il processo di firma.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 5%

---


# Crea modulo iniziale

Il modulo iniziale (Modulo di rifinanziamento) viene utilizzato per la firma di più moduli attivando il flusso di lavoro **Sign Multiple Forms** AEM. È possibile immettere i valori desiderati, ma assicurarsi che al modulo vengano aggiunti i campi seguenti.



| Tipo campo | Nome | Scopo | Nascosto | Valore predefinito |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | signed | Per indicare lo stato di firma | Y | N |
| TextField | guid | Per identificare in modo univoco il modulo | Y | 3889 |
| TextField | customerName | Per acquisire il nome del cliente | N |
| TextField | customerEmail | E-mail del cliente per inviare la notifica | N |
| Casella di controllo | formsToSign | Gli elementi identificano i moduli nel pacchetto | N |



Il modulo iniziale deve essere configurato per attivare un flusso di lavoro AEM denominato **signmultipleforms**
Assicurarsi che il percorso del file di dati sia impostato su **Data.xml**. Ciò è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

## Assets

Il modulo iniziale (Modulo di Rifinanziamento) può essere [scaricato da qui](assets/refinance-form.zip)





