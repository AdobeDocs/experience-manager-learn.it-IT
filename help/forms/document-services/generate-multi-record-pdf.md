---
title: Generazione di più PDF da un file di dati
seo-title: Generazione di più PDF da un file di dati
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---


# Generazione di un set di documenti PDF da un file di dati XML

OutputService offre diversi metodi per creare documenti utilizzando una struttura del modulo e i dati da unire alla struttura del modulo. L&#39;articolo seguente illustra i casi d&#39;uso per generare più pdf da un unico xml grande contenente più record singoli.
Di seguito è riportata la schermata del file xml contenente più record.

![multi-record-xml](assets/multi-record-xml.PNG)

Il file xml dei dati ha 2 record. Ciascun record è rappresentato dall’elemento form1. Questo xml viene passato al metodo [OutputService](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) generatePDFOutputBatch e otteniamo l&#39;elenco dei documenti pdf (uno per record)La firma del metodo generatePDFOutputBatch prende i seguenti parametri

* modelli - Mappa contenente il modello, identificato da una chiave
* data - Mappa contenente documenti di dati xml, identificati dalla chiave
* pdfOutputOptions - opzioni per configurare la generazione di pdf
* batchOptions - opzioni per configurare batch

>[!NOTE]
>
>Questo caso d’uso è disponibile come esempio dal vivo in questo [sito Web](https://forms.enablementadobe.com/content/samples/samples.html?query=0).

## Dettagli caso di utilizzo{#use-case-details}

In questo caso d&#39;uso forniremo una semplice interfaccia Web per caricare il modello e il file di dati (xml). Una volta completato il caricamento dei file, la richiesta di POST viene inviata AEM servlet. Questo servlet estrae i documenti e chiama il metodo generatePDFOutputBatch di OutputService. I pdf generati vengono compressi in un file zip e resi disponibili per l&#39;utente finale da scaricare dal browser Web.

## Codice servlet{#servlet-code}

Segue lo snippet di codice del servlet. Il codice estrae il modello(xdp) e il file di dati (xml) dalla richiesta. Il file modello viene salvato nel file system. Vengono create due mappe: templateMap e dataFileMap che contengono rispettivamente il modello e i file xml(data). Viene quindi effettuata una chiamata al metodo generateMultipleRecords del servizio Document Services.

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

### Codice implementazione interfaccia{#Interface-Implementation-Code}

Il codice seguente genera più file pdf utilizzando il file generatePDFOutputBatch di OutputService e restituisce al servlet chiamante un file zip contenente i file pdf

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

### Implementazione sul server{#Deploy-on-your-server}

Per testare questa funzionalità sul server, seguire le istruzioni riportate di seguito:

* [Scaricate ed estraete il contenuto del file zip nel file system](assets/mult-records-template-and-xml-file.zip). Questo file zip contiene il modello e il file di dati xml.
* [Posizionare il browser sulla console Web Felix](http://localhost:4502/system/console/bundles)
* [Distribuisci il pacchetto](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)DevelopingWithServiceUser.
* [Distribuzione di pacchetti AEMFormsDocumentServices Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Custom che genera i file pdf utilizzando l&#39;API OutputService
* [Impostate il browser per il gestore pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Importa e installa il pacchetto](assets/generate-multiple-pdf-from-xml.zip). Questo pacchetto contiene la pagina html che consente di rilasciare il modello e i file di dati.
* [Impostate il browser su MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Trascinare e rilasciare insieme il modello e il file di dati XML
* Scaricate il file zip creato. Questo file zip contiene i file pdf generati dal servizio di output.

>[!NOTE]
>Esistono diversi modi per attivare questa funzionalità. In questo esempio abbiamo utilizzato un&#39;interfaccia Web per rilasciare il modello e il file di dati per dimostrare la capacità.

