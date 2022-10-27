---
title: Servizio Codice A Barre Con Forms Adattivo
description: Uso del servizio codice a barre per decodificare i codici a barre e comporre i campi modulo dai dati estratti.
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---

# Servizio Codice A Barre Con Forms Adattivo{#barcode-service-with-adaptive-forms}

Questo articolo illustra l’utilizzo del servizio Codice a barre per compilare i moduli adattivi. Il caso d’uso è il seguente:

1. L’utente aggiunge PDF con codice a barre come allegato al modulo adattivo
1. Il percorso dell&#39;allegato viene inviato al servlet
1. Il servlet ha decodificato il codice a barre e restituisce i dati in formato JSON
1. Il modulo adattivo viene quindi compilato utilizzando i dati decodificati

Il codice seguente decodifica il codice a barre e compila un oggetto JSON con i valori decodificati. Il servlet restituisce quindi l’oggetto JSON nella risposta all’applicazione chiamante.



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

Di seguito è riportato il codice del servlet. Questo servlet viene chiamato quando l’utente aggiunge un allegato al modulo adattivo. Il servlet restituisce l’oggetto JSON all’applicazione chiamante. L&#39;applicazione chiamante compila quindi il modulo adattivo con i valori estratti dall&#39;oggetto JSON.

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

Il codice seguente fa parte della libreria client a cui fa riferimento il modulo adattivo. Quando un utente aggiunge l’allegato al modulo adattivo, questo codice viene attivato. Il codice effettua una chiamata GET al servlet con il percorso dell’allegato passato nel parametro della richiesta. I dati ricevuti dalla chiamata del servlet vengono quindi utilizzati per compilare il modulo adattivo.

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>Il modulo adattivo incluso in questo pacchetto è stato creato con AEM Forms 6.4. Se desideri utilizzare questo pacchetto in ambiente AEM Forms 6.3, crea il modulo adattivo AEM modulo 6.3

Linea 12 - Codice personalizzato per ottenere service resolver. Questo bundle è incluso come parte di queste risorse degli articoli.

Riga 23 - Chiama il metodo extractBarCode di DocumentServices per ottenere il popolamento dell&#39;oggetto JSON con dati decodificati

Per eseguire questo sul sistema, segui i seguenti passaggi:

1. [Scarica BarcodeService.zip](assets/barcodeservice.zip) e importare in AEM utilizzando il gestore dei pacchetti
1. [Scaricare e installare il bundle DocumentServices personalizzato](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Scaricare e installare il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Scarica il modulo PDF di esempio](assets/barcode.pdf)
1. Posiziona il browser sul [modulo adattivo di esempio](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. Carica il PDF di esempio fornito
1. Dovresti visualizzare i moduli compilati con i dati
