---
title: Creare un flusso di lavoro principale per attivare il processo di firma
description: Crea un flusso di lavoro per memorizzare i moduli per la firma nel database
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
thumbnail: 6887.jpg
jira: KT-6887
topic: Development
role: Developer
level: Intermediate
exl-id: 338d9522-f6da-4aa7-b5d8-b9fff39ea94b
duration: 70
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---

# Crea flusso di lavoro principale

Il workflow principale viene attivato quando l&#39;utente invia il modulo iniziale (**RefinanceForm**). Di seguito è riportato il flusso di lavoro

![flusso di lavoro principale](assets/main-workflow.PNG)

**Archivia Forms per la firma** è un passaggio del processo personalizzato.

La motivazione per l’implementazione di un passaggio di processo personalizzato è l’estensione di un flusso di lavoro AEM. Il codice seguente implementa un passaggio di processo personalizzato. Il codice estrae i nomi dei moduli da firmare e trasmette i dati del modulo inviati al metodo `insertData` nel servizio SignMultipleForms. Il metodo `insertData` inserisce quindi le righe nel database identificato dall&#39;origine dati **aemformstutorial**.

Il codice in questo passaggio del processo personalizzato fa riferimento al servizio `SignMultipleForms`.



```java
package com.aem.forms.signmultipleforms;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=StoreFormsToSign",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=StoreFormsToSign"
})
public class StoreFormsToSignWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(StoreFormsToSignWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log.debug("The payload  in StoreFormsToSign " + workItem.getWorkflowData().getPayload().toString());
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    String serverURL = arg2.get("PROCESS_ARGS", "string").toString();
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/formsToSign").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      log.debug("The form names to sign are  t" + node.getTextContent());
      String formNamesToSign[] = node.getTextContent().split(",");
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      DOMSource source = new DOMSource(xmlDocument);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      InputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
      StringWriter writer = new StringWriter();
      IOUtils.copy(inputStream, writer, Charset.defaultCharset());
      String formData = writer.toString();
      signMultipleForms.insertData(formNamesToSign, formData, serverURL, workItem, workflowSession);

  }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }
}
```




## Risorse

Il flusso di lavoro di firma di più Forms utilizzato in questo articolo può essere [scaricato da qui](assets/sign-multiple-forms-workflows.zip)

>[!NOTE]
> Assicurati di configurare Day CQ Mail Service per inviare le notifiche e-mail. Il modello di e-mail viene fornito anche nel pacchetto precedente.

## Passaggi successivi

[Aggiorna stato firma alla firma del documento](./update-signature-status.md)
