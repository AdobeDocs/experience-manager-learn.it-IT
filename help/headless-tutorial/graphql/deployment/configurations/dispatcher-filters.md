---
title: Filtri di Dispatcher per AEM GraphQL
description: Scopri come configurare i filtri del Dispatcher di pubblicazione dell’AEM da utilizzare con il GraphQL dell’AEM.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 79
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Filtri del Dispatcher

Adobe Experience Manager as a Cloud Service utilizza i filtri del Dispatcher di pubblicazione dell’AEM per garantire che solo le richieste che devono raggiungere l’AEM raggiungano l’AEM. Per impostazione predefinita, tutte le richieste sono negate e i modelli per gli URL consentiti devono essere aggiunti esplicitamente.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Richiede la configurazione dei filtri di Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Le seguenti configurazioni sono esempi. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Configurazione del filtro di Dispatcher

La configurazione del filtro del Dispatcher di pubblicazione dell’AEM definisce i pattern di URL consentiti per raggiungere l’AEM e deve includere il prefisso URL per l’endpoint di query persistente dell’AEM.

| Il client si collega a | Autore AEM | AEM Publish | Anteprima AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione dei filtri di Dispatcher | ✘ | ✔ | ✔ |

Aggiungi un `allow` regola con il pattern URL `/graphql/execute.json/*`, e garantire l&#39;ID file (ad esempio `/0600`, è univoco nel file farm di esempio).
Questo consente la richiesta HTTP GET all’endpoint di query persistente, ad esempio `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` fino a Pubblicazione AEM.

Se utilizzi Frammenti di esperienza nell’esperienza AEM headless, procedi allo stesso modo per questi percorsi.

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

+ [Un esempio del filtro di Dispatcher è disponibile nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
