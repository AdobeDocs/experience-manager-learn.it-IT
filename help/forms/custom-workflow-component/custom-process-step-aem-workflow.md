---
title: Implementazione della fase del processo personalizzato con la finestra di dialogo
description: Scrittura di allegati di moduli adattivi nel file system utilizzando un passaggio del processo personalizzato
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Passaggio processo personalizzato

Questa esercitazione è destinata ai clienti AEM Forms che devono implementare un componente flusso di lavoro personalizzato. Il primo passaggio nella creazione del componente flusso di lavoro è scrivere il codice Java che sarà associato al componente flusso di lavoro. Per lo scopo di questa esercitazione scriveremo una semplice classe java per memorizzare gli allegati del modulo adattivo al file system. Questo codice java leggerà gli argomenti specificati nel componente del flusso di lavoro.

I seguenti passaggi sono necessari per scrivere la classe java e distribuire la classe come bundle OSGi

## Crea progetto Maven

Il primo passo è quello di creare un progetto Maven utilizzando il tipo di archivio Maven Adobe appropriato. I passaggi dettagliati sono elencati in questo [articolo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Una volta importato il progetto Maven in eclipse, puoi iniziare a scrivere il tuo primo componente OSGi che può essere utilizzato nel passaggio del processo.


### Crea una classe che implementa WorkflowProcess

Apri il progetto Maven nell’ambiente IDE. Espandi **nome progetto** > **nucleo centrale** cartella. Espandi la cartella src/main/java . Dovresti visualizzare un pacchetto che termina con &quot;core&quot;. Crea una classe Java che implementa WorkflowProcess in questo pacchetto. Sarà necessario sovrascrivere il metodo execute . La firma del metodo execute è come segue public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException

In questa esercitazione verranno scritti gli allegati aggiunti al modulo adattivo nel file system come parte del flusso di lavoro AEM.

Per eseguire questo caso d’uso, è stata scritta la seguente classe java

Diamo un&#39;occhiata a questo codice

```java
package com.mysite.core;
import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
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
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);

    log.debug("Got attachments path: " + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
    log.debug("Got save location: " + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* attachmentPath - Si tratta della stessa posizione specificata nel modulo adattivo quando hai configurato l&#39;azione di invio del modulo adattivo per richiamare AEM flusso di lavoro. Si tratta di un nome della cartella in cui si desidera salvare gli allegati in AEM rispetto al payload del flusso di lavoro.

* saveToLocation : percorso in cui salvare gli allegati nel file system del server di AEM.

Questi due valori vengono passati come argomenti di processo utilizzando la finestra di dialogo del componente flusso di lavoro

![ProcessStep](assets/custom-workflow-component.png)

Il servizio QueryBuilder viene utilizzato per eseguire query sui nodi di tipo nt:file nella cartella attachmentPath. Il resto del codice esegue un&#39;iterazione attraverso i risultati della ricerca per creare un oggetto Document e salvarlo nel file system


>[!NOTE]
>
>Poiché si utilizza un oggetto Document specifico per AEM Forms, è necessario includere nel progetto maven la dipendenza aemfd-client-sdk.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### Creare e distribuire

[Crea il bundle come descritto qui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[Assicurati che il bundle sia distribuito e in stato attivo](http://localhost:4502/system/console/bundles)

## Passaggi successivi

Crea il tuo [componente flusso di lavoro personalizzato](./custom-workflow-component.md)

