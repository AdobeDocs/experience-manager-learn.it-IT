---
title: Servizio Codice A Barre Con Forms Adattivo
seo-title: Servizio Codice A Barre Con Forms Adattivo
description: Utilizzo del servizio codice a barre per decodificare i codici a barre e compilare i campi modulo a partire dai dati estratti
seo-description: Utilizzo del servizio codice a barre per decodificare i codici a barre e compilare i campi modulo a partire dai dati estratti
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---


# Servizio codice a barre con Forms{#barcode-service-with-adaptive-forms} adattivo

Questo articolo illustra l&#39;utilizzo del servizio codice a barre per compilare il modulo adattivo. Il caso di utilizzo è il seguente:

1. L&#39;utente aggiunge un PDF con codice a barre come allegato del modulo adattivo
1. Il percorso dell&#39;allegato viene inviato al servlet
1. Il servlet ha decodificato il codice a barre e restituisce i dati in formato JSON
1. Il modulo adattivo viene quindi popolato utilizzando i dati decodificati

Il codice seguente decodifica il codice a barre e compila un oggetto JSON con i valori decodificati. Il servlet quindi restituisce l&#39;oggetto JSON nella risposta all&#39;applicazione chiamante.

Questa funzionalità è disponibile dal vivo. Visitare il [portale di esempi](https://forms.enablementadobe.com/content/samples/samples.html?query=0) e cercare la demo del servizio Codice a barre

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

Di seguito è riportato il codice servlet. Questo servlet viene chiamato quando l&#39;utente aggiunge un allegato al modulo adattivo. Il servlet restituisce l&#39;oggetto JSON all&#39;applicazione chiamante. L&#39;applicazione chiamante compila quindi il modulo adattivo con i valori estratti dall&#39;oggetto JSON.

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

Il codice seguente fa parte della libreria client a cui fa riferimento il modulo adattivo. Quando un utente aggiunge l&#39;allegato al modulo adattivo, questo codice viene attivato. Il codice effettua una chiamata al servlet con il percorso dell’allegato passato nel parametro request. I dati ricevuti dalla chiamata servlet vengono quindi utilizzati per compilare il modulo adattivo.

```
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
>Il modulo adattivo incluso in questo pacchetto è stato creato con  AEM Forms 6.4. Se si intende utilizzare questo pacchetto in  ambiente AEM Forms 6.3, creare il modulo adattivo in AEM modulo 6.3

Linea 12 - Codice personalizzato per ottenere il risolutore del servizio. Questo bundle è incluso come parte di queste risorse di articoli.

Riga 23 - Chiama il metodo estrazioneBarCode di DocumentServices per inserire l&#39;oggetto JSON con dati decodificati

Per eseguire questa operazione sul sistema, attenetevi alla seguente procedura

1. [Scaricate BarcodeService.](assets/barcodeservice.zip) zip importate in AEM utilizzando il gestore pacchetti
1. [Scaricare e installare il pacchetto Document Services personalizzato](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Download e installazione del pacchetto DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Download del modulo PDF di esempio](assets/barcode.pdf)
1. Posizionare il browser sul [modulo adattivo di esempio](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. Caricare il PDF di esempio fornito
1. È necessario visualizzare i moduli compilati con i dati

