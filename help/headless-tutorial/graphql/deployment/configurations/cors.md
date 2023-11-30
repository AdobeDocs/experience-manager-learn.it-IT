---
title: Configurazione CORS per AEM GraphQL
description: Scopri come configurare la condivisione CORS (Cross-origin resource sharing) per l’utilizzo con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2023-08-08T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '627'
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

L’abilitazione di CORS sul servizio di authoring AEM è diversa dai servizi di pubblicazione AEM e di anteprima AEM. Il servizio AEM Author richiede che una configurazione OSGi venga aggiunta alla cartella della modalità di esecuzione del servizio AEM Author e non utilizza una configurazione Dispatcher.

### Configurazione OSGi

La Configuration Factory AEM CORS OSGi definisce i criteri consentiti per accettare le richieste HTTP CORS.

| Il client si collega a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione OSGi CORS | ✔ | ✘ | ✘ |


L’esempio seguente definisce una configurazione OSGi per AEM Author (`../config.author/..`), quindi è attivo solo sul servizio di authoring AEM.

Le proprietà chiave di configurazione sono:

+ `alloworigin` e/o `alloworiginregexp` specifica le origini di esecuzione del client che si connette al Web AEM.
+ `allowedpaths` specifica i pattern di percorso URL consentiti dalle origini specificate.
   + Per supportare le query persistenti AEM GraphQL, aggiungi il seguente pattern: `/graphql/execute.json.*`
   + Per supportare i frammenti di esperienza, aggiungi il seguente pattern: `/content/experience-fragments/.*`
+ `supportedmethods` specifica i metodi HTTP consentiti per le richieste CORS. Per supportare le query persistenti (e i frammenti di esperienza) di AEM GraphQL, aggiungi `GET` .
+ `supportedheaders` include `"Authorization"` in quanto le richieste all’autore AEM devono essere autorizzate.
+ `supportscredentials` è impostato su `true` In caso di richiesta all’autore dell’AEM, quest’ultimo deve essere autorizzato.

[Ulteriori informazioni sulla configurazione OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

L’esempio seguente supporta l’utilizzo di query persistenti di AEM GraphQL su AEM Author. Per utilizzare le query GraphQL definite dal client, aggiungi un URL dell’endpoint GraphQL in `allowedpaths` e `POST` a `supportedmethods`.

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

L’abilitazione di CORS sui servizi di pubblicazione (e anteprima) dell’AEM è diversa dal servizio di authoring dell’AEM. Il servizio di pubblicazione AEM richiede che una configurazione del Dispatcher AEM sia aggiunta alla configurazione del Dispatcher della pubblicazione AEM. La pubblicazione AEM non utilizza un [Configurazione OSGi](#osgi-configuration).

Durante la configurazione di CORS sulla pubblicazione AEM, assicurati:

+ Il `Origin` L’intestazione della richiesta HTTP non può essere inviata al servizio di pubblicazione AEM rimuovendo il `Origin` (se precedentemente aggiunta) dall’intestazione del progetto Dispatcher dell’AEM `clientheaders.any` file. Qualsiasi `Access-Control-` le intestazioni devono essere rimosse dal `clientheaders.any` li gestisce i file e Dispatcher, non il servizio di pubblicazione AEM.
+ Se ne ha [Configurazioni OSGi CORS](#osgi-configuration) sul servizio di pubblicazione AEM, è necessario rimuoverli e migrarne le configurazioni nel [Configurazione vhost di Dispatcher](#set-cors-headers-in-vhost) descritte di seguito.

### Configurazione del Dispatcher

Il Dispatcher del servizio di pubblicazione (e anteprima) dell’AEM deve essere configurato per supportare CORS.

| Il client si collega a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione CORS di Dispatcher | ✘ | ✔ | ✔ |

#### Impostare le intestazioni CORS in vhost

1. Apri il file di configurazione vhost per il servizio di pubblicazione AEM, nel progetto di configurazione di Dispatcher, in genere all’indirizzo `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copia il contenuto del `<IfDefine ENABLE_CORS>...</IfDefine>` blocca qui sotto, nel file di configurazione vhost abilitato.

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
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
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Fai corrispondere le origini desiderate per accedere al servizio di pubblicazione AEM aggiornando l’espressione regolare nella riga seguente. Se sono necessarie più origini, duplica questa riga e aggiorna per ciascun pattern di origine.

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
