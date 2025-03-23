---
title: Registri comuni di AEM Dispatcher
description: Dai un’occhiata alle voci di registro comuni da Dispatcher e scopri cosa significano e come gestirle.
version: Experience Manager 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 229
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '796'
ht-degree: 0%

---

# Registri comuni

[Sommario](./overview.md)

[&lt;- Precedente: URL personalizzati](./disp-vanity-url.md)

## Panoramica

Il documento descrive le voci di registro comuni, il loro significato e come gestirle.

## Avviso GLOB

Voce di registro di esempio:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

A partire dal modulo Dispatcher 4.2.x hanno iniziato a scoraggiare gli utenti a utilizzare il seguente tipo di corrispondenze nel loro file di filtri:

```
/0041 { /type "allow" /glob "* *.css *"   }
```

Invece, è meglio utilizzare la nuova sintassi come:

```
/0041 { /type "allow" /url "*.css" }
```

O anche meglio non corrispondere in una corrispondenza con caratteri jolly affatto:

```
/0041 { /type "allow" /extension "css" }
```

L’esecuzione di uno dei metodi suggeriti eliminerebbe il messaggio di errore dai registri.

## Filtra Rifiuti

>[!NOTE]
>
>Queste voci non vengono sempre visualizzate anche se si verificano dei rifiuti quando il livello di registro è troppo basso. Impostalo su Info o debug per verificare se i filtri rifiutano le richieste.

Voce di registro di esempio:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

oppure:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

>[!CAUTION]
>
>Devi capire se le regole di Dispatcher sono state impostate per escludere tale richiesta. In questo caso, la pagina che ha tentato di visitare è stata rifiutata di proposito e non vogliamo fare nulla al riguardo.

Se il registro è simile alla voce seguente:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Questo significa che il file di progettazione `.eot` è bloccato e si desidera risolvere il problema.
È quindi necessario esaminare il file dei filtri e aggiungere la riga seguente per consentire l&#39;accesso a `.eot` file

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

In questo modo il file viene eseguito correttamente e si interrompe la registrazione.
Se desideri visualizzare gli elementi esclusi, puoi eseguire questo comando nel file di registro:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Timeout dal rendering

Voce di registro di esempio di timeout del socket:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Ciò si verifica quando l’indirizzo IP configurato nella sezione relativa ai rendering della farm è errato. Oppure l’istanza di AEM ha smesso di rispondere o di ascoltare e Dispatcher non è in grado di raggiungerla.

Controlla le regole del firewall, che l’istanza di AEM sia integra e in esecuzione.

Voci di registro di esempio di timeout del gateway:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Ciò significa che l’istanza di AEM aveva un socket aperto che poteva raggiungere e che si è interrotta con la risposta. Ciò significa che l’istanza di AEM era troppo lenta o non integra e Dispatcher ha raggiunto le impostazioni di timeout configurate nella sezione relativa al rendering della farm. Aumenta l’impostazione di timeout oppure integra l’istanza di AEM.

## Livello di memorizzazione in cache

Voce di registro di esempio:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Questo significa che viene misurato il recupero dal livello di rendering rispetto a quello dalla cache. Se vuoi ottenere un valore superiore all&#39;80% dalla cache, segui la [guida](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html?lang=it):

Per ottenere questo numero il più alto possibile.

>[!NOTE]
>
>Anche se le impostazioni della cache si trovano nel file della farm per memorizzare in cache tutto ciò che potrebbe scaricarsi con una frequenza o aggressività eccessive, ciò può causare una percentuale inferiore di riscontri nella cache

## Directory mancante

Voce di registro di esempio:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Questo generalmente viene visualizzato quando si recupera un elemento mentre si verifica contemporaneamente una cancellazione della cache.

Oppure se la directory principale dei documenti dispone di autorizzazioni non valide o se il contesto del file SELinux presente nella cartella non consente ad Apache di creare le nuove sottodirectory necessarie.

Per i problemi di autorizzazione, esamina le autorizzazioni della directory principale dei documenti e accertati che siano simili a:

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

Questo errore si verifica quando il Dispatcher è stato configurato per l’utilizzo del filtro automatico dinamico per consentire gli URL personalizzati, ma la configurazione non è stata completata con l’installazione del pacchetto sul renderer AEM.

Per risolvere questo problema, installa il feature pack per gli URL personalizzati nell’istanza di AEM e consenti che sia pronto per l’utente anonimo. Dettagli [qui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html)

L’impostazione di un URL personalizzato funzionante è simile al seguente:

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

Questo errore indica che da tutti i file della farm disponibili in `/etc/httpd/conf.dispatcher.d/enabled_farms/` non è stato possibile trovare una voce corrispondente dalla sezione `/virtualhost`.

I file della farm corrispondono al traffico in base al nome di dominio o al percorso in cui è stata presentata la richiesta. Utilizza la corrispondenza glob e, se non corrisponde, significa che la farm non è stata configurata correttamente, digita la voce nella farm, oppure che la voce manca del tutto. Se la farm non corrisponde ad alcuna voce, per impostazione predefinita viene utilizzata l’ultima farm inclusa nello stack di file farm inclusi. In questo esempio era `999_ams_publish_farm.any`, che è denominato nome generico di publishfarm.

Di seguito è riportato un file farm di esempio `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` che è stato ridotto per evidenziare le parti rilevanti.

## Elemento servito da

Voce di registro di esempio:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

La pagina è stata recuperata tramite il metodo http GET per il contenuto `/content/we-retail/us/en.html` e ha richiesto 24034 millisecondi. La parte a cui prestare attenzione è alla fine di `publishfarm/0`. Noterai che la destinazione e la corrispondenza coincidono con `publishfarm`. Richiesta recuperata dal rendering 0. Ciò significa che questa pagina doveva essere richiesta ad AEM e quindi memorizzata nella cache. Richiediamo di nuovo questa pagina e vediamo cosa succede al registro.

[Avanti -> File di sola lettura](./immutable-files.md)
