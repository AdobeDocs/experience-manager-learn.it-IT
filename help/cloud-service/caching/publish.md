---
title: Memorizzazione in cache del servizio Publish AEM
description: Panoramica generale del caching del servizio di pubblicazione di AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: 1a1accbe-7706-4f9b-bf63-755090d03c4c
duration: 240
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 2%

---

# Pubblicazione AEM

Il servizio di pubblicazione di AEM dispone di due livelli di caching principali, AEM as a Cloud Service CDN e AEM Dispatcher. Facoltativamente, è possibile inserire una rete CDN gestita dal cliente davanti alla rete CDN di AEM as a Cloud Service. La rete CDN di AEM as a Cloud Service fornisce una distribuzione ai margini dei contenuti, garantendo che le esperienze vengano distribuite agli utenti di tutto il mondo con una latenza ridotta. AEM Dispatcher fornisce la memorizzazione in cache direttamente prima di AEM Publish e viene utilizzato per mitigare il carico non necessario sulla stessa AEM Publish.

![Diagramma introduttivo sulla memorizzazione nella cache di AEM Publish](./assets/publish/publish-all.png){align="center"}

## CDN

La memorizzazione in cache della rete CDN di AEM as a Cloud Service è controllata dalle intestazioni cache di risposta HTTP e ha lo scopo di memorizzare in cache il contenuto per ottimizzare il bilanciamento tra aggiornamento e prestazioni. La rete CDN si trova tra l’utente finale e AEM Dispatcher e viene utilizzata per memorizzare il contenuto nella cache il più vicino possibile all’utente finale, garantendo un’esperienza performante.

![CDN pubblicazione AEM](./assets/publish/publish-cdn.png){align="center"}

La configurazione della modalità in cui la rete CDN memorizza in cache il contenuto è limitata all’impostazione delle intestazioni della cache sulle risposte HTTP. Queste intestazioni di cache sono in genere impostate nelle configurazioni vhost di AEM Dispatcher utilizzando `mod_headers`, ma possono anche essere impostate nel codice Java™ personalizzato in esecuzione nella stessa pubblicazione di AEM.

### Quando vengono memorizzate nella cache le richieste/risposte HTTP?

AEM as a Cloud Service CDN memorizza nella cache solo le risposte HTTP e deve soddisfare tutti i seguenti criteri:

+ Lo stato della risposta HTTP è `2xx` o `3xx`
+ Il metodo di richiesta HTTP è `GET` o `HEAD`
+ È presente almeno una delle seguenti intestazioni di risposta HTTP: `Cache-Control`, `Surrogate-Control` o `Expires`
+ La risposta HTTP può essere di qualsiasi tipo di contenuto, inclusi file HTML, JSON, CSS, JS e binari.

