---
title: Spiegazione dei file di configurazione del Dispatcher
description: Comprendere i file di configurazione, le convenzioni di denominazione e altro ancora.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# Spiegazione dei file di configurazione

[Sommario](./overview.md)

[&lt;- Precedente: Layout dei file di base](./basic-file-layout.md)

In questo documento vengono suddivisi e illustrati ciascuno dei file di configurazione distribuiti in un server Dispatcher standard fornito in Adobe Managed Services. Il loro utilizzo, la convenzione di denominazione, ecc..

## Convenzione di denominazione

Apache Web Server non si preoccupa effettivamente dell&#39;estensione di file quando esegue il targeting con un `Include` o `IncludeOptional` istruzione.  Denominarli correttamente con nomi che eliminano conflitti e confusione aiuta un <b>tonnellata</b>. I nomi utilizzati descrivono l’ambito di applicazione del file, facilitandone la vita. Se tutto è denominato `.conf` questo diventa molto confuso. Vogliamo evitare file ed estensioni con nomi non corretti.  Di seguito è riportato un elenco delle diverse estensioni di file personalizzate e convenzioni di denominazione utilizzate in un’istanza di Dispatcher configurata da AMS tipica.

## File contenuti in conf.d/

| File | Destinazione file | Descrizione |
| ---- | ---------------- | ----------- |
| NOME FILE`.conf` | `/etc/httpd/conf.d/` | Un&#39;installazione predefinita di Enterprise Linux utilizza questa estensione di file e include come posizione per sostituire le impostazioni dichiarate in httpd.conf e consente di aggiungere funzionalità aggiuntive a livello globale in Apache. |
| NOME FILE`.vhost` | Staging: `/etc/httpd/conf.d/available_vhosts/`<br>Attivo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b> I file .vhost non devono essere copiati nella cartella enabled_vhosts, ma usa collegamenti simbolici a un percorso relativo al file available_vhosts/\*.vhost</div></u><br><br> | I file \*.vhost (Virtual Host) sono `<VirtualHosts>`  voci per far corrispondere i nomi host e consentire ad Apache di gestire il traffico di ogni dominio con regole diverse. Da `.vhost` file, altri file come `rewrites`, `whitelisting`, `etc` sarà incluso. |
| NOME FILE`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` archivio file `mod_rewrite` regole da includere e utilizzare esplicitamente da un `vhost` file |
| NOME FILE`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` i file sono inclusi dall&#39;interno del `*.vhost` file. Contiene regex IP o consente regole di negazione per consentire l’inserimento di IP nella whitelist. Se tenti di limitare la visualizzazione di un host virtuale basato su indirizzi IP, genererai uno di questi file e includerlo dal tuo `*.vhost` file |

## File contenuti in conf.modules.d/

| File | Destinazione file | Descrizione |
| --- | --- | --- |
| NOME FILE`.any` | `/etc/httpd/conf.dispatcher.d/` | Il modulo AEM Dispatcher Apache le origini delle impostazioni sono `*.any` file. Il file di inclusione padre predefinito è `conf.dispatcher.d/dispatcher.any` |
| NOME FILE`_farm.any` | Staging: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Attivo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b> questi file di farm non devono essere copiati nel `enabled_farms` ma utilizza `symlinks` a un percorso relativo `available_farms/*_farm.any` file </div> <br/>`*_farm.any` i file sono inclusi all&#39;interno del `conf.dispatcher.d/dispatcher.any` file. Questi file farm padre esistono per controllare il comportamento del modulo per ciascun rendering o tipo di sito web. I file vengono creati nella `available_farms` e abilitato con un `symlink` nel `enabled_farms` directory.  <br/>Li include automaticamente per nome dal `dispatcher.any` file.<br/><b>Linea</b> i file di farm iniziano con `000_` per assicurarti che siano caricati per primi.<br><b>Personalizzato</b> i file di farm devono essere caricati dopo avviando lo schema del numero a `100_` per garantire il comportamento corretto di inclusione. |
| NOME FILE`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` i file sono inclusi dall&#39;interno del `conf.dispatcher.d/enabled_farms/*_farm.any` file. Ogni farm dispone di un set di regole che cambiano il traffico da filtrare e non da indirizzare ai render. |
| NOME FILE`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` i file sono inclusi dall&#39;interno del `conf.dispatcher.d/enabled_farms/*_farm.any` file. Questi file sono un elenco di nomi host o percorsi URI che devono essere confrontati con corrispondenze blob per determinare quale renderer utilizzare per elaborare la richiesta |
| NOME FILE`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` i file sono inclusi dall&#39;interno del `conf.dispatcher.d/enabled_farms/*_farm.any` file. Questi file specificano quali elementi sono memorizzati nella cache e quali no |
| NOME FILE`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` i file sono inclusi all&#39;interno del `conf.dispatcher.d/enabled_farms/*_farm.any` file. Specificano quali indirizzi IP sono autorizzati a inviare richieste di scaricamento e di annullamento della validità. |
| NOME FILE`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` i file sono inclusi all&#39;interno del `conf.dispatcher.d/enabled_farms/*_farm.any` file. Specificano quali intestazioni client devono essere trasmesse a ogni renderer. |
| NOME FILE`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` i file sono inclusi all&#39;interno del `conf.dispatcher.d/enabled_farms/*_farm.any` file. Specificano le impostazioni di IP, porta e timeout per ciascun renderer. Un modulo di rendering appropriato può essere un server livecycle o qualsiasi sistema AEM in cui Dispatcher può recuperare/proxy le richieste da |

