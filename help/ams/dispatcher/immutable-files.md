---
title: File di sola lettura o immutabili del Dispatcher AMS
description: Scopri perché alcuni file sono di sola lettura o non modificabili e come apportare le modifiche funzionali desiderate
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 1%

---


# File di sola lettura o immutabili in AMS

[Sommario](./overview.md)

[&lt;- Precedente: Registri comuni](./common-logs.md)

## Descrizione

Questo documento descrive quali file sono bloccati e non devono essere modificati e come impostare correttamente la configurazione desiderata.

Quando AMS fornisce un sistema, implementa una configurazione di base che rende tutto funzionale e sicuro.  AMS vuole garantire che rimanga come base di funzionalità e sicurezza.  A questo scopo, alcuni file sono contrassegnati come di sola lettura e non possono essere modificati per evitare di modificarli.

Il layout non impedisce di modificarne il comportamento e di ignorare le modifiche necessarie.  Invece di modificare questi file, sovrapporrai il tuo file che sostituisce l&#39;originale.

Questo ti consente anche di essere rassicurato che quando AMS esegue il patch dei Dispatcher con le ultime correzioni e i miglioramenti di sicurezza, i file non verranno modificati.  Potrai quindi continuare a beneficiare dei miglioramenti e adottare solo le modifiche desiderate.
![Mostra una corsia da bowling con una palla che scorre lungo la corsia.  La palla ha una freccia con la parola che ti mostra.  I paraurti a scatto vengono alzati e hanno le parole file immutabili sopra di loro.](assets/immutable-files/bowling-file-immutability.png "immutabilità del file di bowling")
Come illustrato nella figura precedente, i file immutabili non ti impediscono di giocare il gioco.  Ti impediranno di farti del male e di tenerti in corsia.  Questo metodo ci permette di avere alcune caratteristiche chiave:

- Le personalizzazioni vengono gestite nei propri spazi di sicurezza
- La sovrapposizione delle modifiche personalizzate rispecchia quella dei metodi di sovrapposizione in AEM
- Le configurazioni di patch AMS possono essere eseguite senza modificare le personalizzazioni
- Il test dell&#39;installazione di base rispetto alle configurazioni personalizzate può essere fatto contemporaneamente per aiutare a discernere se i problemi sono causati da personalizzazioni o qualcos&#39;altro Quali file?


Elenco tipico dei file distribuiti con un’istanza di Dispatcher:

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

Per determinare quali file non è possibile modificare, esegui il seguente comando su un Dispatcher per vedere:

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Di seguito è riportato un esempio di risposta dei file che non possono essere modificati:

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

Le variabili consentono di apportare modifiche funzionali senza modificare personalmente i file di configurazione.  Alcuni elementi della configurazione possono essere regolati regolando i valori delle variabili.  Un esempio che possiamo evidenziare dal file `/etc/httpd/conf.d/dispatcher_vhost.conf` viene mostrato qui:

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

Scopri come la direttiva DispatcherLogLevel ha una variabile di `DISP_LOG_LEVEL` invece del valore normale che vedete qui.  Sopra quella sezione di codice vedrai anche un&#39;istruzione &quot;include&quot; in un file di variabili.  Il file della variabile `/etc/httpd/conf.d/variables/ams_default.vars` è dove vogliamo guardare dopo.  Di seguito sono elencati i contenuti del file di variabili:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

In precedenza è riportato il valore corrente di `DISP_LOG_LEVEL` variabile è `info`.  Possiamo regolarlo per tracciare o eseguire il debug, o il valore/livello del numero desiderato.  Ora ovunque controlli il livello di log si regola automaticamente.

### Metodo di sovrapposizione

Comprendere i file di livello superiore includono, perché saranno il punto di partenza per effettuare qualsiasi personalizzazione.  Per iniziare con un semplice esempio, abbiamo uno scenario in cui desideri aggiungere un nuovo nome di dominio che intendiamo puntare a questo Dispatcher.  L’esempio di dominio che utilizzeremo è we-retail.adobe.com.  Inizieremo copiando un file di configurazione esistente in un nuovo file in cui possiamo aggiungere le nostre modifiche:

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Abbiamo copiato il file aem_publish.vhost esistente perché ha già ciò che serve per far funzionare le cose e non vogliamo reinventare un inizio già forte.  Ora modifichiamo il nuovo file weretail.vhost e approviamo le modifiche necessarie.

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

Ora abbiamo aggiornato il nostro `ServerName` e `ServerAlias` per far corrispondere i nuovi nomi di dominio e per aggiornare altre intestazioni breadcrumb.  Ora attiviamo il nostro nuovo file per consentire ad Apache di sapere come usare il nostro nuovo file:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Ora il webserver Apache sa che il dominio è qualcosa per cui dovrebbe generare traffico, ma dobbiamo ancora informare il modulo Dispatcher che ha un nuovo nome di dominio da onorare.  Inizieremo creando un nuovo `*_vhost.any` file `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` e all&#39;interno di quel file inseriremo il nome di dominio che vogliamo onorare:

```
"we-retail.adobe.com"
```

Ora dobbiamo creare un nuovo file farm che userà il nostro nuovo file di ingresso vhost, e inizieremo copiando un file di avvio forte sul nostro nuovo.

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

Ora abbiamo aggiornato il nome della farm e l’inclusione utilizzata nella `/virtualhosts` sezione della configurazione farm.  È necessario abilitare questo nuovo file farm in modo che possa utilizzarlo nella configurazione in esecuzione:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Ora ricaricheremmo il servizio server web e useremmo il nostro nuovo dominio!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Notate che abbiamo cambiato solo le parti necessarie per cambiare e sfruttare le inclusioni e il codice esistenti che sono forniti con i file di configurazione della linea di base.  Dobbiamo solo delineare l&#39;elemento che dobbiamo cambiare.  Semplifica le cose e ci permette di mantenere meno codice
</div>