---
title: Mappe del sito
description: Scopri come incrementare l’ottimizzazione SEO (Search Engine Optimization) creando sitemap per AEM Sites.
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 937
source-git-commit: d2714443fa644ba17afdfbed5e6da8091425aeab
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 5%

---

# Mappe del sito

Scopri come incrementare l’ottimizzazione SEO (Search Engine Optimization) creando sitemap per AEM Sites.

>[!WARNING]
>
>Questo video illustra l’utilizzo di URL relativi nella mappa del sito. Le mappe del sito [ devono utilizzare URL assoluti](https://sitemaps.org/protocol.html). Per informazioni su come abilitare gli URL assoluti, consulta [Configurazioni](#absolute-sitemap-urls), in quanto questo argomento non è trattato nel video seguente.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configurazioni

### URL sitemap assoluti{#absolute-sitemap-urls}

La mappa del sito di AEM supporta gli URL assoluti utilizzando [Mapping Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Ciò avviene attraverso la creazione di nodi di mappatura nei servizi AEM che generano sitemap (in genere il servizio AEM Publish).

Un esempio di definizione del nodo di mappatura Sling per `https://wknd.com` può essere definito in `/etc/map/https` come segue:

| Percorso | Nome proprietà | Tipo di proprietà | Valore proprietà |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | Stringa | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | Stringa | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | Stringa | `wknd.com/$1` |

La schermata seguente illustra una configurazione simile ma per `http://wknd.local` (una mappatura del nome host locale in esecuzione su `http`).

![Configurazione URL assoluti sitemap](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Configurazione OSGi dell’utilità di pianificazione di Sitemap

Definisce la [configurazione di fabbrica OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) per la frequenza (utilizzando [espressioni cron](https://cron.help/)) con cui le sitemap vengono rigenerate/generate e memorizzate nella cache in AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.author`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Regola filtro Consentiti Dispatcher

Consenti richieste HTTP per i file di indice e mappa del sito della mappa del sito.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regola di riscrittura server web Apache

Assicurati che `.xml` richieste HTTP sitemap vengano indirizzate alla pagina AEM sottostante corretta. Se non si utilizza l’abbreviazione degli URL o si utilizzano mappature Sling per ottenere l’abbreviazione degli URL, questa configurazione non è necessaria.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Risorse

+ [Documentazione di AEM Sitemap](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [Documentazione di Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Documentazione sitemap](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Documentazione del file di indice Sitemap](https://www.sitemaps.org/protocol.html#index)
+ [Helper Cron](https://cron.help/)
