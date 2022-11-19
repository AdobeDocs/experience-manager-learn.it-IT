---
title: Registri comuni di Dispatcher AEM
description: Dai un’occhiata alle voci di registro comuni di Dispatcher e scopri cosa intendono e come risolverle.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# Registri comuni

[Sommario](./overview.md)

[&lt;- Precedente: URL personalizzati](./disp-vanity-url.md)

## Panoramica

Il documento descriverà le voci di registro comuni che verranno visualizzate, il loro significato e il modo in cui gestirle.

## Avviso GLOB

Voce di registro di esempio:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Dal modulo Dispatcher 4.2.x hanno iniziato a scoraggiare le persone a utilizzare il seguente tipo di corrispondenze nel file dei filtri:

```
/0041 { /type "allow" /glob "* *.css *"   }
```

Invece, è meglio utilizzare la nuova sintassi come:

```
/0041 { /type "allow" /url "*.css" }
```

O ancora meglio non corrispondere su un matcher jolly affatto:

```
/0041 { /type "allow" /extension "css" }
```

Effettuare uno dei metodi suggeriti eliminerebbe quel messaggio di errore dai log.

## Rifiuti filtro


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Queste voci non vengono sempre visualizzate anche se si verificano dei rifiuti se il livello di log è troppo basso. Impostalo su Info o debug per assicurarti di vedere se i filtri rifiutano le richieste.
</div>

Voce di registro di esempio:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

oppure:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>Attenzione:</b>

Comprendi che le regole del Dispatcher sono state impostate per filtrare la richiesta. In questo caso la pagina che si è tentato di visitare è stata rifiutata di proposito e non vorremmo fare nulla con questo.
</div>

Se il registro ha l&#39;aspetto di questa voce:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Questo ci fa sapere che il nostro file di progettazione `.eot` viene bloccato e vogliamo risolvere il problema.
Dovremmo quindi esaminare il file dei filtri e aggiungere la seguente riga per consentire `.eot` file attraverso

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

In questo modo il file viene trasmesso e questo impedisce la registrazione.
Se desideri visualizzare gli elementi filtrati, puoi eseguire questo comando nel file di log:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Timeout dal rendering

Voce di registro di esempio del timeout del socket:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Questo si verifica quando l’indirizzo IP configurato nella sezione renders della farm è errato. L’istanza AEM ha smesso di rispondere o di ascoltare e Dispatcher non è in grado di raggiungerla.

Controlla le regole del firewall e che l’istanza AEM sia in esecuzione e integra.

Voci di registro di esempio del timeout del gateway:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Ciò significa che l&#39;istanza AEM aveva un socket aperto che poteva raggiungere e che si era interrotta con la risposta. Ciò significa che l’istanza di AEM era troppo lenta o non integra e che Dispatcher ha raggiunto le impostazioni di timeout configurate nella sezione di rendering della farm. Puoi aumentare l’impostazione di timeout o rendere l’istanza di AEM integra.

## Livello di memorizzazione nella cache

Voce di registro di esempio:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Questo significa che viene misurato il recupero dal livello di rendering rispetto alla cache. Vuoi raggiungere l’80% più dalla cache e segui la guida [qui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

Per ottenere questo numero il più alto possibile.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Anche se hai le impostazioni della cache nel file di farm per memorizzare in cache tutto ciò che si potrebbe scaricare troppo spesso o troppo aggressivamente che può causare un rapporto di hit della cache in percentuale inferiore
</div>

## Directory mancante

Voce di registro di esempio:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

In genere questo viene visualizzato quando un elemento viene recuperato mentre si verifica una cancellazione della cache nello stesso momento.

La directory radice dei documenti dispone di autorizzazioni non valide oppure il contesto di file SELinux nella cartella non corretto che impedisce ad Apache di creare le nuove sottodirectory necessarie.

Per i problemi di autorizzazione, esamina le autorizzazioni della radice dei documenti e accertati che siano simili a:

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## URL personalizzato non trovato

Voce di registro di esempio:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

Questo errore si verifica quando hai configurato il Dispatcher per utilizzare il filtro automatico dinamico per consentire gli URL personalizzati, ma non hai completato la configurazione installando il pacchetto sul modulo di rendering AEM.

Per risolvere il problema, installa il pacchetto di funzioni dell&#39;url personalizzato sull&#39;istanza AEM e consentiscilo a essere pronto dall&#39;utente anonimo. Dettagli [qui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

Una configurazione di URL personalizzati funzionante si presenta così:

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## Farm mancante

Voce di registro di esempio:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

Questo errore indica che da tutti i file di farm disponibili in `/etc/httpd/conf.dispatcher.d/enabled_farms/` non sono stati in grado di trovare una voce corrispondente dal `/virtualhost` sezione .

I file farm corrispondono al traffico in base al nome di dominio o al percorso in cui la richiesta è arrivata. Utilizza la corrispondenza glob e, se non corrisponde, non hai configurato correttamente la tua farm, digita la voce nella farm o fai perdere completamente la voce. Quando la farm non corrisponde ad alcuna voce, alla fine viene utilizzata l’ultima farm inclusa nello stack di file di farm inclusi. In questo esempio `999_ams_publish_farm.any` che è denominato il nome generico di publishfarm.

Ecco un file farm di esempio `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` è stato ridotto per evidenziare le parti rilevanti.

## Articolo servito da

Voce di registro di esempio:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

La pagina è stata recuperata tramite il metodo GET http per il contenuto `/content/we-retail/us/en.html` e ci sono voluti 24034 millisecondi. La parte a cui volevamo prestare attenzione è alla fine `publishfarm/0`. Vedrai che ha eseguito il targeting e corrisponde al `publishfarm`. Richiesta recuperata dal rendering 0. Ciò significa che questa pagina doveva essere richiesta AEM poi memorizzata nella cache. Ora richiediamo nuovamente questa pagina e vediamo cosa succede al registro.

[Avanti -> File di sola lettura](./immutable-files.md)