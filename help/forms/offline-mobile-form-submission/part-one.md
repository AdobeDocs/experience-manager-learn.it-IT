---
title: Attivazione AEM flusso di lavoro per l’invio di moduli HTML5
seo-title: Attiva AEM flusso di lavoro per l’invio di moduli HTML5
description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
seo-description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---


# Crea profilo personalizzato

In questa parte creeremo un profilo [personalizzato.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Un profilo è responsabile del rendering XDP come HTML. Viene fornito un profilo predefinito per il rendering XDP come HTML. Rappresenta una versione personalizzata del servizio di rappresentazione Mobile Forms. È possibile utilizzare il servizio di rappresentazione per modulo mobile per personalizzare l&#39;aspetto, il comportamento e le interazioni dell&#39;Forms Mobile. Nel nostro profilo personalizzato acquisiremo i dati compilati nel modulo mobile utilizzando l&#39;API guidebridge. Questi dati vengono quindi inviati al servlet personalizzato che genererà un PDF interattivo e lo riverserà nell&#39;applicazione chiamante.

Ottenere i dati del modulo utilizzando l&#39;API `formBridge` JavaScript. Utilizziamo il `getDataXML()` metodo:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

Nel metodo del gestore di risultati positivi si esegue una chiamata al servlet personalizzato in esecuzione in AEM. Questo servlet eseguirà il rendering e restituirà un pdf interattivo con i dati del modulo mobile

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

## Genera PDF interattivo

Di seguito è riportato il codice servlet responsabile per il rendering interattivo di pdf e la restituzione del pdf all&#39;applicazione chiamante. Il servlet richiama `mobileFormToInteractivePdf` il metodo del servizio personalizzato DocumentServices OSGi.

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

### Rendering PDF interattivo

Il codice seguente utilizza l’API [](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) Forms Service per eseguire il rendering di PDF interattivo con i dati del modulo mobile.

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

Per visualizzare la possibilità di scaricare PDF interattivi dal modulo mobile parzialmente completato, [fare clic qui](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content).
Una volta scaricato il PDF, il passaggio successivo consiste nell’inviare il PDF per attivare un flusso di lavoro AEM. Questo flusso di lavoro unisce i dati del PDF inviato e genera un PDF non interattivo per la revisione.

Il profilo personalizzato creato per questo caso di utilizzo è disponibile come parte di questa risorsa di esercitazione.
