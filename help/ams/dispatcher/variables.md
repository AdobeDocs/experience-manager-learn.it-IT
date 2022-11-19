---
title: Utilizzo e comprensione delle variabili nella configurazione del Dispatcher AEM
description: Scopri come utilizzare le variabili nei file di configurazione dei moduli Apache e Dispatcher per portarle al livello successivo.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# Uso e nozioni di base sulle variabili

[Sommario](./overview.md)

[&lt;- Precedente: Informazioni sulla cache](./understanding-cache.md)

Questo documento illustra come sfruttare la potenza delle variabili nel server web Apache e nei file di configurazione del modulo Dispatcher.

## Variabili

Apache supporta le variabili e dalla versione 4.1.9 del modulo Dispatcher le supporta anche!

Possiamo sfruttarli per fare una serie di cose utili come:

- Assicurati che tutto ciò che è specifico per l&#39;ambiente non sia in linea nelle configurazioni ma estratto per assicurarsi che i file di configurazione da dev funzionino in prod con lo stesso output funzionale.
- Attiva/disattiva le funzionalità e modifica i livelli di registro dei file immutabili forniti da AMS e non consente di modificare.
- Modifica che include da utilizzare in base a variabili come `RUNMODE` e `ENV_TYPE`
- Corrispondenza `DocumentRoot`e `VirtualHost` Nomi DNS tra le configurazioni Apache e le configurazioni del modulo.

## Utilizzo delle variabili della linea di base

Poiché i file della linea di base AMS sono di sola lettura e non modificabili, sono disponibili funzioni che possono essere disattivate e attivate e configurate modificando le variabili utilizzate.

### Variabili della linea di base

Le variabili predefinite AMS sono dichiarate nel file `/etc/httpd/conf.d/variables/ootb.vars`.  Questo file non è modificabile, ma esiste per assicurarti che le variabili non abbiano valori null.  Sono inclusi prima e poi dopo di che `/etc/httpd/conf.d/variables/ams_default.vars`.  Puoi modificare quel file per modificare i valori di queste variabili o anche includere gli stessi nomi e valori di variabili nel tuo file!

