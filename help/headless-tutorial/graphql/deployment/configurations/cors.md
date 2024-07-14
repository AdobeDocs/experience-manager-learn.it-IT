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
last-substantial-update: 2024-03-22T00:00:00Z
duration: 184
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 2%

---

# Condivisione delle risorse tra le origini (CORS)

La condivisione CORS (Cross-Origin Resource Sharing) di Adobe Experience Manager as a Cloud Service facilita le proprietà web non AEM a effettuare chiamate lato client basate su browser alle API GraphQL dell’AEM e ad altre risorse AEM headless.

>[!TIP]
>
> Le seguenti configurazioni sono esempi. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Requisito CORS

CORS è richiesto per le connessioni basate su browser alle API GraphQL dell’AEM quando il client che si connette all’AEM NON viene gestito dalla stessa origine (nota anche come host o dominio) dell’AEM.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Richiede la configurazione CORS | ✔ | ✔ | ✘ | ✘ |

## Autore AEM

L’abilitazione di CORS sul servizio di authoring AEM è diversa dai servizi di anteprima AEM Publish e AEM. Il servizio di authoring AEM richiede che una configurazione OSGi venga aggiunta alla cartella della modalità di esecuzione del servizio di authoring AEM e non utilizza una configurazione Dispatcher.

### Configurazione OSGi

La Configuration Factory AEM CORS OSGi definisce i criteri consentiti per accettare le richieste HTTP CORS.

| Il client si collega a | Autore AEM | Pubblicazione AEM | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione OSGi CORS | ✔ | ✘ | ✘ |


L&#39;esempio seguente definisce una configurazione OSGi per l&#39;istanza di creazione AEM (`../config.author/..`) in modo che sia attiva solo sul servizio di creazione AEM.

Le proprietà chiave di configurazione sono:

+ `alloworigin` e/o `alloworiginregexp` specifica le origini su cui viene eseguito il client che si connette al Web AEM.
+ `allowedpaths` specifica i pattern di percorso URL consentiti dalle origini specificate.
   + Per supportare le query persistenti di AEM GraphQL, aggiungere il seguente pattern: `/graphql/execute.json.*`
   + Per supportare i frammenti di esperienza, aggiungere il seguente pattern: `/content/experience-fragments/.*`
+ `supportedmethods` specifica i metodi HTTP consentiti per le richieste CORS. Per supportare le query persistenti (e i frammenti di esperienza) di AEM GraphQL, aggiungere `GET`.
+ `supportedheaders` include `"Authorization"` perché le richieste all&#39;autore AEM devono essere autorizzate.
+ `supportscredentials` è impostato su `true` perché la richiesta all&#39;autore AEM deve essere autorizzata.

[Ulteriori informazioni sulla configurazione OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

L’esempio seguente supporta l’utilizzo di query persistenti di AEM GraphQL su AEM Author. Per utilizzare le query GraphQL definite dal client, aggiungere un URL dell&#39;endpoint GraphQL in `allowedpaths` e `POST` in `supportedmethods`.

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

L’abilitazione di CORS sui servizi AEM Publish (e Anteprima) è diversa dal servizio AEM Author. Il servizio Publish per AEM richiede l&#39;aggiunta di una configurazione AEM Dispatcher alla configurazione Dispatcher del Publish AEM. Il Publish AEM non utilizza una [configurazione OSGi](#osgi-configuration).

Durante la configurazione di CORS su AEM Publish, assicurati:

+ Impossibile inviare l&#39;intestazione di richiesta HTTP `Origin` al servizio Publish AEM rimuovendo l&#39;intestazione `Origin` (se aggiunta in precedenza) dal file `clientheaders.any` del progetto Dispatcher AEM. Tutte le intestazioni `Access-Control-` devono essere rimosse dal file `clientheaders.any` e Dispatcher le gestisce, non il servizio Publish AEM.
+ Se nel servizio Publish AEM sono abilitate [configurazioni OSGi CORS](#osgi-configuration), è necessario rimuoverle e migrarne le configurazioni alla [configurazione Dispatcher vhost](#set-cors-headers-in-vhost) descritta di seguito.

### Configurazione Dispatcher

Il Dispatcher del servizio Publish (e Anteprima) dell’AEM deve essere configurato per supportare CORS.

| Il client si collega a | Autore AEM | Pubblicazione AEM | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione Dispatcher CORS | ✘ | ✔ | ✔ |

#### Impostare le intestazioni CORS in vhost

1. Aprire il file di configurazione vhost per il servizio Publish AEM, nel progetto di configurazione Dispatcher, in genere in `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
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

3. Abbina le origini desiderate che accedono al servizio AEM Publish aggiornando l’espressione regolare nella riga seguente. Se sono necessarie più origini, duplica questa riga e aggiorna per ciascun pattern di origine.

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
