---
title: Debug dell’SDK AEM tramite i registri
description: I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# Debug dell’SDK AEM tramite i registri

Accedendo ai registri dell’SDK dell’AEM, il modulo quickstart Jar locale dell’SDK dell’AEM o gli strumenti di Dispatcher possono fornire informazioni chiave sul debug delle applicazioni AEM.

## Registri AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata. L’Adobe consiglia di mantenere le configurazioni di registrazione di sviluppo locale e AEM as a Cloud Service il più simile possibile, in quanto normalizza la visibilità dei registri negli ambienti di sviluppo quickstart locale dell’SDK dell’AEM e dell’AEM as a Cloud Service, riducendo il raggruppamento e la ridistribuzione della configurazione.

Il [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) configura la registrazione a livello DEBUG per i pacchetti Java dell’applicazione AEM per lo sviluppo locale tramite la configurazione OSGi di Sling Logger disponibile all’indirizzo

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

che accede al `error.log`.

Se la registrazione predefinita non è sufficiente per lo sviluppo locale, la registrazione ad hoc può essere configurata tramite la console web locale quickstart del supporto del registro dell’SDK di AEM, all’indirizzo ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), tuttavia non è consigliabile che le modifiche ad hoc vengano salvate in modo permanente in Git, a meno che non siano necessarie le stesse configurazioni di registro anche per gli ambienti di sviluppo as a Cloud Service per AEM. Tieni presente che le modifiche tramite la console Log Support vengono salvate in modo permanente direttamente nell’archivio quickstart locale dell’SDK dell’AEM.

Le istruzioni di registro Java possono essere visualizzate in `error.log` file:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Spesso è utile &quot;coda&quot; il `error.log` che invia l&#39;output al terminale.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows richiede [Applicazioni tail di terze parti](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o l&#39;uso di [Comando Get-Content di Powershell](https://stackoverflow.com/a/46444596/133936).

## Registri di Dispatcher

I registri di Dispatcher vengono inviati allo stdout quando `bin/docker_run` viene richiamato, tuttavia i registri possono essere accessibili direttamente con nel Docker contain.

### Accesso ai registri nel contenitore Docker{#dispatcher-tools-access-logs}

I registri di Dispatcher possono accedere direttamente nel contenitore Docker all’indirizzo `/etc/httpd/logs`.

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

_Il `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` deve essere sostituito con l’ID contenitore Docker di destinazione elencato dalla sezione `docker ps` comando._


### Copia dei registri Docker nel file system locale{#dispatcher-tools-copy-logs}

I registri di Dispatcher possono essere copiati fuori dal contenitore Docker in `/etc/httpd/logs` nel file system locale per l&#39;ispezione utilizzando lo strumento di analisi del registro preferito. Tieni presente che si tratta di una copia point-in-time e non fornisce aggiornamenti in tempo reale ai registri.

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

_Il `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve essere sostituito con l’ID contenitore Docker di destinazione elencato dalla sezione `docker ps` comando._
