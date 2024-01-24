---
title: Strumenti di debug di Dispatcher
description: Gli strumenti di Dispatcher forniscono un ambiente Apache Web Server containerizzato che può essere utilizzato per simulare in locale Dispatcher del servizio di pubblicazione AEM di AEM as a Cloud Service. Il debug dei registri e dei contenuti della cache degli strumenti di Dispatcher può essere fondamentale per garantire la correttezza dell’applicazione AEM end-to-end e delle configurazioni di cache e sicurezza di supporto.
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 67
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Strumenti di debug di Dispatcher

Gli strumenti di Dispatcher forniscono un ambiente Apache Web Server containerizzato che può essere utilizzato per simulare in locale Dispatcher del servizio di pubblicazione AEM di AEM as a Cloud Service.

Il debug dei registri e dei contenuti della cache degli strumenti di Dispatcher può essere fondamentale per garantire la correttezza dell’applicazione AEM end-to-end e delle configurazioni di cache e sicurezza di supporto.

>[!NOTE]
>
>Poiché Dispatcher Tools è basato su contenitori, ogni volta che viene riavviato, i registri e i contenuti della cache precedenti vengono eliminati.

## Registri degli strumenti di Dispatcher

I registri degli strumenti di Dispatcher sono disponibili tramite `stdout` o `bin/docker_run` o con maggiori dettagli, disponibili nel contenitore Docker all’indirizzo `/etc/https/logs`.

Consulta [Registri di Dispatcher](./logs.md#dispatcher-logs) per istruzioni su come accedere direttamente ai registri del contenitore Docker degli strumenti di Dispatcher.

## Cache degli strumenti di Dispatcher

### Accesso ai registri nel contenitore Docker

La cache di Dispatcher può accedere direttamente nel contenitore Docker all’indirizzo ` /mnt/var/www/html`.

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

### Copia dei registri Docker nel file system locale

I registri di Dispatcher possono essere copiati fuori dal contenitore Docker in `/mnt/var/www/html` nel file system locale per l&#39;ispezione con gli strumenti preferiti. Tieni presente che si tratta di una copia point-in-time e non fornisce aggiornamenti in tempo reale alla cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
