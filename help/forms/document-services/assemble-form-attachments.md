---
title: Assemblare gli allegati del modulo
description: Assemblare gli allegati del modulo nell’ordine specificato
feature: Assembler
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 0%

---

# Assemblare gli allegati del modulo

Questo articolo fornisce risorse per assemblare gli allegati dei moduli adattivi in un ordine specificato. Affinché questo codice di esempio funzioni, gli allegati al modulo devono essere in formato pdf. Di seguito è riportato il caso d’uso.
L’utente che compila un modulo adattivo allega uno o più documenti pdf al modulo.
All’invio del modulo, assemblare gli allegati del modulo per generare un pdf. È possibile specificare l&#39;ordine in cui gli allegati vengono assemblati per generare il pdf finale.

## Crea un componente OSGi che implementa l&#39;interfaccia WorkflowProcess

Creare un componente OSGi che implementa il [com.adobe.granite.workflow.exec.Interfaccia WorkflowProcess](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). Il codice di questo componente può essere associato al componente del passaggio del processo nel flusso di lavoro AEM. Il metodo execute dell&#39;interfaccia com.adobe.granite.workflow.exec.WorkflowProcess è implementato in questo componente.

Quando un modulo adattivo viene inviato per attivare un flusso di lavoro AEM, i dati inviati vengono memorizzati nel file specificato sotto la cartella payload. Ad esempio, questo è il file di dati inviato. È necessario assemblare gli allegati specificati sotto il tag idcard e bankstatement.
![dati inviati](assets/submitted-data.JPG).

### Ottenere i nomi dei tag

L’ordine degli allegati viene specificato come argomenti del passaggio del processo nel flusso di lavoro, come mostrato nella schermata sottostante. Qui stiamo assemblando gli allegati aggiunti alla carta d&#39;identità campo seguito da estratti conto

![fase del processo](assets/process-step.JPG)

Il frammento di codice seguente estrae i nomi degli allegati dagli argomenti del processo

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Crea DDX dai nomi degli allegati

Dobbiamo quindi creare [Descrizione documento XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) documento utilizzato dal servizio Assembler per assemblare i documenti. Di seguito è riportato il DDX creato dagli argomenti del processo. L’elemento NoForms ti consente di appiattire i documenti basati su XFA prima che vengano assemblati. Gli elementi di origine PDF sono nell’ordine corretto come specificato negli argomenti del processo.

![dx-xml](assets/ddx.PNG)

### Crea mappa dei documenti

Quindi creiamo una mappa di documenti con il nome dell&#39;allegato come chiave e l&#39;allegato come valore. Il servizio Query Builder è stato utilizzato per eseguire query sugli allegati sotto il percorso del payload e creare la mappa dei documenti. Questa mappa del documento insieme al DDX è necessaria per il servizio assembler per assemblare il pdf finale.

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### Utilizzare AssemblerService per assemblare i documenti

Dopo la creazione del DDX e della mappa del documento, il passaggio successivo è l&#39;utilizzo di AssemblerService per assemblare i documenti.
Il codice seguente assembla e restituisce il pdf assemblato.

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### Salva il pdf assemblato sotto la cartella payload

Il passaggio finale consiste nel salvare il pdf assemblato sotto la cartella payload. È quindi possibile accedere a questo pdf nei passaggi successivi del flusso di lavoro per un’ulteriore elaborazione.
Il seguente frammento di codice è stato utilizzato per salvare il file sotto la cartella payload

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

Di seguito è riportata la struttura della cartella payload dopo che gli allegati del modulo sono stati assemblati e memorizzati.

![struttura del carico utile](assets/payload-structure.JPG)

### Per far funzionare questa funzionalità sul server AEM

* Scarica la [Modulo di assemblaggio allegati modulo](assets/assemble-form-attachments-af.zip) al sistema locale.
* Importa il modulo dal[Forms E Documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) pagina.
* Scarica [workflow](assets/assemble-form-attachments.zip) e importa in AEM utilizzando il gestore di pacchetti.
* Scarica la [bundle personalizzato](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* Distribuisci e avvia il bundle utilizzando [console web](http://localhost:4502/system/console/bundles)
* Posiziona il browser su [Modulo AssembleAttachments](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* Aggiungere un allegato nel documento ID e un paio di documenti pdf alla sezione rendiconto bancario
* Invia il modulo per attivare il flusso di lavoro
* Controlla il flusso di lavoro [cartella payload in crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) per il pdf assemblato

>[!NOTE]
> Se hai abilitato logger per il bundle personalizzato, il DDX e il file assemblato vengono scritti nella cartella dell&#39;installazione AEM.
