---
title: 'Visualizza documento di registrazione in linea '
description: Unisci i dati dei moduli adattivi con il modello XDP e visualizza PDF in linea utilizzando l’API pdf di incorporamento di document cloud.
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9411
source-git-commit: 7f9a7951b2d9bb780d5374f17bb289c38b2e2ae7
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 1%

---


# Visualizza DoR in linea

Un caso d’uso comune consiste nella visualizzazione di un documento pdf con i dati immessi dal compilatore.

Per eseguire questo caso d’uso abbiamo utilizzato il [API di incorporamento Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

Sono stati eseguiti i seguenti passaggi per completare l’integrazione

## Crea un componente personalizzato per visualizzare il PDF in linea

È stato creato un componente personalizzato (embed-pdf) per incorporare il pdf restituito dalla chiamata POST.

## Libreria client

Il codice seguente viene eseguito quando il `viewPDF` fare clic sul pulsante della casella di controllo. Per generare il pdf, passiamo i dati del modulo adattivo e il nome del modello all’endpoint. Il pdf generato viene quindi visualizzato al compilatore utilizzando la libreria JavaScript pdf incorporata.

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

* Apri XDP in AEM Forms designer.
* Fai clic su File | Proprietà modulo | Anteprima
* Fare clic su Genera anteprima dati
* Fare clic su Genera
* Fornire un nome file significativo come &quot;form-data.xml&quot;

## Genera XSD dai dati xml

È possibile utilizzare uno qualsiasi degli strumenti online gratuiti per [genera XSD](https://www.freeformatter.com/xsd-generator.html) dai dati xml generati nel passaggio precedente.

## Carica il modello

Assicurati di caricare il modello xdp in [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) utilizzo del pulsante crea


## Creare un modulo adattivo

Crea un modulo adattivo basato su XSD dal passaggio precedente.
Aggiungi una nuova scheda all’adattivo. Aggiungi un componente casella di controllo e un componente embed-pdf a questa scheda Assicurati di assegnare un nome alla casella di controllo viewPDF.
Configura il componente embed-pdf come mostrato nella schermata seguente
![embed-pdf](assets/embed-pdf-configuration.png)

**Incorpora la chiave API di PDF** - Questa è la chiave che è possibile utilizzare per incorporare il pdf. Questa chiave funziona solo con localhost. Puoi creare [la tua chiave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associarlo ad un altro dominio.

**Endpoint che restituisce il pdf** - Questo è il servlet personalizzato che unirà i dati con il modello xdp e restituirà il pdf.

**Nome del modello** - Questo è il percorso dell&#39;xdp. In genere, viene memorizzato nella cartella dei documenti del modulo.

**Nome file PDF** - Questa è la stringa che verrà visualizzata nel componente pdf incorporato.

## Creare un servlet personalizzato

È stato creato un servlet personalizzato per unire i dati con il modello XDP e restituire il pdf. Di seguito è elencato il codice per eseguire questa operazione. Il servlet personalizzato fa parte del [bundle embedpdf](assets/embedpdf.core-1.0-SNAPSHOT.jar)

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


## Distribuire l&#39;esempio sul server

Per eseguire il test sul server locale, effettua le seguenti operazioni:

1. [Scarica e installa il bundle embedpdf](assets/embedpdf.core-1.0-SNAPSHOT.jar).
Questo ha il servlet per unire i dati con il modello XDP e riprodurre il pdf.
1. Aggiungi il percorso /bin/getPDFToEmbed nella sezione dei percorsi esclusi del filtro CSRF di Adobe Granite utilizzando il [AEM ConfigMgr](http://localhost:4502/system/console/configMgr). Nell’ambiente di produzione si consiglia di utilizzare la variabile [Quadro di protezione CSRF](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [Importare la libreria client e il componente personalizzato](assets/embed-pdf.zip)
1. [Importare il modulo adattivo e il modello](assets/embed-pdf-form-and-xdp.zip)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. Compila alcuni campi del modulo
1. Passa alla scheda Visualizza PDF. Selezionare la casella di controllo Visualizza pdf. Dovrebbe essere visualizzato un pdf nel modulo compilato con i dati del modulo adattivo