## Problemi evitati

Seguendo la convenzione di denominazione è possibile evitare alcuni errori abbastanza semplici da commettere che possono avere risultati catastrofici.  Vi presenteremo alcuni esempi.

### Esempio di problema

Come esempio del sito per ExampleCo, gli sviluppatori delle configurazioni del Dispatcher hanno creato due file di configurazione.

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

Se la `vhost` viene accidentalmente inserito nel `rewrites` e `rewrites file` viene inserito nella `vhosts` cartella.  Sembrerebbe distribuito correttamente dal nome del file, ma Apache lancerà un *ERRORE* e il problema non sarà immediatamente evidente.

<b>In che modo questo diventa generalmente un problema</b>

Se la `two files` vengono scaricati nel `same` posizione in cui è possibile `overwrite themselves` o renderlo indistinguibile rendendo il processo di installazione un incubo.

<b>Le estensioni dei file sono le stesse e rischiano l&#39;inclusione automatica</b>

Le estensioni dei file sono le stesse e utilizzano l’estensione con inclusione automatica che Apache eseguirà `auto include` qualsiasi `.conf` in molte delle cartelle predefinite.

<b>In che modo questo diventa generalmente un problema</b>

Se il file vhost con estensione `.conf` è inserito nella `/etc/httpd/conf.d/` la cartella proverà a caricarla in memoria su Apache, che è tipicamente ok, ma se il file delle regole di riscrittura con l&#39;estensione di `.conf` viene posizionato in `/etc/httpd/conf.d/` viene incluso automaticamente e applicato globalmente causando risultati confusi e indesiderati.

## Risoluzione

Denomina i file in base alle loro operazioni e in modo sicuro fuori dallo spazio dei nomi delle regole di inclusione automatica.

Se si tratta di un nome di file host virtuale con `.vhost` come estensione.

Se si tratta di un file di regole di riscrittura, denominalo con il sito`_rewrite.rules` come suffisso ed estensione. Questa convenzione di denominazione chiarirà per quale sito si tratta e che si tratta di un insieme di regole di riscrittura.

Se si tratta di un file di regola della whitelist IP, denominalo description`_whitelist.rules` come suffisso ed estensione. Questa convenzione di denominazione fornisce una descrizione delle funzioni e di un insieme di regole di corrispondenza IP.

L&#39;utilizzo di queste convenzioni di denominazione evita problemi, se un file viene spostato in una directory di inclusione automatica che non appartiene.

Ad esempio, per inserire un file denominato con `.rules`, `.any`oppure `.vhost` nella cartella auto-include di `/etc/httpd/conf.d/` non avrebbe alcun effetto.

Se una richiesta di modifica della distribuzione riporta &quot;distribuire exampleco_rewrite.rules ai Dispatcher di produzione&quot;, la persona che distribuisce le modifiche può già sapere che non sta aggiungendo un nuovo sito, sta solo aggiornando le regole di riscrittura come indicato dal nome del file.

### Ordine di inclusione

Quando si estendono funzionalità e configurazioni in Apache Webserver installato su Enterprise Linux si dispone di alcuni importanti ordini di inclusione che si desidera capire

### La linea di base Apache include

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Come mostrato nel diagramma sopra il binario httpd cerca solo il file httpd.conf come file di configurazione.  Tale file contiene le seguenti istruzioni:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Include di livello superiore AMS

Quando abbiamo applicato il nostro standard, abbiamo aggiunto alcuni tipi di file aggiuntivi e abbiamo aggiunto alcuni dei nostri.

