---
title: Memorizzazione in cache delle varianti di pagina con AEM as a Cloud Service
description: Scopri come impostare e utilizzare AEM as a Cloud Service per supportare il caching delle varianti di pagina.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
duration: 149
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 1%

---

# Memorizzazione in cache delle varianti di pagina

Scopri come impostare e utilizzare AEM as a Cloud Service per supportare il caching delle varianti di pagina.

## Casi d’uso di esempio

+ Qualsiasi provider di servizi che offra un set diverso di offerte di servizi e opzioni di prezzo corrispondenti in base alla posizione geografica dell’utente e alla cache delle pagine con contenuti dinamici deve essere gestito in CDN e Dispatcher.

+ Un cliente al dettaglio dispone di negozi in tutto il paese e ogni negozio dispone di offerte diverse in base alla posizione in cui si trova, e la cache delle pagine con contenuti dinamici deve essere gestita in CDN e Dispatcher.

## Panoramica della soluzione

+ Identifica la chiave della variante e il numero di valori che può avere. Nel nostro esempio, variiamo in base allo stato USA, quindi il numero massimo è 50. È abbastanza piccolo da non causare problemi con i limiti delle varianti nella rete CDN. [Rivedi la sezione limitazioni varianti](#variant-limitations).

+ Il codice AEM deve impostare il cookie __&quot;x-aem-variant&quot;__ sullo stato preferito del visitatore (ad esempio `Set-Cookie: x-aem-variant=NY`) nella risposta HTTP corrispondente della richiesta HTTP iniziale.

+ Le richieste successive del visitatore inviano quel cookie (esempio: `"Cookie: x-aem-variant=NY"`) e il cookie viene trasformato a livello CDN in un&#39;intestazione predefinita (ovvero `x-aem-variant:NY`) che viene passata al dispatcher.

+ Una regola di riscrittura Apache modifica il percorso della richiesta per includere il valore di intestazione nell’URL della pagina come selettore Sling di Apache (ad esempio, `/page.variant=NY.html`). Questo consente a AEM Publish di distribuire contenuti diversi in base al selettore e al dispatcher di memorizzare in cache una pagina per variante.

+ La risposta inviata dal Dispatcher AEM deve contenere un&#39;intestazione di risposta HTTP `Vary: x-aem-variant`. Questo indica alla rete CDN di memorizzare diverse copie della cache per diversi valori di intestazione.

>[!TIP]
>
>Ogni volta che viene impostato un cookie (ad es. Set-Cookie: x-aem-variant=NY) la risposta non deve essere memorizzabile in cache (deve avere Cache-Control: private o Cache-Control: no-cache)

## Flusso di richieste HTTP

![Flusso richieste cache variante](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>Il flusso di richiesta HTTP iniziale deve avvenire prima che venga richiesto qualsiasi contenuto che utilizza varianti.

## Utilizzo

1. Per illustrare la funzionalità, verrà utilizzata l&#39;implementazione di [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it) come esempio.

1. Implementa un [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) in AEM per impostare il cookie `x-aem-variant` nella risposta HTTP, con un valore variante.

1. Il CDN dell&#39;AEM trasforma automaticamente il cookie `x-aem-variant` in un&#39;intestazione HTTP con lo stesso nome.

1. Aggiungi al progetto `dispatcher` una regola mod_rewrite del server Web Apache che modifica il percorso della richiesta in modo da includere il selettore delle varianti.

1. Distribuisci il filtro e riscrivi le regole utilizzando Cloud Manager.

1. Verifica il flusso di richiesta complessivo.

## Esempi di codice

+ SlingServletFilter di esempio per impostare il cookie `x-aem-variant` con un valore in AEM.

  ```
  package com.adobe.aem.guides.wknd.core.servlets.filters;
  
  import javax.servlet.*;
  import java.io.IOException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.servlets.annotations.SlingServletFilter;
  import org.apache.sling.servlets.annotations.SlingServletFilterScope;
  import org.osgi.service.component.annotations.Component;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  
  
  // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
  // This code and scope is for example purposes only, and will not interfere with other requests.
  @Component
  @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
          resourceTypes = {"cq:Page"},
          pattern = "/content/wknd/.*",
          extensions = {"html", "json"},
          methods = {"GET"})
  public class PageVariantFilter implements Filter {
      private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
      private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
  
      @Override
      public void init(FilterConfig filterConfig) throws ServletException { }
  
      @Override
      public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
          SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
          SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
  
          // Check is the variant was previously set
          final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
  
          if (existingVariant == null) {
              // Variant has not been set, so set it now
              String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
              slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
              log.debug("x-aem-variant cookie is set with the value {}", newVariant);
          } else {
              log.debug("x-aem-variant previously set with value {}", existingVariant);
          }
  
          filterChain.doFilter(servletRequest, slingResponse);
      }
  
      @Override
      public void destroy() { }
  }
  ```

+ Regola di riscrittura di esempio nel file __dispatcher/src/conf.d/rewrite.rules__ gestito come codice sorgente in Git e distribuito tramite Cloud Manager.

  ```
  ...
  
  RewriteCond %{REQUEST_URI} ^/us/.*  
  RewriteCond %{HTTP:x-aem-variant} ^.*$  
  RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
  
  ...
  ```

## Limitazioni delle varianti

+ La rete CDN dell’AEM può gestire fino a 200 varianti. Ciò significa che l&#39;intestazione `x-aem-variant` può avere fino a 200 valori univoci. Per ulteriori informazioni, controlla i [limiti di configurazione CDN](https://docs.fastly.com/en/guides/resource-limits).

+ Fai attenzione a che il codice variante scelto non superi mai questo numero.  Ad esempio, un ID utente non è una buona chiave in quanto supererebbe facilmente i 200 valori per la maggior parte dei siti web, mentre gli stati/territori di un paese sono più adatti se ci sono meno di 200 stati in quel paese.

>[!NOTE]
>
>Quando le varianti superano le 200, la CDN risponderà con la risposta &quot;Troppe varianti&quot; invece del contenuto della pagina.
