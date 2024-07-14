---
title: Assegnazione di tag e archiviazione di AEM Forms DoR in DAM
description: Questo articolo illustra il caso d’uso per l’archiviazione e l’assegnazione tag del documento record generato da AEM Forms in AEM DAM. L’assegnazione tag al documento viene eseguita in base ai dati del modulo inviato.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
duration: 191
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# Assegnazione di tag e archiviazione di AEM Forms DoR in DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Questo articolo illustra il caso d’uso per l’archiviazione e l’assegnazione tag del documento record generato da AEM Forms in AEM DAM. L’assegnazione tag al documento viene eseguita in base ai dati del modulo inviato.

I clienti chiedono spesso di archiviare e assegnare tag al documento record (DoR) generato da AEM Forms in AEM DAM. L’assegnazione tag del documento deve essere basata sui dati inviati da Adaptive Forms. Ad esempio, se lo stato di impiego nei dati inviati è &quot;Ritirato&quot;, si desidera assegnare al documento il tag &quot;Ritirato&quot; e archiviarlo in DAM.

Il caso d’uso è il seguente:

* Un utente compila un modulo adattivo. Nel modulo adattivo vengono acquisiti lo stato civile dell’utente (ex singolo) e lo stato di occupazione (ex ritirato).
* All’invio del modulo, viene attivato un flusso di lavoro AEM. Questo flusso di lavoro assegna al documento i tag relativi allo stato civile (Single) e allo stato di impiego (Retired) e lo memorizza in DAM.
* Una volta memorizzato il documento in DAM, l’amministratore dovrebbe essere in grado di eseguire ricerche nel documento utilizzando questi tag. Ad esempio, la ricerca su Singolo o Ritirato recupera i DoR appropriati.

Per soddisfare questo caso d’uso è stato scritto un passaggio di processo personalizzato. In questo passaggio recuperiamo i valori degli elementi dati appropriati dai dati inviati. Costruiamo quindi la sezione tag utilizzando questo valore. Ad esempio, se il valore dell&#39;elemento stato civile è &quot;Single&quot;, il titolo del tag diventa **Peak:EmploymentStatus/Single. **Utilizzando l’API TagManager, troviamo il tag e lo applichiamo al DoR.

Di seguito è riportato il codice completo per assegnare tag e memorizzare il documento di record in AEM DAM.

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

Per fare in modo che questo esempio funzioni sul tuo sistema, segui i passaggi elencati di seguito:
* [Distribuire il bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che imposta i tag dai dati del modulo inviati.

* [Scarica il modulo adattivo di esempio](assets/tag-and-store-in-dam-adaptive-form.zip)

* [Vai a Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Fai clic su Crea | File Carica e carica il file tag-and-store-in-dam-adaptive-form.zip

* [Importa le risorse dell&#39;articolo](assets/tag-and-store-in-dam-assets.zip) tramite Gestione pacchetti AEM
* Apri il [modulo di esempio in modalità anteprima](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled). **Compila tutti i campi** e invia il modulo.
* [Vai alla cartella dei picchi in DAM](http://localhost:4502/assets.html/content/dam/Peak). Dovresti vedere DoR nella cartella Peak. Controllare le proprietà del documento. Deve essere contrassegnato in modo appropriato.
Complimenti!! L&#39;esempio è stato installato correttamente nel sistema

* Esaminiamo il [flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) che viene attivato all&#39;invio del modulo.
* Il primo passaggio del flusso di lavoro crea un nome file univoco concatenando il nome dei richiedenti e la provincia di residenza.
* Il secondo passaggio del flusso di lavoro passa la gerarchia dei tag e gli elementi dei campi modulo che devono essere taggati. Il passaggio del processo estrae il valore dai dati inviati e crea il titolo del tag che deve essere contrassegnato nel documento.
* Se desideri memorizzare il DoR in una cartella diversa in DAM, specifica il percorso della cartella utilizzando le proprietà di configurazione specificate nella schermata seguente.

Gli altri due parametri sono specifici per DoR e Percorso file di dati, come specificato nelle opzioni di invio del modulo adattivo. Assicurati che i valori specificati qui corrispondano ai valori specificati nelle opzioni di invio del modulo adattivo.

![Porta tag](assets/tag_dor_service_configuration.gif)
