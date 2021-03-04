---
title: Utilizzo del servizio Assembler in AEM Forms
seo-title: Utilizzo del servizio Assembler in AEM Forms
description: Utilizzo del servizio Assembler in AEM Forms per assemblare più file pdf
seo-description: Utilizzo del servizio Assembler in AEM Forms per assemblare più file pdf
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: Assemblatore
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 3%

---


# Utilizzo del servizio Assembler in AEM Forms{#using-assembler-service-in-aem-forms}

Questo articolo fornisce le risorse per dimostrare la capacità di trascinare e rilasciare più file PDF nel browser e salvare il file pdf assemblato sul tuo file system. Di seguito è riportato il codice del servlet che assembla i file pdf caricati utilizzando il browser.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

Per far funzionare questa funzionalità sul server AEM

* Scarica il file [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) nel sistema locale.
* Carica e installa il pacchetto utilizzando il [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Download[Bundle servizi documenti personalizzati](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Scarica [Sviluppo con Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Distribuisci e avvia i bundle utilizzando la [console web felix](http://localhost:4502/system/console/bundles)
* Posiziona il browser su [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* Trascinare un paio di file PDF

>[!NOTE]
>
>Assicurati che l’installazione di AEM Forms sia completa. Tutti i tuoi bundle devono essere in stato attivo.
>
>Assicurati di aver aggiunto le librerie RSA e BouncyCastle come indicato in questo [Installazione di AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**Avvertenze per questa demo**
>
> * Il codice non gestisce documenti PDF basati su XFA
   >
   > 
* Assicurati di trascinare e rilasciare solo i file PDF
>
>







