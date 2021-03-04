---
title: Implementazione passaggio del processo personalizzato
seo-title: Implementazione passaggio del processo personalizzato
description: Scrittura di allegati di moduli adattivi nel file system utilizzando un passaggio del processo personalizzato
seo-description: Scrittura di allegati di moduli adattivi nel file system utilizzando un passaggio del processo personalizzato
feature: Flusso di lavoro
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '899'
ht-degree: 0%

---


# Passaggio processo personalizzato

Questa esercitazione è destinata ai clienti di AEM Forms che devono implementare un passaggio di processo personalizzato. Un passaggio del processo può eseguire uno script ECMA o chiamare un codice Java personalizzato per eseguire le operazioni. Questa esercitazione illustra i passaggi necessari per implementare WorkflowProcess che viene eseguito dal passaggio del processo.

Il motivo principale per l’implementazione del passaggio del processo personalizzato è l’estensione del flusso di lavoro AEM. Ad esempio, se utilizzi componenti di AEM Forms nel modello di flusso di lavoro, puoi eseguire le operazioni seguenti

* Salvare gli allegati dei moduli adattivi nel file system
* Manipolare i dati inviati

Per eseguire il caso d’uso di cui sopra, in genere scriverai un servizio OSGi che viene eseguito dal passaggio del processo.

## Crea progetto Maven

Il primo passo è quello di creare un progetto Maven utilizzando il tipo di archivio Adobe Maven appropriato. I passaggi dettagliati sono elencati in questo [articolo](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Una volta importato il progetto Maven in eclipse, puoi iniziare a scrivere il tuo primo componente OSGi che può essere utilizzato nel passaggio del processo.


### Crea una classe che implementa WorkflowProcess

Apri il progetto Maven nell’ambiente IDE. Espandi la cartella **project name** > **core** . Espandi la cartella src/main/java . Dovresti visualizzare un pacchetto che termina con &quot;core&quot;. Crea una classe Java che implementa WorkflowProcess in questo pacchetto. Sarà necessario sovrascrivere il metodo execute . La firma del metodo execute è la seguente
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)genera WorkflowException
Il metodo execute permette di accedere alle seguenti 3 variabili

**Elemento** di lavoro: La variabile workItem consente di accedere ai dati relativi al flusso di lavoro. La documentazione API pubblica è disponibile [qui.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Questa variabile workflowSession ti darà la possibilità di controllare il flusso di lavoro. La documentazione API pubblica è disponibile [qui](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Tutti i metadati associati al flusso di lavoro. Tutti gli argomenti di processo passati al passaggio del processo sono disponibili utilizzando l&#39;oggetto MetaDataMap .[Documentazione API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In questa esercitazione, scriveremo gli allegati aggiunti a Modulo adattivo nel file system come parte del flusso di lavoro AEM.

Per eseguire questo caso d’uso, è stata scritta la seguente classe java

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

Linea 1: definisce le proprietà del componente. La proprietà process.label è ciò che verrà visualizzato quando si associa il componente OSGi al passaggio del processo, come mostrato in una delle schermate seguenti.

Righe 13-15 - Gli argomenti di processo passati a questo componente OSGi vengono divisi utilizzando il separatore &quot;,&quot;. I valori di attachmentPath e saveToLocation vengono quindi estratti dall&#39;array di stringhe.

* attachmentPath: si tratta della stessa posizione specificata nel Modulo adattivo quando hai configurato l&#39;azione di invio del Modulo adattivo per richiamare AEM Workflow. Questo è un nome della cartella che desideri salvare gli allegati in AEM rispetto al payload del flusso di lavoro.

* saveToLocation : percorso in cui salvare gli allegati nel file system del server AEM.

Questi due valori vengono passati come argomenti di processo come mostrato nella schermata seguente.

![ProcessStep](assets/implement-process-step.gif)


Linea 19: quindi costruiamo l&#39;attachmentFilePath. Il percorso del file allegato è simile al

    /var/fd/dashboard/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Allegati/allegati

* Gli &quot;Allegati&quot; sono il nome della cartella relativo al payload del flusso di lavoro specificato al momento della configurazione dell’opzione di invio del modulo adattivo.

   ![sottomissioni](assets/af-submit-options.gif)

Righe 24-26 - Ottieni ResourceResolver e poi la risorsa che punta al attachmentFilePath.

Il resto del codice crea oggetti Document ripetendo attraverso l&#39;oggetto secondario della risorsa che punta ad attachmentFilePath utilizzando l&#39;API. Questo oggetto documento è specifico di AEM Forms. Utilizzare quindi il metodo copyToFile dell&#39;oggetto documento per salvare l&#39;oggetto documento.

>[!NOTE]
>
>Poiché si utilizza l’oggetto Document specifico per AEM Forms, è necessario includere nel progetto maven la dipendenza aemfd-client-sdk. L&#39;ID del gruppo è com.adobe.aemfd e l&#39;id dell&#39;artefatto è aemfd-client-sdk.

#### Creare e distribuire

[Crea il bundle come descritto ](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[quiAssicurati che il bundle sia distribuito e in stato attivo](http://localhost:4502/system/console/bundles)

Crea un modello di flusso di lavoro. Trascina il passaggio del processo nel modello di flusso di lavoro. Associa il passaggio del processo a &quot;Salva allegati di moduli adattivi nel file system&quot;.

Fornire gli argomenti di processo necessari separati da una virgola. Ad esempio Allegati, c:\\scrappp\\. Il primo argomento è la cartella in cui gli allegati del modulo adattivo verranno memorizzati in relazione al payload del flusso di lavoro. Deve essere lo stesso valore specificato durante la configurazione dell’azione di invio del modulo adattivo. Il secondo argomento è la posizione in cui si desidera memorizzare gli allegati.

Creare un modulo adattivo. Trascinare il componente File allegati sul modulo. Configura l’azione di invio del modulo per richiamare il flusso di lavoro creato nei passaggi precedenti. Fornire il percorso allegato appropriato.

Salva le impostazioni.

Visualizzare l’anteprima del modulo. Aggiungere un paio di allegati e inviare il modulo. Gli allegati devono essere salvati nel file system nel percorso specificato nel flusso di lavoro.

