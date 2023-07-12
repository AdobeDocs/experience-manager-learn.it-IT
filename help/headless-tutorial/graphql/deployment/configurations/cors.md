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
source-git-commit: 36b4217a899b462296d4389bc96a644da97d5da4
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 1%

---

# Condivisione delle risorse tra le origini (CORS)

La condivisione CORS (Cross-Origin Resource Sharing) di Adobe Experience Manager as a Cloud Service facilita le proprietà web non AEM a effettuare chiamate lato client basate su browser alle API AEM GraphQL.

L’articolo seguente descrive come configurare _origine singola_ accesso a un set specifico di endpoint headless AEM tramite CORS. Origine singola significa che un solo dominio non AEM accede all’AEM, ad esempio https://app.example.com connettendosi a https://www.example.com. L’accesso multiorigine potrebbe non funzionare con questo approccio a causa di problemi di caching.

>[!TIP]
>
> Le seguenti configurazioni sono esempi. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Requisito CORS

CORS è richiesto per le connessioni basate su browser alle API GraphQL dell’AEM quando il client che si connette all’AEM NON viene gestito dalla stessa origine (nota anche come host o dominio) dell’AEM.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Richiede la configurazione CORS | ✔ | ✔ | ✘ | ✘ |

## Configurazione OSGi

La Configuration Factory AEM CORS OSGi definisce i criteri consentiti per accettare le richieste HTTP CORS.

| Il client si collega a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione OSGi CORS | ✔ | ✔ | ✔ |


L’esempio seguente definisce una configurazione OSGi per AEM Publish (`../config.publish/..`), ma può essere aggiunto a [qualsiasi cartella in modalità di esecuzione supportata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Le proprietà chiave di configurazione sono:

+ `alloworigin` e/o `alloworiginregexp` specifica le origini di esecuzione del client che si connette al Web AEM.
+ `allowedpaths` specifica i pattern di percorso URL consentiti dalle origini specificate.
   + Per supportare le query persistenti AEM GraphQL, aggiungi il seguente pattern: `/graphql/execute.json.*`
   + Per supportare i frammenti di esperienza, aggiungi il seguente pattern: `/content/experience-fragments/.*`
+ `supportedmethods` specifica i metodi HTTP consentiti per le richieste CORS. Aggiungi `GET`, per supportare query persistenti (e frammenti di esperienza) di AEM GraphQL.

[Ulteriori informazioni sulla configurazione OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

Questa configurazione di esempio supporta l’utilizzo di query persistenti di AEM GraphQL. Per utilizzare le query GraphQL definite dal client, aggiungi un URL dell’endpoint GraphQL in `allowedpaths` e `POST` a `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

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
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Richieste API GraphQL AEM autorizzate

Quando accedi alle API GraphQL dell’AEM che richiedono un’autorizzazione (in genere AEM Author o contenuti protetti in AEM Publish), assicurati che la configurazione OSGi CORS abbia i valori aggiuntivi:

+ `supportedheaders` anche elenchi `"Authorization"`
+ `supportscredentials` è impostato su `true`

Le richieste autorizzate alle API GraphQL dell’AEM che richiedono la configurazione CORS sono insolite, in quanto si verificano in genere nel contesto di [applicazioni server-to-server](../server-to-server.md) e quindi non richiedono la configurazione CORS. App basate su browser che richiedono configurazioni CORS, ad esempio [app a pagina singola](../spa.md) o [Componenti Web](../web-component.md), in genere si utilizza l’autorizzazione in quanto è difficile proteggere le credenziali.

Ad esempio, queste due impostazioni sono impostate come segue in un `CORSPolicyImpl` Configurazione di fabbrica OSGi:

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### Esempio di configurazione OSGi

+ [Un esempio della configurazione OSGi è disponibile nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Configurazione del Dispatcher

Il Dispatcher del servizio AEM Publish (e Anteprima) deve essere configurato per supportare CORS.

| Il client si collega a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione CORS di Dispatcher | ✘ | ✔ | ✔ |

### Consenti intestazioni CORS nelle richieste HTTP

Aggiornare il `clientheaders.any` per consentire le intestazioni di richiesta HTTP `Origin`,  `Access-Control-Request-Method`, e `Access-Control-Request-Headers` all&#39;AEM, consentendo l&#39;elaborazione della richiesta HTTP da parte del [Configurazione AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Esempio di configurazione di Dispatcher

+ [Un esempio di Dispatcher _intestazioni client_ La configurazione di si trova nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Distribuire intestazioni di risposta HTTP CORS

Configurare la farm di Dispatcher per la cache **Intestazioni di risposta HTTP CORS** per assicurarsi che vengano inclusi quando le query persistenti di AEM GraphQL vengono servite dalla cache di Dispatcher aggiungendo `Access-Control-...` nell&#39;elenco delle intestazioni della cache.

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

#### Esempio di configurazione di Dispatcher

+ [Un esempio di Dispatcher _Intestazioni di risposta HTTP CORS_ La configurazione di si trova nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
