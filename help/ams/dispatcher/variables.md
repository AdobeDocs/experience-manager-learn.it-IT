---
title: Utilizzo e comprensione delle variabili nella configurazione del Dispatcher per AEM
description: Scopri come utilizzare le variabili nei file di configurazione dei moduli Apache e Dispatcher per portarli al livello successivo.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---

# Utilizzo e nozioni di base sulle variabili

[Sommario](./overview.md)

[&lt;- Precedente: informazioni sulla cache](./understanding-cache.md)

Questo documento illustra come sfruttare la potenza delle variabili nel server web Apache e nei file di configurazione del modulo Dispatcher.

## Variabili

Apache supporta le variabili e, dalla versione 4.1.9 del modulo Dispatcher, supporta anche queste.

Possiamo sfruttarli per fare un sacco di cose utili come:

- Assicurati che tutto ciò che è specifico per l’ambiente non sia in linea nelle configurazioni, ma estratto per garantire che i file di configurazione da sviluppo funzionino nella produzione con lo stesso output funzionale.
- Attiva/disattiva le funzioni e modifica i livelli di registro dei file immutabili forniti da AMS e non consentirti di modificare.
- Modifica che include da utilizzare in base a variabili come `RUNMODE` e `ENV_TYPE`
- Corrispondenza `DocumentRoot`di e `VirtualHost` Nomi DNS tra le configurazioni di Apache e le configurazioni dei moduli.

## Utilizzo delle variabili della linea di base

Poiché i file della linea di base AMS sono di sola lettura e immutabili, è possibile attivare e disattivare alcune funzioni e configurarle modificando le variabili che utilizzano.

### Variabili della linea di base

Le variabili predefinite di AMS sono dichiarate nel file `/etc/httpd/conf.d/variables/ootb.vars`.  Questo file non è modificabile, ma esiste per assicurarti che le variabili non abbiano valori Null.  Vengono inclusi prima e dopo rispetto a quelli inclusi `/etc/httpd/conf.d/variables/ams_default.vars`.  Puoi modificare il file per modificare i valori di queste variabili o anche includere gli stessi nomi e valori delle variabili nel file.

Ecco un esempio del contenuto del file `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Esempio 1 - Forza SSL

Le variabili mostrate sopra `AUHOR_FORCE_SSL`, o `PUBLISH_FORCE_SSL` può essere impostato su 1 per attivare le regole di riscrittura che obbligano gli utenti finali quando arrivano su richiesta http a essere reindirizzati a https

Ecco la sintassi del file di configurazione che consente il funzionamento di questo interruttore:

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

Come puoi vedere, le regole di riscrittura includono ciò che ha il codice per reindirizzare il browser degli utenti finali, ma la variabile impostata su 1 è ciò che consente di utilizzare o meno il file

### Esempio 2 - Livello di registrazione

Le variabili `DISP_LOG_LEVEL` può essere utilizzato per impostare ciò che si desidera avere per il livello di registro effettivamente utilizzato nella configurazione in esecuzione.

Di seguito è riportato un esempio di sintassi presente nei file di configurazione della linea di base di AMS:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Per aumentare il livello di registrazione di Dispatcher, aggiorna la `ams_default.vars` variabile `DISP_LOG_LEVEL` al livello desiderato.

Esempio Valori può essere un numero intero o la parola:

| Livello registro | Valore intero | Valore Word |
| --- | --- | --- |
| Traccia | 4 | traccia |
| Debug | 3 | debug |
| Info | 2 | informazioni |
| Avvertenza | 1 | avvertenza |
| Errore | 0 | errore |

### Esempio 3 - Whitelist

Le variabili `AUTHOR_WHITELIST_ENABLED` e `PUBLISH_WHITELIST_ENABLED` può essere impostato su 1 per attivare regole di riscrittura che includono regole per consentire o impedire il traffico dell’utente finale in base all’indirizzo IP.  L’attivazione di questa funzione deve essere combinata con la creazione di un file di regole di whitelist da includere.

Di seguito sono riportati alcuni esempi di sintassi di come la variabile abilita le inclusioni dei file della whitelist e un esempio di file della whitelist

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

Come puoi vedere la `sample_whitelist.rules` applica la restrizione IP, ma l’attivazione della variabile ne consente l’inclusione nella proprietà `sample.vhost`

## Dove inserire le variabili

### Argomenti di avvio server Web

AMS inserirà variabili specifiche per server/topologia negli argomenti di avvio del processo Apache all’interno del file `/etc/sysconfig/httpd`

Questo file ha variabili predefinite, come illustrato di seguito:

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

Questi non sono elementi che puoi modificare ma che puoi sfruttare nei file di configurazione

>[!NOTE]
>
>Poiché questo file viene incluso solo all’avvio del servizio.  Per raccogliere le modifiche è necessario riavviare il servizio.  Ciò significa che un ricaricamento non è sufficiente, ma è necessario riavviare il sistema

### File di variabili (`.vars`)

Le variabili personalizzate fornite dal codice devono essere contenute in `.vars` file nella directory `/etc/httpd/conf.d/variables/`

Questi file possono contenere qualsiasi variabile personalizzata desiderata; alcuni esempi di sintassi sono disponibili nei seguenti file di esempio

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

Quando crei file di variabili, assegnagli un nome in base al loro contenuto e in base agli standard di denominazione forniti nel manuale [qui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  Nell’esempio precedente puoi vedere che il file delle variabili ospita le diverse voci DNS come variabili da utilizzare nei file di configurazione.

## Utilizzo delle variabili

Dopo aver definito le variabili nei file delle variabili, è necessario sapere come utilizzarle correttamente negli altri file di configurazione.

Utilizzeremo l’esempio `.vars` file dall’alto per illustrare un caso d’uso corretto.

Il file verrà creato per includere tutte le variabili basate sull’ambiente a livello globale `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Sappiamo che all’avvio del servizio httpd vengono richiamate le variabili impostate da AMS in `/etc/sysconfig/httpd` e ha l’insieme di variabili `ENV_TYPE` e `RUNMODE`

