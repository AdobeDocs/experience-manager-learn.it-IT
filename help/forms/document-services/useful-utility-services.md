---
title: Servizi di utilità utili
description: Alcuni utili servizi di utilità per sviluppatori AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

Di seguito è riportato il codice utilizzato nella pagina JSP per creare org.w3c.dom.Document da una stringa e convertire il documento e memorizzarlo nell’archivio CRX, come illustrato nel seguente frammento di codice.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Prerequisiti


È necessario distribuire [Sviluppo con ServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e avvia il bundle.


Se desideri salvare i documenti nell’archivio CRX utilizzando questo servizio di utilità, segui la [articolo sviluppo con utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Assicurati di fornire [autorizzazioni richieste](http://localhost:4502/useradmin) sulle cartelle CRX appropriate per l&#39;utente fd-service.
