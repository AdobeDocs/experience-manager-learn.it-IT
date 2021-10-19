---
title: Generare il DoR interattivo con i dati del modulo adattivo
description: Unisci i dati del modulo adattivo con il modello XDP per generare il pdf scaricabile
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
source-git-commit: 2ed78bb8b122acbe69e98d63caee1115615d568f
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 1%

---


# Scarica il file interattivo DoR

Un caso d’uso comune è quello di poter scaricare un DoR interattivo con i dati del Modulo adattivo. Il DoR scaricato verrà quindi completato con Adobe Acrobat o Adobe Reader.

Per eseguire questo caso d’uso, è necessario effettuare le seguenti operazioni

## Genera dati di esempio per XDP

* Apri XDP in AEM Forms designer.
* Fai clic su File | Proprietà modulo | Anteprima
* Fare clic su Genera anteprima dati
* Fare clic su Genera
* Fornire un nome file significativo come &quot;form-data.xml&quot;

## Genera XSD dai dati xml

È possibile utilizzare uno qualsiasi degli strumenti online gratuiti per [genera XSD](https://www.freeformatter.com/xsd-generator.html) dai dati xml generati nel passaggio precedente.

## Creare un modulo adattivo

Crea un modulo adattivo basato su XSD dal passaggio precedente. Associa il modulo per utilizzare la libreria client &quot;irs&quot;. Questa libreria client dispone del codice necessario per effettuare una chiamata POST al servlet che restituisce PDF all&#39;applicazione chiamante. Il codice seguente viene attivato quando il _Scarica PDF_ è selezionato

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## Creare un servlet personalizzato

Crea un servlet personalizzato che unirà i dati con il modello XDP e restituirà il pdf. Di seguito è elencato il codice per eseguire questa operazione. Il servlet personalizzato fa parte del [Bundle AEMFormsDocumentServices.core-1.0-SNAPSHOT](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

Nel codice di esempio, il nome del modello (f8918-r14e_redo-barcode_3 2.xdp) viene codificato in modo fisso. Puoi passare facilmente il nome del modello al servlet per rendere questo codice generico in modo che funzioni con tutti i modelli.


## Distribuire l&#39;esempio sul server

Per eseguire il test sul server locale, effettua le seguenti operazioni:

1. [Scarica e installa il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Scaricare e installare il bundle personalizzato DocumentServices](/hep/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Servlet per unire i dati con il modello XDP e riprodurre in streaming il pdf
1. [Importare la libreria client](assets/irs.zip)
1. [Importare il modulo adattivo](assets/f8918complete.zip)
1. [Importare il modello e lo schema XDP](assets/xdp-template-and-xsd.zip)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Compila alcuni campi del modulo
1. Fai clic su Scarica PDF per ottenere il PDF
