---
title: Implementazione passaggio del processo personalizzato
description: Scrittura di allegati di moduli adattivi nel file system utilizzando un passaggio del processo personalizzato
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---

# Passaggio processo personalizzato

Questa esercitazione è destinata ai clienti AEM Forms che devono implementare un passaggio di processo personalizzato. Un passaggio del processo può eseguire uno script ECMA o chiamare un codice Java personalizzato per eseguire le operazioni. Questa esercitazione illustra i passaggi necessari per implementare WorkflowProcess che viene eseguito dal passaggio del processo.

Il motivo principale per l&#39;implementazione del passaggio del processo personalizzato è l&#39;estensione del flusso di lavoro AEM. Ad esempio, se utilizzi componenti AEM Forms nel modello di flusso di lavoro, puoi eseguire le operazioni seguenti

* Salvare gli allegati dei moduli adattivi nel file system
* Manipolare i dati inviati

Per eseguire il caso d’uso di cui sopra, in genere scriverai un servizio OSGi che viene eseguito dal passaggio del processo.

## Crea progetto Maven

Il primo passo è quello di creare un progetto Maven utilizzando il tipo di archivio Maven Adobe appropriato. I passaggi dettagliati sono elencati in questo [articolo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Una volta importato il progetto Maven in eclipse, puoi iniziare a scrivere il tuo primo componente OSGi che può essere utilizzato nel passaggio del processo.


### Crea una classe che implementa WorkflowProcess

Apri il progetto Maven nell’ambiente IDE. Espandi **nome progetto** > **nucleo centrale** cartella. Espandi la cartella src/main/java . Dovresti visualizzare un pacchetto che termina con &quot;core&quot;. Crea una classe Java che implementa WorkflowProcess in questo pacchetto. Sarà necessario sovrascrivere il metodo execute . La firma del metodo execute è come segue public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException Il metodo execute dà accesso alle seguenti 3 variabili

**ElementoLavoro**: La variabile workItem consente di accedere ai dati relativi al flusso di lavoro. La documentazione API pubblica è disponibile [qui.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Questa variabile workflowSession ti darà la possibilità di controllare il flusso di lavoro. La documentazione API pubblica è disponibile [qui](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Tutti i metadati associati al flusso di lavoro. Tutti gli argomenti di processo passati al passaggio del processo sono disponibili utilizzando l&#39;oggetto MetaDataMap .[Documentazione API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In questa esercitazione verranno scritti gli allegati aggiunti al modulo adattivo nel file system come parte del flusso di lavoro AEM.

Per eseguire questo caso d’uso, è stata scritta la seguente classe java

Diamo un&#39;occhiata a questo codice

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

Linea 1: definisce le proprietà del componente. La proprietà process.label è ciò che verrà visualizzato quando si associa il componente OSGi al passaggio del processo, come mostrato in una delle schermate seguenti.

Righe 13-15 - Gli argomenti di processo passati a questo componente OSGi vengono divisi utilizzando il separatore &quot;,&quot;. I valori di attachmentPath e saveToLocation vengono quindi estratti dall&#39;array di stringhe.

* attachmentPath: si tratta della stessa posizione specificata nel modulo adattivo quando hai configurato l&#39;azione di invio del modulo adattivo per richiamare AEM flusso di lavoro. Si tratta di un nome della cartella in cui si desidera salvare gli allegati in AEM rispetto al payload del flusso di lavoro.

* saveToLocation : percorso in cui salvare gli allegati nel file system del server di AEM.

Questi due valori vengono passati come argomenti di processo come mostrato nella schermata seguente.

![ProcessStep](assets/implement-process-step.gif)

Il servizio QueryBuilder viene utilizzato per eseguire query sui nodi di tipo nt:file nella cartella attachmentPath. Il resto del codice esegue un&#39;iterazione attraverso i risultati della ricerca per creare un oggetto Document e salvarlo nel file system


>[!NOTE]
>
>Poiché si utilizza un oggetto Document specifico per AEM Forms, è necessario includere nel progetto maven la dipendenza aemfd-client-sdk. L&#39;ID del gruppo è com.adobe.aemfd e l&#39;id dell&#39;artefatto è aemfd-client-sdk.

#### Creare e distribuire

[Crea il bundle come descritto qui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[Assicurati che il bundle sia distribuito e in stato attivo](http://localhost:4502/system/console/bundles)

Crea un modello di flusso di lavoro. Trascina il passaggio del processo nel modello di flusso di lavoro. Associa il passaggio del processo a &quot;Salva allegati di moduli adattivi nel file system&quot;.

Fornire gli argomenti di processo necessari separati da una virgola. Ad esempio Allegati, c:\\scrappp\\. Il primo argomento è la cartella in cui gli allegati del modulo adattivo verranno memorizzati in relazione al payload del flusso di lavoro. Deve essere lo stesso valore specificato durante la configurazione dell’azione di invio del modulo adattivo. Il secondo argomento è la posizione in cui si desidera memorizzare gli allegati.

Creare un modulo adattivo. Trascinare il componente File allegati sul modulo. Configura l’azione di invio del modulo per richiamare il flusso di lavoro creato nei passaggi precedenti. Fornire il percorso allegato appropriato.

Salva le impostazioni.

Visualizzare l’anteprima del modulo. Aggiungere un paio di allegati e inviare il modulo. Gli allegati devono essere salvati nel file system nel percorso specificato nel flusso di lavoro.
