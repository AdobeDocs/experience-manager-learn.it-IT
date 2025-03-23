---
title: Filtri Dispatcher per AEM GraphQL
description: Scopri come configurare i filtri AEM Publish Dispatcher per l’utilizzo con AEM GraphQL.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Filtri Dispatcher

Adobe Experience Manager as a Cloud Service utilizza i filtri AEM Publish Dispatcher per garantire che solo le richieste che devono raggiungere AEM raggiungano AEM. Per impostazione predefinita, tutte le richieste sono negate e i modelli per gli URL consentiti devono essere aggiunti esplicitamente.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Richiede la configurazione dei filtri di Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Le seguenti configurazioni sono esempi. Assicurati di regolarli per allinearli ai requisiti del progetto.

## Configurazione filtro Dispatcher

La configurazione del filtro Dispatcher di pubblicazione di AEM definisce i pattern di URL consentiti per raggiungere AEM e deve includere il prefisso URL per l’endpoint di query persistente di AEM.

| Il client si collega a | AEM Author | AEM Publish | Anteprima AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Richiede la configurazione dei filtri di Dispatcher | ✘ | ✔ | ✔ |

Aggiungere una regola `allow` con il pattern URL `/graphql/execute.json/*` e verificare che l&#39;ID file (ad esempio `/0600`, sia univoco nel file farm di esempio).
Consente la richiesta HTTP GET all&#39;endpoint di query persistente, ad esempio `HTTP GET /graphql/execute.json/wknd-shared/adventures-all`, fino a AEM Publish.

Se utilizzi Frammenti di esperienza nell’esperienza headless AEM, procedi allo stesso modo per questi percorsi.

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

+ [Un esempio del filtro Dispatcher è disponibile nel progetto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
