---
title: Debug degli strumenti di Dispatcher
description: Dispatcher Tools fornisce un ambiente Apache Web Server containerizzato che può essere utilizzato per simulare AEM come Dispatcher del servizio AEM Publish di Cloud Services localmente. Il debug dei registri e dei contenuti della cache degli strumenti di Dispatcher può essere fondamentale per garantire che l’applicazione AEM end-to-end e le configurazioni di cache e sicurezza di supporto siano corrette.
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: Sviluppo
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---


# Debug degli strumenti di Dispatcher

Dispatcher Tools fornisce un ambiente Apache Web Server containerizzato che può essere utilizzato per simulare AEM come Dispatcher del servizio AEM Publish di Cloud Services localmente.
Il debug dei registri e dei contenuti della cache degli strumenti di Dispatcher può essere fondamentale per garantire che l’applicazione AEM end-to-end e le configurazioni di cache e sicurezza di supporto siano corrette.

>[!NOTE]
>
>Poiché gli strumenti di Dispatcher sono basati su contenitori, ogni volta che vengono riavviati vengono distrutti i registri precedenti e il contenuto della cache.

## Registri degli strumenti di Dispatcher

I registri degli strumenti del Dispatcher sono disponibili tramite il comando `stdout` o `bin/docker_run` oppure con ulteriori dettagli nel contenitore Docker all’indirizzo `/etc/https/logs`.

Per istruzioni su come accedere direttamente ai registri del contenitore Docker degli strumenti di Dispatcher, consulta [Log di Dispatcher](./logs.md#dispatcher-logs) .

## Cache degli strumenti del dispatcher

### Accesso ai registri nel contenitore Docker

La cache del Dispatcher può accedere direttamente nel contenitore Docker all’indirizzo ` /mnt/var/www/html`.

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

### Copiare i log Docker nel file system locale

I registri del Dispatcher possono essere copiati dal contenitore Docker in `/mnt/var/www/html` al file system locale per essere ispezionati utilizzando gli strumenti preferiti. Questa è una copia point-in-time e non fornisce aggiornamenti in tempo reale alla cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

