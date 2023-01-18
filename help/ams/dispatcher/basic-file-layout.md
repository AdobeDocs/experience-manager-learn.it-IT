---
title: Layout dei file di base del Dispatcher AMS
description: Comprendere il layout di base dei file Apache e Dispatcher.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 1%

---


# Layout dei file di base

[Sommario](./overview.md)

[&lt;- Precedente: Cos’è &quot;Il Dispatcher&quot;](./what-is-the-dispatcher.md)

In questo documento viene illustrato il set di file di configurazione standard AMS e il modo di pensare alla base di questo standard di configurazione

## Struttura predefinita della cartella Enterprise Linux

In AMS l’installazione di base utilizza Enterprise Linux come sistema operativo di base. Quando installi Apache Webserver, ha un set di file di installazione predefinito. Di seguito sono elencati i file predefiniti che vengono installati installando gli RPM di base forniti dall’archivio yum

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

Seguendo e rispettando la progettazione/struttura dell&#39;installazione otteniamo i seguenti vantaggi:

- Maggiore semplicità nel supporto di un layout prevedibile
- Conosciuto automaticamente da chiunque abbia lavorato sulle installazioni HTTPD di Enterprise Linux in passato
- Permette cicli di patch completamente supportati dal sistema operativo senza conflitti o regolazioni manuali
- Evita violazioni SELinux di contesti di file etichettati erroneamente

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Le immagini dei server Adobe Managed Services dispongono in genere di piccole unità radice del sistema operativo.  Mettiamo i nostri dati in un volume separato che è tipicamente montato in `/mnt` Poi usiamo quel volume invece dei valori predefiniti per le seguenti directory predefinite

`DocumentRoot`
- Predefiniti:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Predefiniti: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Tieni presente che le directory vecchie e nuove sono mappate al punto di montaggio originale per evitare confusione.
L&#39;utilizzo di un volume separato non è vitale ma è degno di nota
</div>

## Componenti aggiuntivi AMS

AMS aggiunge all’installazione di base di Apache Web Server.

### Radici documenti

Directory documento predefinite AMS:
- Autore:
   - `/mnt/var/www/author/`
- Pubblicazione:
   - `/mnt/var/www/html/`
- Manutenzione catch-all e del controllo dello stato di salute
   - `/mnt/var/www/default/`

### Staging e directory VirtualHost abilitati

