---
title: Passaggio del processo personalizzato per gli allegati di file ZIP
description: Passaggio di processo personalizzato per aggiungere gli allegati del modulo adattivo a un file zip e archiviare il file zip in una variabile di flusso di lavoro
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: 1131dca8-882d-4904-8691-95468fb708b7
duration: 75
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---


# Passaggio processo personalizzato

È stato implementato un passaggio di processo personalizzato per creare il file zip contenente gli allegati del modulo. Se non conosci la creazione di un bundle OSGi, [segui queste istruzioni](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en).

Il codice nel passaggio del processo personalizzato esegue le seguenti operazioni:

- Esegui una query per tutti gli allegati del modulo adattivo nella cartella del payload. Il nome della cartella viene passato come argomento del processo al passaggio del processo.
- Crea un file zip contenente gli allegati del modulo e archivialo nella cartella del payload.
- Imposta il valore della variabile del flusso di lavoro (no_of_attachments).

```java
package com.aemforms.formattachments.core;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;

import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=Zip form attachments",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Zip form attachments"
})

public class ZipFormAttachments implements WorkflowProcess {

     private static final Logger log = LoggerFactory.getLogger(ZipFormAttachments.class);
     @Reference
     QueryBuilder queryBuilder;

     @Override
     public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException {
             String payloadPath = workItem.getWorkflowData().getPayload().toString();
             log.debug("The payload path is " + payloadPath);
             MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
             Session session = workflowSession.adaptTo(Session.class);
             Map<String, String> map = new HashMap<String, String>();
             map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
             map.put("type", "nt:file");
             Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
             query.setStart(0);
             query.setHitsPerPage(20);
             SearchResult result = query.getResult();
             log.debug("Get result hits " + result.getHits().size());
             ByteArrayOutputStream baos = new ByteArrayOutputStream();
             ZipOutputStream zipOut = new ZipOutputStream(baos);
             int no_of_attachments = result.getHits().size();
             for (Hit hit: result.getHits()) {
                     try {
                             String attachmentPath = hit.getPath();
                             log.debug("The hit path is " + hit.getPath());
                             Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                             InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                             ByteArrayOutputStream buffer = new ByteArrayOutputStream();
                             int nRead;
                             byte[] data = new byte[1024];
                             while ((nRead = attachmentStream.read(data, 0, data.length)) != -1) {
                                     buffer.write(data, 0, nRead);
                             }

                             buffer.flush();
                             byte[] byteArray = buffer.toByteArray();
                             ZipEntry zipEntry = new ZipEntry(hit.getTitle());
                             zipOut.putNextEntry(zipEntry);
                             zipOut.write(byteArray);
                             zipOut.closeEntry();

                     } catch (Exception e) {
                             log.debug("The error message is " + e.getMessage());
                     }
             }
             try {
                    zipOut.close();
                    Node payloadNode = session.getNode(payloadPath);
                    Node zippedFileNode =  payloadNode.addNode("zipped_attachments.zip", "nt:file");
                    javax.jcr.Node resNode = zippedFileNode.addNode("jcr:content", "nt:resource");

                    ValueFactory valueFactory = session.getValueFactory();
                    Document zippedDocument = new Document(baos.toByteArray());

                    Binary contentValue = valueFactory.createBinary(zippedDocument.getInputStream());
                    metaDataMap.put("no_of_attachments", no_of_attachments);

                    workflowSession.updateWorkflowData(workItem.getWorkflow(), workItem.getWorkflow().getWorkflowData());
                    log.debug("Updated workflow");
                    resNode.setProperty("jcr:data", contentValue);
                    session.save();
                    zippedDocument.close();

             } catch (IOException | RepositoryException e) {
                     log.error("Error in closing zipout", e);
             }
     }
}
```

>[!NOTE]
>
> Assicurati che nel flusso di lavoro sia presente una variabile denominata _no_of_attachments_ di tipo Double affinché il codice funzioni.

## Passaggi successivi

[Popolare le variabili di flusso di lavoro ArrayList con allegati e nome dell&#39;allegato](./custom-process-step.md)


