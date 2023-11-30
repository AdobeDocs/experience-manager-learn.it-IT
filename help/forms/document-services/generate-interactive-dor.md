---
title: Generare un DoR interattivo con i dati del modulo adattivo
description: Unisci i dati del modulo adattivo con il modello XDP per generare un PDF scaricabile
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Scarica DoR interattivo

Un caso d’uso comune consiste nel scaricare un DoR interattivo con i dati del modulo adattivo. Il DoR scaricato verrà quindi completato utilizzando Adobe Acrobat o Adobe Reader.

## Il modulo adattivo non è basato sullo schema XSD

Se XDP e il modulo adattivo non sono basati su alcuno schema, segui i seguenti passaggi per generare un documento di record interattivo.

### Creare un modulo adattivo

Crea un modulo adattivo e assicurati che i nomi dei campi del modulo adattivo siano denominati in modo identico ai nomi dei campi nel modello xdp.
Prendi nota del nome dell’elemento principale del modello xdp.
![root-element](assets/xfa-root-element.png)

### Libreria client

Il seguente codice viene attivato quando si attiva il pulsante Download PDF

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

## Modulo adattivo basato su schema XSD

Se l’xdp non è basato su XSD, segui i seguenti passaggi per creare XSD(schema)su cui baserai il modulo adattivo

### Genera dati di esempio per XDP

* Apri XDP in AEM Forms Designer.
* Fai clic su File | Proprietà modulo | Anteprima
* Fai clic su Genera dati di anteprima
* Fai clic su Genera
* Fornisci un nome file significativo, ad esempio &quot;form-data.xml&quot;

### Generare XSD dai dati xml

È possibile utilizzare uno qualsiasi degli strumenti online gratuiti per [genera XSD](https://www.freeformatter.com/xsd-generator.html) dai dati xml generati nel passaggio precedente.

### Creare un modulo adattivo

Crea un modulo adattivo basato sull’XSD del passaggio precedente. Associa il modulo per utilizzare la libreria client &quot;irs&quot;. Questa libreria client ha il codice per effettuare una chiamata POST al servlet che restituisce il PDF all&#39;applicazione chiamante. Il seguente codice viene attivato quando _Scarica PDF_ ha fatto clic su

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

Crea un servlet personalizzato che unirà i dati con il modello XDP e restituirà il pdf. Il codice per eseguire questa operazione è elencato di seguito. Il servlet personalizzato fa parte del [Pacchetto AEMFormsDocumentServices.core-1.0-SNAPSHOT](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

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

Nel codice di esempio, estraiamo il nome xdp e altri parametri dall’oggetto della richiesta. Se il modulo non è basato su XSD, viene creato il documento xml da unire con l’xdp.Se il modulo è basato su XSD, viene semplicemente estratto il nodo appropriato dai dati inviati del modulo adattivo per generare il documento xml da unire con il modello xdp.

## Distribuire l’esempio sul server

Per eseguire il test sul server locale, attieniti alla seguente procedura:

1. [Scaricare e installare il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Scarica e installa il bundle DocumentServices personalizzato](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Questo ha il servlet per unire i dati con il modello XDP e riportare in streaming il PDF
1. [Importare la libreria client](assets/generate-interactive-dor-client-lib.zip)
1. [Importa le risorse articolo (modulo adattivo, modelli XDP e XSD)](assets/generate-interactive-dor-sample-assets.zip)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Compila alcuni campi del modulo.
1. Fai clic su Scarica PDF per scaricare il PDF. Potrebbe essere necessario attendere alcuni secondi per il download del PDF.

>[!NOTE]
>
>Puoi provare lo stesso caso d’uso con [modulo adattivo non basato su xsd](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). Assicurati di trasmettere i parametri appropriati all’endpoint post in streampdf.js che si trova nella clientlib irs.
