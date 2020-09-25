---
title: Personalizza assegnazione notifica attività
description: Includi i dati del modulo nelle e-mail di notifica delle attività assegnate
sub-product: forms
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
translation-type: tm+mt
source-git-commit: c7ae9a51800bb96de24ad577863989053d53da6b
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Personalizza assegnazione notifica attività

Il componente Assegna attività viene utilizzato per assegnare le attività ai partecipanti al flusso di lavoro. Quando un&#39;attività viene assegnata a un utente o a un gruppo, una notifica e-mail viene inviata all&#39;utente o ai membri del gruppo definiti.
Questa notifica e-mail in genere contiene dati dinamici correlati all&#39;attività. Questi dati dinamici vengono recuperati utilizzando le proprietà [di](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification)metadati generate dal sistema.
Per includere i valori dei dati del modulo inviati nella notifica e-mail, è necessario creare una proprietà di metadati personalizzata e quindi utilizzare queste proprietà di metadati personalizzate nel modello e-mail



## Creazione di una proprietà di metadati personalizzata

L’approccio consigliato consiste nel creare un componente OSGI che implementa il metodo getUserMetadata di [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

Il codice seguente crea 4 proprietà di metadati (_firstName_,_lastName_,_reason_ e _amountRequired_) e imposta il relativo valore dai dati inviati. Ad esempio, il valore della proprietà di metadati _firstName_&#x200B;è impostato sul valore dell&#39;elemento denominato firstName dai dati inviati. Il codice seguente presuppone che i dati inviati dal modulo adattivo siano in formato xml. Forms adattivo basato su schema JSON o modello dati modulo genera dati in formato JSON.


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## Utilizzare le proprietà di metadati personalizzate nel modello e-mail di notifica delle attività

Nel modello e-mail potete includere la proprietà metadata utilizzando la sintassi seguente, dove amountRequired è la proprietà metadata `${amountRequested}`

## Configurare Assegna attività per utilizzare la proprietà dei metadati personalizzata

Dopo che il componente OSGi è stato integrato e distribuito AEM server, configurate il componente Assegna attività come illustrato di seguito per utilizzare le proprietà dei metadati personalizzate.


![Notifica attività](assets/task-notification.PNG)

## Abilitare l&#39;uso di proprietà di metadati personalizzate

![Proprietà metadati personalizzate](assets/custom-meta-data-properties.PNG)

## Per provare questo sul server

* [Configura servizio di posta CQ Day](https://docs.adobe.com/content/help/en/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* Associare un ID e-mail valido a un utente [amministratore](http://localhost:4502/security/users.html)
* Scaricate e installate il modello [Workflow e notifica](assets/workflow-and-task-notification-template.zip) tramite [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Scaricate il modulo [](assets/request-travel-authorization.zip) adattivo e importatelo in AEM dai [moduli e dai documenti UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Implementare e avviare il pacchetto [personalizzato](assets/work-items-user-service-bundle.jar) utilizzando la console [Web](http://localhost:4502/system/console/bundles)
* [Anteprima e invio del modulo](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Al momento dell’invio del modulo, la notifica di assegnazione dell’attività viene inviata all’ID e-mail associato all’utente amministratore. La schermata seguente mostra un esempio di notifica di assegnazione delle attività

![Notifica](assets/task-nitification-email.png)

>[!NOTE]
>Il modello e-mail per la notifica dell&#39;attività di assegnazione deve essere nel seguente formato.
>
> oggetto=Attività assegnata - `${workitem_title}`
>
> message=Stringa che rappresenta il modello e-mail senza nuovi caratteri di riga.
