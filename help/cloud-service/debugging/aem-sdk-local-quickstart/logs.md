---
title: Debug dell’SDK AEM tramite i registri
description: I registri fungono da guida per il debug delle applicazioni AEM, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata.
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante, intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---


# Debug dell’SDK AEM tramite i registri

Accedendo ai registri dell’SDK AEM, sia l’SDK di AEM che gli strumenti di avvio rapido locali di Jar o Dispatcher possono fornire informazioni chiave sul debug delle applicazioni AEM.

## Registri AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

I registri fungono da guida per il debug delle applicazioni AEM, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata. Adobe consiglia di mantenere le configurazioni di accesso allo sviluppo locale e AEM as a Cloud Service Dev quanto più simili possibile, in quanto normalizza la visibilità del registro sugli ambienti quickstart locali dell’SDK di AEM e gli ambienti di sviluppo di AEM as a Cloud Service, riducendo il volume di configurazione e la ridistribuzione.

Il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) configura la registrazione a livello di DEBUG per i pacchetti Java dell’applicazione AEM per lo sviluppo locale tramite la configurazione Sling Logger OSGi disponibile all’indirizzo

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

che accede al file `error.log`.

Se la registrazione predefinita non è sufficiente per lo sviluppo locale, la registrazione ad hoc può essere configurata tramite la console web di supporto di registro di AEM SDK, all&#39;indirizzo ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), tuttavia non è consigliabile che le modifiche ad hoc vengano mantenute su Git a meno che queste stesse configurazioni di registro non siano necessarie anche negli ambienti di sviluppo di AEM as a Cloud Service. Tieni presente che le modifiche effettuate tramite la console Supporto registro vengono mantenute direttamente nell’archivio locale di quickstart dell’SDK AEM.

Le istruzioni di registro Java possono essere visualizzate nel file `error.log` :

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Spesso è utile &quot;tail&quot; il `error.log` che trasmette la sua uscita al terminale.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows richiede [applicazioni tail di terze parti](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o l&#39;uso del comando Get-Content di PowerShell](https://stackoverflow.com/a/46444596/133936).[

## Log di Dispatcher

I registri del Dispatcher vengono inviati a stdout quando viene richiamato `bin/docker_run`, tuttavia i registri possono essere accessibili direttamente con nel Docker contengono.

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

_Il  `<CONTAINER ID>` in  `docker exec -it <CONTAINER ID> /bin/sh` deve essere sostituito con l’ID CONTENITORE Docker di destinazione elencato dal  `docker ps` comando ._


### Copiare i log Docker nel file system locale{#dispatcher-tools-copy-logs}

I registri del Dispatcher possono essere copiati dal contenitore Docker in `/etc/httpd/logs` al file system locale per l’ispezione utilizzando il tuo strumento di analisi dei log preferito. Si tratta di una copia point-in-time e non fornisce aggiornamenti in tempo reale ai registri.

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

_Il  `<CONTAINER_ID>` in  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve essere sostituito con l’ID CONTENITORE Docker di destinazione elencato dal  `docker ps` comando ._
