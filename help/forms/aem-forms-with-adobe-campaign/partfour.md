---
title: Crea profilo campagna utilizzando il modello dati del modulo
seo-title: Crea profilo campagna utilizzando il modello dati del modulo
description: Passaggi relativi alla creazione  profilo Adobe Campaign Standard mediante  AEM Forms Form Data Model
seo-description: Passaggi relativi alla creazione  profilo Adobe Campaign Standard mediante  AEM Forms Form Data Model
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 3%

---


# Crea profilo campagna utilizzando il modello dati del modulo {#create-campaign-profile-using-form-data-model}

Passaggi relativi alla creazione  profilo Adobe Campaign Standard mediante  AEM Forms Form Data Model

## Crea autenticazione personalizzata {#create-custom-authentication}

Quando crei un&#39;origine dati con il file swagger,  AEM Forms supporta i seguenti tipi di autenticazione

* Nessuno
* OAuth 2.0
* Autenticazione di base
* Chiave API
* Autenticazione personalizzata

![campaignfdm](assets/campaignfdm.gif)

Dovremo utilizzare l&#39;autenticazione personalizzata per effettuare chiamate REST a  Adobe Campaign Standard.

Per utilizzare l&#39;autenticazione personalizzata, è necessario sviluppare un componente OSGi che implementa l&#39;interfaccia IAuthentication

È necessario implementare il metodo getAuthDetails. Questo metodo restituisce l&#39;oggetto AuthenticationDetails. Questo oggetto AuthenticationDetails avrà le intestazioni HTTP necessarie per effettuare la chiamata REST API ad  Adobe Campaign.

Di seguito è riportato il codice utilizzato per creare l&#39;autenticazione personalizzata. Il metodo getAuthDetails esegue tutto il lavoro. Viene creato l&#39;oggetto AuthenticationDetails. Quindi, a questo oggetto viene aggiunto l&#39;HttpHeaders appropriato e viene restituito l&#39;oggetto.

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

Il primo passaggio consiste nel creare il file swagger. Il file swagger definisce l&#39;API REST che verrà utilizzata per creare un profilo in  Adobe Campaign Standard. Il file swagger definisce i parametri di input e di output dell&#39;API REST.

Un&#39;origine dati viene creata utilizzando il file swagger. Durante la creazione dell&#39;origine dati puoi specificare il tipo di autenticazione. In questo caso utilizzeremo l&#39;autenticazione personalizzata per l&#39;autenticazione  Adobe Campaign. Il codice riportato sopra è stato utilizzato per l&#39;autenticazione  Adobe Campaign.

Il file di swagger di esempio viene fornito come parte della risorsa correlata a questo articolo.**Verificare che l&#39;host e basePath nel file swagger corrispondano all&#39;istanza ACS**

## Verificare la soluzione {#test-the-solution}

Per testare la soluzione, attenetevi alla seguente procedura:
* [Accertatevi di aver seguito i passaggi descritti qui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Scaricate e decomprimete il file per ottenere il file swagger](assets/create-acs-profile-swagger-file.zip)
* Crea origine dati utilizzando il file swagger
Crea modello dati modulo e basalo sull&#39;origine dati creata nel passaggio precedente
* Creare un modulo adattivo basato sul modello dati del modulo creato nel passaggio precedente.
* Trascinare i seguenti elementi dalla scheda origini dati al modulo adattivo

   * E-mail
   * Nome
   * Cognome
   * Cellulare

* Configurare l&#39;azione di invio su &quot;Invia utilizzando il modello dati del modulo&quot;.
* Configurare il modello dati per l&#39;invio appropriato.
* Visualizzare l’anteprima del modulo. Compila i campi e invia.
* Verifica che il profilo sia stato creato in  Adobe Campaign Standard.
