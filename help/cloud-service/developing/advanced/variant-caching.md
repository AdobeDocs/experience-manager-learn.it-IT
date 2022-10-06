---
title: Varianti di pagina nella memorizzazione in cache con AEM as a Cloud Service
description: Scopri come impostare e utilizzare AEM as a cloud service per supportare la memorizzazione in cache delle varianti di pagina.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---

# Varianti di pagina nella cache

Scopri come impostare e utilizzare AEM as a cloud service per supportare la memorizzazione in cache delle varianti di pagina.

## Esempi di casi d’uso

+ Qualsiasi fornitore di servizi che offre un diverso set di offerte di servizi e le relative opzioni di prezzo in base alla geolocalizzazione dell’utente e alla cache delle pagine con contenuto dinamico deve essere gestito in CDN e Dispatcher.

+ Un cliente al dettaglio ha negozi in tutto il paese e ogni negozio ha offerte diverse in base alla posizione in cui si trovano e la cache delle pagine con contenuto dinamico deve essere gestita in CDN e Dispatcher.

## Panoramica della soluzione

+ Identifica la chiave della variante e il numero di valori che potrebbe avere. Nel nostro esempio, variiamo in base allo stato degli Stati Uniti, quindi il numero massimo è 50. Questo è abbastanza piccolo per non causare problemi con i limiti delle varianti alla CDN. [Revisione della sezione sulle limitazioni delle varianti](#variant-limitations).

+ Il codice AEM deve impostare il cookie __&quot;x-aem-variant&quot;__ allo stato preferito del visitatore (ad esempio `Set-Cookie: x-aem-variant=NY`) nella risposta HTTP corrispondente della richiesta HTTP iniziale.

+ Le richieste successive del visitatore inviano quel cookie (ad esempio `"Cookie: x-aem-variant=NY"`) e il cookie viene trasformato a livello di CDN in un’intestazione predefinita (ad es. `x-aem-variant:NY`) che viene passato al dispatcher.

+ Una regola di riscrittura Apache modifica il percorso della richiesta per includere il valore dell&#39;intestazione nell&#39;URL della pagina come selettore Sling Apache (ad esempio `/page.variant=NY.html`). Questo consente ad AEM Publish di distribuire contenuti diversi in base al selettore e al dispatcher di memorizzare nella cache una pagina per variante.

+ La risposta inviata da AEM Dispatcher deve contenere un’intestazione di risposta HTTP `Vary: x-aem-variant`. Questo indica alla CDN di memorizzare diverse copie della cache per diversi valori di intestazione.

>[!TIP]
>
>Ogni volta che viene impostato un cookie (ad esempio Set-Cookie: x-aem-variant=NY) la risposta non deve essere memorizzabile nella cache (deve avere Cache-Control: private o Cache-Control: no-cache)

## flusso di richieste HTTP

![Flusso della richiesta di cache variante](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>Il flusso iniziale di richiesta HTTP di cui sopra deve avvenire prima che venga richiesto qualsiasi contenuto che utilizzi varianti.

## Utilizzo

1. Per illustrare la funzione, utilizzeremo [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it)Implementazione di come esempio.

1. Implementare un [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) in AEM da impostare `x-aem-variant` cookie sulla risposta HTTP, con un valore variante.

1. AEM CDN si trasforma automaticamente `x-aem-variant` in un&#39;intestazione HTTP con lo stesso nome.

1. Aggiungi una regola mod_rewrite del server Web Apache al tuo `dispatcher` , che modifica il percorso della richiesta per includere il selettore della variante.

1. Distribuisci il filtro e riscrivi le regole utilizzando Cloud Manager.

1. Verifica il flusso complessivo della richiesta.

## Esempi di codice

+ Esempio di SlingServletFilter da impostare `x-aem-variant` cookie con un valore in AEM.

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

+ Regola di riscrittura di esempio nel __dispatcher/src/conf.d/rewrite.rules__ file gestito come codice sorgente in Git e distribuito utilizzando Cloud Manager.

   ```
   ...
   
   RewriteCond %{REQUEST_URI} ^/us/.*  
   RewriteCond %{HTTP:x-aem-variant} ^.*$  
   RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
   
   ...
   ```

## Limiti della variante

+ AEM CDN può gestire fino a 200 varianti. Significa che `x-aem-variant` L&#39;intestazione può contenere fino a 200 valori univoci. Per ulteriori informazioni, consulta la sezione [Limiti di configurazione CDN](https://docs.fastly.com/en/guides/resource-limits).

+ Assicurati che la chiave della variante scelta non superi mai questo numero.  Ad esempio, un ID utente non è una buona chiave in quanto supererebbe facilmente 200 valori per la maggior parte dei siti web, mentre gli stati/territori in un paese sono più adatti se ci sono meno di 200 stati in quel paese.

>[!NOTE]
>
>Quando le varianti superano 200, la rete CDN risponderà con la risposta &quot;Troppe varianti&quot; invece del contenuto della pagina.
