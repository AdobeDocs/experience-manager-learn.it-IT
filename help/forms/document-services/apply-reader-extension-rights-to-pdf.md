---
title: Applicare i diritti di utilizzo ai PDF caricati
description: Applicare i diritti di utilizzo ai PDF
version: Experience Manager 6.4, Experience Manager 6.5
feature: Reader Extensions
topic: Development
role: Developer
level: Experienced
exl-id: ea433667-81db-40f7-870d-b16630128871
last-substantial-update: 2020-07-07T00:00:00Z
duration: 129
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Applicazione delle estensioni Reader

Le estensioni Reader consentono di manipolare i diritti di utilizzo sui documenti PDF. I diritti di utilizzo si riferiscono alle funzionalità disponibili in Acrobat ma non in Adobe Reader. Le funzionalità controllate dalle estensioni di Reader includono la possibilità di aggiungere commenti a un documento, compilare moduli e salvare il documento. I documenti PDF a cui sono stati aggiunti diritti di utilizzo sono denominati documenti abilitati per i diritti. L’utente che apre un documento PDF con abilitazione per i diritti in Adobe Reader può eseguire le operazioni abilitate per tale documento.

Per eseguire questo caso d’uso è necessario effettuare le seguenti operazioni:
* [Aggiungere il certificato delle estensioni di Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=it) all&#39;utente `fd-service`.

## Creare un servizio OSGi personalizzato

Crea un servizio OSGi personalizzato che applicherà i diritti di utilizzo ai documenti. Il codice per eseguire questa operazione è elencato di seguito

```java
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = ApplyUsageRights.class)
public class ApplyUsageRights implements ReaderExtendPDF {
        @Reference
        DocAssuranceService docAssuranceService;
        @Reference
        GetResolver getResolver;
        Logger logger = LoggerFactory.getLogger(ApplyUsageRights.class);
        @Override
        public Document applyUsageRights(Document pdfDocument, UsageRights usageRights) {

                ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
                UnlockOptions unlockOptions = null;
                ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
                reOptions.setCredentialAlias("ares");

                reOptions.setResourceResolver(getResolver.getFormsServiceResolver());

                reOptions.setReOptions(reOptionsSpec);
                System.out.println("Applying Usage Rights");

                try {
                        Document readerExtended = docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
                                unlockOptions);
                        reOptions.getResourceResolver().close();
                        return readerExtended;
                } catch (Exception e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                return null;
        }

}
```

## Creare un servlet per lo streaming del PDF esteso del lettore

Il passaggio successivo consiste nel creare un servlet con un metodo POST per restituire all’utente il PDF esteso per il lettore. In questo caso, all&#39;utente viene richiesto di salvare il PDF nel proprio file system. Questo perché PDF viene riprodotto come PDF dinamico e i visualizzatori pdf forniti con i browser non gestiscono i pdf dinamici.

Di seguito è riportato il codice del servlet. Il servlet viene richiamato dall’azione di invio personalizzata di Modulo adattivo.
Il servlet crea un oggetto UsageRights e ne imposta le proprietà in base ai valori immessi dall&#39;utente nel modulo adattivo. Il servlet chiama quindi il metodo applyUsageRights del servizio creato a questo scopo.

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

        private static final long serialVersionUID = -883724052368090823 L;
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
                                        System.out.println("Got form attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);
                                        if (logger.isDebugEnabled()) {
                                                documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        }

                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();

                                        response.setContentType("application/pdf");
                                        response.addHeader("Content-Disposition", "attachment; filename=" + param.getFileName().split("/")[1]);
                                        response.setContentLength((int) fileInputStream.available());
                                        OutputStream responseOutputStream = response.getOutputStream();
                                        documentToReaderExtend.close();
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

Per eseguire il test sul server locale, attieniti alla seguente procedura:
1. [Scaricare e installare il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. Aggiungi la seguente voce al servizio User Mapper di Apache Sling utilizzando la console configMgr come mostrato di seguito

```
       DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

![mapper utente](assets/user-mapper-service.PNG)
1. [Scarica e installa il bundle ares.ares.core-ares](assets/ares.ares.core-ares.jar). Questo include il servizio personalizzato e il servlet per applicare i diritti di utilizzo e riportare in streaming il pdf.
1. [Importare le librerie client e l’invio personalizzato](assets/applyaresdemo.zip)
1. [Importare il modulo adattivo](assets/applyaresform.zip)
1. Aggiungere il certificato Reader Extensions all&#39;utente &quot;fd-service&quot;. Assicurarsi che l&#39;alias sia &quot;**ares**&quot;.
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Seleziona i diritti appropriati e carica il file PDF
1. Fai clic su Invia per ottenere Reader Extended PDF
