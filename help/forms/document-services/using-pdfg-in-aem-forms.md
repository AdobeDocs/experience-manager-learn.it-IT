---
title: Utilizzo di PDFG in  AEM Forms
seo-title: Utilizzo di PDFG in  AEM Forms
description: Dimostrazione della capacità di trascinamento per creare PDF tramite  AEM Forms
seo-description: Dimostrazione della capacità di trascinamento per creare PDF tramite  AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# Utilizzo di PDFG in  AEM Forms{#using-pdfg-in-aem-forms}

Dimostrazione della capacità di trascinamento per creare PDF tramite  AEM Forms

PDFG è l’acronimo di &quot;Generazione PDF&quot;. È quindi possibile convertire in PDF diversi formati di file. I documenti più comuni sono i documenti di Microsoft Office. PDFG fa parte di  AEM Forms dal 6.1.
[Il javadoc per l&#39;API PDFG è elencato qui](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Le risorse associate a questo articolo consentiranno di trascinare i documenti di MS Office o i file JPG nella zona di rilascio della pagina HTML. Una volta eliminato il documento, verrà richiamato il servizio PDFG, il documento verrà convertito in PDF e salvato nel file system di AEM Server.

Per installare le risorse dimostrative, effettuate le seguenti operazioni

1. Configurare il PDFG come indicato in questo documento [qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Seguite la documentazione appropriata relativa alla versione  AEM Forms.
1. [Importate e installate le risorse correlate a questo articolo utilizzando il gestore pacchetti.](assets/createpdfgdemov2.zip)
1. [Passa a post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin il CRX
1. Modifica il percorso di salvataggio in base alle preferenze (riga 9)
1. Salvare le modifiche.
1. Aprite la pagina [ html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) per trascinare e rilasciare i file per la conversione.
1. Rilasciate un file Word o jpg nella zona di rilascio.
1. Il documento di input verrà convertito in PDF e salvato nella stessa posizione specificata al punto 4.

Il frammento di codice seguente mostra l’utilizzo del servizio PDFG per convertire i file in PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

