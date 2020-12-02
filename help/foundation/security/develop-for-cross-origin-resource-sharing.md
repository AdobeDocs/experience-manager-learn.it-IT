---
title: Sviluppo per CORS (Cross Origin Resource Sharing) con AEM
description: Un breve esempio di utilizzo di CORS per accedere AEM contenuto da un'applicazione Web esterna tramite JavaScript lato client.
version: 6.3, 6,4, 6.5
sub-product: servizi di base, servizi di contenuto, siti
feature: null
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---


# Sviluppo per CORS (Cross-Origin Resource Sharing)

Un breve esempio di utilizzo di [!DNL CORS] per accedere AEM contenuto da un&#39;applicazione Web esterna tramite JavaScript lato client.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

In questo video:

* **www.example.** commaps to localhost tramite  `/etc/hosts`
* **aem-publish.** localmap su localhost tramite  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (un wrapper per  [[!DNL Python]SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) distribuisce la pagina HTML tramite la porta 8000.
* [!DNL AEM Dispatcher] è in esecuzione su  [!DNL Apache HTTP Web Server] 2.4 e la richiesta  `aem-publish.local` di inoltro invertito su  `localhost:4503`.

Per ulteriori dettagli, vedere [Informazioni sulla condivisione delle risorse tra le origini (CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML e JavaScript

Questa pagina Web ha una logica che

1. Quando si fa clic sul pulsante
1. Invia una richiesta [!DNL AJAX GET] a `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera la `jcr:title` dalla risposta JSON
1. Inserisce il `jcr:title` nel DOM

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## Configurazione di fabbrica OSGi

La fabbrica di configurazione OSGi per [!DNL Cross-Origin Resource Sharing] è disponibile tramite:

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Configurazione del dispatcher {#dispatcher-configuration}

Per consentire il caching e la gestione delle intestazioni [!DNL CORS] sui contenuti memorizzati nella cache, aggiungete la seguente configurazione a tutti i file AEM Publish `dispatcher.any` che supportano.

```
/cache { 
  ...
  /headers {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Riavviate l&#39;** applicazione server Web dopo aver apportato le modifiche al  `dispatcher.any` file.

È probabile che la cancellazione completa della cache sia necessaria per garantire che le intestazioni siano correttamente memorizzate nella cache sulla richiesta successiva dopo un aggiornamento di configurazione `/headers`.

## Materiali di supporto {#supporting-materials}

* [AEM fabbrica di configurazione OSGi per i criteri di condivisione delle risorse tra origini](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer per macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (compatibile con Windows/macOS/Linux)

* [Informazioni sulla condivisione delle risorse tra le origini (CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [Condivisione risorse tra le origini (W3C)](https://www.w3.org/TR/cors/)
* [Controllo degli accessi HTTP (MDN Mozilla)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

