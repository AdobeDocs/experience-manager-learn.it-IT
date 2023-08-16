---
title: Archivia invio modulo in Azure Storage
description: Archiviare i dati del modulo in Azure Storage tramite API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
source-git-commit: 17f6148ce6f897052d9d13f23e3f1792646eb958
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Archivia gli invii di moduli nell’archiviazione Azure

Questo articolo illustra come effettuare chiamate REST per archiviare i dati AEM Forms inviati nell’archiviazione di Azure.
Per memorizzare i dati del modulo inviati nell’archiviazione di Azure, è necessario seguire la procedura seguente.

## Crea account di archiviazione Azure

[Accedi all’account del portale di Azure e crea un account di archiviazione](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Assegnare un nome significativo all&#39;account di archiviazione, fare clic su Revisione e quindi su Crea. In questo modo viene creato l&#39;account di archiviazione con tutti i valori predefiniti. Ai fini di questo articolo abbiamo denominato il nostro account di archiviazione `aemformstutorial`.

## Creare l’accesso condiviso

Per interagire con il contenitore di archiviazione Azure, verrà utilizzata la firma di accesso condiviso o il metodo SAS di autorizzazione.
Dalla pagina dell&#39;account di archiviazione nel portale, fare clic sulla voce di menu Firma di accesso condiviso a sinistra per aprire la nuova pagina delle impostazioni della chiave di firma di accesso condiviso. Accertati di specificare le impostazioni e la data di fine appropriata come mostrato nella schermata seguente e fai clic sul pulsante Genera SAS e stringa di connessione. Copia l&#39;URL SAS del servizio BLOB. Utilizzeremo questo URL per effettuare le nostre chiamate HTTP
![shared-access-keys](./assets/shared-access-signature.png)

## Crea contenitore

La prossima cosa da fare è creare un contenitore in cui memorizzare i dati provenienti dall’invio dei moduli.
Dalla pagina dell&#39;account di archiviazione, fare clic sulla voce di menu Contenitori a sinistra e creare un contenitore denominato `formssubmissions`. Assicurati che il livello di accesso pubblico sia impostato su privato
![contenitore](./assets/new-container.png)

## Crea richiesta PUT

Il passaggio successivo consiste nel creare una richiesta PUT per memorizzare i dati del modulo inviati nell’archiviazione di Azure. Dovremo modificare l’URL SAS del servizio BLOB per includere il nome del contenitore e l’ID BLOB nell’URL. Ogni invio di moduli deve essere identificato da un ID BLOB univoco. L’ID BLOB univoco viene in genere creato nel codice e inserito nell’URL della richiesta PUT.
Di seguito è riportato l’URL parziale della richiesta PUT. Il `aemformstutorial` è il nome dell’account di archiviazione, formsubmissions è il contenitore in cui verranno memorizzati i dati con un ID BLOB univoco. Il resto dell’URL rimarrà invariato.
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

Di seguito è riportata una funzione scritta per memorizzare i dati del modulo inviato nell’archiviazione di Azure utilizzando una richiesta PUT. Osserva l’utilizzo del nome del contenitore e dell’UUID nell’URL. Puoi creare un servizio OSGi o un servlet Sling utilizzando il codice di esempio elencato di seguito e archiviare gli invii dei moduli nell’archiviazione di Azure.

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## Verifica dati memorizzati nel contenitore

![form-data-in-container](./assets/form-data-in-container.png)





