---
title: Generare il DoR interattivo con i dati del modulo adattivo
description: Unisci i dati del modulo adattivo con il modello XDP per generare il pdf scaricabile
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 1%

---

# Scarica il file interattivo DoR

Un caso d’uso comune è quello di poter scaricare un DoR interattivo con i dati del Modulo adattivo. Il DoR scaricato verrà quindi completato con Adobe Acrobat o Adobe Reader.

## Il modulo adattivo non è basato sullo schema XSD

Se il modulo XDP e adattivo non è basato su alcuno schema, segui i passaggi seguenti per generare un documento di record interattivo.

### Creare un modulo adattivo

Crea un modulo adattivo e accertati che i nomi dei campi del modulo adattivo siano identici ai nomi dei campi nel modello xdp.
Prendi nota del nome dell’elemento principale del modello xdp.
![elemento principale](assets/xfa-root-element.png)

### Lib client

Il codice seguente viene attivato quando viene attivato il pulsante Download PDF .

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
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

## Modulo adattivo basato sullo schema XSD

Se il tuo xdp non è basato su XSD, segui i seguenti passaggi per creare XSD(schema)su cui baserai il tuo modulo adattivo

### Genera dati di esempio per XDP

* Apri XDP in AEM Forms designer.
* Fai clic su File | Proprietà modulo | Anteprima
* Fare clic su Genera anteprima dati
* Fare clic su Genera
* Fornire un nome file significativo come &quot;form-data.xml&quot;

### Genera XSD dai dati xml

È possibile utilizzare uno qualsiasi degli strumenti online gratuiti per [genera XSD](https://www.freeformatter.com/xsd-generator.html) dai dati xml generati nel passaggio precedente.

### Creare un modulo adattivo

Crea un modulo adattivo basato su XSD dal passaggio precedente. Associa il modulo per utilizzare la libreria client &quot;irs&quot;. Questa libreria client dispone del codice necessario per effettuare una chiamata POST al servlet che restituisce PDF all&#39;applicazione chiamante. Il codice seguente viene attivato quando il _Scarica PDF_ è selezionato

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
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
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
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

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

Nel codice di esempio, estraiamo il nome xdp e altri parametri dall’oggetto di richiesta. Se il modulo non è basato su XSD, viene creato il documento xml da unire con l&#39;xdp.Se il modulo è basato su XSD, estraiamo semplicemente il nodo appropriato dai dati inviati dal modulo adattivo per generare il documento xml da unire al modello xdp.

## Distribuire l&#39;esempio sul server

Per eseguire il test sul server locale, effettua le seguenti operazioni:

1. [Scarica e installa il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Scaricare e installare il bundle personalizzato DocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Servlet per unire i dati con il modello XDP e riprodurre in streaming il pdf
1. [Importare la libreria client](assets/generate-interactive-dor-client-lib.zip)
1. [Importa le risorse dell’articolo (moduli adattivi, modelli XDP e XSD)](assets/generate-interactive-dor-sample-assets.zip)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Compila alcuni dei campi del modulo.
1. Fai clic su Scarica PDF per ottenere il PDF. Potrebbe essere necessario attendere alcuni secondi prima del download di PDF.

>[!NOTE]
>
>Puoi provare lo stesso caso d’uso con [modulo adattivo non basato su xsd](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). Assicurati di trasmettere i parametri appropriati all’endpoint post in streampdf.js che si trova nella clientlib della proprietà .
