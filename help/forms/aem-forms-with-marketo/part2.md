---
title: AEM Forms con Marketo (Parte 2)
description: Tutorial per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 132
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 1%

---

# Servizio di autenticazione di Marketo

Le API REST di Marketo sono autenticate con OAuth 2.0 a 2 gambe. È necessario creare un’autenticazione personalizzata per eseguire l’autenticazione in Marketo. Questa autenticazione personalizzata viene generalmente scritta all’interno di un bundle OSGI. Il codice seguente mostra l’autenticatore personalizzato utilizzato come parte di questa esercitazione.

## Servizio di autenticazione personalizzato

Nel codice seguente viene creato l&#39;oggetto AuthenticationDetails con il valore access_token necessario per l&#39;autenticazione in Marketo

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

MarketoAuthenticationService implementa l&#39;interfaccia IAuthentication. Questa interfaccia fa parte di AEM Forms Client SDK. Il servizio ottiene il token di accesso e lo inserisce nell’intestazione HttpHeader di AuthenticationDetails. Una volta popolato l&#39;oggetto HttpHeaders dell&#39;oggetto AuthenticationDetails, l&#39;oggetto AuthenticationDetails viene restituito al livello Dermis del modello dati del modulo.

Presta attenzione alla stringa restituita dal metodo getAuthenticationType. Questa stringa viene utilizzata durante la configurazione dell’origine dati.

### Ottieni token di accesso

Un&#39;interfaccia semplice viene definita con un metodo che restituisce access_token. Il codice per la classe che implementa questa interfaccia è elencato più avanti nella pagina.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

Il seguente codice fa parte del servizio che restituisce l’access_token da utilizzare per effettuare le chiamate REST API. Il codice in questo servizio accede ai parametri di configurazione necessari per effettuare la chiamata GET. Come puoi vedere, trasmettiamo il client_id,client_secret nell’URL del GET per generare l’access_token. Questo access_token viene quindi restituito all&#39;applicazione chiamante.

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

La schermata seguente mostra le proprietà di configurazione da impostare. Queste proprietà di configurazione vengono lette nel codice elencato sopra per ottenere il token di accesso

![config](assets/configuration-settings.png)

### Configurazione

Per creare le proprietà di configurazione è stato utilizzato il codice seguente. Queste proprietà sono specifiche della tua istanza Marketo

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

Il codice seguente legge le proprietà di configurazione e restituisce lo stesso tramite i metodi getter

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. Crea e implementa il bundle sul tuo server AEM.
1. [Scegliere configMgr dal browser](http://localhost:4502/system/console/configMgr) e cerca &quot;Configurazione servizio credenziali Marketo&quot;
1. Specifica le proprietà appropriate specifiche dell’istanza Marketo

## Passaggi successivi

[Crea origine dati basata su servizio RESTful](./part3.md)
