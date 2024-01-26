---
title: Attivare il flusso di lavoro AEM all’invio di moduli HTM5 - Creare un profilo personalizzato
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare il flusso di lavoro AEM
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 101
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Crea profilo personalizzato

In questa parte verrà creato un [profilo personalizzato.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Un profilo è responsabile del rendering di XDP come HTML. Viene fornito un profilo predefinito per il rendering di XDP come HTML. Rappresenta una versione personalizzata del servizio Forms Rendition per dispositivi mobili. Puoi utilizzare il servizio Rappresentazione moduli mobile per personalizzare l’aspetto, il comportamento e le interazioni del Forms mobile. Nel nostro profilo personalizzato acquisiremo i dati compilati nel modulo mobile utilizzando l’API guidebridge. Questi dati vengono quindi inviati a un servlet personalizzato che genererà un PDF interattivo e li invierà nuovamente all’applicazione chiamante.

Ottenere i dati del modulo utilizzando `formBridge` API JavaScript. Noi utilizziamo il `getDataXML()` metodo:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

Nel metodo del gestore di successo effettuiamo una chiamata al servlet personalizzato in esecuzione in AEM. Questo servlet eseguirà il rendering e restituirà il pdf interattivo con i dati del modulo mobile

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Genera Interactive PDF

Di seguito è riportato il codice servlet responsabile del rendering del pdf interattivo e della restituzione del pdf all’applicazione chiamante. Il servlet richiama `mobileFormToInteractivePdf` metodo del servizio OSGi DocumentServices personalizzato.

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
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
      } catch (IOException e) {
        // TODO Add proper error logging
      }
    }
}
```

### Rendering di Interactive PDF

Il codice che segue utilizza [API del servizio Forms](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) per eseguire il rendering di PDF interattivi con i dati del modulo mobile.

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

Per visualizzare la possibilità di scaricare PDF interattivi da un modulo mobile parzialmente compilato: [fai clic qui](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Una volta scaricato il PDF, il passaggio successivo consiste nell’inviare il PDF per attivare un flusso di lavoro AEM. Questo flusso di lavoro unisce i dati del PDF inviato e genera il PDF non interattivo da rivedere.

Il profilo personalizzato creato per questo caso d’uso è disponibile come parte di queste risorse di esercitazione.
