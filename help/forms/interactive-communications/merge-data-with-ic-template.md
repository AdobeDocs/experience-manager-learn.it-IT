---
title: Generazione del documento del canale di stampa mediante unione dei dati
seo-title: Generazione del documento del canale di stampa mediante unione dei dati
description: Scopri come generare il documento del canale di stampa unendo i dati contenuti nel flusso di input
seo-description: Scopri come generare il documento del canale di stampa unendo i dati contenuti nel flusso di input
feature: Comunicazione interattiva
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 1%

---

# Genera documenti del canale di stampa utilizzando i dati inviati

I documenti del canale di stampa vengono generalmente generati recuperando i dati da un’origine dati back-end tramite il servizio get del modello di dati del modulo. In alcuni casi, potrebbe essere necessario generare documenti del canale di stampa con i dati forniti. Ad esempio, il cliente compila la modifica del modulo beneficiario e può essere utile generare un documento del canale di stampa con i dati del modulo inviato. Per eseguire questo caso d’uso è necessario seguire i seguenti passaggi

## Crea servizio di precompilazione

Il nome di servizio &quot;ccm-print-test&quot; verrà utilizzato per accedere a questo servizio . Una volta definito questo servizio di pre-compilazione, puoi accedere a questo servizio nell’implementazione del processo del servlet o del flusso di lavoro per generare il documento del canale di stampa.

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

Lo snippet di codice di implementazione workflowProcess è mostrato di seguito.Questo codice viene eseguito quando la fase di processo nel flusso di lavoro AEM è associata a questa implementazione. Questa implementazione richiede 3 argomenti di processo descritti di seguito:

* Nome del percorso DataFile specificato durante la configurazione del modulo adattivo
* Nome del modello del canale di stampa
* Nome del documento del canale di stampa generato

Linea 98 - Poiché il modulo adattivo è basato su Form Data Model, vengono estratti i dati che risiedono nel nodo dati di afBoundData.
Riga 128 - Il nome del servizio Opzioni dati è impostato. Nota il nome del servizio. Deve corrispondere al nome restituito nella riga 45 dell’elenco di codici precedente.
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

Per eseguire il test sul server, segui i seguenti passaggi:

* [Configura il servizio di posta Day CQ.](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) Questo è necessario per inviare un messaggio e-mail con il documento generato come allegato.
* [Distribuire lo sviluppo con Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Assicurati di aver aggiunto la seguente voce nella configurazione del servizio User Mapper di Apache Sling Service
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [Scarica e decomprimi le risorse correlate a questo articolo nel file system](assets/prefillservice.zip)
* [Importa i pacchetti seguenti utilizzando Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Distribuisci quanto segue utilizzando AEM console Web Felix](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Questo bundle contiene il codice menzionato in questo articolo.

* [Apri ChangeOfBeneficaryForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Assicurati che il modulo adattivo sia configurato per l’invio a AEM flusso di lavoro come mostrato di seguito
   ![immagine](assets/generateic.PNG)
* [Configura il modello di flusso di lavoro.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Assicurati che il passaggio del processo e l’invio di componenti e-mail siano configurati in base all’ambiente in uso
* [Visualizzare l&#39;anteprima di ChangeOfBeneficaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) Compila alcuni dettagli e invia
* Il flusso di lavoro deve essere richiamato e il documento del canale di stampa IC deve essere inviato al destinatario specificato nel componente Invia e-mail come allegato
