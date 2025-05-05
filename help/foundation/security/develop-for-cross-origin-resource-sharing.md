---
title: Sviluppare per la condivisione CORS (Cross-Origin Resource Sharing) con AEM
description: Un breve esempio di utilizzo di CORS per accedere ai contenuti di AEM da un’applicazione web esterna tramite JavaScript lato client.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Sviluppo per la condivisione CORS (Cross-Origin Resource Sharing)

Breve esempio di utilizzo di [!DNL CORS] per accedere al contenuto di AEM da un&#39;applicazione Web esterna tramite JavaScript lato client. In questo esempio viene utilizzata la configurazione OSGi CORS per abilitare l’accesso CORS su AEM. L’approccio di configurazione OSGi è fattibile quando:

* Una singola origine accede al contenuto AEM Publish
* Per AEM Author è necessario l’accesso a CORS

Se è necessario l&#39;accesso multiorigine alla pubblicazione AEM, consulta [questa documentazione](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=it#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

In questo video:

* **www.example.com** è mappato a localhost tramite `/etc/hosts`
* **aem-publish.local** è mappato a localhost tramite `/etc/hosts`
* SimpleHTTPServer (un wrapper per SimpleHTTPServer[&#128279;](https://docs.python.org/2/library/simplehttpserver.html) di [!DNL Python]) sta distribuendo la pagina HTML tramite la porta 8000.
   * _Non più disponibile in Mac App Store. Usa simili come [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] è in esecuzione in [!DNL Apache HTTP Web Server] 2.4 e la richiesta di reverse-proxying a `aem-publish.local` in `localhost:4503` è in esecuzione.

Per ulteriori dettagli, rivedi [Informazioni su Cross-Origin Resource Sharing (CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML e JAVASCRIPT

La logica di questa pagina Web è

1. Facendo clic sul pulsante
1. Invia una richiesta [!DNL AJAX GET] a `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera `jcr:title` dalla risposta JSON
1. Inserisce `jcr:title` nel DOM

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

La factory di configurazione OSGi per [!DNL Cross-Origin Resource Sharing] è disponibile tramite:

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

## Configurazione Dispatcher {#dispatcher-configuration}

### Consentire le intestazioni di richiesta CORS

Per consentire alle intestazioni di richiesta HTTP [di passare ad AEM per l&#39;elaborazione](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it#specifying-the-http-headers-to-pass-through-clientheaders), devono essere consentite nella configurazione `/clientheaders` di Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Memorizzazione nella cache delle intestazioni di risposta CORS

Per consentire il caching e il serving delle intestazioni CORS sul contenuto memorizzato nella cache, aggiungi la seguente configurazione [/cache /headers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it#caching-http-response-headers) al file `dispatcher.any` di pubblicazione di AEM.

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

**Riavviare l&#39;applicazione server Web** dopo aver apportato modifiche al file `dispatcher.any`.

È probabile che la cancellazione completa della cache sia necessaria per garantire che le intestazioni siano memorizzate nella cache in modo appropriato alla successiva richiesta dopo un aggiornamento della configurazione di `/cache /headers`.

## Materiali di supporto {#supporting-materials}

* [Jeeves per macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (compatibile con Windows/macOS/Linux)

* [Condivisione CORS (Cross-Origin Resource Sharing) in AEM](./understand-cross-origin-resource-sharing.md)
* [Condivisione risorse tra le origini (W3C)](https://www.w3.org/TR/cors/)
* [Controllo accesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
