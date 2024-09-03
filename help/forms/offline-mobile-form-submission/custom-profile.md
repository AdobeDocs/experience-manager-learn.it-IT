---
title: Attivare il flusso di lavoro AEM all’invio del modulo HTML5 - Creare un profilo personalizzato
description: Creare un profilo personalizzato per scaricare un pdf interattivo con i dati del modulo parzialmente compilato di HTML5
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# Crea profilo personalizzato

In questa parte verrà creato un profilo personalizzato [.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Un profilo è responsabile del rendering di XDP come HTML. Viene fornito un profilo predefinito per il rendering di XDP come HTML. Rappresenta una versione personalizzata del servizio Forms Rendition per dispositivi mobili. Puoi utilizzare il servizio Rappresentazione moduli mobile per personalizzare l’aspetto, il comportamento e le interazioni del Forms mobile. Nel nostro profilo personalizzato acquisiremo i dati compilati nel modulo mobile utilizzando l’API guidebridge. Questi dati vengono quindi inviati a un servlet personalizzato che genererà un PDF interattivo e li invierà nuovamente all’applicazione chiamante.

Ottenere i dati del modulo utilizzando l&#39;API JavaScript `formBridge`. Si utilizza il metodo `getDataXML()`:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

Nel metodo del gestore di successo effettuiamo una chiamata al servlet personalizzato in esecuzione in AEM. Questo servlet eseguirà il rendering e restituirà il pdf interattivo con i dati del modulo mobile

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    let postURL ="/bin/generateinteractivepdf";
    console.log("The data: " + data);
    xhr.open('POST',postURL);
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    let parts = window.location.pathname.split("/");
    let formName = parts[parts.length-2];
    const updatedFilename = formName.replace(/\.xdp$/, '.pdf');

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
                a.download = updatedFilename;
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Genera Interactive PDF

Di seguito è riportato il codice servlet responsabile del rendering del pdf interattivo e della restituzione del pdf all’applicazione chiamante. Il servlet richiama il metodo `mobileFormToInteractivePdf` del servizio OSGi DocumentServices personalizzato.

```java
package com.aemforms.mobileforms.core.servlets;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import java.io.*;
import java.nio.charset.StandardCharsets;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/generateInteractivePDF"})
public class GeneratePDFFromMobileFormData extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final transient Logger logger = LoggerFactory.getLogger(this.getClass());
    @Reference
    GeneratePDFFromMobileForm generatePDFFromMobileForm;

    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) throws IOException {
        String dataXml = request.getParameter("formData");
        logger.debug("The data is "+dataXml);
        InputStream inputStream = new ByteArrayInputStream(dataXml.getBytes(StandardCharsets.UTF_8));
        Document submittedXml = new Document(inputStream);
       Document interactivePDF =  generatePDFFromMobileForm.generateInteractivePDF(submittedXml,request.getParameter("xdpPath"));
        interactivePDF.copyToFile(new File("interactive.pdf"));
        try {
            InputStream fileInputStream = interactivePDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
            response.setContentLength(fileInputStream.available());
            ServletOutputStream responseOutputStream = response.getOutputStream();

            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }

            responseOutputStream.flush();
            responseOutputStream.close();
        }
        catch(Exception e)
        {
            logger.debug(e.getMessage());
        }


    }
}
```

### Rendering di Interactive PDF

Il codice seguente utilizza l&#39;[API del servizio Forms](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) per eseguire il rendering di Interactive PDF con i dati del modulo mobile.

```java
package com.aemforms.mobileforms.core.documentservices.impl;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.AcrobatVersion;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.adobe.fd.forms.api.PDFFormRenderOptions;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(service = GeneratePDFFromMobileForm.class, immediate = true)
public class GeneratePDFFromMobileFormImpl implements GeneratePDFFromMobileForm {
    @Reference
    FormsService formsService;
    private   transient Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public Document generateInteractivePDF(Document xmlData, String xdpPath)
    {
        String uri = "crx:///content/dam/formsanddocuments";
        String xdpName = xdpPath.substring(31, xdpPath.lastIndexOf("/jcr:content"));
        log.debug("####In mobile form to interactive pdf####   " + xdpName);
        PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
        renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
        renderOptions.setContentRoot(uri);
        Document interactivePDF = null;
        try
        {
            interactivePDF = this.formsService.renderPDFForm(xdpName, xmlData, renderOptions);
        }
        catch (FormsServiceException e)
        {
            log.error(e.getMessage());
        }

        return interactivePDF;

    }
}
```

## Passaggi successivi

[Gestire l’invio di moduli](./handle-form-submission.md)
