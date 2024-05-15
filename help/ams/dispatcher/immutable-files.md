---
title: File di sola lettura o immutabili del dispatcher di AMS
description: Informazioni sul motivo per cui alcuni file sono di sola lettura o non modificabili e su come apportare le modifiche funzionali desiderate
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
duration: 253
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 0%

---

# File di sola lettura o immutabili in AMS

[Sommario](./overview.md)

[&lt;- Precedente: Registri comuni](./common-logs.md)

## Descrizione

Questo documento descrive quali file sono bloccati e non devono essere modificati e come impostare correttamente le impostazioni di configurazione desiderate.

Quando AMS esegue il provisioning di un sistema, implementa una configurazione di base che rende tutto funzionale e sicuro.  AMS vuole garantire che queste funzioni rimangano come una linea di base per la funzionalità e la sicurezza.  A questo scopo, alcuni file sono contrassegnati come di sola lettura e immutabili per evitare di modificarli.

Il layout non impedisce di modificarne il comportamento e di ignorare le modifiche necessarie.  Invece di modificare questi file, sovrapporrai il tuo file che sostituisce l’originale.

Questo ti rassicura anche che, quando AMS esegue le patch dei Dispatcher con le ultime correzioni e miglioramenti di sicurezza, i file non verranno modificati.  Potrai quindi continuare a beneficiare dei miglioramenti e adottare solo le modifiche desiderate.
![Mostra una pista da bowling con una palla che scorre lungo la corsia.  La palla ha una freccia con la parola che vi mostra.  I paraurti delle grondaie sono sollevati e sopra di essi si trovano le parole file immutabili.](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
Come illustrato nell’immagine precedente, i file immutabili non ti impediscono di giocare.  Ti impediscono di danneggiare la prestazione e ti mantengono sulla corsia.  Questo metodo ci permette di avere alcune caratteristiche molto importanti:

- Le personalizzazioni vengono gestite nei propri spazi di sicurezza
- La sovrapposizione delle modifiche personalizzate rispecchia quella dei metodi di sovrapposizione in AEM
- Le configurazioni AMS di patch possono essere eseguite senza modificare le personalizzazioni
- L&#39;installazione di base di test e le configurazioni personalizzate possono essere eseguite contemporaneamente per aiutare a individuare se i problemi sono causati da personalizzazioni o qualcos&#39;altro.


Di seguito è riportato un elenco tipico di file distribuiti con un Dispatcher:

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

Per determinare quali file sono immutabili, puoi eseguire il seguente comando su un Dispatcher per visualizzare:

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Di seguito è riportato un esempio di risposta di quali file non sono modificabili:

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## Come apportare modifiche

### Variabili

Le variabili consentono di apportare modifiche funzionali senza modificare i file di configurazione stessi.  Alcuni elementi della configurazione possono essere regolati regolando i valori delle variabili.  Un esempio che è possibile evidenziare dal file `/etc/httpd/conf.d/dispatcher_vhost.conf` viene visualizzato qui:

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

Scopri come la direttiva DispatcherLogLevel ha una variabile di `DISP_LOG_LEVEL` invece del valore normale che vedete lì.  Sopra quella sezione di codice vedrai anche un’istruzione &quot;include&quot; in un file di variabili.  Il file della variabile `/etc/httpd/conf.d/variables/ams_default.vars` è dove vogliamo guardare dopo.  Di seguito sono elencati i contenuti di tale file di variabili:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Vedi sopra che il valore corrente di `DISP_LOG_LEVEL` la variabile è `info`.  Possiamo modificarlo per tracciare o eseguire il debug o il valore/livello del numero desiderato.  Ora ovunque controlli il livello di registro si regola automaticamente.

### Metodo di sovrapposizione

Devi comprendere il file di inclusione di livello superiore perché sarà il punto di partenza per effettuare qualsiasi personalizzazione.  Per iniziare con un semplice esempio, prendiamo uno scenario in cui vogliamo aggiungere un nuovo nome di dominio che intendiamo far puntare a questo Dispatcher.  L’esempio di dominio che utilizzeremo is we-retail.adobe.com.  Per prima cosa copieremo un file di configurazione esistente in un nuovo file in cui possiamo aggiungere le modifiche:

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Abbiamo copiato il file esistente aem_publish.vhost perché contiene già quello di cui abbiamo bisogno e non è necessario ricreare tutto dall’inizio.  Ora è possibile modificare il nuovo file weretail.vhost e apportare le modifiche necessarie.

Prima:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Dopo:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Ora abbiamo aggiornato il nostro `ServerName` e `ServerAlias` per corrispondere ai nuovi nomi di dominio, nonché per aggiornare altre intestazioni di breadcrumb.  Ora attiviamo il nostro nuovo file per consentire ad Apache di sapere come utilizzarlo:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Ora il server web Apache sa che il dominio è qualcosa per cui dovrebbe produrre traffico, ma dobbiamo ancora informare il modulo Dispatcher che ha un nuovo nome di dominio da rispettare.  Inizieremo creando un nuovo `*_vhost.any` file `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` e all&#39;interno di quel file inseriremo il nome di dominio che vogliamo onorare:

```
"we-retail.adobe.com"
```

Ora dobbiamo creare un nuovo file farm che utilizzerà il nuovo file di accesso vhost e inizieremo copiando un file di avvio sicuro sul nostro nuovo file.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Mostra le modifiche da apportare al file farm

Prima:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

Dopo:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

Ora abbiamo aggiornato il nome della farm e l’inclusione che utilizza nel `/virtualhosts` sezione della configurazione farm.  È necessario abilitare questo nuovo file farm in modo che possa essere utilizzato nella configurazione in esecuzione:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Ora dovremmo semplicemente ricaricare il servizio server web e utilizzare il nostro nuovo dominio!

>[!NOTE]
>
>Da notare che abbiamo cambiato solo le parti necessarie e abbiamo sfruttato le inclusioni e il codice esistenti forniti con i file di configurazione della linea di base.  Dobbiamo solo delineare l&#39;elemento che dobbiamo cambiare.  Semplifica le cose e ci consente di mantenere meno codici

[Successivo -> Verifica stato del Dispatcher](./health-check.md)
