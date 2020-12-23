---
title: Servizi di utilità
description: Alcuni utili servizi di utilità per  sviluppatore AEM Forms
feature: document-services
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 243e26e2403e3d7816a0dd024ffbe73743827c7c
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Servizi di utilità

Questo pacchetto di esempio fornisce utili servizi di utilità che possono essere utilizzati da uno sviluppatore AEM Forms . Sono disponibili i seguenti servizi.


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

Il pacchetto di esempio può essere [scaricato da qui](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Codice di esempio per utilizzare i servizi di utilità

Di seguito è riportato il codice utilizzato nella pagina JSP per creare org.w3c.dom.Document da una stringa e convertire il documento e archiviarlo nell&#39;archivio CRX, come mostrato nel frammento di codice seguente.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Prerequisiti


Sarà necessario distribuire [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e avviare il bundle.


Se si desidera salvare i documenti nel repository CRX utilizzando questi servizi di utilità, seguire il [sviluppo con il servizio utente article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Assicuratevi di fornire all&#39;utente del servizio fd le [autorizzazioni ](http://localhost:4502/useradmin) necessarie nelle cartelle CRX appropriate.

