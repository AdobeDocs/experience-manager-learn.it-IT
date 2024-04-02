---
title: Memorizzazione in cache del servizio di pubblicazione AEM
description: Panoramica generale del caching del servizio Publish as a Cloud Service dall’AEM.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: 1a1accbe-7706-4f9b-bf63-755090d03c4c
duration: 325
source-git-commit: baf81bb43a659e49728a05f83e7be394f7fbfb35
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 2%

---

# Pubblicazione AEM

Il servizio di pubblicazione AEM dispone di due livelli di caching primari, la rete CDN as a Cloud Service dell’AEM e il dispatcher dell’AEM. Facoltativamente, una rete CDN gestita dal cliente può essere posizionata davanti alla rete CDN as a Cloud Service dall’AEM. La rete CDN as a Cloud Service dell’AEM fornisce una distribuzione ai margini dei contenuti, garantendo esperienze distribuite a bassa latenza agli utenti di tutto il mondo. Il Dispatcher per l’AEM fornisce la memorizzazione in cache direttamente prima della pubblicazione dell’AEM e viene utilizzato per attenuare un carico inutile sulla pubblicazione dell’AEM stessa.

![Diagramma introduttivo del caching delle pubblicazioni AEM](./assets/publish/publish-all.png){align="center"}

## CDN

La memorizzazione nella cache della rete CDN di AEM as a Cloud Service è controllata dalle intestazioni cache di risposta HTTP e ha lo scopo di memorizzare il contenuto nella cache per ottimizzare il bilanciamento tra aggiornamento e prestazioni. La CDN si trova tra l’utente finale e il Dispatcher dell’AEM e viene utilizzata per memorizzare il contenuto nella cache il più vicino possibile all’utente finale, garantendo un’esperienza performante.

![CDN pubblicazione AEM](./assets/publish/publish-cdn.png){align="center"}

La configurazione della modalità in cui la rete CDN memorizza in cache il contenuto è limitata all’impostazione delle intestazioni della cache sulle risposte HTTP. Queste intestazioni di cache sono in genere impostate nelle configurazioni host del Dispatcher AEM utilizzando `mod_headers`, ma può anche essere impostato nel codice Java™ personalizzato eseguito nella pubblicazione AEM stessa.

### Quando vengono memorizzate nella cache le richieste/risposte HTTP?

La rete CDN as a Cloud Service dall’AEM memorizza nella cache solo le risposte HTTP e devono essere soddisfatti tutti i seguenti criteri:

+ Lo stato della risposta HTTP è `2xx` o `3xx`
+ Il metodo di richiesta HTTP è `GET` o `HEAD`
+ È presente almeno una delle seguenti intestazioni di risposta HTTP: `Cache-Control`, `Surrogate-Control`, o  `Expires`
+ La risposta HTTP può essere di qualsiasi tipo di contenuto, inclusi file HTML, JSON, CSS, JS e file binari.

