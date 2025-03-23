---
title: Chiamata delle API interne con certificati privati
description: Scopri come chiamare le API interne con certificati privati o autofirmati.
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 332
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 0%

---

# Chiamata delle API interne con certificati privati

Scopri come effettuare chiamate HTTPS da AEM alle API web utilizzando certificati privati o autofirmati.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

Per impostazione predefinita, quando si tenta di stabilire una connessione HTTPS a un’API web che utilizza un certificato autofirmato, la connessione non riesce e viene visualizzato il messaggio di errore:

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

Questo problema si verifica in genere quando il certificato SSL dell&#39;API **non viene rilasciato da un&#39;autorità di certificazione (CA) riconosciuta** e l&#39;applicazione Java™ non è in grado di convalidare il certificato SSL/TLS.

Scopri come chiamare correttamente le API con certificati privati o autofirmati utilizzando [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) e **AEM TrustStore globale**.


## Codice di chiamata API prototipo utilizzando HttpClient

Il codice seguente effettua una connessione HTTPS a un’API web:

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

Il codice utilizza le classi della libreria [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) di [Apache HttpComponent](https://hc.apache.org/) e i relativi metodi.


## HttpClient e caricare il materiale AEM TrustStore

Per chiamare un endpoint API con _certificato privato o autofirmato_, il `SSLContextBuilder` di [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) deve essere caricato con TrustStore di AEM e utilizzato per facilitare la connessione.

Segui i passaggi seguenti:

1. Accedi a **AEM Author** come **amministratore**.
1. Passa a **AEM Author > Tools > Security > Trust Store** e apri **Global Trust Store**. Se si accede per la prima volta, impostare una password per l&#39;archivio fonti attendibili globale.

   ![Archivio attendibile globale](assets/internal-api-call/global-trust-store.png)

1. Per importare un certificato privato, fare clic sul pulsante **Seleziona file di certificato** e selezionare il file di certificato desiderato con estensione `.cer`. Importa facendo clic sul pulsante **Invia**.

1. Aggiorna il codice Java™ come segue. Per utilizzare `@Reference` per ottenere `KeyStoreService` di AEM, il codice chiamante deve essere un componente/servizio OSGi o un modello Sling (in cui è utilizzato `@OsgiService`).

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * Inserisci il servizio OSGi `com.adobe.granite.keystore.KeyStoreService` OOTB nel componente OSGi.
   * Ottenere il TrustStore AEM globale utilizzando `KeyStoreService` e `ResourceResolver`. Questa operazione viene eseguita dal metodo `getAEMTrustStore(...)`.
   * Creare un oggetto di `SSLContextBuilder`. Vedere Java™ [Dettagli API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * Caricare l&#39;AEM TrustStore globale in `SSLContextBuilder` utilizzando il metodo `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)`.
   * Il passaggio `null` per `TrustStrategy` nel metodo precedente garantisce che solo i certificati attendibili di AEM abbiano esito positivo durante l&#39;esecuzione dell&#39;API.


>[!CAUTION]
>
>Le chiamate API con certificati validi emessi da una CA non riescono se eseguite utilizzando l’approccio indicato. Solo le chiamate API con certificati attendibili di AEM possono avere esito positivo quando si segue questo metodo.
>
>Utilizza l&#39;approccio [standard](#prototypical-api-invocation-code-using-httpclient) per eseguire chiamate API di certificati validi emessi da CA, il che significa che solo le API associate a certificati privati devono essere eseguite utilizzando il metodo indicato in precedenza.

## Evita le modifiche al keystore JVM

Un approccio convenzionale per richiamare in modo efficace le API interne con certificati privati comporta la modifica del keystore JVM. Ciò si ottiene importando i certificati privati mediante il comando Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549).

Tuttavia, questo metodo non è allineato con le best practice di sicurezza e AEM offre un&#39;opzione superiore tramite l&#39;utilizzo di **Archivio attendibile globale** e [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).


## Pacchetto soluzione

Il progetto Node.js di esempio demo nel video può essere scaricato da [qui](assets/internal-api-call/REST-APIs.zip).

Il codice servlet AEM è disponibile nel ramo `tutorial/web-api-invocation` del progetto WKND Sites, [vedi](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