Le seguenti directory ti consentono di creare file di configurazione con un’area di gestione temporanea che puoi utilizzare sui file e abilitarli solo quando sono pronti.
- `/etc/httpd/conf.d/available_vhosts/`
   - Questa cartella ospita tutti i file VirtualHost/denominati `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Quando sei pronto a utilizzare il `.vhost` file, hai `available_vhosts` collegarli a una cartella tramite un percorso relativo `enabled_vhosts` directory

### Aggiuntivo `conf.d` Directory

Ci sono altre parti comuni nelle configurazioni di Apache e abbiamo creato delle sottodirectory per consentire un modo pulito di separare quei file e non avere tutti i file in un&#39;unica directory

#### Directory di riscrittura

Questa directory può contenere tutte le `_rewrite.rules` file creati che contengono la tipica rewriteRulesyntax che coinvolge i server web Apache [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) modulo

- `/etc/httpd/conf.d/rewrites/`

#### Directory degli elenchi consentiti

Questa directory può contenere tutte le `_whitelist.rules` file creati che contengono i `IP Allow` o `Require IP`sintassi che coinvolga i server web Apache [controlli di accesso](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Directory delle variabili

Questa directory può contenere tutte le `.vars` file creati che contengono variabili utilizzabili nei file di configurazione

- `/etc/httpd/conf.d/variables/`

### Directory di configurazione specifica per il modulo Dispatcher

Apache Web Server è molto estensibile e quando un modulo ha molti file di configurazione, è consigliabile creare la propria directory di configurazione sotto la directory di base di installazione invece di eseguire il cluttering di quella predefinita.

Seguiamo le best practice e creiamo la nostra

#### Directory dei file di configurazione del modulo

- `/etc/httpd/conf.dispatcher.d/`

#### Staging e farm abilitata

Le seguenti directory ti consentono di creare file di configurazione con un’area di gestione temporanea che puoi utilizzare sui file e abilitarli solo quando sono pronti.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Questa cartella ospita tutti i `/myfarm {` file denominati `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Quando sei pronto a utilizzare il file farm, all’interno della cartella available_farms puoi collegarli utilizzando un percorso relativo nella directory enabled_farms

### Aggiuntivo `conf.dispatcher.d` Directory

Ci sono altre parti che sono sottosezioni delle configurazioni dei file farm di Dispatcher e abbiamo creato delle sottodirectory per consentire un modo pulito di separare quei file e non avere tutti i file in un&#39;unica directory

#### Directory cache

Questa directory contiene tutti i `_cache.any`, `_invalidate.any` file creati che contengono le regole su come si desidera che il modulo gestisca gli elementi di memorizzazione in cache provenienti da AEM e la sintassi delle regole di invalidazione.  Maggiori dettagli su questa sezione sono disponibili qui [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Directory delle intestazioni client

Questa directory può contenere tutte le `_clientheaders.any` file creati che contengono elenchi di intestazioni client da passare a AEM quando arriva una richiesta.  Maggiori dettagli su questa sezione sono [qui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Directory dei filtri

Questa directory può contenere tutte le `_filters.any` file creati che contengono tutte le regole di filtro per bloccare o consentire al traffico attraverso Dispatcher di raggiungere AEM

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Directory dei render

Questa directory può contenere tutte le `_renders.any` file creati che contengono i dettagli di connettività a ogni server back-end da cui il dispatcher utilizzerà il contenuto

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Directory Vhosts

Questa directory può contenere tutte le `_vhosts.any` file creati che contengono un elenco dei nomi di dominio e dei percorsi da abbinare a una particolare farm in un particolare server back-end

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Struttura completa delle cartelle

AMS ha strutturato ciascuno dei file con estensioni di file personalizzate e con l’intento di evitare problemi di spazio dei nomi/conflitti e confusione.

Ecco un esempio di set di file standard da una distribuzione predefinita AMS:

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

## Mantenere la soluzione ideale

Enterprise Linux ha cicli di patch per il pacchetto Apache Webserver (httpd).

Minore è il numero di file predefiniti installati che vengono modificati meglio è, per motivi che se ci sono correzioni di sicurezza patch o miglioramenti di configurazione vengono applicati tramite il comando RPM / Yum non applicherà le correzioni sopra un file modificato.

Invece crea un `.rpmnew` accanto all&#39;originale.  Questo significa che perderai alcune modifiche che avresti voluto e creato più rifiuti nelle cartelle di configurazione.

Ad esempio, gli RPM durante l’installazione dell’aggiornamento esamineranno `httpd.conf` se è nel `unaltered` dichiarare *replace* il file e otterrai gli aggiornamenti vitali.  Se la `httpd.conf` era `altered` allora *non sostituirà* il file e invece creerà un file di riferimento denominato `httpd.conf.rpmnew` e le molte correzioni desiderate saranno in quel file che non si applica all&#39;avvio del servizio.

Enterprise Linux è stato configurato correttamente per gestire questo caso d&#39;uso in modo migliore.  Forniscono aree in cui puoi estendere o sovrascrivere i valori predefiniti impostati per te.  All&#39;interno dell&#39;installazione di base di httpd troverai il file `/etc/httpd/conf/httpd.conf`e presenta una sintassi simile a:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

L’idea è che Apache desideri estendere i moduli e le configurazioni nell’aggiunta di nuovi file al `/etc/httpd/conf.d/` e `/etc/httpd/conf.modules.d/` directory con estensione di file `.conf`

Come esempio perfetto quando aggiungi il modulo Dispatcher ad Apache, crei un modulo `.so` in ` /etc/httpd/modules/` e quindi includerlo aggiungendo un file in `/etc/httpd/conf.modules.d/02-dispatcher.conf` con il contenuto per caricare il modulo `.so` file

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Avviso:</b>
non abbiamo modificato alcun file già esistente fornito da Apache.  Invece abbiamo semplicemente aggiunto le nostre alle directory che dovevano seguire.
</div><br/>

Ora utilizziamo il modulo nel file . <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> che inizializza il modulo e carica il file di configurazione iniziale specifico del modulo

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Noterai che abbiamo aggiunto file e moduli ma non abbiamo modificato alcun file originale.  Questo ci offre la funzionalità desiderata e ci protegge dalle correzioni di patch desiderate mancanti, oltre a mantenere il più alto livello di compatibilità con ogni aggiornamento del pacchetto.

[Avanti -> Spiegazione dei file di configurazione](./explanation-config-files.md)