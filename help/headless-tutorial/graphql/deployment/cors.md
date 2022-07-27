---
title: Configurazione CORS per AEM GraphQL
description: Scopri come configurare Cross-origin resource sharing (CORS) per l’utilizzo con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 2%

---


# Condivisione delle risorse tra le origini (CORS)

La condivisione delle risorse tra le origini di Adobe Experience Manager as a Cloud Service (Cross-Origin Resource Sharing, CORS) facilita le proprietà web non AEM per effettuare chiamate lato client basate su browser alle API GraphQL AEM.

>[!TIP]
>
> Di seguito sono riportati alcuni esempi di configurazioni. Assicurati di regolarli per allinearli ai requisiti del progetto.



## Requisito CORS

CORS è necessario per le connessioni basate su browser alle API GraphQL AEM, quando il client che si connette a AEM NON viene servito dalla stessa origine (nota anche come host o dominio) di AEM.

| Tipo di client | App a pagina singola (SPA) | Componente Web/JS | Mobile | Server-to-server |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Richiede la configurazione CORS | ↓ | ↓ | ✘ | ✘ |

## Configurazione OSGi

La factory di configurazione OSGi AEM CORS definisce i criteri consentiti per accettare le richieste HTTP CORS.

| Il client si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione CORS OSGi | ↓ | ↓ | ↓ |


L’esempio seguente definisce una configurazione OSGi per AEM Publish (`../config.publish/..`), ma può essere aggiunto a [qualsiasi cartella runmode supportata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Le proprietà di configurazione chiave sono:

+ `alloworigin` e/o `alloworiginregexp` specifica le origini su cui viene eseguito il client che si connette a AEM Web.
+ `allowedpaths` specifica i pattern di percorso URL consentiti dalle origini specificate. Per supportare AEM query persistenti GraphQL, utilizza il seguente pattern: `"/graphql/execute.json.*"`
+ `supportedmethods` specifica i metodi HTTP consentiti per le richieste CORS. Aggiungi `GET`, per supportare AEM query persistenti GraphQL.

[Ulteriori informazioni sulla configurazione CORS OSGi.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)


Questa configurazione di esempio supporta l’utilizzo di query persistenti AEM GraphQL. Per utilizzare query GraphQL definite dal client, aggiungi un URL dell&#39;endpoint GraphQL in `allowedpaths` e `POST` a `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```


## Configurazione del Dispatcher

Il Dispatcher del servizio AEM Publish (e Preview) deve essere configurato per supportare CORS.

| Il client si connette a | Autore AEM | Pubblicazione AEM | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione CORS del Dispatcher | ✘ | ↓ | ↓ |

### Consenti intestazioni CORS su richieste HTTP

Aggiorna `clientheaders.any` per consentire le intestazioni di richiesta HTTP `Origin`,  `Access-Control-Request-Method` e `Access-Control-Request-Headers` da trasmettere a AEM, consentendo l’elaborazione della richiesta HTTP da parte del [Configurazione AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### Fornire intestazioni di risposta CORS HTTP

Configurare la farm del dispatcher per la cache **Intestazioni di risposta HTTP CORS** per assicurarti che siano inclusi quando AEM le query persistenti GraphQL vengono servite dalla cache del Dispatcher aggiungendo il `Access-Control-...` nell&#39;elenco delle intestazioni della cache.

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```
