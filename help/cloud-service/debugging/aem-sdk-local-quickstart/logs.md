---
title: Debug dell’SDK AEM tramite i registri
description: I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 411
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Debug dell’SDK AEM tramite i registri

Accedendo ai registri dell’SDK dell’AEM, il file Jar per l’avvio rapido locale dell’SDK dell’AEM o gli strumenti Dispatcher possono fornire informazioni chiave sul debug delle applicazioni AEM.

## Registri AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata. L’Adobe consiglia di mantenere le configurazioni di sviluppo locale e di registrazione di AEM as a Cloud Service Dev il più simile possibile, in quanto normalizza la visibilità del registro negli ambienti quickstart locali dell’SDK dell’AEM e di sviluppo AEM as a Cloud Service, riducendo il raggruppamento e la ridistribuzione della configurazione.

L&#39;[Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) configura la registrazione a livello DEBUG per i pacchetti Java dell&#39;applicazione AEM per lo sviluppo locale tramite la configurazione OSGi Sling Logger trovata in

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

che accede a `error.log`.

Se la registrazione predefinita non è sufficiente per lo sviluppo locale, la registrazione ad hoc può essere configurata tramite la console web locale quickstart del supporto log dell&#39;SDK dell&#39;AEM, all&#39;indirizzo ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)). Si consiglia tuttavia di non salvare le modifiche ad hoc in modo permanente su Git, a meno che non siano necessarie le stesse configurazioni di registro anche negli ambienti di sviluppo AEM as a Cloud Service. Tieni presente che le modifiche tramite la console Log Support vengono salvate in modo permanente direttamente nell’archivio quickstart locale dell’SDK dell’AEM.

Le istruzioni di registro Java possono essere visualizzate nel file `error.log`:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Spesso è utile &quot;coda&quot; `error.log` che invia il suo output al terminale.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows richiede [applicazioni di terze parti](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o l&#39;utilizzo del comando Get-Content di [Powershell](https://stackoverflow.com/a/46444596/133936).

## Registri di Dispatcher

I registri di Dispatcher vengono generati in stdout quando viene richiamato `bin/docker_run`, tuttavia i registri possono essere accessibili direttamente con nel Docker contain.

### Accesso ai registri nel contenitore Docker{#dispatcher-tools-access-logs}

I registri di Dispatcher possono accedere direttamente nel contenitore Docker in `/etc/httpd/logs`.

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

_Il `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` deve essere sostituito con l&#39;ID contenitore Docker di destinazione elencato dal comando `docker ps`._


### Copia dei registri Docker nel file system locale{#dispatcher-tools-copy-logs}

I registri di Dispatcher possono essere copiati dal contenitore Docker in `/etc/httpd/logs` nel file system locale per l&#39;ispezione utilizzando lo strumento di analisi dei registri preferito. Tieni presente che si tratta di una copia point-in-time e non fornisce aggiornamenti in tempo reale ai registri.

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

_Il `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve essere sostituito con l&#39;ID contenitore Docker di destinazione elencato dal comando `docker ps`._
