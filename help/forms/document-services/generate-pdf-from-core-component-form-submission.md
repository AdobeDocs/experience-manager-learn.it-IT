---
title: Generare PDF con dati da un modulo adattivo basato su componenti core
description: Unire i dati dall’invio di moduli basati su componenti core con il modello XDP nel flusso di lavoro
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
source-git-commit: acfd982ce471c8510cb59ed4d353f1d47dcfb5dc
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Genera PDF con dati dall’invio di moduli basati su componenti core

Ecco il testo rivisto con l’iniziale maiuscola &quot;Componenti core&quot;:

Uno scenario tipico prevede la generazione di un PDF dai dati inviati tramite un modulo adattivo basato su Componenti core. Questi dati sono sempre in formato JSON. Per generare un PDF utilizzando l’API di Render PDF, è necessario convertire i dati JSON in formato XML. Il `toString` metodo di `org.json.XML` viene utilizzato per questa conversione. Per ulteriori informazioni, consulta [documentazione di `org.json.XML.toString` metodo](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## Moduli adattivi basati su schema JSON

Per creare uno schema JSON per il modulo adattivo, segui la procedura riportata di seguito:

### Genera dati di esempio per XDP

Per semplificare il processo, segui questi passaggi perfezionati:

1. Apri il file XDP in AEM Forms Designer.
1. Passa a &quot;File&quot; > &quot;Proprietà modulo&quot; > &quot;Anteprima&quot;.
1. Seleziona &quot;Generate Preview Data&quot; (Genera dati anteprima).
1. Fai clic su &quot;Genera&quot;.
1. Assegna un nome file significativo, ad esempio `form-data.xml`.

### Genera schema JSON dai dati XML

È possibile utilizzare qualsiasi strumento online gratuito per [convertire XML in JSON](https://jsonformatter.org/xml-to-jsonschema) utilizzando i dati XML generati nel passaggio precedente.

### Processo di flusso di lavoro personalizzato per convertire JSON in XML

Il codice fornito converte JSON in XML, memorizzando l’XML risultante in una variabile di processo del flusso di lavoro denominata `dataXml`.

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### Crea flusso di lavoro

Per gestire l’invio di un modulo, crea un flusso di lavoro che includa due passaggi:

1. Il passaggio iniziale utilizza un processo personalizzato per trasformare i dati JSON inviati in XML.
1. Il passaggio successivo genera un PDF combinando i dati XML con il modello XDP.

![json-to-xml](assets/json-to-xml-process-step.png)


## Distribuire il codice di esempio

Per eseguire il test sul server locale, segui questi passaggi semplificati:

1. [Scarica e installa il bundle personalizzato tramite la console web OSGi dell’AEM](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [Importare il pacchetto del flusso di lavoro](assets/workflow_to_render_pdf.zip).
1. [Importare il modulo adattivo di esempio e il modello XDP](assets/adaptive_form_and_xdp_template.zip).
1. [Anteprima del modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. Completa alcuni campi modulo.
1. Invia il modulo per avviare il flusso di lavoro AEM.
1. Trova il PDF renderizzato nella cartella del payload del flusso di lavoro.
