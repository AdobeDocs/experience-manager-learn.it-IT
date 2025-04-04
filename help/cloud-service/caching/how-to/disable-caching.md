---
title: Come disabilitare CDN caching
description: Scopri come disabilitare la caching delle risposte HTTP in AEM come CDN di un Cloud Service.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: a98ca7ddc155190b63664239d604d11ad470fdf5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Come disabilitare CDN caching

Scopri come disabilitare la caching delle risposte HTTP in AEM come CDN di un Cloud Service. Il caching delle risposte è controllato da , `Surrogate-Control`o `Expires` da `Cache-Control`intestazioni della cache di risposta HTTP.

Queste intestazioni di cache sono tipicamente impostate in configurazioni AEM Dispatcher vhost usando `mod_headers`, ma possono anche essere impostate in codice Java™ personalizzato in esecuzione in AEM Publish stesso.

## Comportamento caching predefinito

La memorizzazione nella cache delle risposte HTTP in AEM come la rete CDN di un Cloud Service è controllata dalle seguenti intestazioni di risposta HTTP dall&#39;origine `Cache-Control`, `Surrogate-Control`o `Expires`.  Le risposte di origine che contengono `private`, `no-cache` o `no-store` in non sono memorizzate nella  `Cache-Control` cache.

Esaminare il [comportamento](./enable-caching.md#default-caching-behavior) caching predefinito per AEM Publish e Autore quando viene distribuito un progetto AEM basato su un AEM archetipo di progetto.


## Disabilita caching

La disattivazione di caching può avere un impatto negativo sulle prestazioni del AEM come Cloud Service istanza, quindi presta attenzione quando disattivi il comportamento caching predefinito.

Esistono tuttavia alcuni scenari in cui potrebbe essere utile disabilitare caching, ad esempio:

- Sviluppare una nuova funzionalità e vedere subito le modifiche.
- Il contenuto è sicuro (pensato solo per gli utenti autenticati) o dinamico (carrello, dettagli dell&#39;ordine) e non deve essere memorizzato nella cache.

Per disabilitare caching, è possibile aggiornare le intestazioni della cache in due modi.

1. **Dispatcher configurazione vhost:** disponibile solo per AEM Publish.
1. **Codice Java™ personalizzato:** disponibile sia per AEM Publish che per Autore.

Esaminiamo ognuna di queste opzioni.

### Dispatcher configurazione vhost

Questa opzione è l&#39;approccio consigliato per disabilitare caching tuttavia è disponibile solo per AEM Publish. Per aggiornare le intestazioni della cache, utilizza il modulo e `<LocationMatch>` la `mod_headers` direttiva nel file vhost del server HTTP di Apache. La sintassi generale è la seguente:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surroagate-Control
    Header unset Expires

    # Instructs the Browser and the CDN to not cache the response.
    Header always set Cache-Control "private"

    # Instructs only the CDN to not cache the response.
    Header always set Surrogate-Control "private"
</LocationMatch>
```

#### Esempio

Per disabilitare il caching CDN dei tipi **di contenuto CSS per alcuni scopi di risoluzione dei** problemi, seguire questi passaggi.

Si noti che, per ignorare la cache CSS esistente, è necessaria una modifica nel file CSS per generare una nuova chiave di cache per il file CSS.

1. Nel progetto AEM, individua il file vhsot desiderato dalla `dispatcher/src/conf.d/available_vhosts` directory.
1. Aggiorna il file vhost (ad esempio `wknd.vhost`) come segue:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the Browser and the CDN to not cache the response.
       Header always set Cache-Control "private"
   </LocationMatch>
   ```

   I file vhost nella `dispatcher/src/conf.d/enabled_vhosts` directory sono **collegamenti** simbolici ai file nella `dispatcher/src/conf.d/available_vhosts` directory, quindi assicurati di creare collegamenti simbolici se non presenti.
1. Distribuisci le modifiche al vhost nel AEM desiderato come ambiente Cloud Service utilizzando Cloud [Manager - Web Tier Config Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [i comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Codice Java™ personalizzato

Questa opzione è disponibile sia per AEM Publish che per Autore. Per aggiornare le intestazioni della cache, utilizzare l&#39;oggetto `SlingHttpServletResponse` nel codice Java™ personalizzato (Sling servlet, Sling servlet filter). La sintassi generale è la seguente:

```java
response.setHeader("Cache-Control", "private");
```
