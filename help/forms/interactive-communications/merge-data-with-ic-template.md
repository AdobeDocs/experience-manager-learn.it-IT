---
title: Generazione del documento del canale di stampa mediante unione dei dati
seo-title: Generazione del documento del canale di stampa mediante unione dei dati
description: Scopri come generare un documento per il canale di stampa unendo i dati contenuti nel flusso di input
seo-description: Scopri come generare un documento per il canale di stampa unendo i dati contenuti nel flusso di input
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# Genera documenti del canale di stampa utilizzando i dati inviati

I documenti del canale di stampa vengono generalmente generati recuperando i dati da un&#39;origine dati back-end attraverso il servizio get del modello dati del modulo. In alcuni casi, potrebbe essere necessario generare documenti per canali di stampa con i dati forniti. Ad esempio, il cliente compila il cambiamento del modulo beneficiario e può essere necessario generare un documento per canale di stampa con i dati del modulo inviato. Per eseguire questo caso di utilizzo, è necessario seguire i seguenti passaggi

## Crea servizio di precompilazione

Il nome del servizio &quot;ccm-print-test&quot; verrà utilizzato per accedere a questo servizio. Una volta definito il servizio di pre-compilazione, è possibile accedere a questo servizio nell&#39;implementazione del servlet o del processo del flusso di lavoro per generare il documento del canale di stampa.

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### Crea implementazione WorkflowProcess

Il frammento di codice di implementazione WorkflowProcess è illustrato di seguito.Questo codice viene eseguito quando il passaggio del processo nel flusso di lavoro AEM è associato a questa implementazione. Questa implementazione prevede 3 argomenti di processo descritti di seguito:

* Nome del percorso DataFile specificato durante la configurazione del modulo adattivo
* Nome del modello del canale di stampa
* Nome del documento del canale di stampa generato

Riga 98 - Poiché il modulo adattivo è basato su un modello dati modulo, vengono estratti i dati che risiedono nel nodo dati di afBoundData.
Riga 128 - Il nome del servizio Opzioni dati è impostato. Prendete nota del nome del servizio. Deve corrispondere al nome restituito nella riga 45 dell&#39;elenco di codici precedente.
Linea 135 - Il documento viene generato utilizzando il metodo di rendering dell&#39;oggetto PrintChannel


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

Per eseguire il test sul server, attenetevi alla seguente procedura:

* [Configurare il servizio di posta elettronica Day CQ.](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) Questo è necessario per inviare un messaggio e-mail con il documento generato come allegato.
* [Distribuzione di Developing with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Accertatevi di aver aggiunto la seguente voce nella configurazione del servizio Mapper utente Apache Sling Service
* **DevelopingWithServiceUser.core:getformsresources ceresolver=fd-service**
* [Scaricate e decomprimete il file system delle risorse correlate a questo articolo](assets/prefillservice.zip)
* [Importare i pacchetti seguenti tramite Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Distribuzione di quanto segue tramite AEM Felix Web Console](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Questo bundle contiene il codice indicato in questo articolo.

* [Open ChangeOfBeneficialForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Verificate che il modulo adattivo sia configurato per l&#39;invio a AEM Workflow come mostrato di seguito
   ![immagine](assets/generateic.PNG)
* [Configurare il modello di workflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Assicurati che il passaggio del processo e l’invio di componenti e-mail siano configurati in base all’ambiente in uso
* [Visualizzare l&#39;anteprima di ChangeOfBeneficaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) Compila alcuni dettagli e invia
* Il flusso di lavoro deve essere richiamato e il documento del canale di stampa IC deve essere inviato al destinatario specificato nel componente Invia e-mail come allegato
