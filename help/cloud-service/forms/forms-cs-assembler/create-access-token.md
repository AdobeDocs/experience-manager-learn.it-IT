---
title: Creare un token di accesso
description: Scambia il token web JSON (JWT) con le API Adobe IMS per un token di accesso AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 0e4fd0a0-eaa8-490d-b036-713b25974d60
duration: 33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 0%

---

# Scambia il codice JWT con un token di accesso


Il codice JWT creato nel passaggio precedente viene scambiato con le API Adobe IMS per un token di accesso, che può quindi essere utilizzato per accedere ad AEM as a Cloud Service. Per richiedere un token di accesso, invia una richiesta POST contenente JWT, client_id, client_secret al servizio di autenticazione IMS.

Il codice seguente è stato utilizzato per generare il codice JWT di scambio per un token di accesso

```java
public String getAccessToken() {
        
        String jwtToken = getJWTToken();
        GetServiceCredentials getCredentials = new GetServiceCredentials();
        System.out.println("Getting Access Token");

        try {
                HttpClient httpClient = HttpClientBuilder.create().build();
                HttpHost authServer = new HttpHost(getCredentials.getIMS_ENDPOINT(), 443, "https");
                HttpPost authPostRequest = new HttpPost("/ims/exchange/jwt");
                List < NameValuePair > nameValuePairs = new ArrayList < NameValuePair > ();
                nameValuePairs.add(new BasicNameValuePair("jwt_token", jwtToken));
                nameValuePairs.add(new BasicNameValuePair("client_id", getCredentials.getCLIENT_ID()));
                nameValuePairs.add(new BasicNameValuePair("client_secret", getCredentials.getCLIENT_SECRET()));
                authPostRequest.setEntity(new UrlEncodedFormEntity(nameValuePairs, Consts.UTF_8));
                HttpResponse response;
                response = httpClient.execute(authServer, authPostRequest);
                StatusLine statusLine = response.getStatusLine();
                System.out.println("The status code is " + statusLine.getStatusCode());
                HttpEntity result = response.getEntity();
                String jsonResponseStr = EntityUtils.toString(result);
                System.out.println(jsonResponseStr);
                JsonReader jsonReader = new JsonReader(new StringReader(jsonResponseStr));
                JsonObject jsonObject = JsonParser.parseReader(jsonReader).getAsJsonObject();
                
                System.out.println("Returning access_token " + jsonObject.get("access_token").getAsString());
                return jsonObject.get("access_token").getAsString();

        } catch (Exception e) {
                System.out.print("Error: " + e.getMessage());
        }
        return "null";

}
```
