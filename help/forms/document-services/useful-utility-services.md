---
title: Servizi di utilità
description: Alcuni utili servizi di utilità per lo sviluppatore AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---

# Servizi di utilità

Questo bundle di esempio fornisce servizi di utilità utili che possono essere utilizzati da uno sviluppatore AEM Forms. Sono disponibili i seguenti servizi.


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

Il bundle campione può essere [scaricato da qui](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Codice di esempio per utilizzare i servizi di utilità

Di seguito è riportato il codice utilizzato nella pagina JSP per creare org.w3c.dom.Document da stringa e convertire il documento e archiviarlo nell&#39;archivio CRX come mostrato nel seguente frammento di codice.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Prerequisiti


Sarà necessario distribuire [SviluppoWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e avviare il bundle.


Se vuoi salvare i documenti nell&#39;archivio CRX utilizzando questi servizi di utilità, segui il [sviluppo con l’articolo utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Assicurati di fornire [autorizzazioni necessarie](http://localhost:4502/useradmin) sulle cartelle CRX appropriate per l&#39;utente del servizio fd.
