---
title: Generare documenti PDF utilizzando il servizio di output
description: Unisci i dati con il modello XDP per generare i PDF non interattivi
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 8%

---

# Generare documenti PDF utilizzando il servizio di output

Il [servizio di output](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html) è un servizio OSGi che fa parte di AEM Document Services. Supporta vari formati di output e funzioni di progettazione di AEM Forms Designer. Il servizio di output converte i modelli XFA e i dati XML per generare documenti di stampa in formati diversi.

Il servizio di output in AEM Forms as a Cloud Service è molto simile a quello di AEM Forms 6.5, quindi se hai familiarità con l’utilizzo del servizio di output in AEM Forms 6.5, la transizione ad AEM Forms as a Cloud Service dovrebbe essere semplice.

Con il servizio di output, è possibile creare applicazioni che consentono di:

+ Generare i documenti compilando i file modello con dati XML.
+ Genera moduli di output in vari formati, inclusi flussi di stampa non interattivi PDF, PostScript, PCL e ZPL.
+ Generare PDF di stampa da PDF modulo XFA.
+ Generare in blocco documenti PDF, PostScript, PCL e ZPL unendo più set di dati con i modelli forniti.

Questo servizio è progettato per essere utilizzato nel contesto di un’istanza as a Cloud Service di AEM Forms. Il seguente frammento di codice genera un documento PDF in un servlet utilizzando `OutputService`.

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
