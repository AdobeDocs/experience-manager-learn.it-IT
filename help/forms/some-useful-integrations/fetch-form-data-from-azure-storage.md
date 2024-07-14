---
title: Archivia invio modulo in Azure Storage
description: Archiviare i dati del modulo in Azure Storage tramite API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 78
exl-id: 77f93aad-0cab-4e52-b0fd-ae5af23a13d0
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Recupera dati dall’archiviazione Azure

Questo articolo illustra come compilare un modulo adattivo con i dati memorizzati nell’archivio di Azure.
Si presume che i dati del modulo adattivo siano stati memorizzati nell’archivio di Azure e che ora si desideri precompilare il modulo adattivo con tali dati.
>[!NOTE]
>Il codice di questo articolo non funziona con un modulo adattivo basato su componenti core.[L&#39;articolo equivalente per il modulo adattivo basato su componenti core è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/prefill-form-with-data-attachments/introduction.html?lang=en)


## Crea richiesta GET

Il passaggio successivo consiste nel scrivere il codice per recuperare i dati dall’archiviazione di Azure utilizzando l’ID BLOB. Per recuperare i dati è stato scritto il seguente codice. L’URL è stato costruito utilizzando i valori sasToken e storageURI della configurazione OSGi e il blobID passato alla funzione getBlobData

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Quando viene eseguito il rendering di un modulo adattivo con un parametro GUID nell’URL, il componente pagina personalizzato associato al modello recupera e popola il modulo adattivo con i dati dall’archiviazione di Azure.
Di seguito è riportato il codice nell’JSP del componente Pagina associato al modello

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## Testare la soluzione

* [Distribuire il bundle OSGi personalizzato](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importa il modello di modulo adattivo personalizzato e il componente Pagina associato al modello](./assets/store-and-fetch-from-azure.zip)

* [Importare il modulo adattivo di esempio](./assets/bank-account-sample-form.zip)

* [Specificare i valori appropriati nella configurazione del portale di Azure utilizzando la console di configurazione OSGi.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/some-useful-integrations/store-form-data-in-azure-storage.html?lang=en#provide-the-blob-sas-token-and-storage-uri)

* [Anteprima e invio del modulo BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Verifica che i dati siano archiviati nel contenitore di archiviazione Azure desiderato. Copia l’ID BLOB.

* [Visualizza l&#39;anteprima del modulo BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) e specifica l&#39;ID BLOB come parametro GUID nell&#39;URL per il modulo da precompilare con i dati dall&#39;archiviazione di Azure
