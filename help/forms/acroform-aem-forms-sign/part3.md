---
title: Acroforms con AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 3 di un tutorial sull’integrazione di Acroforms con AEM Forms. Verifica il flusso di lavoro e il modulo adattivo sul sistema.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.5
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 1%

---


# Verifica questa funzionalità sul sistema

[Scarica e importa questo pacchetto in AEM](assets/acro-form-aem-form.zip)
Questo pacchetto contiene il flusso di lavoro di esempio e la pagina HTML che consente di creare lo schema da Acroform caricato.

## Configura flusso di lavoro

1. [Apri il modello del flusso di lavoro in modalità di modifica](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Aprire le proprietà di configurazione del passaggio MergeAcroformData.
3. Fare clic sulla scheda Processo.
4. Verifica che gli argomenti che stai trasmettendo siano una cartella valida sul server.
5. Salva le modifiche.

## Creare un modulo adattivo

1. Crea un modulo adattivo utilizzando lo schema creato nel passaggio precedente.
2. Trascina e rilascia alcuni elementi dello schema sul modulo adattivo.
3. Configura l’azione di invio del modulo adattivo per l’invio al flusso di lavoro AEM (MergeAcroformData).
4. **Assicurarsi di specificare il percorso del file di dati come &quot;Data.xml&quot;. Questo è molto importante, poiché il codice di esempio cerca un file denominato Data.xml nel payload del flusso di lavoro.**
5. Anteprima modulo adattivo, compila il modulo e invia.
6. Dovresti trovare PDF con i dati uniti salvati nella cartella specificata al passaggio 4 nel flusso di lavoro di configurazione

>[!NOTE]
>
>Il PDF generato dall’unione di dati con l’acroform viene salvato come pdfdocument.pdf nella cartella payload del flusso di lavoro. Questo documento può quindi essere utilizzato per ulteriori elaborazioni come parte del flusso di lavoro
