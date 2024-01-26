---
title: Utilizzo di PDFG in AEM Forms
description: Dimostrazione della funzionalità di trascinamento per la creazione di PDF con AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 63
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Utilizzo di PDFG in AEM Forms{#using-pdfg-in-aem-forms}

Dimostrazione della funzionalità di trascinamento per la creazione di PDF con AEM Forms

PDFG sta per PDF Generation. Ciò significa che è possibile convertire diversi formati di file in PDF. I documenti più comuni sono i documenti di Microsoft Office. PDFG fa parte di AEM Forms dalla versione 6.1.
[Il JavaScript per l’API PDFG è elencato qui](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Le risorse associate a questo articolo ti consentiranno di trascinare e rilasciare i documenti di MS Office o il file JPG nella zona di rilascio della pagina HTML. Una volta eliminato il documento, viene richiamato il servizio PDFG e convertito il documento in PDF e salvato nel file system del server AEM.

Per installare le risorse demo, effettua le seguenti operazioni

1. Configurare PDFG come indicato in questo documento [qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Segui la documentazione relativa alla tua versione di AEM Forms.
1. [Importa e installa le risorse correlate a questo articolo utilizzando Gestione pacchetti.](assets/createpdfgdemov2.zip)
1. [Passa a post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) in CRX
1. Modifica la posizione di salvataggio in base alle preferenze (riga 9)
1. Salva le modifiche.
1. Apri [pagina html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) per trascinare e rilasciare i file per la conversione.
1. Rilascia un file di Word o un file jpg nella zona di rilascio.
1. Il documento di input viene convertito in PDF e salvato nella stessa posizione di cui al punto 4.

Il seguente snippet di codice mostra l’utilizzo del servizio PDFG per convertire i file in PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