Per impostazione predefinita, le risposte HTTP non memorizzate nella cache da [AEM Dispatcher](#aem-dispatcher) dispongono automaticamente di intestazioni cache di risposta HTTP rimosse per evitare la memorizzazione nella cache in CDN. Questo comportamento può essere accuratamente ignorato utilizzando `mod_headers` con la direttiva `Header always set ...` quando necessario.

### Cosa viene memorizzato nella cache?

AEM as a Cloud Service CDN memorizza in cache quanto segue:

+ Corpo della risposta HTTP
+ Intestazioni di risposta HTTP

In genere, una richiesta/risposta HTTP per un singolo URL viene memorizzata nella cache come oggetto singolo. Tuttavia, la rete CDN può gestire la memorizzazione nella cache di più oggetti per un singolo URL quando l&#39;intestazione `Vary` è impostata sulla risposta HTTP. Evita di specificare `Vary` nelle intestazioni i cui valori non hanno un set di valori strettamente controllato, in quanto ciò può causare molti errori nella cache, riducendo il rapporto di hit della cache. Per supportare il caching di diverse richieste in AEM Dispatcher, [consulta la documentazione sul caching delle varianti](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html).

### Durata cache{#cdn-cache-life}

La rete CDN di pubblicazione di AEM è basata su TTL (time-to-live), il che significa che la durata della cache è determinata dalle intestazioni di risposta HTTP `Cache-Control`, `Surrogate-Control` o `Expires`. Se le intestazioni di cache della risposta HTTP non sono impostate dal progetto e sono soddisfatti i [criteri di idoneità](#when-are-http-requestsresponses-cached), Adobe imposta una durata predefinita della cache di 10 minuti (600 secondi).

Di seguito è illustrato come le intestazioni di cache influiscono sulla durata della cache CDN:

+ L&#39;intestazione di risposta HTTP [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) indica al browser Web e alla rete CDN per quanto tempo memorizzare la risposta nella cache. Il valore è in secondi. Ad esempio, `Cache-Control: max-age=3600` indica al browser Web di memorizzare la risposta nella cache per un&#39;ora. Questo valore viene ignorato dalla rete CDN se è presente anche l&#39;intestazione di risposta HTTP `Surrogate-Control`.
+ L&#39;intestazione di risposta HTTP [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) indica alla rete CDN di AEM per quanto tempo memorizzare la risposta nella cache. Il valore è in secondi. Ad esempio, `Surrogate-Control: max-age=3600` comunica al CDN di memorizzare la risposta nella cache per un&#39;ora.
+ L&#39;intestazione di risposta HTTP [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) indica alla rete CDN di AEM (e al browser Web) per quanto tempo è valida la risposta memorizzata nella cache. Il valore è una data. Ad esempio, `Expires: Sat, 16 Sept 2023 09:00:00 EST` indica al browser Web di memorizzare la risposta nella cache fino alla data e all&#39;ora specificate.

Utilizza `Cache-Control` per controllare la durata della cache quando è la stessa sia per il browser che per la rete CDN. Utilizzare `Surrogate-Control` quando il browser Web deve memorizzare la risposta nella cache per una durata diversa da quella della rete CDN.

#### Durata predefinita della cache

Se una risposta HTTP è idonea per il caching di AEM Dispatcher [per qualificatori superiori](#when-are-http-requestsresponses-cached), i valori predefiniti sono i seguenti, a meno che non sia presente una configurazione personalizzata.

| Tipo di contenuto | Durata predefinita cache CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 5 minuti |
| [Assets (immagini, video, documenti e così via)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 10 minuti |
| [Query persistenti (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 2 ore |
| [Librerie client (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 giorni |
| [Altro](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Non memorizzato in cache |

### Personalizzare le regole della cache

[La configurazione della modalità con cui la rete CDN memorizza in cache il contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#disp) è limitata all&#39;impostazione delle intestazioni della cache nelle risposte HTTP. Queste intestazioni della cache sono in genere impostate nelle configurazioni di AEM Dispatcher `vhost` utilizzando `mod_headers`, ma possono anche essere impostate nel codice Java™ personalizzato in esecuzione nella stessa pubblicazione AEM.

## Dispatcher AEM

![Pubblicazione AEM AEM Dispatcher](./assets/publish/publish-dispatcher.png){align="center"}

### Quando vengono memorizzate nella cache le richieste/risposte HTTP?

Le risposte HTTP per le richieste HTTP corrispondenti vengono memorizzate nella cache quando sono soddisfatti tutti i seguenti criteri:

+ Il metodo di richiesta HTTP è `GET` o `HEAD`
   + `HEAD` richieste HTTP memorizzano nella cache solo le intestazioni di risposta HTTP. Non hanno organi di risposta.
+ Lo stato della risposta HTTP è `200`
+ La risposta HTTP NON è per un file binario.
+ Il percorso URL della richiesta HTTP termina con un&#39;estensione, ad esempio: `.html`, `.json`, `.css`, `.js`, ecc.
+ La richiesta HTTP non contiene autorizzazioni e non è autenticata da AEM.
   + Tuttavia, la memorizzazione nella cache delle richieste autenticate [ può essere abilitata a livello globale](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-when-authentication-is-used) o in modo selettivo tramite [memorizzazione nella cache sensibile alle autorizzazioni](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=it).
+ La richiesta HTTP non contiene parametri di query.
   + Tuttavia, la configurazione di [Parametri di query ignorati](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#ignoring-url-parameters) consente di memorizzare nella cache/gestire le richieste HTTP con i parametri di query ignorati.
+ Il percorso della richiesta HTTP [corrisponde a una regola Dispatcher consentita e non corrisponde a una regola nega](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-documents-to-cache).
+ La risposta HTTP non ha una delle seguenti intestazioni di risposta HTTP impostate da AEM Publish:

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### Cosa viene memorizzato nella cache?

AEM Dispatcher memorizza nella cache quanto segue:

+ Corpo della risposta HTTP
+ Intestazioni di risposta HTTP specificate nella configurazione delle [intestazioni cache di Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-http-response-headers). Vedere la configurazione predefinita fornita con [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Durata cache

AEM Dispatcher memorizza nella cache le risposte HTTP utilizzando i seguenti approcci:

+ Fino a quando l’annullamento della validità non viene attivato tramite meccanismi quali la pubblicazione o l’annullamento della pubblicazione del contenuto.
+ TTL (time-to-live) quando [è configurato in modo esplicito nella configurazione di Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-time-based-cache-invalidation-enablettl). Vedere la configurazione predefinita in [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) esaminando la configurazione `enableTTL`.

#### Durata predefinita della cache

Se una risposta HTTP è idonea per il caching di AEM Dispatcher [per qualificatori superiori](#when-are-http-requestsresponses-cached-1), i valori predefiniti sono i seguenti, a meno che non sia presente una configurazione personalizzata.

| Tipo di contenuto | Durata predefinita cache CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | Fino all’annullamento della validità |
| [Assets (immagini, video, documenti e così via)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | Mai |
| [Query persistenti (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 1 minuto |
| [Librerie client (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 giorni |
| [Altro](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Fino all’annullamento della validità |

### Personalizzare le regole della cache

La cache di AEM Dispatcher può essere configurata tramite la [configurazione Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache), inclusi:

+ Cosa viene memorizzato nella cache
+ Quali parti della cache vengono invalidate al momento della pubblicazione o dell’annullamento della pubblicazione
+ Quali parametri di query della richiesta HTTP vengono ignorati durante la valutazione della cache
+ Quali intestazioni di risposta HTTP vengono memorizzate nella cache
+ Attivazione o disattivazione del caching TTL
+ ... e molto altro

Se si utilizza `mod_headers` per impostare le intestazioni della cache, la configurazione di `vhost` non influirà sul caching di Dispatcher (basato su TTL), in quanto queste vengono aggiunte alla risposta HTTP dopo che AEM Dispatcher elabora la risposta. Per influenzare il caching di Dispatcher tramite le intestazioni di risposta HTTP, è necessario un codice Java™ personalizzato in esecuzione in AEM Publish che imposti le intestazioni di risposta HTTP appropriate.
