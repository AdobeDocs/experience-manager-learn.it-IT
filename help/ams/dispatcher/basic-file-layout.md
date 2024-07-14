---
title: Layout dei file di base di AMS Dispatcher
description: Comprendere il layout di base dei file Apache e Dispatcher.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 308
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 0%

---

# Layout dei file di base

[Sommario](./overview.md)

[&lt;- Precedente: Che cos&#39;è &quot;Il Dispatcher&quot;](./what-is-the-dispatcher.md)

Questo documento illustra il set di file di configurazione standard AMS e il concetto alla base di questo standard di configurazione

## Struttura predefinita delle cartelle di Enterprise Linux

In AMS l&#39;installazione di base utilizza Enterprise Linux come sistema operativo di base. Durante l’installazione di Apache Webserver, è presente un set di file di installazione predefinito. Di seguito sono riportati i file predefiniti che vengono installati installando i RPM di base forniti dall&#39;archivio YUM

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

Seguendo e rispettando la progettazione/struttura dell&#39;installazione, otteniamo i seguenti vantaggi:

- Supporto più semplice di un layout prevedibile
- Automaticamente familiare a tutti coloro che hanno lavorato sulle installazioni HTTPD di Enterprise Linux in passato
- Consente cicli di patch completamente supportati dal sistema operativo senza conflitti o regolazioni manuali
- Evita violazioni SELinux di contesti di file con etichetta errata

>[!BEGINSHADEBOX &quot;Nota&quot;]

Le immagini dei server Managed Services di Adobe hanno in genere unità radice del sistema operativo di piccole dimensioni.  I dati vengono inseriti in un volume separato, in genere montato in `/mnt`
Quindi utilizziamo quel volume invece dei valori predefiniti per le seguenti directory predefinite

`DocumentRoot`
- Predefinito:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Predefinito: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Tieni presente che le directory vecchia e nuova sono mappate al punto di montaggio originale per evitare confusione.
L&#39;utilizzo di un volume separato non è fondamentale, ma è importante

>[!ENDSHADEBOX]

## Componenti aggiuntivi AMS

AMS aggiunge all’installazione di base del server web Apache.

### Directory principali documenti

Directory principali documenti predefinite AMS:
- Autore:
   - `/mnt/var/www/author/`
- Publish:
   - `/mnt/var/www/html/`
- Manutenzione di Catch-All e Health Check
   - `/mnt/var/www/default/`

### Directory VirtualHost abilitate e di gestione temporanea

Le directory seguenti consentono di creare file di configurazione con un&#39;area di gestione temporanea che è possibile utilizzare per i file e attivarli solo quando sono pronti.
- `/etc/httpd/conf.d/available_vhosts/`
   - Questa cartella ospita tutti i file VirtualHost o denominati `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Quando si è pronti a utilizzare i file `.vhost`, nella cartella `available_vhosts` è presente un collegamento simbolico utilizzando un percorso relativo nella directory `enabled_vhosts`

### Altre `conf.d` directory

Esistono altri elementi comuni nelle configurazioni di Apache e abbiamo creato sottodirectory per consentire di separare in modo ordinato tali file e non avere tutti i file in una directory

#### Riscrive la directory

Questa directory può contenere tutti i `_rewrite.rules` file creati che contengono la tipica RewriteRulesyntax che coinvolge il modulo [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) dei server web Apache

- `/etc/httpd/conf.d/rewrites/`

#### Directory whitelists

Questa directory può contenere tutti i `_whitelist.rules` file creati che contengono la tipica sintassi `IP Allow` o `Require IP`che coinvolge i server web Apache [controlli di accesso](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Directory variabili

Questa directory può contenere tutti i `.vars` file creati che contengono variabili che è possibile utilizzare nei file di configurazione

- `/etc/httpd/conf.d/variables/`

### Directory di configurazione specifica per il modulo Dispatcher

Il server web Apache è molto estensibile e, quando un modulo dispone di molti file di configurazione, è consigliabile creare una directory di configurazione personalizzata nella directory della base di installazione, invece di riempire quella predefinita.

Seguiamo la best practice e ne creiamo una nostra

#### Directory del file di configurazione del modulo

- `/etc/httpd/conf.dispatcher.d/`

#### Staging e farm abilitata

Le directory seguenti consentono di creare file di configurazione con un&#39;area di gestione temporanea che è possibile utilizzare per i file e attivarli solo quando sono pronti.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Questa cartella ospita tutti i file `/myfarm {` denominati `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Quando si è pronti a utilizzare il file farm, all&#39;interno della cartella available_farms è presente un collegamento simbolico utilizzando un percorso relativo nella directory enabled_farms

### Altre `conf.dispatcher.d` directory

Sono disponibili parti aggiuntive che costituiscono sottosezioni delle configurazioni dei file di farm di Dispatcher. Abbiamo creato sottodirectory per consentire di separare correttamente tali file e non disporre di tutti i file in una directory

#### Directory cache

Questa directory contiene tutti i `_cache.any`, `_invalidate.any` file creati che contengono le regole su come si desidera che il modulo gestisca gli elementi di caching provenienti da AEM, nonché la sintassi delle regole di invalidazione.  Ulteriori dettagli su questa sezione sono disponibili qui [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Directory intestazioni client

Questa directory può contenere tutti i file `_clientheaders.any` creati che contengono gli elenchi delle intestazioni client da passare all&#39;AEM quando arriva una richiesta.  Ulteriori dettagli su questa sezione sono [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Directory filtri

Questa directory può contenere tutti i file `_filters.any` creati che contengono tutte le regole del filtro per bloccare o consentire il traffico attraverso Dispatcher per raggiungere l&#39;AEM

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Directory dei rendering

Questa directory può contenere tutti i file `_renders.any` creati che contengono i dettagli di connettività a ciascun server backend da cui Dispatcher utilizzerà il contenuto

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Directory Vhosts

Questa directory può contenere tutti i file `_vhosts.any` creati che contengono un elenco dei nomi di dominio e dei percorsi da associare a una particolare farm a un particolare server back-end

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Struttura completa delle cartelle

AMS ha strutturato ciascuno dei file con estensioni di file personalizzate con l’intenzione di evitare problemi/conflitti di spazio dei nomi e qualsiasi confusione.

Di seguito è riportato un esempio di un set di file standard da una distribuzione predefinita di AMS:

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## La scelta ideale

Enterprise Linux dispone di cicli di patch per il pacchetto Apache Webserver (httpd).

I file predefiniti meno installati si modificano meglio, per motivi che se ci sono correzioni di sicurezza o miglioramenti di configurazione sono applicati tramite il comando RPM / Yum non applicherà le correzioni sopra un file alterato.

Viene invece creato un file `.rpmnew` accanto all&#39;originale.  In questo modo, potresti perdere alcune modifiche desiderate e creare più oggetti inattivi nelle cartelle di configurazione.

ovvero l&#39;RPM durante l&#39;installazione dell&#39;aggiornamento esaminerà `httpd.conf` se è nello stato `unaltered`, *sostituirà* il file e si otterranno gli aggiornamenti vitali.  Se `httpd.conf` era `altered`, *non sostituirà* il file, ma creerà un file di riferimento denominato `httpd.conf.rpmnew` e le numerose correzioni desiderate si troveranno in tale file che non si applica all&#39;avvio del servizio.

Enterprise Linux è stato configurato correttamente per gestire questo caso d’uso in modo migliore.  Offrono aree in cui è possibile estendere o ignorare i valori predefiniti impostati.  All&#39;interno dell&#39;installazione di base di httpd si troverà il file `/etc/httpd/conf/httpd.conf` con la seguente sintassi:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

L&#39;idea è che Apache desideri estendere i moduli e le configurazioni aggiungendo nuovi file alle directory `/etc/httpd/conf.d/` e `/etc/httpd/conf.modules.d/` con estensione di file di `.conf`

Come esempio perfetto quando si aggiunge il modulo Dispatcher ad Apache si crea un file modulo `.so` in ` /etc/httpd/modules/` e quindi lo si include aggiungendo un file in `/etc/httpd/conf.modules.d/02-dispatcher.conf` con il contenuto per caricare il file modulo `.so`

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

>[!NOTE]
>
>Non sono stati modificati i file già esistenti forniti da Apache. Invece, abbiamo aggiunto la nostra alle directory che avrebbero dovuto usare.

Ora il modulo viene utilizzato nel file <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> che inizializza il modulo e carica il file di configurazione iniziale specifico del modulo

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Anche in questo caso, noterete che abbiamo aggiunto file e moduli ma non modificato alcun file originale.  Questo ci offre la funzionalità desiderata e ci protegge da eventuali correzioni di patch desiderate mancanti, oltre a mantenere il massimo livello di compatibilità con ogni aggiornamento del pacchetto.

[Avanti -> Spiegazione dei file di configurazione](./explanation-config-files.md)
