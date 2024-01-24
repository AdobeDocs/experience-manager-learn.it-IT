---
title: Generazione di più PDF da un file di dati
description: OutputService fornisce una serie di metodi per creare documenti utilizzando una struttura di modulo e dati da unire con la struttura del modulo. Scopri come generare più PDF da un unico XML di grandi dimensioni contenente più record singoli.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
last-substantial-update: 2020-01-07T00:00:00Z
duration: 167
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# Generare un set di documenti PDF da un file di dati XML

OutputService fornisce una serie di metodi per creare documenti utilizzando una struttura di modulo e dati da unire con la struttura del modulo. L’articolo seguente spiega il caso d’uso per generare più PDF da un unico file xml di grandi dimensioni contenente più record singoli.
Di seguito è riportata la schermata di un file xml contenente più record.

![multi-record-xml](assets/multi-record-xml.PNG)

L’XML dati ha 2 record. Ogni record è rappresentato dall&#39;elemento form1. Questo xml viene passato a OutputService [generatePDFOutputBatch, metodo](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) otteniamo l’elenco dei documenti pdf (uno per record) La firma del metodo generatePDFOutputBatch accetta i seguenti parametri

* modelli: mappa contenente il modello, identificato da una chiave
* data: mappa contenente documenti di dati xml, identificati dalla chiave
* pdfOutputOptions: opzioni per configurare la generazione di pdf
* batchOptions: opzioni per configurare il batch



## Dettagli del caso d’uso{#use-case-details}

In questo caso d’uso forniremo una semplice interfaccia web per caricare il modello e il file di dati (xml). Una volta completato il caricamento dei file e inviata la richiesta POST al servlet AEM. Questo servlet estrae i documenti e chiama il metodo generatePDFOutputBatch di OutputService. I PDF generati vengono compressi in un file zip e resi disponibili per il download dall’utente finale dal browser web.

## Codice servlet{#servlet-code}

Di seguito è riportato lo snippet di codice del servlet. Il codice estrae il modello (xdp) e il file di dati (xml) dalla richiesta. Il file modello viene salvato nel file system. Vengono create due mappe: templateMap e dataFileMap che contengono rispettivamente il modello e i file xml(data). Viene quindi effettuata una chiamata al metodo generateMultipleRecords del servizio DocumentServices.

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

Il codice seguente genera più file PDF utilizzando generatePDFOutputBatch di OutputService e restituisce un file zip contenente i file PDF al servlet chiamante

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

Per testare questa funzionalità sul server, attieniti alle seguenti istruzioni:

* [Scarica ed estrai il contenuto del file zip nel file system](assets/mult-records-template-and-xml-file.zip).Questo file zip contiene il modello e il file di dati xml.
* [Puntare il browser alla console Web Felix](http://localhost:4502/system/console/bundles)
* [Distribuire il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Distribuisci bundle AEMFormsDocumentServices personalizzato](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Pacchetto personalizzato che genera i PDF utilizzando l’API OutputService
* [Puntare il browser a Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Importare e installare il pacchetto](assets/generate-multiple-pdf-from-xml.zip). Questo pacchetto contiene una pagina HTML che consente di rilasciare il modello e i file di dati.
* [Puntare il browser a MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Trascina e rilascia il modello e il file di dati xml insieme
* Scarica il file zip creato. Questo file zip contiene i file pdf generati dal servizio di output.

>[!NOTE]
>Questa funzionalità può essere attivata in diversi modi. In questo esempio abbiamo utilizzato un’interfaccia web per rilasciare il modello e il file di dati per dimostrare la funzionalità.
