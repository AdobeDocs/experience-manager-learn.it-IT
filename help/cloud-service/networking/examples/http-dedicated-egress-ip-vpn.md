---
title: Connessioni HTTP/HTTPS per indirizzo IP in uscita dedicato e VPN
description: Scopri come effettuare richieste HTTP/HTTPS da AEM as a Cloud Service a servizi web esterni in esecuzione per indirizzo IP in uscita dedicato e VPN
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
duration: 70
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Connessioni HTTP/HTTPS per indirizzo IP in uscita dedicato e VPN

Le connessioni HTTP/HTTPS vengono automaticamente escluse da AEM as a Cloud Service con indirizzo IP in uscita dedicato o VPN e non richiedono regole `portForwards` speciali.

## Supporto di rete avanzato

Il codice di esempio seguente è supportato dalle seguenti opzioni di rete avanzate.

Prima di seguire questa esercitazione, assicurati che la configurazione di rete avanzata VPN](../advanced-networking.md#advanced-networking) o l&#39;indirizzo IP in uscita dedicato [ sia stata configurata.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Rete privata virtuale](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Questo esempio di codice è valido solo per [Indirizzo IP di uscita dedicato](../dedicated-egress-ip-address.md) e [VPN](../vpn.md). Un esempio di codice simile ma diverso è disponibile per [connessioni HTTP/HTTPS su porte non standard per l&#39;uscita della porta flessibile](./http-on-non-standard-ports-flexible-port-egress.md).

## Esempio di codice

Questo esempio di codice Java™ fa parte di un servizio OSGi che può essere eseguito in AEM as a Cloud Service e che effettua una connessione HTTP a un server web esterno su 8080. Le connessioni HTTPS (o HTTP) vengono automaticamente escluse da AEM as a Cloud Service e non richiedono uno sviluppo speciale.

>[!NOTE]
> Si consiglia di utilizzare le [API Java™ 11 HTTP](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) per effettuare chiamate HTTP/HTTPS dall&#39;AEM.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

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
