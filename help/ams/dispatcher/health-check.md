---
title: Verifica stato del dispatcher di AMS
description: AMS fornisce uno script cgi-bin per il controllo dello stato che i load balancer cloud eseguiranno per verificare se l’AEM è integro e deve rimanere in servizio per il traffico pubblico.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 69b4e469-52cc-441b-b6e5-2fe7ef18da90
duration: 306
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 0%

---

# Verifica stato del dispatcher di AMS

[Sommario](./overview.md)

[&lt;- Precedente: File di sola lettura](./immutable-files.md)

Quando hai installato una linea di base AMS, il dispatcher è dotato di alcune versioni gratuite.  Una di queste funzionalità è un set di script di verifica stato.
Questi script consentono al load balancer che si trova di fronte allo stack AEM di sapere quali gambe sono sane e di mantenerle in servizio.

![Animated GIF che mostra il flusso di traffico](assets/load-balancer-healthcheck/health-check.gif "Passaggi della verifica stato")

## Verifica stato di base del load balancer

Quando il traffico del cliente arriva tramite Internet per raggiungere l’istanza AEM, passa attraverso un load balancer

![L’immagine mostra il flusso di traffico da Internet ad AEM tramite un load balancer](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

Ogni richiesta proveniente dal load balancer arrotonderà robin a ogni istanza.  Il load balancer dispone di un meccanismo di verifica dello stato integrato per assicurarsi che stia inviando traffico a un host integro.

Il controllo predefinito è in genere un controllo delle porte per verificare se i server di destinazione nel load balancer sono in ascolto sul traffico delle porte attivato (ad esempio, TCP 80 e 443)

> `Note:` Mentre questo funziona non ha un vero e proprio metro di valutazione se l&#39;AEM è sano.  Viene verificato solo se Dispatcher (server web Apache) è in esecuzione.

## Verifica stato AMS

Per evitare di inviare traffico a un dispatcher sano che si trova di fronte a un’istanza AEM non sana, AMS ha creato alcuni extra che valutano lo stato della gamba e non solo del Dispatcher.

![L&#39;immagine mostra i diversi elementi per il funzionamento del controllo di integrità](assets/load-balancer-healthcheck/health-check-pieces.png "guarigione - pezzi")

Il controllo dello stato di salute comprende i seguenti elementi
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

Copriremo cosa ogni pezzo è impostato per fare e la loro importanza

### Pacchetto AEM

Per indicare se l’AEM funziona è necessario che esegua alcune operazioni di base di compilazione delle pagine e distribuisca la pagina.  Adobe Managed Services ha creato un pacchetto di base che contiene la pagina di test.  La pagina verifica che l’archivio sia attivo e che sia possibile eseguire il rendering delle risorse e del modello di pagina.

![L’immagine mostra il pacchetto AMS nel gestore di pacchetti CRX](assets/load-balancer-healthcheck/health-check-package.png "health-check-package")

Ecco la pagina.  Mostrerà l’ID archivio dell’installazione

![Immagine che illustra la pagina Regent di AMS](assets/load-balancer-healthcheck/health-check-page.png "health-check-page")

> `Note:` Ci assicuriamo che la pagina non sia memorizzabile in cache.  Non controllerebbe lo stato effettivo se ogni volta che restituisse una pagina memorizzata in cache!

Questo è l&#39;endpoint &quot;leggero&quot; che possiamo testare per vedere che l&#39;AEM è funzionante.

### Configurazione del load balancer

I load balancer vengono configurati in modo da puntare a un endpoint CGI-BIN anziché utilizzare un controllo delle porte.

![L’immagine mostra la configurazione del controllo di integrità del load balancer di AWS](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![L’immagine mostra la configurazione del controllo di integrità del load balancer di Azure](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Host Virtuali Di Controllo Integrità Apache

#### Host virtuale CGI-BIN `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

Questo è il `<VirtualHost>` File di configurazione Apache che abilita l’esecuzione dei file CGI-Bin.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` i file cgi-bin sono script che possono essere eseguiti.  Questo può essere un vettore di attacco vulnerabile e questi script utilizzati da AMS non sono accessibili pubblicamente, ma sono disponibili solo per il test eseguito dal load balancer.


#### Host virtuali di manutenzione non integri

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

Questi file sono denominati `000_` come prefisso di proposito.  È configurato intenzionalmente per utilizzare lo stesso nome di dominio del sito live.  L’intenzione è quella di abilitare questo file quando il controllo di integrità rileva la presenza di un problema con uno dei backend dell’AEM.  Quindi offri una pagina di errore invece di un semplice codice di risposta HTTP 503 senza pagina.  Ruberà il traffico dal normale `.vhost` perché è stato caricato prima di `.vhost` durante la condivisione dello stesso `ServerName` o `ServerAlias`.  Il risultato è che le pagine destinate a un particolare dominio devono passare al vhost non integro invece di quello predefinito da cui normalmente passa il traffico.

Quando vengono eseguiti gli script di verifica stato, questi disconnettono lo stato di integrità corrente.  Una volta al minuto sul server è in esecuzione un processo cronologico che cerca voci non integre nel registro.  Se rileva che l’istanza AEM dell’autore non è integra, abilita il collegamento simbolico:

Voce registro:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron raccoglie l’errore e reagisce:

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

Puoi controllare se i siti di authoring o pubblicati possono presentare questo errore di caricamento della pagina configurando l’impostazione della modalità di ricaricamento in `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

Opzioni valide:
- author
   - Questa è l&#39;opzione predefinita.
   - Verrà visualizzata una pagina di manutenzione per l’autore quando non è integro
- pubblicazione
   - Questa opzione consente di impostare una pagina di manutenzione per l’editore quando non è integra
- tutti
   - Questa opzione consente di impostare una pagina di manutenzione per l’autore o l’editore o per entrambi i siti se non sono integri
- nessuno
   - Questa opzione ignora questa funzione della verifica stato

Osservando il `VirtualHost` impostandoli, vedrai che caricano lo stesso documento come pagina di errore per ogni richiesta che arriva quando è abilitato:

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

Invece di una pagina vuota riceveranno questa pagina.

![L’immagine mostra la pagina di manutenzione predefinita](assets/load-balancer-healthcheck/unhealthy-page.png "unhealthy-page")

### Script CGI-Bin

Il CSE può configurare 5 script diversi nelle impostazioni del load balancer che modificano il comportamento o i criteri quando estrarre un Dispatcher dal load balancer.

#### /bin/checkauthor

Questo script, se utilizzato, controlla e registra tutte le istanze in primo piano, ma restituisce un errore solo se `author` Istanza AEM non integra

> `Note:` Tieni presente che se l’istanza AEM di pubblicazione non è integra, il dispatcher rimane in servizio per consentire il flusso di traffico verso l’istanza AEM di authoring

#### /bin/checkpublish (predefinito)

Questo script, se utilizzato, controlla e registra tutte le istanze in primo piano, ma restituisce un errore solo se `publish` Istanza AEM non integra

> `Note:` Tieni presente che se l’istanza AEM dell’autore non fosse integra, dispatcher rimarrebbe in servizio per consentire il flusso di traffico verso l’istanza AEM di pubblicazione

#### /bin/checkeither

Questo script, se utilizzato, controlla e registra tutte le istanze in primo piano, ma restituisce un errore solo se `author` o `publisher` Istanza AEM non integra

> `Note:` Tieni presente che se l’istanza AEM di pubblicazione o l’istanza AEM di authoring non fosse integra, il dispatcher si ritirerebbe dal servizio.  Significa che se uno di loro fosse sano, non riceverebbe traffico

#### /bin/checkboth

Questo script, se utilizzato, controlla e registra tutte le istanze in primo piano, ma restituisce un errore solo se `author` e `publisher` Istanza AEM non integra

> `Note:` Tieni presente che se l’istanza AEM di pubblicazione o l’istanza AEM di authoring non fosse integra, il dispatcher non si ritirerebbe dal servizio.  Ciò significa che se uno di questi non fosse integro continuerebbe a ricevere traffico e a fornire errori alle persone che richiedono risorse.

#### /bin/integro

Questo script, se utilizzato, controlla e registra tutte le istanze che sta fronteggiando, ma tornerà integro indipendentemente dal fatto che l’AEM restituisca o meno un errore.

> `Note:` Questo script viene utilizzato quando il controllo di integrità non funziona come desiderato e consente a un override di mantenere le istanze AEM nel load balancer.

[Successivo -> Collegamenti simbolici GIT](./git-symlinks.md)
