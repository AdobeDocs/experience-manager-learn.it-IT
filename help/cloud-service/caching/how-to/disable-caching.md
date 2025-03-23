---
title: Disabilitare il caching CDN
description: Scopri come disattivare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service.
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Disabilitare il caching CDN

Scopri come disattivare la memorizzazione nella cache delle risposte HTTP nella rete CDN di AEM as a Cloud Service. La memorizzazione nella cache delle risposte è controllata da `Cache-Control`, `Surrogate-Control` o `Expires` intestazioni cache di risposta HTTP.

Queste intestazioni di cache sono in genere impostate nelle configurazioni vhost di AEM Dispatcher utilizzando `mod_headers`, ma possono anche essere impostate nel codice Java™ personalizzato in esecuzione nella stessa pubblicazione di AEM.

## Comportamento di caching predefinito

Rivedi il comportamento di caching predefinito per AEM Publish e Author quando viene distribuito un progetto AEM basato su [Archetipo progetto AEM](./enable-caching.md#default-caching-behavior).

## Disattiva caching

La disattivazione della memorizzazione nella cache può avere un impatto negativo sulle prestazioni dell’istanza di AEM as a Cloud Service, pertanto è necessario prestare attenzione quando si disattiva il comportamento di memorizzazione nella cache predefinito.

Tuttavia, in alcuni casi può essere utile disattivare la memorizzazione in cache, ad esempio:

- Lo sviluppo di una nuova funzione e desidera vedere immediatamente le modifiche.
- Il contenuto è sicuro (destinato solo agli utenti autenticati) o dinamico (carrello, dettagli ordine) e non deve essere memorizzato in cache.

Per disabilitare il caching, puoi aggiornare le intestazioni della cache in due modi.

1. **Configurazione vhost Dispatcher:** disponibile solo per AEM Publish.
1. **Codice Java™ personalizzato:** Disponibile sia per AEM Publish che per Author.

Esaminiamo ognuna di queste opzioni.

### Configurazione vhost Dispatcher

Questa opzione è l’approccio consigliato per disabilitare la memorizzazione in cache, ma è disponibile solo per AEM Publish. Per aggiornare le intestazioni della cache, utilizzare il modulo `mod_headers` e la direttiva `<LocationMatch>` nel file vhost del server HTTP Apache. La sintassi generale è la seguente:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### Esempio

Per disattivare la memorizzazione nella cache CDN dei **tipi di contenuto CSS** per alcune operazioni di risoluzione dei problemi, eseguire la procedura seguente.

Per ignorare la cache CSS esistente, è necessario modificare il file CSS per generare una nuova chiave cache per il file CSS.

1. Nel progetto AEM, individuare il file vhsot desiderato dalla directory `dispatcher/src/conf.d/available_vhosts`.
1. Aggiornare il file vhost (ad esempio `wknd.vhost`) come segue:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   I file vhost nella directory `dispatcher/src/conf.d/enabled_vhosts` sono **symlinks** ai file nella directory `dispatcher/src/conf.d/available_vhosts`. Assicurarsi quindi di creare symlink se non presenti.
1. Distribuisci le modifiche vhost nell&#39;ambiente AEM as a Cloud Service desiderato utilizzando [Cloud Manager - Pipeline di configurazione a livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Codice Java™ personalizzato

Questa opzione è disponibile sia per AEM Publish che per Author. Per aggiornare le intestazioni della cache, utilizzare l&#39;oggetto `SlingHttpServletResponse` nel codice Java™ personalizzato (servlet Sling, filtro servlet Sling). La sintassi generale è la seguente:

```java
response.setHeader("Cache-Control", "private");
```
