---
title: Tutorial per aggiungere i tag di metadati specificati dall’utente
description: Crea un invio personalizzato per memorizzare i dati del modulo con i tag di metadati in Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14501
duration: 75
exl-id: 5cd5e37e-9881-4fce-a0cb-402d738f83ae
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---

# Creare un invio personalizzato per gestire l’invio del modulo

AEM Forms CS fornisce un’azione di invio predefinita per memorizzare i dati del modulo in Azure, ma non dispone della funzionalità per creare tag di indice BLOB nel BLOB. Per soddisfare il caso d’uso, è stato creato un servizio di invio personalizzato per memorizzare i dati inviati in Azure e creare tag di dati di indice BLOB utilizzando i campi contrassegnati come ricercabili nel modulo.

[Il gestore di invio personalizzato di esempio per il modulo adattivo basato su componenti core è disponibile qui](https://github.com/adobe/aem-core-forms-components/blob/master/it/core/src/main/java/com/adobe/cq/forms/core/components/it/service/CustomAFSubmitService.java#L56). Il seguente invio personalizzato è stato scritto per gestire l’invio del modulo

```java
package com.aemforms.saveandfecthfromazure.prefill;

import com.adobe.aemds.guide.common.GuideValidationResult;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.adobe.forms.common.service.FileAttachmentWrapper;
import com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage;
import org.apache.commons.lang3.StringUtils;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.UUID;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class CustomAFSubmitService implements FormSubmitActionService {
    private static final String serviceName = "Store Form Submission with Metadata tags in Azure";
    private static Logger logger = LoggerFactory.getLogger(CustomAFSubmitService.class);

    @Reference
    DataManager dataManager;
    @Reference
    StoreAndFetchDataFromAzureStorage storeAndFetchDataFromAzureStorage;

    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        Map<String, Object> result = new HashMap<>();
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.FALSE);
        try {

            String guideContainerPath = formSubmitInfo.getFormContainerPath();

            String submittedFormName = formSubmitInfo.getFormContainerResource().getParent().getParent().getName();
            logger.debug("The submitted form name is "+submittedFormName);
            String data = formSubmitInfo.getData();
            logger.debug(data);
            String metadataTags = storeAndFetchDataFromAzureStorage.getMetaDataTags(submittedFormName,formSubmitInfo.getFormContainerPath(),formSubmitInfo.getFormContainerResource().getResourceResolver().adaptTo(Session.class),data);
            String uniqueID = UUID.randomUUID().toString();
            logger.debug("The metadata tags are  "+metadataTags);
            // The code for saveFormDatainAzure is listed in this article
            logger.debug(storeAndFetchDataFromAzureStorage.saveFormDatainAzure(data,metadataTags));
            
            if(dataManager != null) {
                dataManager.put(uniqueID, data);
               
            }
            logger.info("AF Submission successful using custom submit service for: {}", guideContainerPath);
            result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
            result.put(DataManager.UNIQUE_ID, uniqueID);
            // adding id here so that this available in redirect parameters in final thank you page
            Map<String, Object> redirectParamMap = new HashMap<String, Object>() {{
                put(DataManager.UNIQUE_ID, uniqueID);
            }};
            // todo: move this to constant, once forms SDK is released
            result.put("fd:redirectParameters", redirectParamMap);
        } catch (Exception ex) {
            logger.error("Error while using the AF Submit service", ex);
            GuideValidationResult guideValidationResult = new GuideValidationResult();
            guideValidationResult.setOriginCode("500");
            guideValidationResult.setErrorMessage("Internal server error");
            result.put(GuideConstants.FORM_SUBMISSION_ERROR, guideValidationResult);
        }
        return result;
    }

}
```

## Implementazione del servizio saveFormDatainAzure

```java
    @Override
    public String saveFormDatainAzure(String formData,String metaDataTags) {
        String sasToken = azurePortalConfigurationService.getSASToken();
        String storageURI = azurePortalConfigurationService.getStorageURI();
        logger.debug("The SAS Token is "+sasToken);
        logger.debug("The Storage URL is "+storageURI);
        logger.debug("The form data is "+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        String putRequestURL = storageURI+uuid.toString();
        putRequestURL = putRequestURL+sasToken;
        logger.debug("The put Request "+putRequestURL);
        HttpPut httpPut = new HttpPut(putRequestURL);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        httpPut.addHeader("x-ms-tags",metaDataTags);

        try
        {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            logger.debug("Response code "+response.getStatusLine().getStatusCode());
            if(response.getStatusLine().getStatusCode() == 201)
            {
                return uuid.toString();
            }
        }
        catch (IOException e)
        {
            logger.debug("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return null;
    }
```
