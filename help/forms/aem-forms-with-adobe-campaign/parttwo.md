---
title: Creazione di un profilo di campagna per l’invio di moduli adattivi
description: Questo articolo illustra i passaggi necessari per creare un profilo in Adobe Campaign Standard per l’invio di un modulo adattivo. Questo processo utilizza un meccanismo di invio personalizzato per gestire l’invio di moduli adattivi.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: deef09d9-82ec-4e61-b7ee-e72d1cd4e9e0
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 0%

---

# Creazione di un profilo di campagna per l’invio di moduli adattivi {#creating-campaign-profile-on-adaptive-form-submission}

Questo articolo illustra i passaggi necessari per creare un profilo in Adobe Campaign Standard per l’invio di un modulo adattivo. Questo processo utilizza un meccanismo di invio personalizzato per gestire l’invio di moduli adattivi.

Questa esercitazione illustra i passaggi necessari per creare un profilo Campaign per l’invio di moduli adattivi. Per eseguire questo caso d’uso, è necessario effettuare le seguenti operazioni

* Creare un servizio AEM (CampaignService) per creare un profilo Adobe Campaign Standard utilizzando l’API REST
* Creare un’azione di invio personalizzata per la gestione dell’invio di moduli adattivi
* Richiamare il metodo createProfile di CampaignService

## Crea servizio AEM {#create-aem-service}

Crea AEM servizio per creare un profilo Adobe Campaign. Questo servizio AEM recupererà le credenziali Adobe Campaign dalla configurazione OSGI. Una volta ottenute le credenziali della campagna, viene generato il token di accesso e viene effettuata la chiamata al token di accesso HTTP Post per creare il profilo in Adobe Campaign. Di seguito è riportato il codice per la creazione del profilo .

```java
package aemformwithcampaign.core.services.impl;

import static io.jsonwebtoken.SignatureAlgorithm.RS256;

import java.io.BufferedInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.UnsupportedEncodingException;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.KeySpec;
import java.security.spec.PKCS8EncodedKeySpec;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

import org.apache.http.Consts;
import org.apache.http.HttpEntity;
import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
import io.jsonwebtoken.Jwts;

@Component(service = CampaignService.class, immediate = true)
public class CampaignServiceImpl implements CampaignService {
private final Logger log = LoggerFactory.getLogger(getClass());

@Reference
CampaignConfigurationService config;
@Reference
GetResolver getResolver;
private static final String SERVER_FQDN = "mc.adobe.io";
private static final String AUTH_SERVER_FQDN = "ims-na1.adobelogin.com";
private static final String AUTH_ENDPOINT = "/ims/exchange/jwt/";
private static final String CREATE_PROFILE_ENDPOINT = "/campaign/profileAndServicesExt/profile/";

@SuppressWarnings("unused")
@Override
public String getAccessToken() throws IOException, NoSuchAlgorithmException, InvalidKeySpecException {
// TODO Auto-generated method stub
log.info("JWT: Generating Token");

String apikey = config.getApiKey();
log.debug("The API Key i got was " + apikey);
String techact = config.getTechAcct();
String orgid = config.getOrgId();
String clientsecret = config.getClientSecret();
String realm = config.getDomainRealm();

HttpClient httpClient = HttpClientBuilder.create().build();
Long expirationTime = System.currentTimeMillis() / 1000 + 86400L;
try {
ResourceResolver rr = getResolver.getServiceResolver();
Resource privateKeyRes = rr.getResource("/etc/key/campaign/private.key");
InputStream is = privateKeyRes.adaptTo(InputStream.class);
BufferedInputStream bis = new BufferedInputStream(is);
ByteArrayOutputStream buf = new ByteArrayOutputStream();
int result = bis.read();
while (result != -1) {
byte b = (byte) result;
buf.write(b);
result = bis.read();
}

String privatekeyString = buf.toString();
privatekeyString = privatekeyString.replaceAll("\\n", "").replace("-----BEGIN PRIVATE KEY-----", "")
.replace("-----END PRIVATE KEY-----", "");
log.debug("The sanitized private key string is " + privatekeyString);
// Create the private key
KeyFactory keyFactory = KeyFactory.getInstance("RSA");
log.debug("The key factory algorithm is " + keyFactory.getAlgorithm());
byte[] byteArray = privatekeyString.getBytes();
log.debug("The array length is " + byteArray.length);
// KeySpec ks = new
// PKCS8EncodedKeySpec(Base64.getDecoder().decode(keyString));
byte[] encodedBytes = javax.xml.bind.DatatypeConverter.parseBase64Binary(privatekeyString);
// KeySpec ks = new PKCS8EncodedKeySpec(byteArray);
KeySpec ks = new PKCS8EncodedKeySpec(encodedBytes);
String metascopes[] = new String[] { "ent_campaign_sdk" };
RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(ks);
HashMap<String, Object> jwtClaims = new HashMap<>();
jwtClaims.put("iss", orgid);
jwtClaims.put("sub", techact);
jwtClaims.put("exp", expirationTime);
jwtClaims.put("aud", "https://" + AUTH_SERVER_FQDN + "/c/" + apikey);
// jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + realm,
// true);
for (String metascope : metascopes) {
jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + metascope, java.lang.Boolean.TRUE);
}

// Create the final JWT token
String jwtToken = Jwts.builder().setClaims(jwtClaims).signWith(RS256, privateKey).compact();
log.debug("#####The jwtToken is  ####" + jwtToken + "#######");
HttpHost authServer = new HttpHost(AUTH_SERVER_FQDN, 443, "https");
HttpPost authPostRequest = new HttpPost(AUTH_ENDPOINT);
authPostRequest.addHeader("Cache-Control", "no-cache");
List<NameValuePair> params = new ArrayList<NameValuePair>();
params.add(new BasicNameValuePair("client_id", apikey));
params.add(new BasicNameValuePair("client_secret", clientsecret));
params.add(new BasicNameValuePair("jwt_token", jwtToken));
authPostRequest.setEntity(new UrlEncodedFormEntity(params, Consts.UTF_8));
HttpResponse response = httpClient.execute(authServer, authPostRequest);
if (200 != response.getStatusLine().getStatusCode()) {
HttpEntity ent = response.getEntity();
String content = EntityUtils.toString(ent);
log.error("JWT: Server Returned Error\n", response.getStatusLine().getReasonPhrase());
log.error("ERROR DETAILS: \n", content);
throw new IOException("Server returned error: " + response.getStatusLine().getReasonPhrase());
}
HttpEntity entity = response.getEntity();
JsonObject jo = new JsonParser().parse(EntityUtils.toString(entity)).getAsJsonObject();

log.debug("Returning access_token " + jo.get("access_token").getAsString());
return jo.get("access_token").getAsString();


} catch (Exception e) {
e.printStackTrace();
}
return null;
}

@Override
public String createProfile(JsonObject profile) {
// TODO Auto-generated method stub
String jwtToken = null;
try {
jwtToken = getAccessToken();
} catch (NoSuchAlgorithmException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (InvalidKeySpecException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (IOException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
}
String tenant = config.getTenant();
String apikey = config.getApiKey();
String path = "/" + tenant + CREATE_PROFILE_ENDPOINT;
log.debug("The API Key is " + apikey);
log.debug("###The Path is " + path);
HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
HttpPost postReq = new HttpPost(path);
postReq.addHeader("Cache-Control", "no-cache");
postReq.addHeader("Content-Type", "application/json");
postReq.addHeader("X-Api-Key", apikey);
postReq.addHeader("Authorization", "Bearer " + jwtToken);
StringEntity se = null;
log.debug("Creating profile for" + profile.toString());
try {
se = new StringEntity(profile.toString());

} catch (UnsupportedEncodingException e1) {
// TODO Auto-generated catch block
e1.printStackTrace();
}
postReq.setEntity(se);
HttpClient httpClient = HttpClientBuilder.create().build();
HttpResponse result;
try {
result = httpClient.execute(server, postReq);
// JSONObject responseJson = new
// JSONObject(EntityUtils.toString(result.getEntity()));
JsonObject responseJson = new JsonParser().parse(EntityUtils.toString(result.getEntity()))
.getAsJsonObject();
log.debug("The response on creating profile is " + responseJson.toString());
return responseJson.get("PKey").getAsString();
// return responseJson.getString("PKey");

} catch (ClientProtocolException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (IOException e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

return null;
}

}
```