Quando questo valore globale `.conf` il file viene estratto verrà estratto in anticipo perché l’ordine di inclusione dei file in `conf.d` è l&#39;ordine di caricamento alfanumerico significa 000 nel nome file per garantire che venga caricato prima degli altri file nella directory.

L’istruzione include utilizza anche una variabile nel nome del file.  Questo può cambiare quale file verrà effettivamente caricato in base al valore presente in `ENV_TYPE` e `RUNMODE` variabili.

Se il `ENV_TYPE` il valore è `dev` il file che viene utilizzato è:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Se il `ENV_TYPE` il valore è `stage` il file che viene utilizzato è:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Se il `RUNMODE` il valore è `preview` il file che viene utilizzato è:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Quando quel file viene incluso ci consentirà di utilizzare i nomi delle variabili memorizzati all&#39;interno di.

Nel nostro `/etc/httpd/conf.d/available_vhosts/weretail.vhost` file possiamo cambiare la normale sintassi che funzionava solo per dev:

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Con una nuova sintassi che utilizza il potere delle variabili per funzionare per dev, stage e prod:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

Nel nostro `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` file possiamo cambiare la normale sintassi che funzionava solo per dev:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Con una nuova sintassi che utilizza il potere delle variabili per funzionare per dev, stage e prod:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Queste variabili possono essere riutilizzate per personalizzare le impostazioni in esecuzione senza dover disporre di file distribuiti diversi in base all’ambiente.  Essenzialmente puoi modellare i file di configurazione con l’utilizzo di variabili e includere file basati su variabili.

## Visualizzazione dei valori delle variabili

A volte, quando si utilizzano le variabili, è necessario eseguire una ricerca per vedere quali potrebbero essere i valori nei file di configurazione.  È possibile visualizzare le variabili risolte eseguendo i seguenti comandi sul server:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Aspetto delle variabili nella configurazione Apache compilata:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Aspetto delle variabili nella configurazione compilata di Dispatcher:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

Nell&#39;output dei comandi vengono visualizzate le differenze tra la variabile nel file di configurazione e l&#39;output compilato.

Esempio di configurazione

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

Eseguire ora i comandi per visualizzare l&#39;output compilato

Configurazione Apache compilata:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configurazione Dispatcher Compilata:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Avanti -> Svuotamento](./disp-flushing.md)
