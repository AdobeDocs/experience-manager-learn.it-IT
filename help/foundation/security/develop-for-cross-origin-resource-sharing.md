---
title: Sviluppare per la condivisione CORS (Cross-Origin Resource Sharing) con AEM
description: Un breve esempio di utilizzo di CORS per accedere ai contenuti AEM da un’applicazione web esterna tramite JavaScript lato client.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 356
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Sviluppo per la condivisione CORS (Cross-Origin Resource Sharing)

Un breve esempio di utilizzo [!DNL CORS] per accedere ai contenuti AEM da un’applicazione web esterna tramite JavaScript lato client. In questo esempio viene utilizzata la configurazione OSGi CORS per abilitare l’accesso CORS all’AEM. L’approccio di configurazione OSGi è fattibile quando:

* Una singola origine accede al contenuto della pubblicazione AEM
* Per l’authoring AEM è necessario l’accesso CORS

Se è richiesto l’accesso multiorigine alla pubblicazione dell’AEM, consulta [questa documentazione](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

In questo video:

* **www.example.com** mappa su localhost tramite `/etc/hosts`
* **aem-publish.local** mappa su localhost tramite `/etc/hosts`
* SimpleHTTPServer (un wrapper per [[!DNL Python]SimpleHTTPServer di](https://docs.python.org/2/library/simplehttpserver.html)) sta servendo la pagina HTML tramite la porta 8000.
   * _Non più disponibile in Mac App Store. Utilizza simili, ad esempio [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] è in esecuzione su [!DNL Apache HTTP Web Server] 2.4 e richiesta di reverse-proxy a `aem-publish.local` a `localhost:4503`.

Per ulteriori dettagli, consulta [Condivisione CORS (Cross-Origin Resource Sharing) nell’AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML e JavaScript

La logica di questa pagina Web è

1. Facendo clic sul pulsante
1. Crea un [!DNL AJAX GET] richiesta a `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera il `jcr:title` formare la risposta JSON
1. Inietta il `jcr:title` nel DOM

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

Configurazione OSGi factory per [!DNL Cross-Origin Resource Sharing] è disponibile tramite:

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

### Consentire le intestazioni di richiesta CORS

Per consentire il necessario [Intestazioni di richiesta HTTP da passare all’AEM per l’elaborazione](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), devono essere consentiti nel `/clientheaders` configurazione.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Memorizzazione nella cache delle intestazioni di risposta CORS

Per consentire la memorizzazione in cache e il serving delle intestazioni CORS sul contenuto memorizzato in cache, aggiungi quanto segue [/cache /headers, configurazione](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it#caching-http-response-headers) alla pubblicazione AEM `dispatcher.any` file.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

**Riavviare l&#39;applicazione server Web** dopo aver apportato modifiche al `dispatcher.any` file.

È probabile che la cancellazione completa della cache sia necessaria per garantire che le intestazioni siano memorizzate nella cache in modo appropriato alla successiva richiesta dopo un `/cache /headers` aggiornamento della configurazione.

## Materiali di supporto {#supporting-materials}

* [Jeeves per macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (compatibile con Windows/macOS/Linux)

* [Condivisione CORS (Cross-Origin Resource Sharing) nell’AEM](./understand-cross-origin-resource-sharing.md)
* [Condivisione risorse tra origini (W3C)](https://www.w3.org/TR/cors/)
* [Controllo accesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