## Invio personalizzato {#custom-submit}

Creare un gestore di invio personalizzato per gestire l’invio del modulo adattivo. In questo gestore di invio personalizzato effettueremo una chiamata al metodo createProfile di CampaignService. Il metodo createProfile accetta un oggetto JSONObject che rappresenta il profilo da creare.

Per ulteriori informazioni sul gestore di invio personalizzato in AEM Forms, segui questo [collegamento](/help/forms/adaptive-forms/custom-submit-aem-forms-article.md)

Di seguito è riportato il codice nell’invio personalizzato

```java
aemforms.campaign.core.CampaignService addNewProfile = sling.getService(aemforms.campaign.core.CampaignService.class);
com.google.gson.JsonObject profile = new com.google.gson.JsonObject();
profile.addProperty("email",request.getParameter("email"));
profile.addProperty("firstName",request.getParameter("fname"));
profile.addProperty("lastName",request.getParameter("lname"));
profile.addProperty("mobilePhone",request.getParameter("phone"));

String pkey = addNewProfile.createProfile(profile);
```

## Verificare la soluzione {#test-the-solution}

Una volta definiti il servizio e l’azione di invio personalizzata, siamo pronti per testare la nostra soluzione. Per testare la soluzione, effettua le seguenti operazioni


* [Assicurati di aver seguito i passaggi descritti qui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Importa modulo adattivo e gestore di invio personalizzato utilizzando package manager](assets/create-acs-profile-on-af-submission.zip).Questo pacchetto contiene il modulo adattivo configurato per l’invio all’azione di invio personalizzata.
* Visualizzare in anteprima l&#39;anteprima del [modulo](http://localhost:4502/content/dam/formsanddocuments/createcampaignprofile/jcr:content?wcmmode=disabled)
* Compila tutti i campi e invia
* Verrà creato un nuovo profilo nella tua istanza ACS
