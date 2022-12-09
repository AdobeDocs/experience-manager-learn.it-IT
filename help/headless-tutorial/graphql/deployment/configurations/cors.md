---
title: Configurazione CORS per AEM GraphQL
description: Scopri come configurare Cross-origin resource sharing (CORS) per l’utilizzo con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: 6f1000db880c3126a01fa0b74abdb39ffc38a227
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 1%

---


# Condivisione delle risorse tra le origini (CORS)

La condivisione delle risorse tra le origini di Adobe Experience Manager as a Cloud Service (Cross-Origin Resource Sharing, CORS) consente di effettuare chiamate lato client basate su browser alle API di GraphQL senza AEM proprietà web.

>[!TIP]
>
> Di seguito sono riportati alcuni esempi di configurazioni. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Requisito CORS

CORS è necessario per le connessioni basate su browser alle API GraphQL AEM, quando il client che si connette a AEM NON viene servito dalla stessa origine (nota anche come host o dominio) di AEM.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Richiede la configurazione CORS | ↓ | ↓ | ✘ | ✘ |

## Configurazione OSGi

La factory di configurazione OSGi AEM CORS definisce i criteri consentiti per accettare le richieste HTTP CORS.

| Il client si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione CORS OSGi | ↓ | ↓ | ↓ |


L’esempio seguente definisce una configurazione OSGi per AEM Publish (`../config.publish/..`), ma può essere aggiunto a [qualsiasi cartella in modalità di esecuzione supportata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Le proprietà di configurazione chiave sono:

+ `alloworigin` e/o `alloworiginregexp` specifica le origini su cui il client si connette a AEM Web.
+ `allowedpaths` specifica i pattern di percorso URL consentiti dalle origini specificate.
   + Per supportare AEM query persistenti GraphQL, aggiungi il seguente pattern: `/graphql/execute.json.*`
   + Per supportare i frammenti esperienza, aggiungi il seguente pattern: `/content/experience-fragments/.*`
+ `supportedmethods` specifica i metodi HTTP consentiti per le richieste CORS. Aggiungi `GET`, per supportare query persistenti AEM GraphQL (e Frammenti esperienza).

[Ulteriori informazioni sulla configurazione CORS OSGi.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

Questa configurazione di esempio supporta l’utilizzo di query persistenti AEM GraphQL. Per utilizzare query GraphQL definite dal client, aggiungi un URL endpoint GraphQL in `allowedpaths` e `POST` a `supportedmethods`.

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
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Richieste API GraphQL AEM autorizzate

Quando accedi AEM API GraphQL che richiedono un’autorizzazione (in genere AEM Author o contenuti protetti su AEM Publish) assicurati che la configurazione OSGi CORS abbia i valori aggiuntivi:

+ `supportedheaders` elenchi `"Authorization"`
+ `supportscredentials` è impostato su `true`

Le richieste autorizzate a AEM API GraphQL che richiedono la configurazione CORS sono insolite, in quanto si verificano in genere nel contesto di [app server-to-server](../server-to-server.md) e quindi, non richiedono la configurazione CORS. App basate su browser che richiedono configurazioni CORS, ad esempio [app a pagina singola](../spa.md) o [Componenti web](../web-component.md), in genere l’autorizzazione viene utilizzata in quanto è difficile proteggere le credenziali .

Ad esempio, queste due impostazioni vengono impostate come segue in un `CORSPolicyImpl` Configurazione di fabbrica OSGi:

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

+ [Un esempio della configurazione OSGi si trova nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Configurazione del Dispatcher

Il Dispatcher del servizio AEM Publish (e Preview) deve essere configurato per supportare CORS.

| Il client si connette a | Autore AEM | Pubblicazione AEM | Anteprima AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione CORS del Dispatcher | ✘ | ↓ | ↓ |

### Consenti intestazioni CORS su richieste HTTP

Aggiorna `clientheaders.any` per consentire le intestazioni di richiesta HTTP `Origin`,  `Access-Control-Request-Method`e `Access-Control-Request-Headers` da trasmettere a AEM, consentendo l’elaborazione della richiesta HTTP da parte del [Configurazione AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Esempio di configurazione del Dispatcher

+ [Esempio di Dispatcher _intestazioni client_ configurazione disponibile nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Fornire intestazioni di risposta CORS HTTP

Configurare la farm del dispatcher per la cache **Intestazioni di risposta HTTP CORS** per assicurarti che siano inclusi quando AEM query persistenti GraphQL vengono servite dalla cache del Dispatcher aggiungendo il `Access-Control-...` nell&#39;elenco delle intestazioni della cache.

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

#### Esempio di configurazione del Dispatcher

+ [Esempio di Dispatcher _Intestazioni di risposta HTTP CORS_ configurazione disponibile nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
