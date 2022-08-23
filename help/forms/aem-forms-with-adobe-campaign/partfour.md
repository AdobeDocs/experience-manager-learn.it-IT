---
title: Creare un profilo di campagna utilizzando il modello dati del modulo
description: Passaggi coinvolti nella creazione di un profilo Adobe Campaign Standard tramite AEM Forms Form Data Model
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 3%

---

# Creare un profilo di campagna utilizzando il modello dati del modulo {#create-campaign-profile-using-form-data-model}

Passaggi coinvolti nella creazione di un profilo Adobe Campaign Standard tramite AEM Forms Form Data Model

## Crea autenticazione personalizzata {#create-custom-authentication}

Quando si crea Origine dati con il file swagger, AEM Forms supporta i seguenti tipi di autenticazione

* Nessuno
* OAuth 2.0
* Autenticazione di base
* Chiave API
* Autenticazione personalizzata

![campagna elettorale](assets/campaignfdm.gif)

Dovremo utilizzare l’autenticazione personalizzata per effettuare chiamate REST ad Adobe Campaign Standard.

Per utilizzare l’autenticazione personalizzata, è necessario sviluppare un componente OSGi che implementa l’interfaccia di autenticazione IA

È necessario implementare il metodo getAuthDetails. Questo metodo restituirà l&#39;oggetto AuthenticationDetails. Questo oggetto AuthenticationDetails avrà le intestazioni HTTP richieste impostate che sono necessarie per effettuare la chiamata REST API ad Adobe Campaign.

Di seguito è riportato il codice utilizzato per creare l&#39;autenticazione personalizzata. Il metodo getAuthDetails esegue tutto il lavoro. Creiamo l&#39;oggetto AuthenticationDetails. Quindi aggiungiamo le intestazioni HttpHeaders appropriate a questo oggetto e restituiamo questo oggetto.

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## Crea origine dati {#create-data-source}

Il primo passo è quello di creare il file swagger. Il file swagger definisce l’API REST che verrà utilizzata per creare un profilo in Adobe Campaign Standard. Il file swagger definisce i parametri di input e i parametri di output dell’API REST.

Un’origine dati viene creata utilizzando il file swagger. Quando crei Origine dati puoi specificare il tipo di autenticazione. In questo caso utilizzeremo l&#39;autenticazione personalizzata per l&#39;autenticazione su Adobe Campaign.Il codice elencato sopra è stato utilizzato per l&#39;autenticazione su Adobe Campaign.

Il file di esempio swagger viene fornito come parte della risorsa correlata a questo articolo.**Assicurati di modificare l&#39;host e basePath nel file swagger in modo che corrispondano all&#39;istanza ACS**

## Verificare la soluzione {#test-the-solution}

Per testare la soluzione segui i seguenti passaggi:
* [Assicurati di aver seguito i passaggi descritti qui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Scarica e decomprimi il file per ottenere il file swagger](assets/create-acs-profile-swagger-file.zip)
* Crea origine dati utilizzando il file swagger Crea modello dati modulo e basalo sull’origine dati creata nel passaggio precedente
* Crea un modulo adattivo basato sul modello dati del modulo creato nel passaggio precedente.
* Trascina i seguenti elementi dalla scheda origini dati al modulo adattivo

   * E-mail
   * Nome
   * Cognome
   * Telefono cellulare

* Configurare l’azione di invio per l’invio tramite il modello dati modulo.
* Configura il modello dati per l’invio appropriato.
* Visualizzare l’anteprima del modulo. Compila i campi e invia.
* Verifica che il profilo sia creato in Adobe Campaign Standard.
