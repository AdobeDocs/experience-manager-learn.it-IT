---
title: Implementazione passo processo personalizzato
seo-title: Implementazione passo processo personalizzato
description: Scrittura di allegati di moduli adattivi nel file system tramite un passaggio di processo personalizzato
seo-description: Scrittura di allegati di moduli adattivi nel file system tramite un passaggio di processo personalizzato
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '895'
ht-degree: 0%

---


# Passaggio processo personalizzato

Questa esercitazione è destinata  clienti AEM Forms che devono implementare un passaggio di processo personalizzato. Un passaggio del processo può eseguire uno script ECMA o chiamare il codice Java personalizzato per eseguire le operazioni. Questa esercitazione spiega i passaggi necessari per implementare WorkflowProcess che viene eseguito dal passaggio del processo.

Il motivo principale per l&#39;implementazione del passaggio del processo personalizzato è l&#39;estensione del flusso di lavoro AEM. Ad esempio, se utilizzate  componenti AEM Forms nel modello di workflow, potete eseguire le operazioni seguenti

* Salvare gli allegati dei moduli adattivi nel file system
* Manipolare i dati inviati

Per eseguire il caso d’uso di cui sopra, in genere scriverete un servizio OSGi che viene eseguito dal passaggio del processo.

## Crea progetto Paradiso

Il primo passo è quello di creare un progetto di corvo utilizzando il Adobe appropriato Maven Archetype. I passaggi dettagliati sono elencati in questo [articolo](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Dopo aver importato il progetto &quot;Paradiso&quot; in eclissi, è possibile iniziare a scrivere il primo componente OSGi che può essere utilizzato nella fase di elaborazione.


### Crea classe che implementa WorkflowProcess

Aprite il progetto &quot;lievito&quot; nella vostra eclisse IDE. Espandete **project name** > cartella **core** . Espandete la cartella src/main/java. Dovresti vedere un pacchetto che termina con &quot;core&quot;. Crea una classe Java che implementa WorkflowProcess in questo pacchetto. Sarà necessario sostituire il metodo execute. La firma del metodo execute è come segue: void execute(WorkItem workItem, WorkflowSessionSession, MetaDataMap processArguments) genera WorkflowExceptionIl metodo execute dà accesso alle 3 variabili seguenti

**Elemento** di lavoro: La variabile workItem consente di accedere ai dati relativi al flusso di lavoro. La documentazione API pubblica è disponibile [qui.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Questa variabile workflowSession consente di controllare il flusso di lavoro. La documentazione API pubblica è disponibile [qui](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Tutti i metadati associati al flusso di lavoro. Qualsiasi argomento di processo passato al passaggio del processo è disponibile tramite l&#39;oggetto MetaDataMap.[Documentazione API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In questa esercitazione verranno scritti gli allegati aggiunti al modulo adattivo nel file system come parte del flusso di lavoro AEM.

Per eseguire questo esempio di utilizzo, è stata scritta la seguente classe Java

Diamo un&#39;occhiata a questo codice

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
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
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

Linea 1 - definisce le proprietà del componente. La proprietà process.label è ciò che verrà visualizzato quando si associa il componente OSGi al passaggio del processo, come illustrato in una delle schermate sottostanti.

Righe 13-15 - Gli argomenti di processo passati a questo componente OSGi vengono suddivisi utilizzando il separatore &quot;,&quot;. I valori di attachmentPath e saveToLocation vengono quindi estratti dall&#39;array di stringhe.

* attachmentPath - Si tratta della stessa posizione specificata nel modulo adattivo quando è stata configurata l&#39;azione di invio del modulo adattivo per richiamare AEM flusso di lavoro. Si tratta di un nome della cartella in cui si desidera salvare gli allegati in AEM rispetto al payload del flusso di lavoro.

* saveToLocation: si tratta del percorso in cui salvare gli allegati nel file system del server AEM.

Questi due valori vengono passati come argomenti di processo come mostrato nella schermata seguente.

![ProcessStep](assets/implement-process-step.gif)


Linea 19: viene quindi creato attachmentFilePath. Il percorso del file allegato è simile a

    /var/fd/dashboard/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Allegati/allegati

* &quot;Allegati&quot; è il nome della cartella relativa al payload del flusso di lavoro specificato al momento della configurazione dell&#39;opzione di invio del modulo adattivo.

   ![invii](assets/af-submit-options.gif)

Righe 24-26 - Get ResourceResolver e quindi la risorsa che punta a attachmentFilePath.

Il resto del codice crea oggetti Document eseguendo un&#39;iterazione attraverso l&#39;oggetto secondario della risorsa che punta ad attachmentFilePath utilizzando l&#39;API. Questo oggetto document è specifico di  AEM Forms. Quindi, utilizzare il metodo copyToFile dell&#39;oggetto document per salvare l&#39;oggetto document.

>[!NOTE]
Poiché si utilizza l&#39;oggetto Document specifico per  AEM Forms, è necessario includere nel progetto maven la dipendenza aemfd-client-sdk. L&#39;ID gruppo è com.adobe.aemfd e l&#39;ID artifact è aemfd-client-sdk.

#### Creazione e implementazione

[Creare il bundle come descritto qui](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)[Verificare che il bundle sia distribuito e in stato attivo](http://localhost:4502/system/console/bundles)

Creare un modello di workflow. Trascinate e rilasciate il passaggio del processo nel modello di workflow. Associare il passaggio del processo a &quot;Salva allegati di moduli adattivi nel file system&quot;.

Fornire gli argomenti di processo necessari separati da una virgola. Ad esempio Allegati,c:\\scrappp\\. Il primo argomento è la cartella in cui gli allegati del modulo adattivo verranno memorizzati in relazione al payload del flusso di lavoro. Deve corrispondere allo stesso valore specificato durante la configurazione dell&#39;azione di invio del modulo adattivo. Il secondo argomento è la posizione in cui memorizzare gli allegati.

Creare un modulo adattivo. Trascinare sul modulo il componente Allegati file. Configurare l&#39;azione di invio del modulo per richiamare il flusso di lavoro creato nei passaggi precedenti. Specificare il percorso dell&#39;allegato appropriato.

Salvate le impostazioni.

Visualizzare l’anteprima del modulo. Aggiungere un paio di allegati e inviare il modulo. Gli allegati devono essere salvati nel file system nel percorso specificato nel flusso di lavoro.

