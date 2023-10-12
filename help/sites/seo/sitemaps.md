---
title: Sitemap
description: Scopri come incrementare l’ottimizzazione SEO (Search Engine Optimization) creando sitemap per AEM Sites.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 5%

---

# Sitemap

Scopri come incrementare l’ottimizzazione SEO (Search Engine Optimization) creando sitemap per AEM Sites.

>[!WARNING]
>
>Questo video illustra l’utilizzo di URL relativi nella mappa del sito. Sitemap [deve utilizzare URL assoluti](https://sitemaps.org/protocol.html). Consulta [Configurazioni](#absolute-sitemap-urls) per informazioni su come abilitare gli URL assoluti, in quanto ciò non è trattato nel video seguente.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configurazioni

### URL sitemap assoluti{#absolute-sitemap-urls}

La mappa del sito AEM supporta gli URL assoluti utilizzando [Mappatura Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Ciò avviene attraverso la creazione di nodi di mappatura sui servizi AEM che generano sitemap (in genere il servizio di pubblicazione AEM).

Esempio di definizione del nodo di mappatura Sling per `https://wknd.com` può essere definito in `/etc/map/https` come segue:

| Percorso | Nome proprietà | Tipo di proprietà | Valore proprietà |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | Stringa | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | Stringa | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | Stringa | `wknd.com/$1` |

La schermata seguente illustra una configurazione simile, ma per `http://wknd.local` (mapping di nome host locale in esecuzione su `http`).

![Configurazione degli URL assoluti di Sitemap](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Configurazione OSGi dell’utilità di pianificazione di Sitemap

Definisce il [Configurazione di fabbrica OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) per la frequenza (utilizzando [espressioni cron](http://www.cronmaker.com/)) le sitemap vengono rigenerate e memorizzate nella cache dell&#39;AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Regola di filtro Consentiti di Dispatcher

Consenti richieste HTTP per i file di indice e mappa del sito della mappa del sito.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regola di riscrittura server web Apache

Assicurare `.xml` Le richieste HTTP di sitemap vengono indirizzate alla pagina AEM sottostante corretta. Se non si utilizza l’abbreviazione degli URL o si utilizzano mappature Sling per ottenere l’abbreviazione degli URL, questa configurazione non è necessaria.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Riferimenti

+ [Documentazione di AEM Sitemap](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [Documentazione di Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentazione di Sitemap.org Sitemap](https://www.sitemaps.org/protocol.html)
+ [Documentazione del file di indice Sitemap.org Sitemap](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)