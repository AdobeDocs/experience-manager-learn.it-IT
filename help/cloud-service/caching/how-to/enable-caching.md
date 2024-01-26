---
title: Abilitare il caching CDN
description: Scopri come abilitare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 200
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Abilitare il caching CDN

Scopri come abilitare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service. La memorizzazione nella cache delle risposte è controllata da `Cache-Control`, `Surrogate-Control`, o `Expires` Intestazioni cache di risposta HTTP.

Queste intestazioni di cache sono in genere impostate nelle configurazioni host del Dispatcher AEM utilizzando `mod_headers`, ma può anche essere impostato nel codice Java™ personalizzato eseguito nella pubblicazione AEM stessa.

## Comportamento di caching predefinito

Quando NON sono presenti configurazioni personalizzate, vengono utilizzati i valori predefiniti. Nella schermata seguente puoi visualizzare il comportamento di caching predefinito per Pubblicazione e authoring AEM quando un [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) basato su `mynewsite` Progetto AEM implementato.

![Comportamento di caching predefinito](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Rivedi [Pubblicazione AEM - Durata predefinita della cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) e [Autore AEM - Durata predefinita cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) per ulteriori informazioni.

In sintesi, AEM as a Cloud Service memorizza nella cache la maggior parte dei tipi di contenuto (HTML, JSON, JS, CSS e Assets) in AEM Publish e alcuni tipi di contenuto (JS, CSS) in AEM Author.

## Abilita caching

Per modificare il comportamento di caching predefinito, puoi aggiornare le intestazioni della cache in due modi.

1. **Configurazione vhost di Dispatcher:** Disponibile solo per pubblicazione AEM.
1. **Codice Java™ personalizzato:** Disponibile sia per pubblicazione AEM che per creazione.

Esaminiamo ognuna di queste opzioni.

### Configurazione vhost di Dispatcher

Questa opzione è l’approccio consigliato per abilitare il caching, tuttavia è disponibile solo per la pubblicazione AEM. Per aggiornare le intestazioni della cache, utilizza `mod_headers` modulo e `<LocationMatch>` nel file vhost del server HTTP Apache. La sintassi generale è la seguente:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

Di seguito viene riepilogato lo scopo di ogni **intestazione** e applicabile **attributi** per l’intestazione.

|                     | Browser web | CDN | Descrizione |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | Questa intestazione controlla il browser web e la durata della cache CDN. |
| Surrogate-Control | ✘ | ✔ | Questa intestazione controlla la durata della cache CDN. |
| Scade | ✔ | ✔ | Questa intestazione controlla il browser web e la durata della cache CDN. |


- **età massima**: questo attributo controlla il TTL o &quot;time to live&quot; del contenuto della risposta in secondi.
- **stale-while-revalidate**: questo attributo controlla _stato non aggiornato_ il trattamento del contenuto della risposta a livello di CDN quando viene ricevuta la richiesta rientra nel periodo specificato in secondi. Il _stato non aggiornato_ è il periodo di tempo dopo la scadenza del TTL e prima della riconvalida della risposta.
- **stale-if-error**: questo attributo controlla _stato non aggiornato_ trattamento del contenuto di risposta a livello CDN quando il server di origine non è disponibile e la richiesta ricevuta rientra nel periodo specificato in secondi.

Rivedi [stabilità e riconvalida](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) dettagli per ulteriori informazioni.

#### Esempio

Per aumentare la durata del browser web e della cache CDN del **Tipo di contenuto HTML** a _10 minuti_ senza trattamento di stato stantio, effettuare le seguenti operazioni:

1. Nel progetto AEM, individua il file vhsot desiderato da `dispatcher/src/conf.d/available_vhosts` directory.
1. Aggiorna il vhost (ad es. `wknd.vhost`) file come segue:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   I file vhost in `dispatcher/src/conf.d/enabled_vhosts` directory sono **symlink** ai file in `dispatcher/src/conf.d/available_vhosts` , quindi assicurati di creare symlink se non presente.
1. Implementare le modifiche vhost nell’ambiente AEM as a Cloud Service desiderato utilizzando [Cloud Manager - Pipeline di configurazione a livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Tuttavia, per avere valori diversi per il browser web e la durata della cache CDN, puoi utilizzare `Surrogate-Control` nell’esempio precedente. Allo stesso modo, per far scadere la cache a una data e un’ora specifiche, puoi utilizzare `Expires` intestazione. Inoltre, utilizzando `stale-while-revalidate` e `stale-if-error` , è possibile controllare il trattamento dello stato non aggiornato del contenuto della risposta. Il progetto WKND dell’AEM ha [trattamento dello stato stale di riferimento](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) Configurazione della cache CDN.

Allo stesso modo, puoi aggiornare le intestazioni della cache anche per altri tipi di contenuto (JSON, JS, CSS e Assets).

### Codice Java™ personalizzato

Questa opzione è disponibile sia per la pubblicazione AEM che per l’authoring. Tuttavia, si sconsiglia di abilitare il caching in AEM Author e di mantenere il comportamento di caching predefinito.

Per aggiornare le intestazioni della cache, utilizza `HttpServletResponse` oggetto nel codice Java™ personalizzato (servlet Sling, filtro servlet Sling). La sintassi generale è la seguente:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
