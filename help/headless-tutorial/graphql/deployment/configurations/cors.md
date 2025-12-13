---
title: Configurazione CORS per AEM GraphQL
description: Scopri come configurare la condivisione CORS (Cross-origin resource sharing) per l’utilizzo con AEM GraphQL.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2024-03-22T00:00:00Z
duration: 185
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 2%

---

# Condivisione delle risorse tra le origini (CORS)

La condivisione CORS (Cross-Origin Resource Sharing) di Adobe Experience Manager as a Cloud Service facilita le proprietà web non AEM nell’effettuare chiamate lato client basate su browser alle API GraphQL di AEM e ad altre risorse headless di AEM.

>[!TIP]
>
> Le seguenti configurazioni sono esempi. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Requisito CORS

CORS è richiesto per le connessioni basate su browser alle API GraphQL di AEM, quando il client che si connette ad AEM NON viene gestito dalla stessa origine (nota anche come host o dominio) di AEM.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Dispositivi mobili](../mobile.md) | [Server-to-server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Richiede la configurazione CORS | ✔ | ✔ | ✘ | ✘ |

## AEM Author

L’abilitazione di CORS sul servizio AEM Author è diversa dai servizi AEM Publish e AEM Preview. Il servizio AEM Author richiede che una configurazione OSGi venga aggiunta alla cartella della modalità di esecuzione del servizio AEM Author e non utilizza una configurazione Dispatcher.

### Configurazione OSGi

Il factory di configurazione AEM CORS OSGi definisce i criteri consentiti per l’accettazione delle richieste HTTP CORS.

| Il client si collega a | AEM Author | Pubblicazione AEM | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione OSGi CORS | ✔ | ✘ | ✘ |


L&#39;esempio seguente definisce una configurazione OSGi per AEM Author (`../config.author/..`) in modo che sia attiva solo sul servizio AEM Author.

Le proprietà chiave di configurazione sono:

+ `alloworigin` e/o `alloworiginregexp` specifica le origini su cui viene eseguito il client che si connette ad AEM web.
+ `allowedpaths` specifica i pattern di percorso URL consentiti dalle origini specificate.
   + Per supportare le query persistenti di AEM GraphQL, aggiungere il seguente pattern: `/graphql/execute.json.*`
   + Per supportare i frammenti di esperienza, aggiungere il seguente pattern: `/content/experience-fragments/.*`
+ `supportedmethods` specifica i metodi HTTP consentiti per le richieste CORS. Per supportare le query persistenti (e i frammenti di esperienza) di AEM GraphQL, aggiungere `GET`.
+ `supportedheaders` include `"Authorization"` perché le richieste ad AEM Author devono essere autorizzate.
+ `supportscredentials` è impostato su `true` perché la richiesta ad AEM Author deve essere autorizzata.

[Ulteriori informazioni sulla configurazione OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=it)

L’esempio seguente supporta l’utilizzo di query persistenti di AEM GraphQL in AEM Author. Per utilizzare le query GraphQL definite dal client, aggiungere un URL dell&#39;endpoint GraphQL in `allowedpaths` e `POST` in `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### Esempio di configurazione OSGi

+ [Un esempio della configurazione OSGi è disponibile nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Pubblicazione AEM

L’abilitazione di CORS sui servizi di pubblicazione (e anteprima) di AEM è diversa dal servizio di authoring di AEM. Il servizio di pubblicazione di AEM richiede che una configurazione di AEM Dispatcher sia aggiunta alla configurazione di Dispatcher di AEM Publish. AEM Publish non utilizza una configurazione [OSGi](#osgi-configuration).

Durante la configurazione di CORS in AEM Publish, assicurati:

+ Impossibile inviare l&#39;intestazione di richiesta HTTP `Origin` al servizio di pubblicazione AEM rimuovendo l&#39;intestazione `Origin` (se aggiunta in precedenza) dal file `clientheaders.any` del progetto AEM Dispatcher. Tutte le intestazioni `Access-Control-` devono essere rimosse dal file `clientheaders.any` e Dispatcher le gestisce, non il servizio AEM Publish.
+ Se nel servizio di pubblicazione AEM sono abilitate [configurazioni OSGi CORS](#osgi-configuration), devi rimuoverle e migrarne le configurazioni alla [configurazione vhost Dispatcher](#set-cors-headers-in-vhost) descritta di seguito.

### Configurazione del Dispatcher

Il Dispatcher del servizio AEM Publish (e Anteprima) deve essere configurato per supportare CORS.

| Il client si collega a | AEM Author | Pubblicazione AEM | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione Dispatcher CORS | ✘ | ✔ | ✔ |

#### Impostare le intestazioni CORS in vhost

1. Aprire il file di configurazione vhost per il servizio di pubblicazione AEM, nel progetto di configurazione Dispatcher, in genere in `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copiare il contenuto del blocco `<IfDefine ENABLE_CORS>...</IfDefine>` seguente nel file di configurazione vhost abilitato.

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'http://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'https://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Usa le origini desiderate per accedere al servizio AEM Publish aggiornando l’espressione regolare nella riga seguente. Se sono necessarie più origini, duplica questa riga e aggiorna per ciascun pattern di origine.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + Ad esempio, per abilitare l’accesso CORS dalle origini:

      + Qualsiasi sottodominio in `https://example.com`
      + Qualsiasi porta su `http://localhost`

     Sostituire la riga con le due righe seguenti:

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Esempio di configurazione di Dispatcher

+ [Un esempio della configurazione di Dispatcher è disponibile nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
