---
title: Utilizzo dei frammenti nel servizio di output
description: Generare documenti PDF con frammenti residenti nell’archivio crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
exl-id: d7887e2e-c2d4-4f0c-b117-ba7c41ea539a
duration: 106
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Generazione di documenti PDF tramite frammenti{#developing-with-output-and-forms-services-in-aem-forms}


In questo articolo utilizzeremo il servizio di output per generare file pdf utilizzando frammenti xdp. L’xdp principale e i frammenti risiedono nell’archivio crx. È importante simulare la struttura delle cartelle del file system in AEM. Ad esempio, se utilizzi un frammento nella cartella Frammenti nel tuo XDP, devi creare una cartella denominata **frammenti** nella cartella base in AEM. La cartella base conterrà il modello xdp di base. Ad esempio, se nel file system è presente la seguente struttura
* c:\xdptemplates - Conterrà il modello xdp di base
* c:\xdptemplates\fragments - Questa cartella conterrà frammenti e il modello principale farà riferimento al frammento come mostrato di seguito
  ![frammento-xdp](assets/survey-fragment.png).
* La cartella xdpdocuments conterrà il modello di base e i frammenti nella cartella **fragments**

Puoi creare la struttura richiesta utilizzando [forms and document ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Di seguito è riportata la struttura di cartelle per l’XDP di esempio che utilizza 2 frammenti
![moduli&amp;documento](assets/fragment-folder-structure-ui.png)


* Servizio di output: in genere questo servizio viene utilizzato per unire i dati xml con un modello xdp o un PDF per generare un PDF appiattito. Per ulteriori dettagli, fare riferimento a [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) per il servizio di output. In questo esempio utilizziamo frammenti che risiedono nell’archivio crx.


Il codice seguente è stato utilizzato per includere frammenti nel file PDF

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**Per eseguire il test del pacchetto di esempio nel sistema**

* [Scarica e importa i file xdp di esempio in AEM](assets/xdp-templates-fragments.zip)
* [Scaricare e installare il pacchetto utilizzando Gestione pacchetti AEM](assets/using-fragments-assets.zip)
* [L’XDP e i frammenti di esempio possono essere scaricati da qui](assets/xdptemplates.zip)

**Dopo aver installato il pacchetto, sarà necessario inserire nell&#39;elenco Consentiti i seguenti URL in Adobe Granite CSRF Filter.**

1. Segui i passaggi indicati di seguito per inserire nell&#39;elenco Consentiti i percorsi menzionati in precedenza.
1. [Accesso a configMgr](http://localhost:4502/system/console/configMgr)
1. Cerca Adobe di filtro CSRF Granite
1. Aggiungi il seguente percorso nelle sezioni escluse e salva
1. /content/AemFormsSamples/usingfragments

Esistono diversi modi per testare il codice di esempio. Il metodo più rapido e semplice consiste nell’utilizzare l’app Postman. Postman consente di effettuare richieste POST al server. Installa l’app Postman sul sistema.
Avvia l’app e immetti il seguente URL per testare l’API di esportazione dei dati

Accertati di aver selezionato &quot;POST&quot; dall’elenco a discesa
http://localhost:4502/content/AemFormsSamples/usingfragments.html
Assicurarsi di specificare Autorizzazione come Autenticazione di base. Specificare il nome utente e la password del server AEM
Passa alla scheda &quot;Corpo&quot; e specifica i parametri di richiesta come mostrato nell’immagine seguente
![esporta](assets/using-fragment-postman.png)
Quindi fare clic sul pulsante Invia

[Puoi importare questa raccolta postman per testare l’API](assets/usingfragments.postman_collection.json)
