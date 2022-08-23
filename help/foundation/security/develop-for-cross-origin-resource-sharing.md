---
title: Sviluppa per CORS (Cross-Origin Resource Sharing) con AEM
description: Un breve esempio di utilizzo di CORS per accedere AEM contenuto da un’applicazione web esterna tramite JavaScript lato client.
version: 6,4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# Sviluppa per la condivisione delle risorse tra le origini (CORS, Cross-Origin Resource Sharing)

Un breve esempio di utilizzo della leva finanziaria [!DNL CORS] per accedere AEM contenuto da un’applicazione web esterna tramite JavaScript lato client.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

In questo video:

* **www.example.com** mappa su localhost tramite `/etc/hosts`
* **aem-publish.local** mappa su localhost tramite `/etc/hosts`
* SimpleHTTPServer (un wrapper per per [[!DNL Python]s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) serve la pagina HTML tramite la porta 8000.
   * _Non più disponibile in Mac App Store. Usa simili, ad esempio [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] è in esecuzione [!DNL Apache HTTP Web Server] 2.4 e la richiesta di reverse proxy a `aem-publish.local` a `localhost:4503`.

Per ulteriori dettagli, consulta [Informazioni sulla condivisione delle risorse tra le origini (Cross-Origin Resource Sharing, CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML e JavaScript

Questa pagina Web ha una logica che

1. Facendo clic sul pulsante
1. Rende un [!DNL AJAX GET] richiesta a `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera la `jcr:title` dalla risposta JSON
1. Inietta `jcr:title` nel DOM

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

## Configurazione del Dispatcher {#dispatcher-configuration}

Per consentire la memorizzazione in cache e il serving delle intestazioni CORS sul contenuto in cache, aggiungi quanto segue [/clientheaders configurazione](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) a tutti i supporti AEM Publish `dispatcher.any` file.

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Riavvia l&#39;applicazione server web** dopo aver apportato modifiche al `dispatcher.any` file.

Probabilmente cancella la cache interamente necessaria per garantire che le intestazioni siano correttamente memorizzate nella cache nella richiesta successiva dopo un `/clientheaders` aggiornamento della configurazione.

## Materiali di supporto {#supporting-materials}

* [AEM fabbrica di configurazione OSGi per i criteri di condivisione delle risorse tra origini diverse](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Jeeves per macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (compatibile con Windows/macOS/Linux)

* [Informazioni sulla condivisione delle risorse tra le origini (Cross-Origin Resource Sharing, CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [Condivisione risorse tra le origini (W3C)](https://www.w3.org/TR/cors/)
* [Controllo degli accessi HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
