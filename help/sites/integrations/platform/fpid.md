---
title: Generare FPID di Adobe Experience Platform con AEM Sites
description: Scopri come generare o aggiornare i cookie FPID di Adobe Experience Platform utilizzando AEM Sites.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 316
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '982'
ht-degree: 0%

---

# Generare FPID di Experience Platform con AEM Sites

L’integrazione dei siti Adobe Experience Manager (AEM) con Adobe Experience Platform (AEP) richiede all’AEM di generare e mantenere un cookie FPID (First-Party Device ID) univoco per poter tenere traccia in modo univoco dell’attività dell’utente.

Leggere la documentazione di supporto per [scopri i dettagli della collaborazione tra gli ID dispositivo di prima parte e gli ID Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

Di seguito è riportata una panoramica del funzionamento degli FPID quando si utilizza l’AEM come host web.

![FPID ed ECID con AEM](./assets/aem-platform-fpid-architecture.png)

## Generare e mantenere l’FPID con AEM

Il servizio di pubblicazione AEM ottimizza le prestazioni memorizzando nella cache le richieste il maggior numero possibile di richieste, sia nella cache CDN che in quella del dispatcher AEM.

È fondamentale che le richieste HTTP che generano il cookie FPID univoco per utente e restituiscono il valore FPID non vengano mai memorizzate nella cache e servite direttamente da AEM Publish, che può implementare una logica per garantire l’univocità.

Evita di generare il cookie FPID nelle richieste di pagine web o di altre risorse memorizzabili in cache, in quanto la combinazione dei requisiti di univocità dell’FPID renderebbe queste risorse non memorizzabili in cache.

Il diagramma seguente descrive come il servizio di pubblicazione AEM gestisce gli FPID.

![Diagramma di flusso dell’FPID e dell’AEM](./assets/aem-fpid-flow.png)

1. Il browser web richiede una pagina web ospitata dall’AEM. La richiesta può essere gestita utilizzando una copia memorizzata nella cache della pagina web dalla CDN o dalla cache del Dispatcher AEM.
1. Se la pagina web non può essere servita da cache CDN o Dispatcher AEM, la richiesta raggiunge il servizio di pubblicazione AEM, che genera la pagina web richiesta.
1. La pagina web viene quindi restituita al browser web, popolando le cache che non potevano servire la richiesta. Con l’AEM, prevedi che le percentuali di hit della cache di Dispatcher CDN e AEM siano superiori al 90%.
1. La pagina web contiene JavaScript che effettua una richiesta XHR (AJAX) asincrona non memorizzabile in cache a un servlet FPID personalizzato nel servizio di pubblicazione AEM. Poiché si tratta di una richiesta non memorizzabile in cache (in virtù del suo parametro di query casuale e delle intestazioni Cache-Control), non viene mai memorizzata nella cache da CDN o Dispatcher AEM e raggiunge sempre il servizio di pubblicazione AEM per generare la risposta.
1. Il servlet FPID personalizzato nel servizio di pubblicazione AEM elabora la richiesta, generando un nuovo FPID quando non viene trovato alcun cookie FPID esistente, o estende la durata di qualsiasi cookie FPID esistente. Il servlet restituisce anche l’FPID nel corpo della risposta per l’utilizzo da parte di JavaScript lato client. Fortunatamente la logica del servlet FPID personalizzato è leggera e impedisce a questa richiesta di influire sulle prestazioni del servizio di pubblicazione AEM.
1. La risposta per la richiesta XHR restituisce al browser con il cookie FPID e l’FPID come JSON nel corpo della risposta per l’utilizzo da parte di Platform Web SDK.

## Esempio di codice

Il codice e la configurazione seguenti possono essere distribuiti al servizio di pubblicazione AEM per creare un endpoint che genera o estende la durata di un cookie FPID esistente e restituisce l’FPID come JSON.

### Servlet per cookie AEM FPID

È necessario creare un endpoint HTTP AEM per generare o estendere un cookie FPID, utilizzando un [Servlet Sling](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ Il servlet è associato a `/bin/aem/fpid` poiché per accedervi non è necessaria l’autenticazione. Se è richiesta l’autenticazione, effettua un binding a un tipo di risorsa Sling.
+ Il servlet accetta le richieste HTTP GET. La risposta è contrassegnata con `Cache-Control: no-store` per evitare il caching, ma questo endpoint deve essere richiesto utilizzando anche parametri di query univoci per il busting della cache.

Quando una richiesta HTTP raggiunge il servlet, quest’ultimo controlla se nella richiesta esiste un cookie FPID:

+ Se è presente un cookie FPID, estendi la durata del cookie e raccogli il relativo valore per scriverlo nella risposta.
+ Se non esiste un cookie FPID, genera un nuovo cookie FPID e salva il valore da scrivere nella risposta.

Il servlet scrive quindi l’FPID nella risposta come oggetto JSON nel formato: `{ fpid: "<FPID VALUE>" }`.

È importante fornire l’FPID al client nel corpo, poiché il cookie FPID è contrassegnato `HttpOnly`, il che significa che solo il server può leggerne il valore, mentre JavaScript lato client non può.

Il valore FPID dal corpo della risposta viene utilizzato per parametrizzare le chiamate utilizzando Platform Web SDK.

Di seguito è riportato un esempio di codice di un endpoint servlet AEM (disponibile tramite `HTTP GET /bin/aep/fpid`) che genera o aggiorna un cookie FPID e restituisce l’FPID come JSON.

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### Script HTML

È necessario aggiungere alla pagina un JavaScript lato client personalizzato per richiamare in modo asincrono il servlet, generando o aggiornando il cookie FPID e restituendo l’FPID nella risposta.

Questo script JavaScript viene in genere aggiunto alla pagina utilizzando uno dei seguenti metodi:

+ [Tag in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Libreria client AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

La chiamata XHR al servlet FPID dell’AEM personalizzato è veloce, anche se asincrona, quindi è possibile per un utente visitare una pagina web servita dall’AEM e spostarsi prima che la richiesta possa essere completata.
In questo caso, lo stesso processo tenterà di nuovo al successivo caricamento di una pagina web da AEM.

Il GET HTTP al servlet FPID dell’AEM (`/bin/aep/fpid`) è parametrizzato con un parametro di query casuale per garantire che qualsiasi infrastruttura tra il browser e il servizio di pubblicazione AEM non memorizzi in cache la risposta della richiesta.
Analogamente, il `Cache-Control: no-store` viene aggiunta un’intestazione di richiesta per supportare l’evitazione del caching.

In seguito a una chiamata del servlet dell’FPID dell’AEM, l’FPID viene recuperato dalla risposta JSON e utilizzato dal [Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) per inviarlo alle API Experienci Platform.

Per ulteriori informazioni su, consulta la documentazione di Experienci Platform. [utilizzo degli FPID in identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### Filtro di autorizzazione Dispatcher

Infine, le richieste HTTP di GET al servlet FPID personalizzato devono essere consentite tramite il dispatcher dell’AEM `filter.any` configurazione.

Se questa configurazione di Dispatcher non viene implementata correttamente, il GET HTTP richiede a `/bin/aep/fpid` si ottiene un 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Risorse di Experience Platform

Consulta la seguente documentazione di Experience Platform per gli ID dispositivo di prime parti (FPID) e per la gestione dei dati di identità con Platform Web SDK.

+ [Generare ID dispositivo di prime parti](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [ID dispositivo di prime parti nell’SDK per web di Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Dati di identità in Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
