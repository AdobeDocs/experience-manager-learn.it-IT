---
title: Controllo dello stato del Dispatcher AMS
description: AMS fornisce uno script cgi-bin di controllo dello stato che i bilanciatori di caricamento cloud verranno eseguiti per vedere se AEM è sano e dovrebbe rimanere in servizio per il traffico pubblico.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: d6b7d63ba02ca73d6c1674d90db53c6eebab3bd2
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 1%

---

# Controllo dello stato del Dispatcher AMS

[Sommario](./overview.md)

[&lt;- Precedente: File di sola lettura](./immutable-files.md)

Quando si dispone di una linea di base AMS installata dispatcher, viene fornito con alcuni freebies.  Una di queste funzionalità è un insieme di script di controllo dello stato di salute.
Questi script consentono al load balancer che esegue lo stack AEM di sapere quali gambe sono sane e di mantenerle in servizio.

![GIF animato che mostra il flusso di traffico](assets/load-balancer-healthcheck/health-check.gif "Passaggi del controllo dello stato")

## Verifica stato del bilanciamento del carico di base

Quando il traffico dei clienti arriva attraverso Internet per raggiungere la tua istanza di AEM, passerà attraverso un load balancer

![L&#39;immagine mostra il flusso di traffico da Internet ad aem tramite un load balancer](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

Ogni richiesta che arriva tramite il load balancer arrotonda il robin a ogni istanza.  Il load balancer dispone di un meccanismo di controllo dello stato integrato per assicurarsi che invii traffico a un host integro.

Il controllo predefinito è in genere un controllo della porta per vedere se i server di destinazione nel load balancer sono in ascolto sul traffico della porta si accende (cioè TCP 80 e 443)

> `Note:` Mentre questo funziona non ha un vero indicatore su se AEM è sano.  Verifica solo se Dispatcher (server web Apache) è in esecuzione.

## Verifica integrità AMS

Per evitare di inviare traffico a un dispatcher sano che sta affrontando un&#39;istanza di AEM non sana, AMS ha creato alcuni extra che valutano la salute della gamba e non solo il Dispatcher.

![L&#39;immagine mostra i diversi pezzi per il controllo dello stato di salute al lavoro](assets/load-balancer-healthcheck/health-check-pieces.png "controlli sanitari")

Il controllo dello stato di salute comprende i seguenti pezzi
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

Scopriremo cosa ogni pezzo deve fare e la sua importanza

### Pacchetto AEM

Per indicare se AEM funziona, è necessario che esegua alcune operazioni di base di compilazione delle pagine e distribuisca la pagina.  Adobe Managed Services ha creato un pacchetto di base contenente la pagina di test.  La pagina verifica che l’archivio sia attivo e che sia possibile eseguire il rendering delle risorse e del modello di pagina.

![L&#39;immagine mostra il pacchetto AMS nel gestore di pacchetti CRX](assets/load-balancer-healthcheck/health-check-package.png "pacchetto di controllo sanitario")

Ecco la pagina.  Verrà visualizzato l&#39;ID del repository dell&#39;installazione

![L’immagine mostra la pagina Regent AMS](assets/load-balancer-healthcheck/health-check-page.png "pagina di verifica dello stato")

> `Note:` Ci assicuriamo che la pagina non sia memorizzabile nella cache.  Non controllerebbe lo stato effettivo se ogni volta che restituiva una pagina memorizzata nella cache.

Questo è l&#39;endpoint del peso leggero che possiamo verificare per vedere che AEM è in funzione.

### Configurazione del load balancer

Configuriamo i bilanciatori di carico in modo che puntino a un endpoint CGI-BIN invece di utilizzare un controllo di porta.

![L&#39;immagine mostra la configurazione del controllo dello stato di salute del load balancer di AWS](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![L&#39;immagine mostra la configurazione del controllo dello stato di salute del load balancer di Azure](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Verifica dello stato di Apache degli host virtuali

#### Host virtuale CGI-BIN `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

Questa è la `<VirtualHost>` File di configurazione Apache che consente l&#39;esecuzione dei file CGI-Bin.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` i file cgi-bin sono script eseguibili.  Questo può essere un vettore di attacco vulnerabile e questi script utilizzati da AMS non sono accessibili al pubblico solo dal load balancer su cui eseguire il test.


#### Host virtuali di manutenzione non sicuri

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

Questi file sono denominati `000_` come prefisso di proposito.  È configurato intenzionalmente per utilizzare lo stesso nome di dominio del sito live.  L&#39;intenzione è quella di abilitare questo file quando il controllo di integrità rileva un problema con uno dei backend AEM.  Quindi offri una pagina di errore invece di un solo codice di risposta HTTP 503 senza pagina.  Ruberà il traffico dal normale `.vhost` file perché è caricato prima di quello `.vhost` file durante la condivisione dello stesso `ServerName` o `ServerAlias`.  Il risultato è che le pagine destinate a un determinato dominio passano all’host non integro invece di quello predefinito con il traffico normale.

Quando gli script di controllo dello stato vengono eseguiti, disconnettono lo stato di integrità corrente.  Una volta al minuto c&#39;è un cronjob in esecuzione sul server che cerca voci non integre nel log.  Se rileva che l’istanza di AEM dell’autore non è integra, abilita il symlink:

Voce di registro:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron per rilevare l&#39;errore e reagire:

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

Puoi controllare se l’autore o i siti pubblicati possono caricare questa pagina di errore configurando l’impostazione della modalità di ricaricamento in `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

Opzioni valide:
- author
   - Questa è l’opzione predefinita.
   - In questo modo verrà creata una pagina di manutenzione per l’autore quando non è integro
- pubblicazione
   - Questa opzione consente di impostare una pagina di manutenzione per l’editore quando non è integra
- all
   - Questa opzione consente di impostare una pagina di manutenzione per l’autore o l’editore o per entrambi se non sono integri
- nessuno
   - Questa opzione ignora questa funzione del controllo dello stato di salute

Quando osservi `VirtualHost` per queste impostazioni verrà visualizzato che caricano lo stesso documento di una pagina di errore per ogni richiesta che viene attivata:

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

Il codice di risposta è ancora un `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

Invece di una pagina vuota, riceveranno invece questa pagina.

![L&#39;immagine mostra la pagina di manutenzione predefinita](assets/load-balancer-healthcheck/unhealthy-page.png "pagina non valida")

### Script CGI-Bin

Nelle impostazioni del load balancer è possibile configurare 5 script diversi dal CSE che modificano il comportamento o i criteri per estrarre un Dispatcher dal load balancer.

#### /bin/checkauthor

Questo script utilizzato controllerà e registrerà tutte le istanze in cui si trova in primo piano ma restituirà un errore solo se il `author` AEM&#39;istanza non è valida

> `Note:` Tieni presente che se l’istanza di pubblicazione AEM non era integra, il dispatcher resterebbe in servizio per consentire il flusso di traffico verso l’istanza di authoring AEM

#### /bin/checkpublish (predefinito)

Questo script utilizzato controllerà e registrerà tutte le istanze in cui si trova in primo piano ma restituirà un errore solo se il `publish` AEM&#39;istanza non è valida

> `Note:` Tieni presente che se l’istanza di AEM dell’autore non è integra, il dispatcher resterà in servizio per consentire il flusso del traffico verso l’istanza di AEM di pubblicazione

#### /bin/checkeyer

Questo script utilizzato controllerà e registrerà tutte le istanze in cui si trova in primo piano ma restituirà un errore solo se il `author` o `publisher` AEM&#39;istanza non è valida

> `Note:` Tieni presente che se l’istanza di pubblicazione AEM o l’istanza AEM dell’autore non era integra, il dispatcher ritirerebbe il servizio.  Ciò significa che se uno di loro fosse sano non riceverebbe traffico

#### /bin/checkboth

Questo script utilizzato controllerà e registrerà tutte le istanze in cui si trova in primo piano ma restituirà un errore solo se il `author` e `publisher` AEM&#39;istanza non è valida

> `Note:` Tieni presente che se l’istanza di pubblicazione AEM o l’istanza di AEM dell’autore non era integra, il dispatcher non ritirerà il servizio.  Ciò significa che se uno di loro non è in buona salute continuerà a ricevere traffico e a dare errori alle persone che richiedono risorse.

#### /bin/health

Questo script utilizzato controllerà e registrerà tutte le istanze in cui si trova in primo piano ma restituirà comunque integro indipendentemente dal fatto che AEM restituisca o meno un errore.

> `Note:` Questo script viene utilizzato quando il controllo di integrità non funziona come desiderato e consente a un override di mantenere AEM istanze nel load balancer.