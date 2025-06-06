---
title: Connessioni HTTP/HTTPS su porte non standard per l’uscita della porta flessibile
description: Scopri come effettuare richieste HTTP/HTTPS da AEM as a Cloud Service a servizi web esterni in esecuzione su porte non standard per l’uscita della porta flessibile.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
duration: 86
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Connessioni HTTP/HTTPS su porte non standard per l’uscita della porta flessibile

Le connessioni HTTP/HTTPS su porte non standard (non 80/443) devono essere escluse da AEM as a Cloud Service, tuttavia non richiedono regole `portForwards` speciali e possono utilizzare la rete avanzata di AEM `AEM_PROXY_HOST` e una porta proxy riservata `AEM_HTTP_PROXY_PORT` o `AEM_HTTPS_PROXY_PORT` a seconda che la destinazione sia HTTP/HTTPS.

## Supporto di rete avanzato

Il codice di esempio seguente è supportato dalle seguenti opzioni di rete avanzate.

Prima di seguire questa esercitazione, assicurati che la configurazione di rete avanzata [appropriata](../advanced-networking.md#advanced-networking) sia stata configurata.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Rete privata virtuale](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Questo esempio di codice è valido solo per [Uscita porta flessibile](../flexible-port-egress.md). Un esempio di codice simile ma diverso è disponibile per [connessioni HTTP/HTTPS su porte non standard per indirizzo IP di uscita dedicato e VPN](./http-dedicated-egress-ip-vpn.md).

## Esempio di codice

Questo esempio di codice Java™ fa parte di un servizio OSGi che può essere eseguito in AEM as a Cloud Service e che effettua una connessione HTTP a un server web esterno su 8080. Le connessioni ai server web HTTPS utilizzano le variabili di ambiente `AEM_PROXY_HOST` e `AEM_HTTPS_PROXY_PORT` (impostazione predefinita `proxy.tunnel:3128` nelle versioni AEM &lt; 6094).

>[!NOTE]
> Si consiglia di utilizzare le [API Java™ 11 HTTP](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) per effettuare chiamate HTTP/HTTPS da AEM.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
