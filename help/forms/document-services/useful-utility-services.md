---
title: Servizi di utilità utili
description: Alcuni utili servizi di utilità per sviluppatori AEM Forms
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Servizi di utilità utili

Questo bundle di esempio fornisce utili servizi di utilità che possono essere utilizzati da uno sviluppatore AEM Forms. Sono disponibili i seguenti servizi.


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

Il bundle di esempio può essere [scaricato da qui](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Codice di esempio per l&#39;utilizzo dei servizi di utilità

Di seguito è riportato il codice utilizzato nella pagina JSP per creare org.w3c.dom.Document da una stringa e convertire il documento e memorizzarlo nell&#39;archivio CRX, come illustrato nel seguente frammento di codice.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Prerequisiti


È necessario distribuire [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar?lang=it) e avviare il bundle.


Se vuoi salvare i documenti nell&#39;archivio di CRX utilizzando questo servizio di utilità, segui l&#39;articolo [sviluppo con utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=it#adaptive-forms). Assicurati di fornire all&#39;utente fd-service le [autorizzazioni richieste](http://localhost:4502/useradmin) nelle cartelle di CRX appropriate.
