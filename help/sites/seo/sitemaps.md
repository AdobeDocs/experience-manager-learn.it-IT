---
title: Sitemaps
description: Scopri come incrementare il SEO creando sitemap per AEM Sites.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 71f1d32c12742cdb644dec50050d147395c3f3b6
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# Sitemaps

Scopri come incrementare il SEO creando sitemap per AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Riferimenti

+ [Documentazione AEM Sitemap](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentazione della mappa del sito Apache Sling](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentazione Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Documentazione del file di indice Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronista](http://www.cronmaker.com/)

## Configurazioni

### Configurazione OSGi dello scheduler di Sitemap

Definisce la [Configurazione di fabbrica OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) per la frequenza (utilizzando [espressioni cron](http://www.cronmaker.com)) le mappe dei siti verranno rigenerate e memorizzate nella cache in AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Regola del filtro Consentiti da Dispatcher

Consenti richieste HTTP per l&#39;indice della mappa del sito e i file della mappa del sito.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regola di riscrittura del server web Apache

Assicurati `.xml` le richieste HTTP della mappa del sito vengono indirizzate alla pagina AEM sottostante corretta. Se non si utilizza la abbreviazione degli URL, o se le mappature Sling vengono utilizzate per ottenere la riduzione degli URL, questa configurazione non è necessaria.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
