---
title: Connessioni HTTP/HTTPS per indirizzo IP in uscita dedicato e VPN
description: Scopri come rendere as a Cloud Service le richieste HTTP/HTTPS da AEM a servizi web esterni in esecuzione per indirizzo IP in uscita dedicato e VPN
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 0%

---

# Connessioni HTTP/HTTPS per indirizzo IP in uscita dedicato e VPN

Le connessioni HTTP/HTTPS vengono automaticamente escluse dall’AEM as a Cloud Service con indirizzo IP in uscita dedicato o VPN e non richiedono alcun tipo di connessione speciale `portForwards` regole.

## Supporto di rete avanzato

Il codice di esempio seguente è supportato dalle seguenti opzioni di rete avanzate.

Assicurati che [indirizzo IP o VPN in uscita dedicato](../advanced-networking.md#advanced-networking) la configurazione di rete avanzata è stata impostata prima di seguire questa esercitazione.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Virtual Private Network](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Questo esempio di codice è solo per [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) e [VPN](../vpn.md). Esempio di codice simile ma diverso disponibile per [Connessioni HTTP/HTTPS su porte non standard per l’uscita della porta flessibile](./http-on-non-standard-ports-flexible-port-egress.md).

## Esempio di codice

Questo esempio di codice Java™ è di un servizio OSGi che può essere eseguito in AEM as a Cloud Service e che effettua una connessione HTTP a un server web esterno su 8080. Le connessioni HTTPS (o HTTP) vengono automaticamente escluse dall’AEM as a Cloud Service e non richiedono uno sviluppo speciale.

>[!NOTE]
> Si consiglia di [API HTTP Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) vengono utilizzati per effettuare chiamate HTTP/HTTPS dall’AEM.

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
