---
title: Spiegazione dei file di configurazione di Dispatcher
description: Comprendi i file di configurazione, le convenzioni di denominazione e altro ancora.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1694'
ht-degree: 0%

---

# Spiegazione dei file di configurazione

[Sommario](./overview.md)

[&lt;- Precedente: Layout dei file di base](./basic-file-layout.md)

Questo documento analizza e spiega tutti i file di configurazione distribuiti in un server Dispatcher standard fornito in Adobe Managed Services. Il loro uso, le convenzioni di denominazione, ecc...

## Convenzione di denominazione

Apache Web Server non si preoccupa effettivamente dell’estensione di un file quando esegue il targeting con un’istruzione `Include` o `IncludeOptional`.  Assegnare nomi corretti che eliminino conflitti e confusione aiuta a <b>tonnellata</b>. I nomi utilizzati descrivono l’ambito di applicazione del file per facilitarne l’utilizzo. Se tutto si chiama `.conf`, si crea molta confusione. Vogliamo evitare file ed estensioni con nomi errati.  Di seguito è riportato un elenco delle diverse estensioni di file personalizzati e convenzioni di denominazione utilizzate in un tipico Dispatcher configurato in AMS.

## File contenuti in conf.d/

| File | Destinazione file | Descrizione |
| ---- | ---------------- | ----------- |
| NOME FILE`.conf` | `/etc/httpd/conf.d/` | Un’installazione predefinita di Enterprise Linux utilizza questa estensione di file e include la cartella come posizione per ignorare le impostazioni dichiarate in httpd.conf e consentire di aggiungere funzionalità aggiuntive a livello globale in Apache. |
| NOME FILE`.vhost` | In attesa: `/etc/httpd/conf.d/available_vhosts/`<br>Attivo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><b>Nota:</b> i file .vhost non devono essere copiati nella cartella enabled_vhosts ma utilizzano i collegamenti simbolici a un percorso relativo del file available_vhosts/\*.vhost</u><br><br> | I file \*.vhost (host virtuale) sono `<VirtualHosts>`  voci corrispondenti ai nomi host e che consentono ad Apache di gestire ogni traffico di dominio con regole diverse. Dal file `.vhost`, verranno inclusi altri file come `rewrites`, `whitelisting`, `etc`. |
| NOME FILE`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | In `*_rewrite.rules` file sono archiviate `mod_rewrite` regole da includere e utilizzare in modo esplicito in un file `vhost` |
| NOME FILE`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` file inclusi nei file `*.vhost`. Contiene IP regex o consente regole di negazione per consentire l’inserimento di IP nella whitelist. Se si tenta di limitare la visualizzazione di un host virtuale basato su indirizzi IP, verrà generato uno di questi file e verrà incluso nel file `*.vhost` |

## File contenuti in conf.dispatcher.d/

| File | Destinazione file | Descrizione |
| --- | --- | --- |
| NOME FILE`.any` | `/etc/httpd/conf.dispatcher.d/` | Il modulo Apache di AEM Dispatcher origina le impostazioni da `*.any` file. Il file di inclusione padre predefinito è `conf.dispatcher.d/dispatcher.any` |
| NOME FILE`_farm.any` | In attesa: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Attivo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><b>Nota:</b> questi file della farm non devono essere copiati nella cartella `enabled_farms` ma utilizzano `symlinks` in un percorso relativo al file `available_farms/*_farm.any` <br/>`*_farm.any` sono inclusi nel file `conf.dispatcher.d/dispatcher.any`. Questi file della farm padre consentono di controllare il comportamento del modulo per ogni tipo di rendering o sito Web. I file vengono creati nella directory `available_farms` e abilitati con `symlink` nella directory `enabled_farms`.  <br/>Le include automaticamente per nome dal file `dispatcher.any`.<br/><b>Previsione</b> i file della farm iniziano con `000_` per assicurarsi che vengano caricati per primi.<br><b>I file di farm personalizzati</b> devono essere caricati dopo avviando lo schema numerico in `100_` per garantire il corretto comportamento di inclusione. |
| NOME FILE`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` file inclusi nei file `conf.dispatcher.d/enabled_farms/*_farm.any`. Ogni farm dispone di un set di regole che modificano il traffico da filtrare e non da inviare ai renderer. |
| NOME FILE`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` file inclusi nei file `conf.dispatcher.d/enabled_farms/*_farm.any`. Questi file sono un elenco di nomi host o percorsi URI a cui deve corrispondere la corrispondenza BLOB per determinare quale renderer utilizzare per soddisfare tale richiesta |
| NOME FILE`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` file inclusi nei file `conf.dispatcher.d/enabled_farms/*_farm.any`. Questi file specificano quali elementi vengono memorizzati in cache e quali no |
| NOME FILE`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` file inclusi nei file `conf.dispatcher.d/enabled_farms/*_farm.any`. Specificano gli indirizzi IP autorizzati a inviare richieste di scaricamento e invalidamento. |
| NOME FILE`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` file inclusi nei file `conf.dispatcher.d/enabled_farms/*_farm.any`. Specificano le intestazioni client da trasmettere a ogni renderer. |
| NOME FILE`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` file inclusi nei file `conf.dispatcher.d/enabled_farms/*_farm.any`. Specificano le impostazioni IP, porta e timeout per ciascun renderer. Un renderer appropriato può essere un server livecycle o qualsiasi sistema AEM da cui Dispatcher può recuperare o inoltrare le richieste |

## Problemi evitati

Quando si segue la convenzione di denominazione è possibile evitare alcuni errori abbastanza facili da commettere che possono avere risultati catastrofici.  Seguiremo alcuni esempi.

### Esempio di problema

Come esempio di sito per ExampleCo sono stati creati due file di configurazione dagli sviluppatori delle configurazioni Dispatcher.

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

Se il file `vhost` viene inserito accidentalmente nella cartella `rewrites` e `rewrites file` viene inserito nella cartella `vhosts`.  Il nome file potrebbe essere stato distribuito correttamente, tuttavia Apache genererà un *ERRORE* e il problema non sarà immediatamente evidente.

<b>Questo problema si pone in genere</b>

Se i `two files` vengono scaricati nel percorso `same`, possono essere `overwrite themselves` o renderli indistinguibili, rendendo il processo di distribuzione un incubo.

<b>Le estensioni dei file sono le stesse e includono automaticamente i file proni</b>

Le estensioni dei file sono le stesse e utilizzano l&#39;estensione inclusa automaticamente che Apache `auto include` qualsiasi file `.conf` in molte delle sue cartelle predefinite.

<b>Questo problema si pone in genere</b>

Se il file vhost con estensione di `.conf` viene inserito nella cartella `/etc/httpd/conf.d/`, tenterà di caricarlo in memoria su Apache, in genere senza problemi, ma se il file delle regole di riscrittura con estensione di `.conf` viene inserito nella cartella `/etc/httpd/conf.d/`, verrà incluso automaticamente e applicato globalmente, causando risultati confusi e indesiderati.

## Risoluzione

Denomina i file in base alle operazioni eseguite e in modo sicuro fuori dallo spazio dei nomi delle regole di inclusione automatica.

Se si tratta di un nome di file host virtuale, con `.vhost` come estensione.

Se si tratta di un file di regole di riscrittura, assegnargli un nome con sito`_rewrite.rules` come suffisso ed estensione. Questa convenzione per i nomi chiarirà a quale sito è destinata e che si tratta di un set di regole di riscrittura.

Se si tratta di un file di regole della whitelist IP, denominalo `_whitelist.rules` come suffisso ed estensione. Questa convenzione per i nomi fornirà una descrizione del suo scopo e del fatto che si tratta di un set di regole di corrispondenza IP.

L’utilizzo di queste convenzioni di denominazione evita problemi, se un file viene spostato in una directory di inclusione automatica a cui non appartiene.

Ad esempio, inserire un file denominato con `.rules`, `.any` o `.vhost` nella cartella di inclusione automatica di `/etc/httpd/conf.d/` non avrebbe alcun effetto.

Se una richiesta di modifica della distribuzione riporta la dicitura &quot;distribuisci example_rewrite.rules ai Dispatcher di produzione&quot;, chi distribuisce le modifiche può già sapere che non sta aggiungendo un nuovo sito, ma sta semplicemente aggiornando le regole di riscrittura come indicato dal nome del file.

### Includi ordine

Quando si estendono funzionalità e configurazioni in Apache Webserver installato su Enterprise Linux si hanno alcuni importanti ordini di inclusione che si desidera comprendere

### La linea di base di Apache include

![La linea di base del server web Apache HTTPD include](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Come mostrato nel diagramma precedente, il file binario httpd cerca solo il file httpd.conf come file di configurazione.  Tale file contiene le seguenti istruzioni:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Include il primo livello di AMS

Quando abbiamo applicato il nostro standard abbiamo aggiunto alcuni tipi di file aggiuntivi e include.

Di seguito sono elencate le directory della linea di base di AMS e i principali livelli di inclusione
![La linea di base AMS include un file iniziale dispatcher_vhost.conf che includerà tutti i file con *.vhost dalla directory /etc/httpd/conf.d/enabled_vhosts/.  Gli elementi nella directory /etc/httpd/conf.d/enabled_vhosts/ sono collegamenti simbolici al file di configurazione effettivo presente in /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Basandoci sulla linea di base di Apache, viene mostrato come AMS ha creato alcune cartelle aggiuntive e include di primo livello per `conf.d` cartelle, nonché directory specifiche per i moduli nidificate in `/etc/httpd/conf.dispatcher.d/`

Quando Apache viene caricato, eseguirà il pull in `/etc/httpd/conf.modules.d/02-dispatcher.conf` e il file includerà il file binario `/etc/httpd/modules/mod_dispatcher.so` nel suo stato di esecuzione.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Per utilizzare il modulo in `<VirtualHost />`, viene rilasciato un file di configurazione in `/etc/httpd/conf.d/` denominato `dispatcher_vhost.conf`, all&#39;interno del quale vengono visualizzati i parametri di base necessari per il funzionamento del modulo:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Come vedi sopra, questo include il file di livello principale `dispatcher.any` per il modulo Dispatcher per raccogliere i file di configurazione da `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Presta attenzione al contenuto di questo file:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

Il file di livello principale `dispatcher.any` include tutti i file di farm abilitati che risiedono in `/etc/httpd/conf.dispatcher.d/enabled_farms/` con il nome di file `FILENAME_farm.any`, che segue la convenzione di denominazione standard.

Più avanti nel file `dispatcher_vhost.conf` menzionato in precedenza viene inoltre eseguita un&#39;istruzione include per abilitare ogni file host virtuale abilitato che risiede in `/etc/httpd/conf.d/enabled_vhosts/` con il nome file di `FILENAME.vhost`, che segue la convenzione di denominazione standard.

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

Dopo che il livello superiore include la risoluzione, hanno altre inclusioni secondarie che vale la pena menzionare.  Ecco un diagramma di alto livello su come i file farm e vhosts includono altri elementi secondari

### AMS Virtual Host include

![In questa immagine viene illustrato come un file .vhost include file da variabili, whitelist e riscrive cartelle](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Quando uno qualsiasi dei file `.vhost` della directory `/etc/httpd/conf.d/availabled_vhosts/` viene collegato alla directory `/etc/httpd/conf.d/enabled_vhosts/`, verrà utilizzato nella configurazione in esecuzione.

Nei file `.vhost` sono presenti sottoinclusioni basate sui pezzi comuni trovati.  Cose come variabili, whitelist e regole di riscrittura.

Il file `.vhost` includerà istruzioni di inclusione per ciascun file in base alla posizione in cui devono essere incluse nel file `.vhost`.  Di seguito è riportata una sintassi di esempio di un file `.vhost` come riferimento valido:

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

Come puoi vedere nell’esempio precedente, esiste un’inclusione per le variabili necessarie in questo file di configurazione che vengono successivamente utilizzate.

All&#39;interno del file `/etc/httpd/conf.d/variables/weretail.vars` è possibile vedere quali variabili sono definite:

```
Define MAIN_DOMAIN dev.weretail.com
```

È inoltre possibile visualizzare una riga che include un elenco di `_whitelist.rules` file che limitano gli utenti autorizzati a visualizzare il contenuto in base a criteri di whitelist diversi.  Esaminiamo il contenuto di uno dei file della whitelist `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

È inoltre possibile visualizzare una riga che include un set di regole di riscrittura.  Diamo un&#39;occhiata al contenuto del file `weretail_rewrite.rules`:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### La farm AMS include

![&lt;FILENAME>_farms.any includerà i file .any secondari per completare la configurazione di una farm.  In questa immagine puoi vedere che una farm includerà ogni file di sezione di livello superiore, cache, clientheaders, filtri, rendering e vhosts .any file](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Quando un file FILENAME_farm.any della directory `/etc/httpd/conf.dispatcher.d/available_farms/` viene collegato alla directory `/etc/httpd/conf.dispatcher.d/enabled_farms/`, verrà utilizzato nella configurazione in esecuzione.

I file della farm contengono sottoinclusioni basate su [sezioni di primo livello della farm](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) come cache, clientheaders, filtri, rendering e vhosts.

I file `FILENAME_farm.any` avranno istruzioni di inclusione per ciascun file in base alla posizione in cui devono essere inclusi nel file farm.  Di seguito è riportata una sintassi di esempio di un file `FILENAME_farm.any` come riferimento valido:

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

Poiché è possibile vedere ogni sezione per la farm weretail invece di avere tutta la sintassi necessaria, si utilizza invece un’istruzione &quot;include&quot;.

Vediamo la sintassi di alcuni di questi include per capire come apparirebbe ogni sotto include

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Come puoi vedere, si tratta di un nuovo elenco di nomi di dominio separati da righe che deve essere riprodotto da questa farm rispetto agli altri.

Esaminiamo ora `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Avanti -> Informazioni sulla cache](./understanding-cache.md)
