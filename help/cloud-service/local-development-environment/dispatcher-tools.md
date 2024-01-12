---
title: Configurare gli strumenti di Dispatcher per lo sviluppo as a Cloud Service dell’AEM
description: Gli strumenti Dispatcher dell’SDK per AEM facilitano lo sviluppo locale di progetti Adobe Experience Manager (AEM) semplificando l’installazione, l’esecuzione e la risoluzione dei problemi in locale.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 765
source-git-commit: 55f5cef46f7451ebb5b42b8cf17e71efeb0329c2
workflow-type: tm+mt
source-wordcount: '1621'
ht-degree: 4%

---

# Configurare strumenti Dispatcher locali {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Strumenti Dispatcher locali"
>abstract="Dispatcher è parte integrante dell’architettura complessiva di Experience Manager e necessario nella configurazione di sviluppo locale. L’SDK di AEM as a Cloud Service include la versione consigliata degli strumenti Dispatcher, che facilita la configurazione, la convalida e la simulazione di Dispatcher a livello locale."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=it" text="Dispatcher nel cloud"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=it" text="Scarica l’SDK di AEM as a Cloud Service"

Dispatcher di Adobe Experience Manager (AEM) è un modulo server web Apache HTTP che fornisce sicurezza e prestazioni tra il livello di CDN e di pubblicazione AEM. Dispatcher è parte integrante dell’architettura Experience Manager complessiva e deve far parte della configurazione di sviluppo locale.

L’SDK per AEM as a Cloud Service include la versione consigliata degli strumenti di Dispatcher che facilita la configurazione della convalida e la simulazione locale di Dispatcher. Gli strumenti di Dispatcher sono composti da:

+ un set di base di file di configurazione del server web HTTP Apache e del Dispatcher, che si trova in `.../dispatcher-sdk-x.x.x/src`
+ uno strumento CLI per la convalida della configurazione, disponibile all&#39;indirizzo `.../dispatcher-sdk-x.x.x/bin/validate`
+ uno strumento CLI per la generazione della configurazione, disponibile all&#39;indirizzo `.../dispatcher-sdk-x.x.x/bin/validator`
+ uno strumento CLI per la distribuzione della configurazione, disponibile all&#39;indirizzo `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ un file di configurazione immutabile che sovrascrive lo strumento CLI, disponibile all&#39;indirizzo `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ un’immagine Docker che esegue il server web Apache HTTP con il modulo Dispatcher

Tieni presente che `~` viene utilizzato come abbreviazione per la directory utente. In Windows, equivale a `%HOMEPATH%`.

>[!NOTE]
>
> I video in questa pagina sono stati registrati su macOS. Gli utenti di Windows possono seguire le istruzioni, ma utilizzare i comandi di Windows equivalenti di Dispatcher Tools, forniti con ogni video.

## Prerequisiti

1. Gli utenti di Windows devono utilizzare Windows 10 Professional (o una versione che supporta Docker)
1. Installa [Experience Manager Publish Quickstart Jar](./aem-runtime.md) sulla macchina di sviluppo locale.