Per impostazione predefinita, le risposte HTTP non vengono memorizzate nella cache da [Dispatcher AEM](#aem-dispatcher) rimuovi automaticamente eventuali intestazioni cache di risposta HTTP per evitare la memorizzazione nella cache in CDN. Questo comportamento può essere attentamente ignorato utilizzando `mod_headers` con `Header always set ...` direttiva, se necessario.

### Cosa viene memorizzato nella cache?

La rete CDN as a Cloud Service dall’AEM memorizza nella cache quanto segue:

+ Corpo della risposta HTTP
+ Intestazioni di risposta HTTP

In genere, una richiesta/risposta HTTP per un singolo URL viene memorizzata nella cache come oggetto singolo. Tuttavia, la rete CDN può gestire la memorizzazione nella cache di più oggetti per un singolo URL, quando `Vary` l’intestazione è impostata sulla risposta HTTP. Evita di specificare `Vary` sulle intestazioni i cui valori non hanno un set di valori strettamente controllato, poiché questo può causare molti mancati riscontri nella cache, riducendo il rapporto di hit della cache. Per supportare la memorizzazione nella cache di richieste diverse in Dispatcher AEM, [consulta la documentazione sul caching delle varianti](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html).

### Durata cache{#cdn-cache-life}

La rete CDN di pubblicazione dell’AEM è basata su TTL (time-to-live), il che significa che la durata della cache è determinata dalla proprietà `Cache-Control`, `Surrogate-Control`, o `Expires` Intestazioni di risposta HTTP. Se le intestazioni di cache di risposta HTTP non sono impostate dal progetto e il [criteri di idoneità](#when-are-http-requestsresponses-cached) sono soddisfatte, Adobe imposta una durata predefinita della cache di 10 minuti (600 secondi).

Di seguito è illustrato come le intestazioni di cache influiscono sulla durata della cache CDN:

+ [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) L’intestazione di risposta HTTP indica al browser web e alla rete CDN per quanto tempo memorizzare la risposta nella cache. Il valore è in secondi. Ad esempio: `Cache-Control: max-age=3600` indica al browser web di memorizzare la risposta nella cache per un’ora. Questo valore viene ignorato dal CDN se `Surrogate-Control` È presente anche l’intestazione di risposta HTTP.
+ [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) L’intestazione di risposta HTTP indica alla rete CDN dell’AEM per quanto tempo memorizzare la risposta nella cache. Il valore è in secondi. Ad esempio: `Surrogate-Control: max-age=3600` comunica alla CDN di memorizzare in cache la risposta per un’ora.
+ [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) L’intestazione di risposta HTTP indica alla rete CDN dell’AEM (e al browser web) per quanto tempo la risposta memorizzata nella cache è valida. Il valore è una data. Ad esempio: `Expires: Sat, 16 Sept 2023 09:00:00 EST` indica al browser web di memorizzare la risposta nella cache fino alla data e all’ora specificate.

Utilizzare `Cache-Control` per controllare la durata della cache quando è la stessa sia per il browser che per la CDN. Utilizzare `Surrogate-Control` quando il browser web deve memorizzare la risposta nella cache per una durata diversa dalla rete CDN.

#### Durata predefinita della cache

Se una risposta HTTP è idonea per il caching del Dispatcher AEM [per qualificatori sopra](#when-are-http-requestsresponses-cached), a meno che non sia presente una configurazione personalizzata, i valori predefiniti sono i seguenti.

| Tipo di contenuto | Durata predefinita cache CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 5 minuti |
| [Risorse (immagini, video, documenti e così via)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 10 minuti |
| [Query persistenti (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 2 ore |
| [Librerie client (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 giorni |
| [Altro](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Non memorizzato in cache |

### Personalizzare le regole della cache

[Configurazione della modalità in cui la rete CDN memorizza in cache il contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#disp) è limitato all’impostazione delle intestazioni della cache sulle risposte HTTP. Queste intestazioni della cache sono in genere impostate in Dispatcher per AEM `vhost` configurazioni tramite `mod_headers`, ma può anche essere impostato nel codice Java™ personalizzato eseguito nella pubblicazione AEM stessa.

## Dispatcher AEM

![Dispatcher AEM Publish AEM](./assets/publish/publish-dispatcher.png){align="center"}

### Quando vengono memorizzate nella cache le richieste/risposte HTTP?

Le risposte HTTP per le richieste HTTP corrispondenti vengono memorizzate nella cache quando sono soddisfatti tutti i seguenti criteri:

+ Il metodo di richiesta HTTP è `GET` o `HEAD`
   + `HEAD` Le richieste HTTP memorizzano nella cache solo le intestazioni di risposta HTTP. Non hanno organi di risposta.
+ Lo stato della risposta HTTP è `200`
+ La risposta HTTP NON è per un file binario.
+ Il percorso URL della richiesta HTTP termina con un’estensione, ad esempio: `.html`, `.json`, `.css`, `.js`, ecc.
+ La richiesta HTTP non contiene autorizzazioni e non è autenticata dall’AEM.
   + Tuttavia, la memorizzazione nella cache delle richieste autenticate [può essere abilitato a livello globale](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-when-authentication-is-used) o selettivamente tramite [caching sensibile alle autorizzazioni](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=it).
+ La richiesta HTTP non contiene parametri di query.
   + Tuttavia, la configurazione [Parametri di query ignorati](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#ignoring-url-parameters) consente di memorizzare in cache/gestire dalla cache le richieste HTTP con i parametri di query ignorati.
+ Percorso della richiesta HTTP [corrisponde a una regola di Dispatcher consentita e non corrisponde a una regola di negazione](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-documents-to-cache).
+ La risposta HTTP non ha una delle seguenti intestazioni di risposta HTTP impostate da Pubblicazione AEM:

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### Cosa viene memorizzato nella cache?

AEM Dispatcher memorizza nella cache quanto segue:

+ Corpo della risposta HTTP
+ Intestazioni di risposta HTTP specificate nel file di [configurazione delle intestazioni cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-http-response-headers). Visualizza la configurazione predefinita fornita con [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Durata cache

Il Dispatcher AEM memorizza nella cache le risposte HTTP utilizzando i seguenti approcci:

+ Fino a quando l’annullamento della validità non viene attivato tramite meccanismi quali la pubblicazione o l’annullamento della pubblicazione del contenuto.
+ TTL (time-to-live), se esplicito [configurato nella configurazione di Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-time-based-cache-invalidation-enablettl). Vedi la configurazione predefinita in [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) rivedendo il `enableTTL` configurazione.

#### Durata predefinita della cache

Se una risposta HTTP è idonea per il caching del Dispatcher AEM [per qualificatori sopra](#when-are-http-requestsresponses-cached-1), a meno che non sia presente una configurazione personalizzata, i valori predefiniti sono i seguenti.

| Tipo di contenuto | Durata predefinita cache CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | Fino all’annullamento della validità |
| [Risorse (immagini, video, documenti e così via)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | Mai |
| [Query persistenti (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 1 minuto |
| [Librerie client (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 giorni |
| [Altro](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Fino all’annullamento della validità |

### Personalizzare le regole della cache

La cache del Dispatcher AEM può essere configurata tramite [Configurazione del Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache) tra cui:

+ Cosa viene memorizzato nella cache
+ Quali parti della cache vengono invalidate al momento della pubblicazione o dell’annullamento della pubblicazione
+ Quali parametri di query della richiesta HTTP vengono ignorati durante la valutazione della cache
+ Quali intestazioni di risposta HTTP vengono memorizzate nella cache
+ Attivazione o disattivazione del caching TTL
+ ... e molto altro

Utilizzo di `mod_headers` per impostare le intestazioni della cache `vhost` La configurazione di non influirà sul caching di Dispatcher (basato su TTL), in quanto questi vengono aggiunti alla risposta HTTP dopo che il Dispatcher AEM ha elaborato la risposta. Per influenzare il caching di Dispatcher tramite intestazioni di risposta HTTP, è necessario un codice Java™ personalizzato in esecuzione in AEM Publish che imposti le intestazioni di risposta HTTP appropriate.
