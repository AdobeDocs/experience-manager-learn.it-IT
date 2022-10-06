---
title: Conversione di stringhe separate da virgola in array di stringhe nel flusso di lavoro AEM Forms
description: quando il modello dati del modulo dispone di un array di stringhe come uno dei parametri di input, sarà necessario massaggiare i dati generati dall’azione di invio di un modulo adattivo prima di richiamare l’azione di invio del modello dati del modulo.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
kt: 8507
exl-id: 9ad69407-2413-416f-9cec-43f88989b31d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# Conversione di una stringa separata da virgole in una matrice di stringhe {#setting-value-of-json-data-element-in-aem-forms-workflow}

Quando il modulo è basato su un modello di dati modulo con una matrice di stringhe come parametro di input, è necessario manipolare i dati del modulo adattivo inviati per inserire una matrice di stringhe. Ad esempio, se hai associato un campo casella di controllo a un elemento del modello dati del modulo di tipo matrice stringa, i dati del campo casella di controllo si trovano in un formato stringa separato da virgole. Il codice di esempio riportato di seguito mostra come sostituire la stringa separata da virgole con una matrice di stringhe.

## Creare un passaggio del processo

Un passaggio del processo viene utilizzato in un flusso di lavoro AEM quando si desidera che il flusso di lavoro esegua una determinata logica. Il passaggio del processo può essere associato a uno script ECMA o a un servizio OSGi. Il nostro passaggio di processo personalizzato esegue il servizio OSGi.

I dati inviati sono nel seguente formato. Il valore dell&#39;elemento businessUnits è una stringa separata da virgole che deve essere convertita in una matrice di stringa.

![dati inviati](assets/submitted-data-string.png)

I dati di input per l’endpoint rimanente associato al modello dati del modulo richiedono un array di stringhe come mostrato in questa schermata. Il codice personalizzato nel passaggio del processo converte i dati inviati nel formato corretto.

![fdm-string-array](assets/string-array-fdm.png)

Passiamo al passaggio del processo il percorso dell’oggetto JSON e il nome dell’elemento. Il codice nel passaggio del processo sostituisce i valori separati da virgola dell&#39;elemento in una matrice di stringhe.
![fase del processo](assets/create-string-array.png)

>[!NOTE]
>
>Assicurati che il percorso del file dati nelle opzioni di invio del modulo adattivo sia impostato su &quot;Data.xml&quot;. Questo perché il codice nel passaggio del processo cerca un file denominato Data.xml sotto la cartella payload.

## Codice fase del processo

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

@Component(property = {
    Constants.SERVICE_DESCRIPTION + "=Create String Array",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Replace comma seperated string with string array"
})

public class CreateStringArray implements WorkflowProcess {
    private static final Logger log = LoggerFactory.getLogger(CreateStringArray.class);
    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
        log.debug("The string I got was ..." + arg2.get("PROCESS_ARGS", "string").toString());
        String[] arguments = arg2.get("PROCESS_ARGS", "string").toString().split(",");
        String objectName = arguments[0];
        String propertyName = arguments[1];

        String objects[] = objectName.split("\\.");
        System.out.println("The params is " + propertyName);
        log.debug("The params string is " + objectName);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload  in set Elmement Value in Json is  " + workItem.getWorkflowData().getPayload().toString());
        String dataFilePath = payloadPath + "/Data.xml/jcr:content";
        Session session = workflowSession.adaptTo(Session.class);
        Node submittedDataNode = null;
        try {
            submittedDataNode = session.getNode(dataFilePath);

            InputStream submittedDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(submittedDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();

            String inputStr;
            while ((inputStr = streamReader.readLine()) != null)
                stringBuilder.append(inputStr);
            JsonParser jsonParser = new JsonParser();
            JsonObject jsonObject = jsonParser.parse(stringBuilder.toString()).getAsJsonObject();
            System.out.println("The json object that I got was " + jsonObject);
            JsonObject targetObject = null;

            for (int i = 0; i < objects.length - 1; i++) {
                System.out.println("The object name is " + objects[i]);
                if (i == 0) {
                    targetObject = jsonObject.get(objects[i]).getAsJsonObject();
                } else {
                    targetObject = targetObject.get(objects[i]).getAsJsonObject();

                }

            }

            System.out.println("The final object is " + targetObject.toString());
            String businessUnits = targetObject.get(propertyName).getAsString();
            System.out.println("The values of " + propertyName + " are " + businessUnits);

            JsonArray jsonArray = new JsonArray();

            String[] businessUnitsArray = businessUnits.split(",");
            for (String name: businessUnitsArray) {
                jsonArray.add(name);
            }

            targetObject.add(propertyName, jsonArray);
            System.out.println(" After updating the property " + targetObject.toString());
            InputStream is = new ByteArrayInputStream(jsonObject.toString().getBytes());
            System.out.println("The changed json data  is " + jsonObject.toString());
            Binary binary = session.getValueFactory().createBinary(is);
            submittedDataNode.setProperty("jcr:data", binary);
            session.save();

        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

    }
}
```

Il bundle campione può essere [scaricato da qui](assets/CreateStringArray.CreateStringArray.core-1.0-SNAPSHOT.jar)
