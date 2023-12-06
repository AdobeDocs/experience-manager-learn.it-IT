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
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '897'
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

    &quot;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    # Rimuove l&#39;intestazione di risposta del nome, se esistente. Se sono presenti più intestazioni con lo stesso nome, verranno rimosse tutte.
    Intestazione unset Cache-Control
    Intestazione unset Surrogate-Control
    Scadenza annullamento impostazione intestazione
    
    # Indica al browser web e alla rete CDN di memorizzare nella cache la risposta per il valore &quot;max-age&quot; (XXX) secondi. Gli attributi &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controllano il trattamento dello stato stale a livello CDN.
    Set di intestazioni Cache-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # Indica alla rete CDN di memorizzare nella cache la risposta per il valore &quot;max-age&quot; (XXX) secondi. Gli attributi &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controllano il trattamento dello stato stale a livello CDN.
    Set di intestazioni Surrogate-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # Indica al browser Web e alla rete CDN di memorizzare la risposta nella cache fino alla data e all&#39;ora specificate.
    Scade il set di intestazioni &quot;Sun, 31 dicembre 2023 23:59:59 GMT&quot;
    &lt;/locationmatch>
    &quot;

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

       &quot;conf
       &lt;locationmatch content=&quot;&quot;>*\.(html)$&quot;>
       # Rimuove l&#39;intestazione di risposta se presente
       Intestazione unset Cache-Control
       
       # Indica al browser web e alla rete CDN di memorizzare nella cache la risposta per un valore di età massima (600) secondi.
       Set di intestazioni Cache-Control &quot;max-age=600&quot;
       &lt;/locationmatch>
       &quot;
   I file vhost in `dispatcher/src/conf.d/enabled_vhosts` directory sono **symlink** ai file in `dispatcher/src/conf.d/available_vhosts` , quindi assicurati di creare symlink se non presente.
1. Implementare le modifiche vhost nell’ambiente AEM as a Cloud Service desiderato utilizzando [Cloud Manager - Pipeline di configurazione a livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Tuttavia, per avere valori diversi per il browser web e la durata della cache CDN, puoi utilizzare `Surrogate-Control` nell’esempio precedente. Allo stesso modo, per far scadere la cache a una data e un’ora specifiche, puoi utilizzare `Expires` intestazione. Inoltre, utilizzando `stale-while-revalidate` e `stale-if-error` , è possibile controllare il trattamento dello stato non aggiornato del contenuto della risposta. Il progetto WKND dell’AEM ha [trattamento dello stato stale di riferimento](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) Configurazione della cache CDN.

Allo stesso modo, puoi aggiornare le intestazioni della cache anche per altri tipi di contenuto (JSON, JS, CSS e Assets).

### Codice Java™ personalizzato

Questa opzione è disponibile sia per la pubblicazione AEM che per l’authoring. Tuttavia, si sconsiglia di abilitare il caching in AEM Author e di mantenere il comportamento di caching predefinito.

Per aggiornare le intestazioni della cache, utilizza `HttpServletResponse` oggetto nel codice Java™ personalizzato (servlet Sling, filtro servlet Sling). La sintassi generale è la seguente:

    &quot;java
    // Indica al browser web e alla rete CDN di memorizzare in cache la risposta per il valore &quot;max-age&quot; (XXX) secondi. Gli attributi &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controllano il trattamento dello stato stale a livello CDN.
    response.setHeader(&quot;Cache-Control&quot;, &quot;max-age=XXX, stale-while-revalidate=XXX, stale-if-error=XXX&quot;);
    
    // Indica alla rete CDN di memorizzare nella cache la risposta per il valore &quot;max-age&quot; (XXX) secondi. Gli attributi &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controllano il trattamento dello stato stale a livello CDN.
    response.setHeader(&quot;Surrogate-Control&quot;, &quot;max-age=XXX, stale-while-revalidate=XXX, stale-if-error=XXX&quot;);
    
    // Indica al browser web e alla rete CDN di memorizzare la risposta nella cache fino alla data e all’ora specificate.
    response.setHeader(&quot;Scade&quot;, &quot;Sun, 31 dicembre 2023 23:59:59 GMT&quot;);
    &quot;
