---
title: Servizi di utilità
description: Alcuni utili servizi di utilità per gli sviluppatori di AEM Forms
feature: Moduli adattivi
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 3%

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

Il bundle di esempio può essere [scaricato da qui](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Codice di esempio per utilizzare i servizi di utilità

Di seguito è riportato il codice utilizzato nella pagina JSP per creare org.w3c.dom.Document da stringa e convertire il documento e archiviarlo nell&#39;archivio CRX come mostrato nel seguente frammento di codice.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Prerequisiti


Sarà necessario distribuire [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e avviare il bundle.


Se desideri salvare i documenti nell&#39;archivio CRX utilizzando questi servizi di utilità, segui il [sviluppo con l&#39;articolo utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Assicurati di fornire all&#39;utente del servizio fd le [autorizzazioni richieste](http://localhost:4502/useradmin) sulle cartelle CRX appropriate.

