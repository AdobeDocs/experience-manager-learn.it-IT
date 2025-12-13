---
title: Autenticazione Mutual Transport Layer Security (mTLS) da AEM
description: Scopri come effettuare chiamate HTTPS da AEM alle API web che richiedono l’autenticazione Mutual Transport Layer Security (mTLS).
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Developer
level: Experienced
jira: KT-13881
thumbnail: KT-13881.png
doc-type: Article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
duration: 495
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 100%

---

# Autenticazione Mutual Transport Layer Security (mTLS) da AEM

Scopri come effettuare chiamate HTTPS da AEM alle API web che richiedono l’autenticazione Mutual Transport Layer Security (mTLS).

>[!VIDEO](https://video.tv.adobe.com/v/3447867?captions=ita&quality=12&learn=on)

L’autenticazione mTLS o TLS bidirezionale migliora la sicurezza del protocollo TLS richiedendo l’**autenticazione reciproca di client e server**. Questa autenticazione viene eseguita utilizzando certificati digitali. Viene comunemente utilizzata in scenari in cui la sicurezza e la verifica dell’identità sono fondamentali.

Per impostazione predefinita, quando si tenta di stabilire una connessione HTTPS a un’API web che richiede l’autenticazione mTLS, la connessione non riesce e viene visualizzato un messaggio di errore:

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

Questo problema si verifica quando il client non presenta un certificato per l’autenticazione.

Scopri come chiamare correttamente le API che richiedono l’autenticazione mTLS utilizzando [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) e il **Registro chiavi e il TrustStore di AEM**.


## HttpClient e caricare il materiale sul Registro chiavi di AEM

Ad alto livello, per richiamare un’API protetta da mTLS da AEM sono necessari i seguenti passaggi.

### Generazione di certificati AEM

Richiedi l certificato AEM collaborando con il team di sicurezza della tua organizzazione. Il team di sicurezza fornisce o richiede i dettagli relativi al certificato, tra cui la chiave, la richiesta di firma del certificato (CSR, Certificate Signing Request), e il certificato viene rilasciato utilizzando la CSR.

A scopo dimostrativo, genera i dettagli relativi al certificato come chiave, richiesta di firma del certificato (CSR, Certificate Signing Request). Nell’esempio seguente, per rilasciare il certificato viene utilizzata una CA con firma autonoma.

- Innanzitutto, genera il certificato dell’Autorità di certificazione (CA) interna.

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- Genera il certificato AEM.

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- Converti la chiave privata di AEM in formato DER. Il Registro chiavi di AEM richiede la chiave privata in formato DER.

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>I certificati CA con firma autonoma vengono utilizzati solo a scopo di sviluppo. Per la produzione, utilizza un’autorità di certificazione (CA) attendibile per rilasciare il certificato.


### Scambio di certificati

Se per il certificato AEM utilizzi una CA con firma autonoma, come indicato in precedenza, invia il certificato o il certificato dell’Autorità di certificazione (CA) interna al provider API.

Inoltre, se il provider API utilizza un certificato CA con firma autonoma, riceverà il certificato o il certificato dell’Autorità di certificazione (CA) interna dal provider API.

### Importazione dei certificati

Per importare il certificato di AEM, segui i passaggi seguenti:

1. Accedi all’**authoring AEM** come **amministratore**. 

1. Passa a **Authoring AEM > Strumenti > Sicurezza > Utenti > Crea o seleziona un utente esistente**.

   ![Crea o seleziona un utente esistente](assets/mutual-tls-authentication/create-or-select-user.png)

   A scopo dimostrativo, viene creato un nuovo utente denominato `mtl-demo-user`.

1. Per aprire le **Proprietà utente**, fai clic sul nome utente.

1. Fai clic sulla scheda **Registro chiavi** e quindi sul pulsante **Crea registro chiavi**. Quindi nella finestra di dialogo **Imposta password di accesso al registro chiavi**, imposta una password del registro chiavi per questo utente e fai clic su Salva.

   ![Crea registro chiavi](assets/mutual-tls-authentication/create-keystore.png)

1. Nella nuova schermata, nella sezione **AGGIUNGI CHIAVE PRIVATA DAL FILE DER**, segui i passaggi seguenti:

   1. Immetti l’alias.

   1. Importa la chiave privata AEM in formato DER, generata in precedenza.

   1. Importa i file della catena di certificati generati in precedenza.

   1. Fai clic su Invia.

      ![Importa la chiave privata AEM](assets/mutual-tls-authentication/import-aem-private-key.png)

1. Verifica che il certificato sia stato importato correttamente.

   ![Chiave privata e certificato AEM importati](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

Se il provider API utilizza un certificato CA con firma autonoma, importa il certificato ricevuto nel TrustStore di AEM. Segui i passaggi descritti [qui](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html?lang=it#httpclient-and-load-aem-truststore-material).

Allo stesso modo, se AEM utilizza un certificato CA con firma autonoma, richiedi al provider API di importarlo.

### Prototipo di codice di chiamata API mTLS con HttpClient

Aggiorna il codice Java™ come segue. Per utilizzare l’annotazione `@Reference` per ottenere il servizio `KeyStoreService` di AEM, il codice chiamante deve essere un componente/servizio OSGi o un modello Sling (in cui è utilizzato `@OsgiService`).


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- Inserisci nel tuo componente OSGi il servizio OSGi `com.adobe.granite.keystore.KeyStoreService` integrato.
- Ottieni il registro chiavi AEM dell’utente utilizzando `KeyStoreService` e `ResourceResolver`. Questa operazione viene eseguita dal metodo `getAEMKeyStore(...)`.
- Se il provider API utilizza un certificato CA con firma autonoma, ottieni il TrustStore di AEM globale con il metodo `getAEMTrustStore(...)`.
- Crea un oggetto di `SSLContextBuilder`. Consulta [Dettagli API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html) di Java™.
- Carica il registro chiavi AEM dell’utente in `SSLContextBuilder` utilizzando il metodo `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)`.
- La password del registro chiavi è la password impostata durante la creazione del registro chiavi. Deve essere memorizzata nella configurazione OSGi. Consulta [Valori di configurazione del segreto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=it#secret-configuration-values).

## Evitare modifiche al registro chiavi JVM

Un approccio convenzionale per richiamare in modo efficace le API mTLS con certificati privati comporta la modifica del registro chiavi JVM. Questo si ottiene importando i certificati privati mediante il comando [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) di Java™.

Tuttavia, questo metodo non è allineato con le best practice di sicurezza e AEM offre un’opzione superiore tramite l’utilizzo di **registri chiave specifici dell’utente e TrustStore globale** e [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).

## Pacchetto di soluzioni

Il progetto Node.js di esempio illustrato nel video può essere scaricato da [qui](assets/internal-api-call/REST-APIs.zip).

Il codice servlet di AEM è disponibile nel ramo `tutorial/web-api-invocation` del progetto WKND Sites, disponibile [qui](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
