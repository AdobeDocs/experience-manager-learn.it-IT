---
title: Archivia invio modulo in Azure Storage
description: Archiviare i dati del modulo in Azure Storage tramite API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
kt: 14238
source-git-commit: 5e761ef180182b47c4fd2822b0ad98484db23aab
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Recupera dati dall’archiviazione Azure

Questo articolo illustra come compilare un modulo adattivo con i dati memorizzati nell’archivio di Azure.
Si presume che i dati del modulo adattivo siano stati memorizzati nell’archivio di Azure e che ora si desideri precompilare il modulo adattivo con tali dati.

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

        log.error("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.error("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Quando viene eseguito il rendering di un modulo adattivo con un `guid` Nell’URL, il componente pagina personalizzato associato al modello recupera e popola il modulo adattivo con i dati dell’archiviazione di Azure.
Il componente page associato al modello ha il seguente codice JSP.

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

* Specifica i valori appropriati nella configurazione del portale di Azure utilizzando la console di configurazione OSGi
* [Anteprima e invio del modulo BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Verifica che i dati siano archiviati nel contenitore di archiviazione Azure desiderato. Copia l’ID BLOB.
* [Anteprima del modulo BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) e specifica l’ID BLOB come parametro GUID nell’URL del modulo da precompilare con i dati dell’archiviazione Azure

