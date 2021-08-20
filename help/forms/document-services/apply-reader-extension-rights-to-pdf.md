---
title: Rendering di XDP in PDF con diritti di utilizzo
description: Applica diritti di utilizzo a pdf
version: 6.4,6.5
feature: Estensioni Reader
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Applicazione delle estensioni del Reader

Estensioni di Reader consente di manipolare i diritti di utilizzo sui documenti PDF. I diritti di utilizzo si riferiscono alle funzionalità disponibili in Acrobat ma non in Adobe Reader. La funzionalità controllata da Estensioni Reader consente di aggiungere commenti a un documento, compilare moduli e salvare il documento. I documenti PDF a cui sono stati aggiunti i diritti di utilizzo sono denominati documenti abilitati per i diritti. Un utente che apre un documento PDF abilitato per i diritti in Adobe Reader può eseguire le operazioni abilitate per tale documento.
Per testare questa funzionalità, prova questo [link](https://forms.enablementadobe.com/content/forms/af/applyreaderextensions.html).

Per eseguire questo caso d’uso, è necessario effettuare le seguenti operazioni:
* [Aggiungi il certificato Reader estensioni ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) all’ `fd-service` utente.

* Crea un servizio OSGi personalizzato che applicherà i diritti di utilizzo ai documenti. Il codice per eseguire questa operazione è elencato di seguito

```java
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service=ApplyUsageRights.class,immediate = true)
public class ApplyUsageRights implements ReaderExtendPDF {
@Reference
DocAssuranceService docAssuranceService;
@Reference
GetResolver getResolver;
@Override
public Document applyUsageRights(Document pdfDocument,UsageRights usageRights) {
      ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
      UnlockOptions unlockOptions = null;
      ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
      reOptions.setCredentialAlias("ares");
      reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
      reOptions.setReOptions(reOptionsSpec);
    try {
          return docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
          unlockOptions);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return null;
}

}
```

## Crea servlet per lo streaming del PDF {#create-servlet-to-stream-the-pdf}

Il passaggio successivo consiste nel creare un servlet con un metodo POST per restituire all’utente il PDF esteso del lettore. In questo caso, all’utente verrà richiesto di salvare il PDF nel proprio file system. Questo perché il PDF viene rappresentato come PDF dinamico e i visualizzatori pdf che sono dotati dei browser non gestiscono i pdf dinamici.

Di seguito è riportato il codice del servlet. Il servlet verrà richiamato dall&#39;azione **customsubmit** di Modulo adattivo.
Servlet crea l&#39;oggetto UsageRights e le imposta le proprietà in base ai valori immessi dall&#39;utente nel Modulo adattivo. Il servlet chiama quindi il metodo **applyUsageRights** del servizio creato a questo scopo.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Map;

import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];
                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);

                                        documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        documentToReaderExtend.close();
                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();
                                        documentToReaderExtend.close();
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
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

                }

        }

}
```

Per eseguire il test sul server locale, effettua le seguenti operazioni:
1. [Scarica e installa il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Scarica e installa il bundle](assets/ares.ares.core-ares.jar) ares.ares.core-ares. Questo dispone del servizio personalizzato e del servlet per applicare i diritti di utilizzo e lo streaming del pdf
1. [Importare le librerie client e l’invio personalizzato](assets/applyaresdemo.zip)
1. [Importare il modulo adattivo](assets/applyaresform.zip)
1. Aggiungi il certificato di estensioni di Reader all’utente &quot;fd-service&quot;
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Seleziona i diritti appropriati e carica il file PDF
1. Fare clic su Invia per ottenere il PDF esteso del Reader


