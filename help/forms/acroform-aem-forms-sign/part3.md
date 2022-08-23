---
title: Acroformi con AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 3 di un tutorial sull’integrazione di Acrobat con AEM Forms. Verifica il flusso di lavoro e il modulo adattivo sul sistema.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# Verificare questa funzionalità nel sistema

[Scarica e importa questo pacchetto in AEM](assets/acro-form-aem-form.zip)
Questo pacchetto contiene il flusso di lavoro di esempio e la pagina HTML che consente di creare lo schema dall’Acroform caricato.

## Configura flusso di lavoro

1. [Apri il modello di flusso di lavoro in modalità di modifica](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Apri le proprietà di configurazione del passaggio MergeAcroformData .
3. Fai clic sulla scheda Processo .
4. Assicurati che gli argomenti che stai passando siano una cartella valida sul server.
5. Salva le modifiche.

## Creare un modulo adattivo

1. Crea un modulo adattivo utilizzando lo schema creato nel passaggio precedente.
2. Trascina alcuni elementi dello schema nel modulo adattivo.
3. Configura l’azione di invio del modulo adattivo per l’invio a AEM flusso di lavoro (MergeAcroformData).
4. **Assicurati di specificare il percorso del file di dati come &quot;Data.xml&quot;. Questo è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del flusso di lavoro.**
5. Visualizza l’anteprima del modulo adattivo, compila il modulo e invia.
6. Dovresti visualizzare PDF con i dati uniti salvati nella cartella specificata al passaggio 4 nel flusso di lavoro di configurazione

>[!NOTE]
>
>Il pdf generato dall’unione dei dati con l’acroform viene salvato come pdfdocument.pdf nella cartella payload del flusso di lavoro. Questo documento può quindi essere utilizzato per un’ulteriore elaborazione nell’ambito del flusso di lavoro