+ Se necessario, installa la versione più recente [Sito web di riferimento AEM](https://github.com/adobe/aem-guides-wknd/releases) sul servizio di pubblicazione locale AEM. Questo sito web viene utilizzato in questa esercitazione per visualizzare un’istanza di Dispatcher funzionante.

1. Installa e avvia la versione più recente di [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) sul computer di sviluppo locale.

## Scaricare gli strumenti di Dispatcher (come parte dell’SDK per AEM)

L’SDK per AEM as a Cloud Service, o AEM SDK, contiene gli strumenti di Dispatcher utilizzati per eseguire il server web Apache HTTP con il modulo Dispatcher localmente per lo sviluppo e il Jar QuickStart compatibile.

Se l’SDK as a Cloud Service dall’AEM è già stato scaricato in [configurare il runtime AEM locale](./aem-runtime.md), non è necessario scaricarlo di nuovo.

1. Accedi a [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) con il tuo Adobe ID
   + La tua organizzazione Adobe __deve__ essere predisposto per AEM as a Cloud Service a scaricare l&#39;SDK AEM as a Cloud Service
1. Fai clic sull’ultima __SDK AEM__ riga dei risultati da scaricare

## Estrarre gli strumenti di Dispatcher dallo zip dell’SDK dell’AEM

>[!TIP]
>
> Gli utenti di Windows non possono avere spazi o caratteri speciali nel percorso della cartella contenente gli strumenti del Dispatcher locale. Se nel percorso sono presenti spazi, `docker_run.cmd` non riesce.

La versione degli strumenti di Dispatcher è diversa da quella dell’SDK dell’AEM. Assicurati che la versione degli strumenti di Dispatcher sia fornita tramite la versione dell’SDK per AEM corrispondente alla versione as a Cloud Service per AEM.

1. Decomprimi il download `aem-sdk-xxx.zip` file
1. Decomprimere gli strumenti di Dispatcher in `~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Decomprimi `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (creazione di cartelle mancanti in base alle esigenze).

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Tutti i comandi emessi di seguito presuppongono che la directory di lavoro corrente contenga il contenuto degli strumenti di Dispatcher in espansione.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Questo video utilizza macOS a scopo illustrativo. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili.*

## Comprendere i file di configurazione di Dispatcher

>[!TIP]
Experience Manager di progetti creati da [Progetto AEM Archetipo Maven](https://github.com/adobe/aem-project-archetype) sono precompilati in questo set di file di configurazione di Dispatcher, pertanto non è necessario copiarli dalla cartella Src degli strumenti di Dispatcher.

Gli strumenti di Dispatcher forniscono un set di file di configurazione del server web Apache HTTP e del Dispatcher che definiscono il comportamento per tutti gli ambienti, incluso lo sviluppo locale.

Questi file sono destinati a essere copiati in un progetto Maven di Experience Manager in `dispatcher/src` , se non esistono nel progetto Experience Manager Maven.

Una descrizione completa dei file di configurazione è disponibile nella sezione Strumenti di Dispatcher non compressi come `dispatcher-sdk-x.x.x/docs/Config.html`.

## Convalida configurazioni

Facoltativamente, le configurazioni del server web Dispatcher e Apache (tramite `httpd -t`) può essere convalidato utilizzando `validate` script (da non confondere con il `validator` eseguibile). Il `validate` script offre un modo pratico di eseguire [tre fasi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) del `validator`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Eseguire Dispatcher localmente

AEM Dispatcher viene eseguito localmente utilizzando Docker rispetto al `src` File di configurazione del server web Dispatcher e Apache.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

Il `docker_run_hot_reload` eseguibile preferito rispetto a `docker_run` ricaricando i file di configurazione man mano che vengono modificati, senza dover terminare e riavviare manualmente `docker_run`. In alternativa: `docker_run` può essere utilizzato ma richiede la chiusura e il riavvio manuale `docker_run` quando vengono modificati i file di configurazione.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

Il `docker_run_hot_reload` eseguibile preferito rispetto a `docker_run` ricaricando i file di configurazione man mano che vengono modificati, senza dover terminare e riavviare manualmente `docker_run`. In alternativa: `docker_run` può essere utilizzato ma richiede la chiusura e il riavvio manuale `docker_run` quando vengono modificati i file di configurazione.

>[!ENDTABS]

Il `<aem-publish-host>` può essere impostato su `host.docker.internal`, un nome DNS speciale fornito da Docker nel contenitore che viene risolto nell’IP del computer host. Se il `host.docker.internal` non si risolve, consulta la [risoluzione dei problemi](#troubleshooting-host-docker-internal) sezione successiva.

Ad esempio, per avviare il contenitore Docker di Dispatcher utilizzando i file di configurazione predefiniti forniti dagli strumenti di Dispatcher:

Avvia il contenitore Docker di Dispatcher che fornisce il percorso della cartella src di configurazione Dispatcher:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

Il servizio di pubblicazione dell’SDK per AEM as a Cloud Service, in esecuzione a livello locale sulla porta 4503, è disponibile tramite Dispatcher all’indirizzo `http://localhost:8080`.

Per eseguire gli strumenti di Dispatcher rispetto alla configurazione di Dispatcher di un progetto Experience Manager, fai clic su `dispatcher/src` cartella.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Registri degli strumenti di Dispatcher

I registri di Dispatcher sono utili durante lo sviluppo locale per capire se e perché le richieste HTTP sono bloccate. Il livello di registro può essere impostato inserendo come prefisso l’esecuzione di `docker_run` con parametri di ambiente.

I registri degli strumenti di Dispatcher vengono emessi in uscita standard quando `docker_run` viene eseguito.

I parametri utili per il debug di Dispatcher includono:

+ `DISP_LOG_LEVEL=Debug` imposta la registrazione del modulo Dispatcher al livello di debug
   + Valore predefinito: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` imposta la registrazione del modulo di riscrittura del server web Apache HTTP sul livello Debug
   + Valore predefinito: `Warn`
+ `DISP_RUN_MODE` imposta la &quot;modalità di esecuzione&quot; dell’ambiente Dispatcher, caricando i file di configurazione di Dispatcher per le modalità di esecuzione corrispondenti.
   + Impostazione predefinita `dev`
+ Valori validi: `dev`, `stage`, o `prod`

Uno o più parametri che possono essere trasmessi a `docker_run`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Accesso al file di registro

È possibile accedere direttamente ai registri del server web Apache e del Dispatcher AEM nel contenitore Docker:

+ [Accesso ai registri nel contenitore Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copia dei registri Docker nel file system locale](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quando aggiornare gli strumenti di Dispatcher{#dispatcher-tools-version}

Le versioni degli strumenti di Dispatcher aumentano meno frequentemente rispetto all’Experience Manager e pertanto gli strumenti di Dispatcher richiedono meno aggiornamenti nell’ambiente di sviluppo locale.

La versione consigliata degli strumenti di Dispatcher è quella fornita in bundle con l’SDK as a Cloud Service dell’AEM che corrisponde alla versione as a Cloud Service dell’Experience Manager. La versione dell’AEM as a Cloud Service è disponibile tramite [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambienti__, per ambiente specificato da __Versione AEM__ etichetta

![Versione Experience Manager](./assets/dispatcher-tools/aem-version.png)

*La versione degli strumenti di Dispatcher non corrisponde a quella dell’Experience Manager.*

## Come aggiornare il set di base delle configurazioni di Apache e Dispatcher

Il set di base della configurazione di Apache e Dispatcher viene regolarmente migliorato e rilasciato con la versione dell’SDK as a Cloud Service per l’AEM. È consigliabile incorporare i miglioramenti della configurazione di base nel progetto AEM ed evitare [convalida locale](#validate-configurations) Errori di pipeline di e Cloud Manager. Aggiornali utilizzando `update_maven.sh` script da `.../dispatcher-sdk-x.x.x/bin` cartella.

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*Questo video utilizza macOS a scopo illustrativo. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili.*


Supponiamo che tu abbia creato un progetto AEM in passato utilizzando [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype), le configurazioni di base di Apache e Dispatcher erano correnti. Utilizzando queste configurazioni di base, le configurazioni specifiche del progetto sono state create riutilizzando e copiando i file come `*.vhost`, `*.conf`, `*.farm` e `*.any` dal `dispatcher/src/conf.d` e `dispatcher/src/conf.dispatcher.d` cartelle. La convalida del Dispatcher locale e le pipeline di Cloud Manager funzionavano correttamente.

Nel frattempo, le configurazioni di base di Apache e Dispatcher sono state migliorate per vari motivi, come nuove funzioni, correzioni di sicurezza e ottimizzazione. Vengono rilasciati tramite una versione più recente degli Strumenti di Dispatcher nell’ambito del rilascio as a Cloud Service dell’AEM.

Ora, quando si convalidano le configurazioni del Dispatcher specifiche per il progetto rispetto alla versione più recente degli strumenti di Dispatcher, si verifica un errore. Per risolvere questo problema, è necessario aggiornare le configurazioni della linea di base utilizzando i passaggi seguenti:

+ Verifica che la convalida non riesca rispetto alla versione più recente degli strumenti di Dispatcher

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Aggiornare i file immutabili utilizzando `update_maven.sh` script

  ```shell
  $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Updating dispatcher configuration at folder 
  running in 'extract' mode
  running in 'extract' mode
  reading immutable file list from /etc/httpd/immutable.files.txt
  preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
  ...
  immutable files extraction COMPLETE
  fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
  Cloud manager validator 2.0.53
  ```

+ Verifica i file immutabili aggiornati come `dispatcher_vhost.conf`, `default.vhost`, e `default.farm` e, se necessario, apportare modifiche rilevanti ai file personalizzati derivati da tali file.

+ Convalida la configurazione, deve passare

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ Dopo la verifica locale delle modifiche, esegui il commit dei file di configurazione aggiornati

## Risoluzione dei problemi

### docker_run restituisce il messaggio &#39;In attesa della disponibilità di host.docker.internal&#39;{#troubleshooting-host-docker-internal}

Il `host.docker.internal` è un nome host fornito al Docker contain che viene risolto nell’host. Per docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

>A partire da Docker 18.03 si consiglia di connettersi al nome DNS speciale host.docker.internal, che viene risolto nell&#39;indirizzo IP interno utilizzato dall&#39;host

Quando `bin/docker_run src host.docker.internal:4503 8080` risultati nel messaggio __In attesa che host.docker.internal sia disponibile__, quindi:

1. Verificare che la versione installata di Docker sia 18.03 o successiva
1. È possibile che sia stato configurato un computer locale che impedisce la registrazione/risoluzione del `host.docker.internal` nome. Utilizza piuttosto l’IP locale.

>[!BEGINTABS]

>[!TAB macOS]

+ Da Terminal, esegui `ifconfig` e registra l&#39;host __inet__ Indirizzo IP, in genere __en0__ dispositivo.
+ Quindi esegui `docker_run` utilizzo dell&#39;indirizzo IP host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ Dal prompt dei comandi, eseguire `ipconfig`, e registra il __Indirizzo IPv4__ del computer host.
+ Quindi, esegui `docker_run` utilizzando questo indirizzo IP: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ Da Terminal, esegui `ifconfig` e registra l&#39;host __inet__ Indirizzo IP, in genere __en0__ dispositivo.
+ Quindi esegui `docker_run` utilizzo dell&#39;indirizzo IP host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### Errore di esempio

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Risorse aggiuntive

+ [Scaricare l’SDK dell’AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Scarica Docker](https://www.docker.com/)
+ [Scarica il sito web di riferimento AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentazione di Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it)