Ecco un esempio del contenuto del file `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Esempio 1 - Forza SSL

Variabili mostrate sopra `AUHOR_FORCE_SSL`oppure `PUBLISH_FORCE_SSL` può essere impostato su 1 per attivare le regole di riscrittura che obbligano gli utenti finali che accedono a una richiesta http a essere reindirizzati a https

Di seguito è riportata la sintassi del file di configurazione che consente il funzionamento di questo interruttore:

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

Come puoi vedere le regole di riscrittura includono ciò che ha il codice per reindirizzare il browser degli utenti finali, ma la variabile impostata su 1 è ciò che consente o meno l&#39;utilizzo del file

### Esempio 2 - Livello di registrazione

Le variabili `DISP_LOG_LEVEL` può essere utilizzato per impostare ciò che si desidera per il livello di log effettivamente utilizzato nella configurazione in esecuzione.

Di seguito è riportato un esempio di sintassi presente nei file di configurazione della linea di base ams:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Se hai bisogno di aumentare il livello di registrazione di Dispatcher, devi solo aggiornare il `ams_default.vars` variable `DISP_LOG_LEVEL` al livello desiderato.

I valori di esempio possono essere un numero intero o una parola:

| Livello registro | Valore intero | Valore Word |
| --- | --- | --- |
| Traccia | 4 | traccia |
| Debug | 3 | debug |
| Info | 2 | informazioni |
| Avvertenza | 1 | avvertire |
| Errore | 0 | errore |

### Esempio 3 - Elenchi consentiti

Le variabili `AUTHOR_WHITELIST_ENABLED` e `PUBLISH_WHITELIST_ENABLED` può essere impostato su 1 per attivare regole di riscrittura che includono regole che consentono o impediscono il traffico degli utenti finali in base all’indirizzo IP.  L’attivazione di questa funzione deve essere combinata con la creazione di un file di regole della whitelist da includere.

Di seguito sono riportati alcuni esempi di sintassi di come la variabile abilita gli &quot;include&quot; dei file della whitelist e un esempio di file della whitelist

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

Come puoi vedere la `sample_whitelist.rules` applica la restrizione IP, ma l’attivazione della variabile ne consente l’inclusione nella variabile `sample.vhost`

## Dove collocare le variabili

### Argomenti di avvio del server web

AMS inserirà nel file variabili specifiche per server/topologia negli argomenti di avvio del processo Apache `/etc/sysconfig/httpd`

Questo file ha variabili predefinite come mostrato qui:

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

Non si tratta di qualcosa che puoi modificare, ma è utile da sfruttare nei file di configurazione

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

A causa del fatto che questo file viene incluso solo all&#39;avvio del servizio.  Per rilevare le modifiche è necessario riavviare il servizio.  Ciò significa che un ricaricamento non è sufficiente, ma è necessario riavviare il sistema
</div>

### File variabili (`.vars`)

Le variabili personalizzate fornite dal codice devono essere presenti in `.vars` file all’interno della directory `/etc/httpd/conf.d/variables/`

Questi file possono avere qualsiasi variabile personalizzata desiderata e alcuni esempi di sintassi possono essere visualizzati nei seguenti file di esempio

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

Quando crei le tue variabili, denominale in base al loro contenuto e per seguire gli standard di denominazione forniti nel manuale [qui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  Nell’esempio precedente puoi vedere che il file delle variabili ospita le diverse voci DNS come variabili da utilizzare nei file di configurazione.

## Utilizzo delle variabili

Ora che hai definito le variabili all&#39;interno dei file delle variabili, vuoi sapere come usarle correttamente all&#39;interno degli altri file di configurazione.

Useremo l&#39;esempio `.vars` file dall&#39;alto per illustrare un caso d&#39;uso corretto.

Vogliamo includere globalmente tutte le variabili basate sull&#39;ambiente e creeremo il file `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Sappiamo che all’avvio del servizio httpd estrae le variabili impostate da AMS in `/etc/sysconfig/httpd` e ha l&#39;insieme di variabili `ENV_TYPE` e `RUNMODE`

Quando questo è globale `.conf` il file viene estratto sarà estratto in anticipo perché l&#39;ordine di inclusione dei file in `conf.d` è un ordine di caricamento alfanumerico significa che 000 nel nome del file assicurerà che venga caricato prima degli altri file nella directory.

L’istruzione include utilizza anche una variabile nel nome del file.  Questo può cambiare il file che verrà effettivamente caricato in base al valore presente in `ENV_TYPE` e `RUNMODE` variabili.

Se la `ENV_TYPE` value is `dev` quindi il file che viene utilizzato è:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Se la `ENV_TYPE` value is `stage` quindi il file che viene utilizzato è:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Se la `RUNMODE` value is `preview` quindi il file che viene utilizzato è:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Quando quel file viene incluso, ci permetterà di utilizzare i nomi delle variabili memorizzati all&#39;interno.

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

Queste variabili hanno una grande quantità di riutilizzo per individuare le impostazioni in esecuzione senza dover disporre di file distribuiti diversi per ambiente.  In sostanza puoi modellare i file di configurazione con l’uso di variabili e includere file basati su variabili.

## Visualizzazione dei valori variabili

A volte, quando si utilizzano variabili, è necessario cercare i valori che potrebbero essere presenti nei file di configurazione.  Esiste un modo per visualizzare le variabili risolte eseguendo i seguenti comandi sul server:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Come si presentavano le variabili nella configurazione Apache compilata:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Visualizzazione delle variabili nella configurazione del Dispatcher compilata:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

Dall&#39;output dei comandi si vedranno le differenze della variabile nel file di configurazione rispetto all&#39;output compilato.

Configurazione di esempio

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

Esegui ora i comandi per visualizzare l&#39;output compilato

Configurazione Apache compilata:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configurazione del Dispatcher compilata:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Successivo -> Scaricamento](./disp-flushing.md)