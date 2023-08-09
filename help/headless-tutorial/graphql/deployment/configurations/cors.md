---
title: Configurazione CORS per AEM GraphQL
description: Scopri come configurare la condivisione CORS (Cross-origin resource sharing) per l’utilizzo con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2023-08-08T00:00:00Z
source-git-commit: f8fd13d3f315aa0bd9f268b9fe81b9d9c17b243c
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 2%

---

# Condivisione delle risorse tra le origini (CORS)

La condivisione CORS (Cross-Origin Resource Sharing) di Adobe Experience Manager as a Cloud Service facilita le proprietà web non AEM a effettuare chiamate lato client basate su browser alle API AEM GraphQL e ad altre risorse AEM headless.

>[!TIP]
>
> Le seguenti configurazioni sono esempi. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Requisito CORS

CORS è richiesto per le connessioni basate su browser alle API GraphQL dell’AEM quando il client che si connette all’AEM NON viene gestito dalla stessa origine (nota anche come host o dominio) dell’AEM.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Richiede la configurazione CORS | ✔ | ✔ | ✘ | ✘ |

## Autore AEM

L’abilitazione di CORS sul servizio AEM Author è diversa dai servizi AEM Publish e AEM Preview. Il servizio AEM Author richiede che una configurazione OSGi venga aggiunta alla cartella della modalità di esecuzione del servizio AEM Author e non utilizza una configurazione Dispatcher.

### Configurazione OSGi

La Configuration Factory AEM CORS OSGi definisce i criteri consentiti per accettare le richieste HTTP CORS.

| Il client si collega a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione OSGi CORS | ✔ | ✘ | ✘ |


L’esempio seguente definisce una configurazione OSGi per AEM Author (`../config.author/..`) è quindi attivo solo sul servizio AEM Author.

Le proprietà chiave di configurazione sono:

+ `alloworigin` e/o `alloworiginregexp` specifica le origini di esecuzione del client che si connette al Web AEM.
+ `allowedpaths` specifica i pattern di percorso URL consentiti dalle origini specificate.
   + Per supportare le query persistenti AEM GraphQL, aggiungi il seguente pattern: `/graphql/execute.json.*`
   + Per supportare i frammenti di esperienza, aggiungi il seguente pattern: `/content/experience-fragments/.*`
+ `supportedmethods` specifica i metodi HTTP consentiti per le richieste CORS. Per supportare le query persistenti (e i frammenti di esperienza) di AEM GraphQL, aggiungi `GET` .
+ `supportedheaders` include `"Authorization"` Le richieste inviate ad AEM Author devono essere autorizzate.
+ `supportscredentials` è impostato su `true` come richiesta ad AEM Author deve essere autorizzato.

[Ulteriori informazioni sulla configurazione OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

L’esempio seguente supporta l’utilizzo di query persistenti di AEM GraphQL in AEM Author. Per utilizzare le query GraphQL definite dal client, aggiungi un URL dell’endpoint GraphQL in `allowedpaths` e `POST` a `supportedmethods`.

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

## AEM Publish

L’abilitazione di CORS sui servizi di pubblicazione (e anteprima) di AEM è diversa dal servizio di authoring di AEM. Il servizio di pubblicazione AEM richiede che una configurazione del Dispatcher AEM sia aggiunta alla configurazione del Dispatcher di pubblicazione AEM. AEM Publish non utilizza un’ [Configurazione OSGi](#osgi-configuration).

Durante la configurazione di CORS in AEM Publish, assicurati:

+ Il `Origin` L’intestazione della richiesta HTTP non può essere inviata al servizio AEM Publish rimuovendo il `Origin` (se precedentemente aggiunta) dall’intestazione del progetto Dispatcher dell’AEM `clientheaders.any` file. Qualsiasi `Access-Control-` le intestazioni devono essere rimosse dal `clientheaders.any` vengono gestiti da file e Dispatcher, non dal servizio AEM Publish.
+ Se ne ha [Configurazioni OSGi CORS](#osgi-configuration) sul servizio AEM Publish, devi rimuoverle e migrarne le configurazioni nel [Configurazione vhost di Dispatcher](#set-cors-headers-in-vhost) descritte di seguito.

### Configurazione del Dispatcher

Il Dispatcher del servizio AEM Publish (e Anteprima) deve essere configurato per supportare CORS.

| Il client si collega a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione CORS di Dispatcher | ✘ | ✔ | ✔ |

#### Imposta variabile di ambiente Dispatcher

1. Apri il file global.vars per la configurazione del Dispatcher AEM, in genere all’indirizzo `dispatcher/src/conf.d/variables/global.vars`.
2. Aggiungi quanto segue al file:

   ```
   # Enable CORS handling in the dispatcher
   #
   # By default, CORS is handled by the AEM publish server.
   # If you uncomment and define the ENABLE_CORS variable, then CORS will be handled in the dispatcher.
   # See the default.vhost file for a suggested dispatcher configuration. Note that:
   #   a. You will need to adapt the regex from default.vhost to match your CORS domains
   #   b. Remove the "Origin" header (if it exists) from the clientheaders.any file
   #   c. If you have any CORS domains configured in your AEM publish server origin, you have to move those to the dispatcher
   #       (i.e. accordingly update regex in default.vhost to match those domains)
   #
   Define ENABLE_CORS
   ```

#### Impostare le intestazioni CORS in vhost

1. Apri il file di configurazione vhost per il servizio di pubblicazione AEM, nel progetto di configurazione di Dispatcher, in genere all’indirizzo `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copia il contenuto del `<IfDefine ENABLE_CORS>...</IfDefine>` blocca qui sotto, nel file di configurazione vhost abilitato.

   ```{ highlight="19"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
       ...
       <IfDefine ENABLE_CORS>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch '%{REQUEST_SCHEME}://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
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
   
         # Remove Origin before sending to AEM Publish
         RequestHeader unset Origin
   
         ################## End of CORS configuration ##################
       </IfDefine>
       ...
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
