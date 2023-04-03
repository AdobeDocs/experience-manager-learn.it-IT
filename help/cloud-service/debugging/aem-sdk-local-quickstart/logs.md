---
title: Debug AEM SDK tramite i registri
description: I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM, ma dipendono dalla registrazione appropriata nell’applicazione AEM distribuita.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# Debug AEM SDK tramite i registri

Accedendo ai registri dell’SDK AEM, gli strumenti locali quickstart Jar dell’SDK AEM o Dispatcher Tools possono fornire informazioni chiave sul debug AEM applicazioni.

## Registri AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM, ma dipendono dalla registrazione appropriata nell’applicazione AEM distribuita. Adobe consiglia di mantenere le configurazioni di sviluppo locale e di AEM logging sviluppo as a Cloud Service quanto più simili possibile, in quanto normalizza la visibilità del registro sugli ambienti quickstart locale dell’SDK AEM e AEM sviluppo di as a Cloud Service, riducendo il volume di configurazione e la ridistribuzione.

La [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype) configura la registrazione a livello di DEBUG per i pacchetti Java dell’applicazione AEM per lo sviluppo locale tramite la configurazione Sling Logger OSGi disponibile in

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

che accede al `error.log`.

Se la registrazione predefinita non è sufficiente per lo sviluppo locale, la registrazione ad hoc può essere configurata tramite la console web locale di supporto registro di AEM SDK, all’indirizzo ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), tuttavia, non si consiglia di mantenere le modifiche ad hoc su Git a meno che queste stesse configurazioni di registro non siano necessarie anche AEM ambienti di sviluppo as a Cloud Service. Tieni presente che le modifiche effettuate tramite la console Supporto registro vengono mantenute direttamente nell’archivio locale dell’SDK AEM.

Le istruzioni di registro Java possono essere visualizzate nella `error.log` file:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Spesso è utile &quot;tail&quot; il `error.log` che trasmette la sua uscita al terminale.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows richiede [Applicazioni tail di terze parti](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o l&#39;uso di [Comando Get-Content di Powershell](https://stackoverflow.com/a/46444596/133936).

## Log di Dispatcher

I registri del Dispatcher vengono inviati allo stdout quando `bin/docker_run` viene richiamato, tuttavia i registri possono essere accessibili direttamente con in Docker contain.

### Accesso ai registri nel contenitore Docker{#dispatcher-tools-access-logs}

I registri del Dispatcher possono accedere direttamente nel contenitore Docker all’indirizzo `/etc/httpd/logs`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_La `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` deve essere sostituito con l’ID Docker CONTAINER di destinazione elencato nella `docker ps` comando._


### Copiare i log Docker nel file system locale{#dispatcher-tools-copy-logs}

I registri del Dispatcher possono essere copiati dal contenitore Docker in `/etc/httpd/logs` al file system locale per l&#39;ispezione utilizzando lo strumento di analisi del log preferito. Si tratta di una copia point-in-time e non fornisce aggiornamenti in tempo reale ai registri.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_La `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve essere sostituito con l’ID Docker CONTAINER di destinazione elencato nella `docker ps` comando._