Di seguito sono riportate le directory della linea di base AMS e gli include di livello superiore
![La linea di base AMS include inizia con un dispatcher_vhost.conf che includerà qualsiasi file con *.vhost dalla directory /etc/httpd/conf.d/enabled_vhosts/ .  Gli elementi nella directory /etc/httpd/conf.d/enabled_vhosts/ sono collegamenti simbolici al file di configurazione effettivo presente in /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Partendo dalla linea di base di Apache mostriamo come AMS ha creato alcune cartelle aggiuntive e include di livello superiore per `conf.d` cartelle e directory specifiche del modulo nidificate in `/etc/httpd/conf.dispatcher.d/`

Quando Apache carica, effettua il pull in `/etc/httpd/conf.modules.d/02-dispatcher.conf` e quel file includerà il file binario `/etc/httpd/modules/mod_dispatcher.so` lo stato in cui è in esecuzione.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Per utilizzare il modulo nel nostro `<VirtualHost />` rilasciamo un file di configurazione in `/etc/httpd/conf.d/` denominato `dispatcher_vhost.conf` e all&#39;interno di questo file vedrai come impostare i parametri di base necessari per il funzionamento del modulo:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Come puoi vedere sopra, questo include il livello superiore `dispatcher.any` file per il nostro modulo Dispatcher per raccogliere i file di configurazione da `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Presta attenzione al contenuto di questo file:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

Livello superiore `dispatcher.any` il file include tutti i file farm abilitati presenti in `/etc/httpd/conf.dispatcher.d/enabled_farms/` con il nome file di `FILENAME_farm.any` conforme alla convenzione di denominazione standard.

Più tardi nella `dispatcher_vhost.conf` file menzionato in precedenza facciamo anche un&#39;istruzione include per abilitare ogni file host virtuale abilitato in `/etc/httpd/conf.d/enabled_vhosts/` con il nome del file `FILENAME.vhost` conforme alla convenzione di denominazione standard.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

In ciascuno dei nostri file .vhost noterai che il modulo Dispatcher viene inizializzato come gestore di file predefinito per una directory.  Ecco un esempio di file .vhost per mostrare la sintassi:

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

Una volta che gli &quot;include di livello superiore&quot; risolvono, vale la pena citare altri &quot;sotto-include&quot;.  Ecco un diagramma di alto livello su come i file farm e vhosts includono altri sottoelementi

### Include host virtuali AMS

![Questa immagine mostra come un file .vhost include file da variabili, elenchi consentiti e cartelle di riscrittura](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Quando uno qualsiasi `.vhost` file da `/etc/httpd/conf.d/availabled_vhosts/` la directory viene collegata in modo simbolico `/etc/httpd/conf.d/enabled_vhosts/` verranno utilizzati nella configurazione in esecuzione.

La `.vhost` i file hanno sotto-include in base a parti comuni che abbiamo trovato.  Cose come variabili, whitelist e regole di riscrittura.

La `.vhost` il file avrà istruzioni &quot;include&quot; per ogni file in base a dove deve essere incluso nel `.vhost` file.  Esempio di sintassi di un `.vhost` file come buon riferimento:

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

Come puoi vedere nell’esempio precedente c’è un include per le variabili necessarie in questo file di configurazione che vengono utilizzate in seguito.

All’interno del file `/etc/httpd/conf.d/variables/weretail.vars` possiamo vedere quali variabili sono definite:

```
Define MAIN_DOMAIN dev.weretail.com
```

È inoltre possibile visualizzare una riga che include un elenco `_whitelist.rules` file che limitano gli utenti in grado di visualizzare il contenuto in base a diversi criteri di elenchi consentiti.  Diamo un&#39;occhiata al contenuto di uno dei file della whitelist `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

Puoi anche visualizzare una riga che include un set di regole di riscrittura.  Diamo un&#39;occhiata ai contenuti del `weretail_rewrite.rules` file:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### Include farm AMS

![<FILENAME>_farms.any includerà i file sub.any per completare una configurazione farm.  In questa immagine puoi vedere che una farm includerà ogni file di sezione di primo livello cache, intestazioni client, filtri, rendering e vhosts .any](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

In caso di file FILENAME_farm.any da `/etc/httpd/conf.dispatcher.d/available_farms/` la directory viene collegata in modo simbolico `/etc/httpd/conf.dispatcher.d/enabled_farms/` verranno utilizzati nella configurazione in esecuzione.

I file di farm dispongono di sotto-elementi in base a [sezioni principali dell&#39;azienda](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#defining-farms-farms) come cache, intestazioni client, filtri, render e vhosts.

La `FILENAME_farm.any` I file avranno istruzioni di inclusione per ogni file in base a dove devono essere inclusi nel file farm.  Esempio di sintassi di un `FILENAME_farm.any` file come buon riferimento:

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

Come puoi vedere ogni sezione per la farm weretail invece di avere tutta la sintassi necessaria, utilizza invece un’istruzione &quot;include&quot;.

Diamo un&#39;occhiata alla sintassi di alcune di queste &quot;include&quot; per avere l&#39;idea di come apparirebbe ogni sottoinclusa

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Come puoi vedere, si tratta di un nuovo elenco separato da righe di nomi di dominio che deve essere eseguito il rendering da questa farm sugli altri.

Ora diamo un&#39;occhiata al `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Successivo -> Informazioni sulla cache](./understanding-cache.md)