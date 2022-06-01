---
title: Connessioni HTTP/HTTPS per indirizzo IP in uscita dedicato e VPN
description: Scopri come effettuare richieste HTTP/HTTPS da AEM as a Cloud Service a servizi web esterni in esecuzione per l’indirizzo IP dedicato e la VPN
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# Connessioni HTTP/HTTPS per indirizzo IP in uscita dedicato e VPN

Le connessioni HTTP/HTTPS devono essere proxy fuori AEM as a Cloud Service, ma non sono necessarie connessioni speciali `portForwards` e possono utilizzare AEM reti avanzate `AEM_HTTP_PROXY_HOST`, `AEM_HTTP_PROXY_PORT`, `AEM_HTTPS_PROXY_HOST`e `AEM_HTTPS_PROXY_PORT`.

## Supporto di rete avanzato

L&#39;esempio di codice seguente è supportato dalle seguenti opzioni di rete avanzate.

Assicurati che [appropriato](../advanced-networking.md#advanced-networking) la configurazione di rete avanzata è stata impostata prima di seguire questa esercitazione.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Rete privata virtuale](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ↓ | ↓ |

>[!CAUTION]
>
> Questo esempio di codice è valido solo per [Indirizzo IP Egress dedicato](../dedicated-egress-ip-address.md) e [VPN](../vpn.md). Un esempio di codice simile ma diverso è disponibile per [Connessioni HTTP/HTTPS su porte non standard per Flexible Port Egress](./http-on-non-standard-ports-flexible-port-egress.md).

## Esempio di codice

Questo esempio di codice Java™ è di un servizio OSGi che può essere eseguito in AEM as a Cloud Service che effettua una connessione HTTP a un server web esterno su 8080. Le connessioni ai server web HTTPS utilizzano `AEM_HTTPS_PROXY_HOST` e `AEM_HTTPS_PROXY_PORT` anziché  `AEM_HTTP_PROXY_HOST` e `AEM_HTTP_PROXY_PORT`.

>[!NOTE]
> Si consiglia di [API HTTP Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) vengono utilizzati per effettuare chiamate HTTP/HTTPS da AEM.

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

        // If the URL is http, use System.getenv("AEM_HTTP_PROXY_HOST") and System.getenv("AEM_HTTP_PROXY_PORT")
        // Else if the URL is https, us System.getenv("AEM_HTTPS_PROXY_HOST") and System.getenv("AEM_HTTPS_PROXY_PORT")

        if (System.getenv("AEM_HTTP_PROXY_HOST") != null) {
            // Create a ProxySelector that maps to AEM's provided AEM_HTTP_PROXY_HOST and AEM_HTTP_PROXY_PORT
            ProxySelector proxySelector = ProxySelector.of(
                    new InetSocketAddress(System.getenv("AEM_HTTP_PROXY_HOST"),
                            Integer.parseInt(System.getenv("AEM_HTTP_PROXY_PORT"))));
            // Create an HttpClient and provide the proxy selector that will use AEM's native HTTP proxy configuration
            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_HTTP_PROXY");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_HTTP_PROXY");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
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
