---
title: Abilitare il caching CDN
description: Scopri come abilitare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Abilitare il caching CDN

Scopri come abilitare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service. La memorizzazione nella cache delle risposte è controllata da `Cache-Control`, `Surrogate-Control` o `Expires` intestazioni cache di risposta HTTP.

Queste intestazioni di cache sono in genere impostate nelle configurazioni vhost di AEM Dispatcher utilizzando `mod_headers`, ma possono anche essere impostate nel codice Java™ personalizzato in esecuzione nella stessa pubblicazione di AEM.

## Comportamento di caching predefinito

Quando NON sono presenti configurazioni personalizzate, vengono utilizzati i valori predefiniti. Nella schermata seguente è possibile visualizzare il comportamento di caching predefinito per AEM Publish e Author quando viene distribuito un [progetto AEM basato su Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype) `mynewsite`.

![Comportamento predefinito per la memorizzazione nella cache](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Rivedi [Pubblicazione AEM - Durata predefinita cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) e [Autore AEM - Durata predefinita cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) per ulteriori informazioni.

In sintesi, AEM as a Cloud Service memorizza nella cache la maggior parte dei tipi di contenuto (HTML, JSON, JS, CSS e Assets) in AEM Publish e alcuni tipi di contenuto (JS, CSS) in AEM Author.

## Abilita caching

Per modificare il comportamento di caching predefinito, puoi aggiornare le intestazioni della cache in due modi.

1. **Configurazione vhost Dispatcher:** disponibile solo per AEM Publish.
1. **Codice Java™ personalizzato:** Disponibile sia per AEM Publish che per Author.

Esaminiamo ognuna di queste opzioni.

### Configurazione vhost Dispatcher

Questa opzione è l’approccio consigliato per l’abilitazione del caching, ma è disponibile solo per AEM Publish. Per aggiornare le intestazioni della cache, utilizzare il modulo `mod_headers` e la direttiva `<LocationMatch>` nel file vhost del server HTTP Apache. La sintassi generale è la seguente:

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

Di seguito viene riepilogato lo scopo di ogni **intestazione** e **attributi** applicabili per l&#39;intestazione.

|                     | Browser web | CDN | Descrizione |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | Questa intestazione controlla il browser web e la durata della cache CDN. |
| Surrogate-Control | ✘ | ✔ | Questa intestazione controlla la durata della cache CDN. |
| Scade | ✔ | ✔ | Questa intestazione controlla il browser web e la durata della cache CDN. |


- **max-age**: questo attributo controlla il TTL o &quot;time to live&quot; del contenuto della risposta in secondi.
- **stale-while-revalidate**: questo attributo controlla il trattamento _stale state_ del contenuto della risposta a livello CDN quando la richiesta ricevuta rientra nel periodo specificato in secondi. Lo _stato non aggiornato_ è il periodo di tempo dopo la scadenza del TTL e prima che la risposta venga riconvalidata.
- **stale-if-error**: questo attributo controlla il trattamento _stale state_ del contenuto della risposta a livello CDN quando il server di origine non è disponibile e la richiesta ricevuta rientra nel periodo specificato in secondi.

Rivedi i dettagli [stabilità e riconvalida](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) per ulteriori informazioni.

#### Esempio

Per aumentare la durata della cache CDN e del browser Web del tipo di contenuto **HTML** a _10 minuti_ senza trattamento dello stato non aggiornato, eseguire la procedura seguente:

1. Nel progetto AEM, individuare il file vhsot desiderato dalla directory `dispatcher/src/conf.d/available_vhosts`.
1. Aggiornare il file vhost (ad esempio `wknd.vhost`) come segue:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   I file vhost nella directory `dispatcher/src/conf.d/enabled_vhosts` sono **symlinks** ai file nella directory `dispatcher/src/conf.d/available_vhosts`. Assicurarsi quindi di creare symlink se non presenti.
1. Distribuisci le modifiche vhost nell&#39;ambiente AEM as a Cloud Service desiderato utilizzando [Cloud Manager - Pipeline di configurazione a livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Tuttavia, per avere valori diversi per il browser web e la durata della cache CDN, puoi utilizzare l&#39;intestazione `Surrogate-Control` nell&#39;esempio precedente. Allo stesso modo, per far scadere la cache a una data e un&#39;ora specifiche, è possibile utilizzare l&#39;intestazione `Expires`. Inoltre, utilizzando gli attributi `stale-while-revalidate` e `stale-if-error`, è possibile controllare il trattamento dello stato non aggiornato del contenuto della risposta. Il progetto AEM WKND ha una configurazione della cache CDN [reference stale state treatment](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155).

Allo stesso modo, puoi aggiornare le intestazioni della cache anche per altri tipi di contenuto (JSON, JS, CSS e Assets).

### Codice Java™ personalizzato

Questa opzione è disponibile sia per AEM Publish che per Author. Tuttavia, si sconsiglia di abilitare il caching in AEM Author e di mantenere il comportamento di caching predefinito.

Per aggiornare le intestazioni della cache, utilizzare l&#39;oggetto `HttpServletResponse` nel codice Java™ personalizzato (servlet Sling, filtro servlet Sling). La sintassi generale è la seguente:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
