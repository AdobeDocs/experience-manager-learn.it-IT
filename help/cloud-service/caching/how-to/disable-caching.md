---
title: Disabilitare il caching CDN
description: Scopri come disabilitare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---


# Disabilitare il caching CDN

Scopri come disabilitare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service. La memorizzazione nella cache delle risposte è controllata da `Cache-Control`, `Surrogate-Control`, o `Expires` Intestazioni cache di risposta HTTP.

Queste intestazioni di cache sono in genere impostate nelle configurazioni host del Dispatcher AEM utilizzando `mod_headers`, ma può anche essere impostato nel codice Java™ personalizzato eseguito nella pubblicazione AEM stessa.

## Comportamento di caching predefinito

Rivedi il comportamento di caching predefinito per Pubblicazione e authoring AEM quando un [Archetipo progetto AEM](./enable-caching.md#default-caching-behavior) viene implementato un progetto basato sull’AEM.

## Disattiva caching

La disattivazione della memorizzazione nella cache può avere un impatto negativo sulle prestazioni dell’istanza AEM as a Cloud Service, pertanto occorre prestare attenzione quando si disattiva il comportamento predefinito della memorizzazione nella cache.

Tuttavia, in alcuni casi può essere utile disattivare la memorizzazione in cache, ad esempio:

- Lo sviluppo di una nuova funzione e desidera vedere immediatamente le modifiche.
- Il contenuto è sicuro (destinato solo agli utenti autenticati) o dinamico (carrello, dettagli ordine) e non deve essere memorizzato in cache.

Per disabilitare il caching, puoi aggiornare le intestazioni della cache in due modi.

1. **Configurazione vhost di Dispatcher:** Disponibile solo per pubblicazione AEM.
1. **Codice Java™ personalizzato:** Disponibile sia per pubblicazione AEM che per creazione.

Esaminiamo ognuna di queste opzioni.

### Configurazione vhost di Dispatcher

Questa opzione è l’approccio consigliato per disabilitare la memorizzazione in cache, ma è disponibile solo per la pubblicazione AEM. Per aggiornare le intestazioni della cache, utilizza `mod_headers` modulo e `<LocationMatch>` nel file vhost del server HTTP Apache. La sintassi generale è la seguente:

    &quot;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    # Rimuove l&#39;intestazione di risposta del nome, se esistente. Se sono presenti più intestazioni con lo stesso nome, verranno rimosse tutte.
    Intestazione unset Cache-Control
    Scadenza annullamento impostazione intestazione
    
    # Indica alla rete CDN di non memorizzare la risposta nella cache.
    Set di intestazioni Cache-Control &quot;private&quot;
    &lt;/locationmatch>
    &quot;

#### Esempio

Per disattivare la memorizzazione in cache CDN di **Tipi di contenuto CSS** per alcuni scopi di risoluzione dei problemi, segui questi passaggi.

Per ignorare la cache CSS esistente, è necessario modificare il file CSS per generare una nuova chiave cache per il file CSS.

1. Nel progetto AEM, individua il file vhsot desiderato da `dispatcher/src/conf.d/available_vhosts` directory.
1. Aggiorna il vhost (ad es. `wknd.vhost`) file come segue:

       &quot;conf
       &lt;locationmatch etc.clientlibs=&quot;&quot;>*\.(css)$&quot;>
       # Rimuove l&#39;intestazione di risposta del nome, se esistente. Se sono presenti più intestazioni con lo stesso nome, verranno rimosse tutte.
       Intestazione unset Cache-Control
       Scadenza annullamento impostazione intestazione
       
       # Indica alla rete CDN di non memorizzare la risposta nella cache.
       Set di intestazioni Cache-Control &quot;private&quot;
       &lt;/locationmatch>
       &quot;
   I file vhost in `dispatcher/src/conf.d/enabled_vhosts` directory sono **symlink** ai file in `dispatcher/src/conf.d/available_vhosts` , quindi assicurati di creare symlink se non presente.
1. Implementare le modifiche vhost nell’ambiente AEM as a Cloud Service desiderato utilizzando [Cloud Manager - Pipeline di configurazione a livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Codice Java™ personalizzato

Questa opzione è disponibile sia per la pubblicazione AEM che per l’authoring. Per aggiornare le intestazioni della cache, utilizza `SlingHttpServletResponse` oggetto nel codice Java™ personalizzato (servlet Sling, filtro servlet Sling). La sintassi generale è la seguente:

    &quot;java
    response.setHeader(&quot;Cache-Control&quot;, &quot;private&quot;);
    &quot;
