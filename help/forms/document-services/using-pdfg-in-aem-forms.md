---
title: Utilizzo di PDFG in AEM Forms
description: Dimostrazione della funzionalità di trascinamento della selezione per creare PDF con AEM Forms
feature: PDF Generator
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 52
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Utilizzo di PDFG in AEM Forms{#using-pdfg-in-aem-forms}

Dimostrazione della funzionalità di trascinamento della selezione per creare PDF con AEM Forms

PDFG è l&#39;acronimo di PDF Generation. Ciò significa che è possibile convertire diversi formati di file in PDF. I documenti più comuni sono i documenti di Microsoft Office. PDFG fa parte di AEM Forms dalla versione 6.1.
[Il file javadoc per l&#39;API PDFG è elencato qui](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Le risorse associate a questo articolo ti consentiranno di trascinare e rilasciare i documenti di MS Office o il file JPG nella zona di rilascio della pagina HTML. Una volta eliminato il documento, viene richiamato il servizio PDFG, viene convertito in PDF e salvato nel file system di AEM Server.

Per installare le risorse demo, effettua le seguenti operazioni

1. Configurare PDFG come indicato in questo documento [qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Segui la documentazione relativa alla tua versione di AEM Forms.
1. [Importa e installa le risorse correlate a questo articolo utilizzando Gestione pacchetti.](assets/createpdfgdemov2.zip)
1. [Passa a post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) nel CRX
1. Modifica la posizione di salvataggio in base alle preferenze (riga 9)
1. Salva le modifiche.
1. Apri la [pagina HTML](http://localhost:4502/content/DocumentServices/CreatePDFG.html) per trascinare i file per la conversione.
1. Rilascia un file di Word o un file jpg nella zona di rilascio.
1. Il documento di input viene convertito in PDF e salvato nella stessa posizione specificata al punto 4.

Il seguente snippet di codice mostra l’utilizzo del servizio PDFG per convertire i file in PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
