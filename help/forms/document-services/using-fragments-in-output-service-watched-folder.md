---
title: Uso dei frammenti nel servizio di output con la cartella controllata
description: Genera documenti pdf con frammenti residenti nell'archivio crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Generazione di documenti pdf con frammenti tramite script ECMA{#developing-with-output-and-forms-services-in-aem-forms}


In questo articolo utilizzeremo il servizio di output per generare file pdf utilizzando frammenti xdp. L’xdp principale e i frammenti risiedono nell’archivio crx. È importante imitare la struttura delle cartelle del file system in AEM. Ad esempio, se utilizzi un frammento presente nella cartella frammenti dell’xdp, devi creare una cartella denominata **frammenti** sotto la cartella di base in AEM. La cartella base conterrà il modello xdp di base. Ad esempio, se nel file system è presente la struttura seguente
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* La cartella xdpdocuments conterrà il modello di base e i frammenti in **frammenti** cartella

È possibile creare la struttura richiesta utilizzando [interfaccia utente moduli e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Di seguito è riportata la struttura delle cartelle per l’xdp di esempio che utilizza 2 frammenti
![moduli&amp;documenti](assets/fragment-folder-structure-ui.png)


* Servizio di output: in genere questo servizio viene utilizzato per unire dati xml con modello xdp o pdf per generare pdf appiattiti. Per ulteriori informazioni, consulta la [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) per il servizio Output. In questo esempio utilizziamo frammenti che risiedono nell’archivio crx.


Il seguente script ECMA è stato utilizzato per generare PDF. Osserva l’uso di ResourceResolver e ResourceResolverHelper nel codice. ResourceReolver è necessario perché questo codice è in esecuzione al di fuori di qualsiasi contesto utente.

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**Per testare il pacchetto di esempio sul sistema**
* [Distribuire il bundle DevelopingWithServiceUSer](assets/DevelopingWithServiceUser.jar)
* Aggiungi la voce **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** nella modifica del servizio mappatore utente come mostrato nella schermata seguente
   ![modifica del mappatore utente](assets/user-mapper-service-amendment.png)
* [Scaricare e importare i file xdp di esempio e gli script ECMA](assets/watched-folder-fragments-ecma.zip).
Questo creerà una struttura di cartelle controllate nella cartella c:/fragmentsandoutputservice

* [Estrarre il file di dati di esempio](assets/usingFragmentsSampleData.zip) e inseriscilo nella cartella di installazione della cartella controllata(c:\fragmentsandoutputservice\install)

* Controlla la cartella dei risultati della configurazione della cartella controllata per il file pdf generato
