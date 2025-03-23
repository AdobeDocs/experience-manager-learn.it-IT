---
title: Genera token di accesso
description: Codice di esempio per generare un token di accesso per richiamare l’API di Forms Document Services
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 100cab4b-16bd-4358-b0f0-61dbfd64d412
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Genera token di accesso

Un token di accesso è una credenziale utilizzata per autenticare e autorizzare le richieste a un’API REST. Generalmente viene emesso da un server di autenticazione (come OAuth 2.0 o OpenID Connect) dopo che un utente o un’applicazione ha effettuato correttamente l’accesso. Quando si chiama un’API di Forms Document Services protetta, nell’intestazione della richiesta viene incluso un token di accesso per verificare l’identità del client.
Di seguito è riportato un esempio di richiesta di invio di un token di accesso

```java
POST /api/data HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOiJIUzI1...
```

Il seguente codice è stato utilizzato per generare il token di accesso

```java
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class AccessTokenService {
    private static final Logger logger = LoggerFactory.getLogger(AccessTokenService.class);
    
    public String getAccessToken() {
        ClassLoader classLoader = AccessTokenService.class.getClassLoader();

        try (InputStream inputStream = classLoader.getResourceAsStream("credentials/server_credentials.json")) {
            if (inputStream == null) {
                logger.error("File not found: credentials/server_credentials.json");
                throw new IllegalArgumentException("Missing credentials file");
            }

            try (InputStreamReader reader = new InputStreamReader(inputStream, StandardCharsets.UTF_8);
                 CloseableHttpClient httpClient = HttpClients.createDefault()) {
                 
                JsonObject jsonObject = JsonParser.parseReader(reader).getAsJsonObject();
                String clientId = jsonObject.get("clientId").getAsString();
                String clientSecret = jsonObject.get("clientSecret").getAsString();
                String adobeIMSV3TokenEndpointURL = jsonObject.get("adobeIMSV3TokenEndpointURL").getAsString();
                String scopes = jsonObject.get("scopes").getAsString();

                HttpPost request = new HttpPost(adobeIMSV3TokenEndpointURL);
                request.setHeader("Content-Type", "application/x-www-form-urlencoded");

                String requestBody = "grant_type=client_credentials&client_id=" + clientId +
                        "&client_secret=" + clientSecret +
                        "&scope=" + scopes;

                request.setEntity(new StringEntity(requestBody, ContentType.APPLICATION_FORM_URLENCODED));

                logger.info("Requesting access token from {}", adobeIMSV3TokenEndpointURL);

                try (CloseableHttpResponse response = httpClient.execute(request)) {
                    String responseString = EntityUtils.toString(response.getEntity());
                    JsonObject jsonResponse = JsonParser.parseString(responseString).getAsJsonObject();
                    
                    if (!jsonResponse.has("access_token")) {
                        logger.error("Access token not found in response: {}", responseString);
                        return null;
                    }

                    String accessToken = jsonResponse.get("access_token").getAsString();
                    logger.info("Successfully obtained access token.");
                    return accessToken;
                }
            }
        } catch (Exception e) {
            logger.error("Error fetching access token", e);
        }
        return null;
    }
}
```

La classe AccessTokenService è responsabile del recupero di un token di accesso OAuth dal servizio di autenticazione IMS di Adobe. Legge le credenziali del client da un file JSON (server_credentials.json), crea una richiesta di autenticazione e la invia all’endpoint del token di Adobe. La risposta viene quindi analizzata per estrarre il token di accesso, utilizzato per le chiamate API sicure. La classe include la registrazione corretta e la gestione degli errori per garantire affidabilità e debug più semplice.

## Passaggi successivi

[Effettuare chiamate API](./make-api-calls.md)
