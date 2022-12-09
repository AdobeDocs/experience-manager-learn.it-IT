---
title: Filtri del Dispatcher per AEM GraphQL
description: Scopri come configurare i filtri di AEM Publish Dispatcher per l’utilizzo con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: 442020d854d8f42c5d8a1340afd907548875866e
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 2%

---


# Filtri del Dispatcher

Adobe Experience Manager as a Cloud Service utilizza i filtri AEM Publish Dispatcher per garantire solo le richieste che devono raggiungere AEM raggiungere AEM di portata. Per impostazione predefinita, tutte le richieste sono negate e i pattern per gli URL consentiti devono essere aggiunti esplicitamente.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Richiede la configurazione dei filtri del Dispatcher | ↓ | ↓ | ↓ | ↓ |

>[!TIP]
>
> Di seguito sono riportati alcuni esempi di configurazioni. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Configurazione del filtro del Dispatcher

La configurazione del filtro AEM Publish Dispatcher definisce i pattern URL consentiti per raggiungere AEM e deve includere il prefisso URL per l’endpoint di query AEM persistente.

| Il client si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione dei filtri del Dispatcher | ✘ | ↓ | ↓ |

Aggiungi un `allow` regola con il pattern URL `/graphql/execute.json/*`, e assicurati l&#39;ID del file (ad esempio `/0600`, è univoco nel file farm di esempio).
Ciò consente la richiesta HTTP GET all’endpoint della query persistente, ad esempio `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` da ad AEM Publish.

Se utilizzi i frammenti esperienza nell’esperienza AEM headless, procedi allo stesso modo per questi percorsi.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### Esempio di configurazione dei filtri

+ [Un esempio del filtro Dispatcher si trova nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
