---
title: Utilizzo di PDFG in AEM Forms
description: Dimostrazione della funzionalità di trascinamento della selezione per la creazione di PDF tramite AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 2%

---

# Utilizzo di PDFG in AEM Forms{#using-pdfg-in-aem-forms}

Dimostrazione della funzionalità di trascinamento della selezione per la creazione di PDF tramite AEM Forms

PDFG sta per generazione PDF. Ciò significa che è possibile convertire una varietà di formati di file in PDF. I documenti più comuni sono i documenti di Microsoft Office. PDFG fa parte di AEM Forms dalla versione 6.1.
[Javadoc per API PDFG è elencato qui](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Le risorse associate a questo articolo consentono di trascinare e rilasciare documenti MS office o file JPG nella zona di rilascio della pagina HTML. Una volta che il documento viene rilasciato, richiama il servizio PDFG e converte il documento in PDF e lo salva nel file system di AEM Server.

Per installare le risorse demo, effettua le seguenti operazioni

1. Configura PDFG come indicato in questo documento [qui](https://helpx.adobe.com/it/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Segui la documentazione appropriata relativa alla tua versione di AEM Forms.
1. [Importa e installa le risorse correlate a questo articolo utilizzando il gestore di pacchetti.](assets/createpdfgdemov2.zip)
1. [Passa a post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) nel tuo CRX
1. Modifica la posizione di salvataggio in base alle tue preferenze (riga 9)
1. Salva le modifiche.
1. Apri [pagina HTML](http://localhost:4502/content/DocumentServices/CreatePDFG.html) per trascinare file per la conversione.
1. Rilasciare un file word o un file jpg nella zona di rilascio.
1. Il documento di input viene convertito in PDF e salvato nella stessa posizione specificata al punto 4.

Il seguente frammento di codice mostra l&#39;utilizzo del servizio PDFG per convertire i file in PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
