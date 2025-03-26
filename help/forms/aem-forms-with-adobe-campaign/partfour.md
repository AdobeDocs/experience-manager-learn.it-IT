---
title: Creare un profilo di Campaign utilizzando il modello dati del modulo
description: Passaggi necessari per la creazione del profilo Adobe Campaign Standard tramite AEM Forms Form Data Model
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
duration: 110
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 3%

---

# Creare un profilo di Campaign utilizzando il modello dati del modulo {#create-campaign-profile-using-form-data-model}

Passaggi necessari per la creazione del profilo Adobe Campaign Standard tramite AEM Forms Form Data Model

## Crea autenticazione personalizzata {#create-custom-authentication}

Durante la creazione di Data Source con il file swagger, AEM Forms supporta i seguenti tipi di autenticazione

* Nessuno
* OAuth 2.0
* Autenticazione di base
* Chiave API
* Autenticazione personalizzata

![campaignFdm](assets/campaignfdm.gif)

Dovremo utilizzare l’autenticazione personalizzata per effettuare chiamate REST ad Adobe Campaign Standard.

Per utilizzare l’autenticazione personalizzata, è necessario sviluppare un componente OSGi che implementi l’interfaccia IAuthentication

È necessario implementare il metodo getAuthDetails. Questo metodo restituirà l&#39;oggetto AuthenticationDetails. Per questo oggetto AuthenticationDetails verranno impostate le intestazioni HTTP necessarie per effettuare la chiamata REST API ad Adobe Campaign.

Di seguito è riportato il codice utilizzato per creare l&#39;autenticazione personalizzata. Il metodo getAuthDetails esegue tutto il lavoro. Viene creato l’oggetto AuthenticationDetails. A questo oggetto verranno quindi aggiunte le intestazioni HTTP appropriate e verrà restituito questo oggetto.

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

## Creazione di Data Source {#create-data-source}

Il primo passaggio consiste nel creare il file Swagger. Il file swagger definisce l’API REST che verrà utilizzata per creare un profilo in Adobe Campaign Standard. Il file swagger definisce i parametri di input e i parametri di output dell’API REST.

Un&#39;origine dati viene creata utilizzando il file Swagger. Durante la creazione di Data Source è possibile specificare il tipo di autenticazione. In questo caso utilizzeremo l’autenticazione personalizzata per l’autenticazione su Adobe Campaign. Il codice elencato sopra è stato utilizzato per l’autenticazione su Adobe Campaign.

Un file swagger di esempio viene fornito come parte del file della risorsa relativo a questo articolo.**Assicurati di modificare l&#39;host e basePath nel file Swagger in modo che corrispondano alla tua istanza ACS**

## Testare la soluzione {#test-the-solution}

Per testare la soluzione, segui i passaggi seguenti:
* [Assicurarsi di aver seguito i passaggi descritti qui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Scarica e decomprimi questo file per ottenere il file Swagger](assets/create-acs-profile-swagger-file.zip)
* Creare Data Source utilizzando il file Swagger
Crea modello dati modulo e basalo sul Source dati creato nel passaggio precedente
* Crea un modulo adattivo basato sul modello dati del modulo creato nel passaggio precedente.
* Trascina e rilascia i seguenti elementi dalla scheda origini dati al modulo adattivo

   * E-mail
   * Nome
   * Cognome
   * Telefono cellulare

* Configura l’azione di invio su &quot;Invia utilizzando il modello dati del modulo&quot;.
* Configura il modello dati per inviare in modo appropriato.
* Visualizzare l&#39;anteprima del modulo. Compila i campi e invia.
* Verifica che il profilo sia stato creato in Adobe Campaign Standard.
