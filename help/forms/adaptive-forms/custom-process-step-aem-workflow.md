---
title: Implementazione passaggio processo personalizzato
description: Scrittura degli allegati del modulo adattivo nel file system mediante un passaggio di processo personalizzato
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 203
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Passaggio processo personalizzato

Questo tutorial è destinato ai clienti di AEM Forms che devono implementare un passaggio di processo personalizzato. Un passaggio del processo può eseguire uno script ECMA o richiamare un codice Java™ personalizzato per eseguire le operazioni. Questo tutorial illustra i passaggi necessari per implementare WorkflowProcess che viene eseguito dal passaggio del processo.

Il motivo principale per l’implementazione di un passaggio di processo personalizzato è l’estensione del flusso di lavoro di AEM. Ad esempio, se utilizzi i componenti AEM Forms nel modello di flusso di lavoro, potrebbe essere utile eseguire le seguenti operazioni

* Salva gli allegati del modulo adattivo nel file system
* Manipolare i dati inviati

Per eseguire il caso d’uso precedente, in genere si scrive un servizio OSGi che viene eseguito dal passaggio del processo.

## Crea progetto Maven

Il primo passaggio consiste nel creare un progetto Maven utilizzando l’archetipo Adobe Maven appropriato. I passaggi dettagliati sono elencati in questo [articolo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Una volta importato il progetto Maven in Eclipse, sei pronto per iniziare a scrivere il primo componente OSGi che può essere utilizzato nel passaggio del processo.


### Crea classe che implementa WorkflowProcess

Apri il progetto Maven nell’IDE Eclipse. Espandere la cartella **projectname** > **core**. Espandere la cartella `src/main/java`. Dovresti trovare un pacchetto che termina con `core`. Creare una classe Java™ che implementa WorkflowProcess in questo pacchetto. Sarà necessario sovrascrivere il metodo execute. La firma del metodo execute è la seguente:

```java
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException 
```

Il metodo execute consente di accedere alle tre variabili seguenti:

**WorkItem**: la variabile workItem consentirà l&#39;accesso ai dati relativi al flusso di lavoro. La documentazione API pubblica è disponibile [qui.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: questa variabile workflowSession consente di controllare il flusso di lavoro. La documentazione API pubblica è disponibile [qui](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html).

**MetaDataMap**: tutti i metadati associati al flusso di lavoro. Tutti gli argomenti di processo passati alla fase del processo sono disponibili utilizzando l&#39;oggetto MetaDataMap.[Documentazione API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In questa esercitazione scriveremo gli allegati aggiunti al modulo adattivo nel file system come parte del flusso di lavoro di AEM.

Per eseguire questo caso d’uso, è stata scritta la seguente classe Java™

Vediamo questo codice

```java
package com.learningaemforms.adobe.core;

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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
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
                log.debug("Error saving file " + e.getMessage());
            }
```

Riga 1 - definisce le proprietà del componente. La proprietà `process.label` è quella che verrà visualizzata quando si associa il componente OSGi al passaggio del processo, come mostrato in una delle schermate seguenti.

Righe 13-15 - Gli argomenti di processo passati a questo componente OSGi vengono suddivisi utilizzando il separatore &quot;,&quot;. I valori per attachmentPath e saveToLocation vengono quindi estratti dalla matrice di stringhe.

* attachmentPath: si tratta della stessa posizione specificata nel modulo adattivo quando è stata configurata l’azione di invio del modulo adattivo per richiamare AEM Workflow. Questo è il nome della cartella in cui vuoi salvare gli allegati in AEM, relativo al payload del flusso di lavoro.

* saveToLocation: è la posizione in cui si desidera salvare gli allegati nel file system del server AEM.

Questi due valori vengono passati come argomenti del processo, come illustrato nella schermata seguente.

![PassaggioProcesso](assets/implement-process-step.gif)

Il servizio QueryBuilder viene utilizzato per eseguire query sui nodi di tipo `nt:file` nella cartella attachmentsPath. Il resto del codice scorre i risultati della ricerca per creare l&#39;oggetto Document e salvarlo nel file system.


>[!NOTE]
>
>Poiché si utilizza un oggetto Document specifico di AEM Forms, è necessario includere nel progetto Maven la dipendenza aemfd-client-sdk. L&#39;ID gruppo è `com.adobe.aemfd` e l&#39;ID artefatti è `aemfd-client-sdk`.

#### Creare e distribuire

[Crea il bundle come descritto qui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[Assicurarsi che il bundle sia distribuito e in stato attivo](http://localhost:4502/system/console/bundles)

Crea un modello di flusso di lavoro. Trascina e rilascia il passaggio del processo nel modello di flusso di lavoro. Associare la fase del processo con &quot;Salva allegati modulo adattivo nel file system&quot;.

Fornisci gli argomenti di processo necessari separati da una virgola. Ad esempio Allegati,c:\\scrappp\\. Il primo argomento è la cartella in cui verranno memorizzati gli allegati del modulo adattivo relativi al payload del flusso di lavoro. Questo deve essere lo stesso valore specificato durante la configurazione dell’azione di invio del modulo adattivo. Il secondo argomento è la posizione in cui si desidera memorizzare gli allegati.

Creare un modulo adattivo. Trascina il componente File allegati sul modulo. Configura l&#39;azione di invio del modulo per richiamare il flusso di lavoro creato nei passaggi precedenti. Specificare il percorso di collegamento appropriato.

Salva le impostazioni.

Visualizzare l&#39;anteprima del modulo. Aggiungi un paio di allegati e invia il modulo. Gli allegati devono essere salvati nel file system nel percorso specificato dall&#39;utente nel flusso di lavoro.
