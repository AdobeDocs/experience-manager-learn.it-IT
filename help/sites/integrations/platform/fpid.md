---
title: Generare FPID Adobe Experience Platform con AEM
description: Scopri come generare o aggiornare i cookie FPID di Adobe Experience Platform utilizzando AEM.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
source-git-commit: d1e105a4083b34e7a3f220a59d4608ef39d39032
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# Generare FPID di Experience Platform con AEM

L’integrazione di Adobe Experience Manager (AEM) con Adobe Experience Platform (AEP) richiede AEM per generare e mantenere un cookie FPID (Device ID) univoco per il tracciamento dell’attività dell’utente.

Leggi la documentazione di supporto a [scopri in che modo gli ID dispositivo e gli ID Experience Cloud di prima parte funzionano insieme](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

Di seguito è riportata una panoramica del funzionamento degli FPID quando si utilizza AEM come host web.

![FPID ed ECID con AEM](./assets/aem-platform-fpid-architecture.png)

## Genera e persiste il FPID con AEM

Il servizio AEM Publish ottimizza le prestazioni memorizzando nella cache il maggior numero possibile di richieste, sia nella cache CDN che AEM Dispatcher.

È fondamentale che le richieste HTTP generino il cookie FPID univoco per utente e restituiscano il valore FPID non vengano mai memorizzate nella cache e vengano servite direttamente da AEM Publish, in grado di implementare la logica per garantire l’univocità.

Evita di generare il cookie FPID sulle richieste per pagine web o altre risorse memorizzabili in cache, in quanto la combinazione del requisito di univocità di FPID renderebbe queste risorse inmemorizzabili nella cache.

Il diagramma seguente descrive come il servizio AEM Publish gestisce gli FPID.

![Diagramma di flusso FPID e AEM](./assets/aem-fpid-flow.png)

1. Il browser Web richiede una pagina web ospitata da AEM. La richiesta può essere servita utilizzando una copia in cache della pagina web da CDN o AEM cache del Dispatcher.
1. Se la pagina web non può essere servita da CDN o AEM cache del Dispatcher, la richiesta raggiunge il servizio AEM Publish, che genera la pagina web richiesta.
1. La pagina web viene quindi restituita al browser Web, popolando le cache che non potevano soddisfare la richiesta. Con AEM, si prevede che le percentuali di hit della cache di CDN e AEM Dispatcher siano superiori al 90%.
1. La pagina web contiene JavaScript che effettua una richiesta XHR (AJAX) asincrona non memorizzabile nella cache a un servlet FPID personalizzato in AEM Publish Service. Poiché si tratta di una richiesta non memorizzabile nella cache (in virtù del parametro di query casuale e delle intestazioni Cache-Control), non viene mai memorizzata nella cache da CDN o AEM Dispatcher, e raggiunge sempre il servizio AEM Publish per generare la risposta.
1. Il servlet FPID personalizzato nel servizio AEM Publish elabora la richiesta, genera un nuovo FPID quando non viene trovato alcun cookie FPID esistente o estende il live di qualsiasi cookie FPID esistente. Il servlet restituisce anche il FPID nel corpo della risposta da utilizzare per JavaScript lato client. Fortunatamente la logica del servlet FPID personalizzata è leggera, impedendo a questa richiesta di influire sulle prestazioni del servizio AEM Publish.
1. La risposta per la richiesta XHR restituisce al browser con il cookie FPID e il FPID come JSON nel corpo della risposta per l’utilizzo da parte di Platform Web SDK.

## Esempio di codice

Il codice e la configurazione seguenti possono essere distribuiti al servizio AEM Publish per creare un endpoint che genera, o estende la durata di un cookie FPID esistente e restituisce il FPID come JSON.

### AEM servlet cookie FPID

È necessario creare un endpoint HTTP AEM per generare o estendere un cookie FPID, utilizzando un [Servlet Sling](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ Il servlet è associato a `/bin/aem/fpid` poiché l’autenticazione non è necessaria per accedervi. Se è richiesta l’autenticazione, eseguire il binding a un tipo di risorsa Sling.
+ Il servlet accetta richieste HTTP GET. La risposta è contrassegnata con `Cache-Control: no-store` per evitare il caching, ma questo endpoint deve essere richiesto utilizzando anche parametri di query univoci di svuotamento della cache.

Quando una richiesta HTTP raggiunge il servlet, il servlet controlla se nella richiesta esiste un cookie FPID:

+ Se esiste un cookie FPID, estende la durata del cookie e raccogli il suo valore da scrivere nella risposta.
+ Se non esiste un cookie FPID, genera un nuovo cookie FPID e salva il valore da scrivere nella risposta.

Il servlet scrive quindi il FPID nella risposta come oggetto JSON nel modulo: `{ fpid: "<FPID VALUE>" }`.
È importante fornire il FPID al client nel corpo in quanto il cookie FPID è contrassegnato `HttpOnly`, che significa che solo il server può leggere il suo valore e che JavaScript lato client non può farlo.
Il valore FPID dal corpo della risposta viene utilizzato per parametrizzare le chiamate tramite l’SDK per web di Platform.

Di seguito è riportato un codice di esempio di un endpoint del servlet AEM (disponibile tramite `HTTP GET /bin/aep/fpid`) che genera o aggiorna un cookie FPID e restituisce il FPID come JSON.

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

### script HTML

Per richiamare il servlet in modo asincrono, è necessario aggiungere alla pagina un JavaScript personalizzato lato client, generando o aggiornando il cookie FPID e restituendo il FPID nella risposta.

Questo script JavaScript è tipicamente aggiunto alla pagina utilizzando uno dei seguenti metodi:

+ [Tag in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Libreria client AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

La chiamata XHR al servlet FPID AEM personalizzato è rapida, anche se asincrona, quindi è possibile per un utente visitare una pagina web servita da AEM e navigare via prima che la richiesta possa essere completata.
In questo caso, lo stesso processo tenterà di nuovo di caricare la pagina successiva di una pagina web da AEM.

HTTP GET al servlet FPID AEM (`/bin/aep/fpid`) è parametrizzato con un parametro di query casuale per garantire che qualsiasi infrastruttura tra il browser e il servizio AEM Publish non memorizzi in cache la risposta della richiesta.
Analogamente, la `Cache-Control: no-store` l’intestazione della richiesta viene aggiunta per supportare l’eliminazione della memorizzazione in cache.

Dopo una chiamata del servlet FPID AEM, il FPID viene recuperato dalla risposta JSON e utilizzato dal [SDK per web per Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) per inviarlo alle API di Experience Platform.

Consulta la documentazione di Experience Platform per ulteriori informazioni su [utilizzo di FPID in identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

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

### Filtro Consentiti del Dispatcher

Infine, le richieste HTTP GET al servlet FPID personalizzato devono essere consentite tramite AEM Dispatcher `filter.any` configurazione.

Se questa configurazione del Dispatcher non è implementata correttamente, HTTP GET le richieste a `/bin/aep/fpid` risulta in un 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Risorse di Experience Platform

Consulta la seguente documentazione di Experience Platform per gli ID dispositivo di prime parti (FPID) e per gestire i dati di identità con Platform Web SDK.

+ [Generare ID dispositivo di prime parti](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [ID dispositivo di prime parti nell’SDK per web di Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Dati di identità nell’SDK per web di Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)


