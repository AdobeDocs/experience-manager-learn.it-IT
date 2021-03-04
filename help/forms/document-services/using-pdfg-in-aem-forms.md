---
title: Utilizzo del PDFG in AEM Forms
seo-title: Utilizzo del PDFG in AEM Forms
description: Dimostrazione della funzione di trascinamento della selezione per la creazione di PDF con AEM Forms
seo-description: Dimostrazione della funzione di trascinamento della selezione per la creazione di PDF con AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 2%

---


# Utilizzo del PDFG in AEM Forms{#using-pdfg-in-aem-forms}

Dimostrazione della funzione di trascinamento della selezione per la creazione di PDF con AEM Forms

PDFG sta per generazione PDF. Ciò significa che è possibile convertire una varietà di formati di file in PDF. I documenti più comuni sono i documenti di Microsoft Office. PDFG fa parte di AEM Forms dalla 6.1.
[Il javadoc per l&#39;API PDFG è elencato qui](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Le risorse associate a questo articolo ti consentono di trascinare e rilasciare documenti MS office o file JPG nella zona di rilascio della pagina HTML. Una volta eliminato il documento, richiama il servizio PDFG e converte il documento in PDF e lo salva nel file system di AEM Server.

Per installare le risorse demo, effettua le seguenti operazioni

1. Configura il PDFG come indicato in questo documento [qui](https://helpx.adobe.com/it/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Segui la documentazione appropriata relativa alla tua versione di AEM Forms.
1. [Importa e installa le risorse correlate a questo articolo utilizzando il gestore di pacchetti.](assets/createpdfgdemov2.zip)
1. [Passa a post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin il tuo CRX
1. Modifica la posizione di salvataggio in base alle tue preferenze (riga 9)
1. Salva le modifiche.
1. Apri la [ pagina HTML](http://localhost:4502/content/DocumentServices/CreatePDFG.html) per trascinare e rilasciare i file per la conversione.
1. Rilasciare un file word o un file jpg nella zona di rilascio.
1. Il documento di input viene convertito in PDF e salvato nella stessa posizione specificata al punto 4.

Il seguente frammento di codice mostra l&#39;utilizzo del servizio PDFG per convertire i file in PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

