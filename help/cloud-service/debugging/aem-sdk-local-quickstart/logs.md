---
title: Debug AEM SDK tramite registri
description: I registri fungono da front-line per il debug AEM applicazioni, ma dipendono dall’accesso adeguato nell’applicazione AEM distribuita.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 2%

---


# Debug AEM SDK tramite registri

Accedendo ai registri dell&#39;SDK AEM, gli strumenti Jar o Dispatcher locali dell&#39;SDK AEM possono fornire informazioni chiave sul debug AEM applicazioni.

## Registri AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

I registri fungono da front-line per il debug AEM applicazioni, ma dipendono dall’accesso adeguato nell’applicazione AEM distribuita.  Adobe consiglia di mantenere le configurazioni di sviluppo locale e AEM come un Cloud Service di registrazione per sviluppatori il più possibile simili, in quanto normalizza la visibilità del registro sul quickstart locale dell’SDK AEM e AEM come ambienti  Dev, riducendo il volume di configurazione e la ridistribuzione.

Il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) configura la registrazione a livello DEBUG per i pacchetti Java dell&#39;applicazione AEM per lo sviluppo locale tramite la configurazione Sling Logger OSGi disponibile all&#39;indirizzo

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

che accede al `error.log`.

Se la registrazione predefinita non è sufficiente per lo sviluppo locale, è possibile configurare la registrazione ad hoc tramite AEM console Web di assistenza rapida locale dell’SDK, all’indirizzo ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)). Tuttavia, non è consigliabile che le modifiche ad hoc siano persistenti in Git a meno che le stesse configurazioni di registro siano necessarie anche in AEM come ambienti Cloud Service Dev. Le modifiche effettuate tramite la console di supporto registro vengono memorizzate direttamente nell’archivio locale dell’SDK AEM.

Le istruzioni di registro Java possono essere visualizzate nel `error.log` file:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Spesso è utile &quot;coda&quot; il `error.log` quale trasmette la sua uscita al terminale.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows richiede applicazioni [di coda di](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) terze parti o l&#39;utilizzo del comando [Get-Content di](https://stackoverflow.com/a/46444596/133936)PowerShell.

## Registri del dispatcher

I registri del dispatcher vengono inviati a stdout quando `bin/docker_run` viene richiamato, ma i registri possono essere utilizzati direttamente con nel Docker.

### Accesso ai registri nel contenitore Docker

I registri del dispatcher possono accedere direttamente nel contenitore Docker all&#39;indirizzo `/etc/httpd/logs`.

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

### Copia dei log del Docker nel file system locale

I registri del dispatcher possono essere copiati dal contenitore Docker `/etc/httpd/logs` al file system locale per l&#39;ispezione utilizzando il tuo strumento di analisi dei log preferito. Si tratta di una copia point-in-time e non fornisce aggiornamenti in tempo reale ai registri.

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

