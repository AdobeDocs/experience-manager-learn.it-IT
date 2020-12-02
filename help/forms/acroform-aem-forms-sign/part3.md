---
title: Acrobat con  AEM Forms
seo-title: Unisci dati modulo adattivo con Acrobat
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 1%

---


# Verificare questa funzionalità nel sistema

[Scaricate e importate il pacchetto in ](assets/acro-form-aem-form.zip)
AEMTil pacchetto contiene il flusso di lavoro di esempio e la pagina html che consente di creare lo schema dall’Acroform caricato.

## Configura flusso di lavoro

1. [Aprire il modello di workflow in modalità](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html) di modifica.
2. Aprire le proprietà di configurazione del passaggio MergeAcroformData.
3. Fare clic sulla scheda Processo.
4. Verificate che gli argomenti che state passando siano una cartella valida sul server.
5. Salva le modifiche.

## Crea modulo adattivo

1. Creare un modulo adattivo utilizzando lo schema creato nel passaggio precedente.
2. Trascinare alcuni elementi dello schema nel modulo adattivo.
3. Configurare l’azione di invio del modulo adattivo per l’invio al flusso di lavoro AEM (MergeAcroformData).
4. **Assicurarsi di specificare il percorso del file di dati come &quot;Data.xml&quot;. Ciò è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del flusso di lavoro.**
5. Visualizzare l’anteprima del modulo adattivo, compilare il modulo e inviare il modulo.
6. Nel flusso di lavoro di configurazione, il file PDF con i dati uniti salvati nella cartella specificata al punto 4 deve essere visualizzato

>[!NOTE]
>
>Il pdf generato dall&#39;unione dei dati con l&#39;acroform viene salvato come pdfdocument.pdf nella cartella payload del flusso di lavoro. Questo documento può essere utilizzato per un&#39;ulteriore elaborazione nell&#39;ambito del flusso di lavoro
