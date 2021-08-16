---
title: Generazione di più pdf da un file di dati
description: OutputService fornisce diversi metodi per creare documenti utilizzando una struttura del modulo e i dati da unire alla struttura del modulo. Scopri come generare più pdf da un unico grande xml contenente più record singoli.
feature: Servizio di output
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: fb6c21a9a88b5ebcbfb14213182a9b8cba6fe6ae
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 1%

---


# Generare un set di documenti PDF da un file di dati xml

OutputService fornisce diversi metodi per creare documenti utilizzando una struttura del modulo e i dati da unire alla struttura del modulo. L&#39;articolo seguente spiega il caso d&#39;uso per generare più pdf da un unico grande xml contenente più record singoli.
Di seguito è riportata la schermata del file xml contenente più record.

![multi-record-xml](assets/multi-record-xml.PNG)

I dati xml hanno 2 record. Ciascun record è rappresentato dall’elemento form1. Questo xml viene passato al metodo OutputService [generatePDFOutputBatch](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) otteniamo l&#39;elenco dei documenti pdf (uno per record)
La firma del metodo generatePDFOutputBatch prende i seguenti parametri

* templates: mappa contenente il modello, identificato da una chiave
* data - Mappa contenente documenti di dati xml, identificati dalla chiave
* pdfOutputOptions - opzioni per configurare la generazione di pdf
* batchOptions - opzioni per configurare batch

>[!NOTE]
>
>Questo caso d&#39;uso è disponibile come esempio live in questo [sito web](https://forms.enablementadobe.com/content/samples/samples.html?query=0).

## Dettagli del caso d’uso{#use-case-details}

In questo caso d’uso forniremo una semplice interfaccia web per caricare il modello e il file di dati (xml). Una volta completato il caricamento dei file e la richiesta di POST viene inviata al servlet AEM. Questo servlet estrae i documenti e chiama il metodo generatePDFOutputBatch di OutputService. I pdf generati vengono compressi in un file zip e resi disponibili all’utente finale per il download dal browser web.

## Codice servlet{#servlet-code}

Di seguito è riportato lo snippet di codice dal servlet. Il codice estrae il template(xdp) e il file di dati(xml) dalla richiesta. Il file modello viene salvato nel file system. Vengono create due mappe: templateMap e dataFileMap che contengono rispettivamente il modello e i file xml(data). Viene quindi effettuata una chiamata al metodo generateMultipleRecords del servizio DocumentServices.

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### Codice di implementazione dell’interfaccia{#Interface-Implementation-Code}

Il codice seguente genera più pdf utilizzando generatePDFOutputBatch di OutputService e restituisce al servlet chiamante un file zip contenente i file pdf

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### Distribuisci sul server{#Deploy-on-your-server}

Per testare questa funzionalità sul server, segui le seguenti istruzioni:

* [Scarica ed estrae il contenuto del file zip nel file system](assets/mult-records-template-and-xml-file.zip). Questo file zip contiene il modello e il file di dati xml.
* [Posiziona il browser sulla console Web Felix](http://localhost:4502/system/console/bundles)
* [Distribuisci il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Distribuisci il bundle personalizzato AEMFormsDocumentServices Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Custom che genera i pdf utilizzando l&#39;API OutputService
* [Posiziona il tuo browser nel gestore dei pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Importa e installa il pacchetto](assets/generate-multiple-pdf-from-xml.zip). Questo pacchetto contiene la pagina HTML che ti consente di rilasciare il modello e i file di dati.
* [Posiziona il browser su MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Trascina e rilascia insieme il modello e il file di dati xml
* Scarica il file zip creato. Questo file zip contiene i file pdf generati dal servizio di output.

>[!NOTE]
>Esistono diversi modi per attivare questa funzionalità. In questo esempio abbiamo utilizzato un’interfaccia web per rilasciare il modello e il file di dati per dimostrare la funzionalità.

