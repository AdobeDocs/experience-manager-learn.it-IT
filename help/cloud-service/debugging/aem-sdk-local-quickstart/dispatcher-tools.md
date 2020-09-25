---
title: Debugging degli strumenti del dispatcher
description: Dispatcher Tools fornisce un ambiente Apache Web Server containerizzato che può essere utilizzato per simulare AEM come Dispatcher  servizio AEM Publish localmente. Il debug dei registri e dei contenuti della cache di Dispatcher Tools può essere fondamentale per garantire che l'applicazione end-to-end AEM e le configurazioni di cache e protezione supportate siano corrette.
feature: dispatcher, aem-sdk
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Debugging degli strumenti del dispatcher

Dispatcher Tools fornisce un ambiente Apache Web Server containerizzato che può essere utilizzato per simulare AEM come Dispatcher  servizio AEM Publish localmente.
Il debug dei registri e dei contenuti della cache di Dispatcher Tools può essere fondamentale per garantire che l&#39;applicazione end-to-end AEM e le configurazioni di cache e protezione supportate siano corrette.

>[!NOTE]
>
>Poiché Dispatcher Tools è basato su contenitore, ogni volta che viene riavviato, i registri precedenti e il contenuto della cache vengono distrutti.

## Dispatcher Tools logs

I registri degli strumenti del dispatcher sono disponibili tramite il `stdout` comando o il `bin/docker_run` , oppure con maggiori dettagli, nel contenitore Docker all&#39;indirizzo `/etc/https/logs`.

Per istruzioni su come accedere direttamente ai registri del contenitore [Dispatcher Tools&#39;s Docker, vedere Registri](./logs.md#dispatcher-logs) del dispatcher.

## Cache degli strumenti del dispatcher

### Accesso ai registri nel contenitore Docker

La cache del Dispatcher può accedere direttamente nel contenitore Docker all&#39;indirizzo ` /mnt/var/www/html`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### Copia dei log del Docker nel file system locale

I registri del dispatcher possono essere copiati dal contenitore Docker `/mnt/var/www/html` al file system locale per l&#39;ispezione utilizzando i vostri strumenti preferiti. Si tratta di una copia point-in-time e non fornisce aggiornamenti in tempo reale alla cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

