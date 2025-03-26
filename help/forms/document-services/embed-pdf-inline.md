---
title: Visualizza documento di record in linea
description: Unisci i dati dei moduli adattivi con il modello XDP e visualizza PDF in linea utilizzando l’API pdf per l’incorporamento di document cloud.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 165
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# Visualizza documento record in linea

Un caso d’uso comune è la visualizzazione di un documento PDF con i dati immessi dal compilatore del modulo.

Per eseguire questo caso d&#39;uso è stata utilizzata l&#39;[API di incorporamento Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

Per completare l’integrazione sono stati eseguiti i seguenti passaggi

## Crea un componente personalizzato per visualizzare il PDF in linea

È stato creato un componente personalizzato (embed-pdf) per incorporare il pdf restituito dalla chiamata POST.

## Libreria client

Il codice seguente viene eseguito quando si fa clic sul pulsante di selezione `viewPDF`. I dati del modulo adattivo e il nome del modello vengono passati all’endpoint per generare il pdf. Il pdf generato viene quindi visualizzato nel compilatore di moduli utilizzando la libreria JavaScript PDF incorporata.

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## Genera dati di esempio per XDP

* Apri XDP in AEM Forms Designer.
* Fai clic su File | Proprietà modulo | Anteprima
* Fai clic su Genera dati di anteprima
* Fai clic su Genera
* Fornisci un nome file significativo, ad esempio &quot;form-data.xml&quot;

## Generare XSD dai dati xml

Puoi utilizzare uno degli strumenti online gratuiti per [generare XSD](https://www.freeformatter.com/xsd-generator.html) dai dati xml generati nel passaggio precedente.

## Carica il modello

Assicurati di caricare il modello xdp in [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) utilizzando il pulsante Crea


## Creare un modulo adattivo

Crea un modulo adattivo basato sull’XSD del passaggio precedente.
Aggiungi una nuova scheda all’adattatore. Aggiungi un componente casella di controllo e un componente pdf da incorporare a questa scheda
Accertatevi di denominare la casella di controllo viewPDF.
Configura il componente incorpora-pdf come illustrato nella schermata seguente
![pdf-incorporato](assets/embed-pdf-configuration.png)

**Incorpora chiave API PDF**: è la chiave che puoi usare per incorporare il pdf. Questa chiave funziona solo con localhost. Puoi creare [la tua chiave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associarla ad un altro dominio.

**Endpoint che restituisce il pdf**: questo è il servlet personalizzato che unirà i dati con il modello xdp e restituirà il pdf.

**Nome modello** - Percorso dell&#39;XDP. In genere, viene memorizzato nella cartella formsanddocuments.

**Nome file PDF**: stringa che verrà visualizzata nel componente pdf incorporato.

## Creare un servlet personalizzato

È stato creato un servlet personalizzato per unire i dati con il modello XDP e restituire il pdf. Il codice per eseguire questa operazione è elencato di seguito. Il servlet personalizzato fa parte del [bundle embedded pdf](assets/embedpdf.core-1.0-SNAPSHOT.jar)

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
         response.setContentType("application/pdf");
         response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
         response.setContentLength((int) fileInputStream.available());
         OutputStream responseOutputStream = response.getOutputStream();
         int bytes;
         while ((bytes = fileInputStream.read()) != -1) {
            responseOutputStream.write(bytes);
         }
         responseOutputStream.flush();
         responseOutputStream.close();

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## Distribuire l’esempio sul server

Per eseguire il test sul server locale, attieniti alla seguente procedura:

1. [Scarica e installa il bundle di incorporpdf](assets/embedpdf.core-1.0-SNAPSHOT.jar).
Questo ha il servlet per unire i dati con il modello XDP e riportare in streaming il pdf.
1. Aggiungi il percorso /bin/getPDFToEmbed nella sezione percorsi esclusi del filtro CSRF di Adobe Granite utilizzando [AEM ConfigMgr](http://localhost:4502/system/console/configMgr). Nell&#39;ambiente di produzione è consigliabile utilizzare il [framework di protezione CSRF](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [Importare la libreria client e il componente personalizzato](assets/embed-pdf.zip)
1. [Importare il modulo e il modello adattivo](assets/embed-pdf-form-and-xdp.zip)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. Compila alcuni campi del modulo
1. Passa alla scheda Visualizza PDF. Selezionare la casella di controllo Visualizza PDF. Dovresti visualizzare un pdf nel modulo compilato con i dati del modulo adattivo
