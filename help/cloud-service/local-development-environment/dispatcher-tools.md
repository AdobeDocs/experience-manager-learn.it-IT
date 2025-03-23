---
title: Configurare gli strumenti Dispatcher per lo sviluppo AEM as a Cloud Service
description: Gli strumenti Dispatcher di AEM SDK facilitano lo sviluppo locale di progetti Adobe Experience Manager (AEM) semplificando l’installazione, l’esecuzione e la risoluzione dei problemi di Dispatcher a livello locale.
version: Experience Manager as a Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 4%

---

# Configurare strumenti Dispatcher locali {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Strumenti Dispatcher locali"
>abstract="Dispatcher è parte integrante dell’architettura complessiva di Experience Manager e necessario nella configurazione di sviluppo locale. L’SDK di AEM as a Cloud Service include la versione consigliata degli strumenti Dispatcher, che facilita la configurazione, la convalida e la simulazione di Dispatcher a livello locale."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=it" text="Dispatcher nel cloud"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=it" text="Scarica l’SDK di AEM as a Cloud Service"

Dispatcher di Adobe Experience Manager (AEM) è un modulo server web Apache HTTP che fornisce sicurezza e prestazioni tra il livello di CDN e AEM Publish. Dispatcher è parte integrante dell’architettura Experience Manager complessiva e deve far parte della configurazione di sviluppo locale.

AEM as a Cloud Service SDK include la versione consigliata di Dispatcher Tools che facilita la configurazione della convalida e la simulazione locale di Dispatcher. Strumenti di Dispatcher è composto da:

+ un set di base di file di configurazione Dispatcher e server Web HTTP Apache, situato in `.../dispatcher-sdk-x.x.x/src`
+ uno strumento CLI di convalida della configurazione, disponibile in `.../dispatcher-sdk-x.x.x/bin/validate`
+ uno strumento CLI di generazione della configurazione, disponibile in `.../dispatcher-sdk-x.x.x/bin/validator`
+ uno strumento CLI di distribuzione della configurazione, disponibile in `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ file di configurazione immutabile che sovrascrivono lo strumento CLI, disponibile in `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ un’immagine Docker che esegue il server web Apache HTTP con il modulo Dispatcher

`~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows equivale a `%HOMEPATH%`.

>[!NOTE]
>
> I video in questa pagina sono stati registrati su macOS. Gli utenti di Windows possono seguire le istruzioni, ma utilizzare i comandi equivalenti di Dispatcher Tools per Windows, forniti con ogni video.

## Prerequisiti

1. Gli utenti di Windows devono utilizzare Windows 10 Professional (o una versione che supporta Docker)
1. Installa [Experience Manager Publish Quickstart Jar](./aem-runtime.md) nel computer di sviluppo locale.

+ È possibile installare il [sito Web di riferimento AEM](https://github.com/adobe/aem-guides-wknd/releases) più recente nel servizio di pubblicazione AEM locale. Questo sito Web viene utilizzato in questo tutorial per visualizzare un Dispatcher funzionante.

1. Installare e avviare la versione più recente di [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) nel computer di sviluppo locale.

## Scaricare gli strumenti di Dispatcher (come parte di AEM SDK)

AEM as a Cloud Service SDK, o AEM SDK, contiene gli strumenti Dispatcher utilizzati per eseguire il server web Apache HTTP con il modulo Dispatcher locale per lo sviluppo e il file JAR QuickStart compatibile.

Se AEM as a Cloud Service SDK è già stato scaricato in [configurare il runtime AEM locale](./aem-runtime.md), non è necessario scaricarlo di nuovo.

1. Accedi a [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) con il tuo Adobe ID
   + È necessario eseguire il provisioning della tua organizzazione Adobe __1} per consentire ad AEM as a Cloud Service di scaricare AEM as a Cloud Service SDK__
1. Fai clic sull&#39;ultima __riga dei risultati di AEM SDK__ da scaricare

## Estrai gli strumenti Dispatcher dallo zip AEM SDK

>[!TIP]
>
> Gli utenti di Windows non possono disporre di spazi o caratteri speciali nel percorso della cartella contenente gli strumenti di Dispatcher locali. Se nel percorso sono presenti spazi, `docker_run.cmd` non produrrà alcun risultato.

La versione di Dispatcher Tools è diversa da quella di AEM SDK. Assicurati che la versione degli Strumenti di Dispatcher sia fornita tramite la versione di AEM SDK corrispondente alla versione di AEM as a Cloud Service.

1. Decomprimi il file `aem-sdk-xxx.zip` scaricato
1. Decomprimi gli strumenti di Dispatcher in `~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Decomprimi `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (creando le cartelle mancanti in base alle esigenze).

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Tutti i comandi emessi di seguito presuppongono che la directory di lavoro corrente contenga il contenuto degli strumenti di Dispatcher in espansione.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Questo video utilizza macOS a scopo illustrativo. È possibile utilizzare i comandi Windows/Linux equivalenti per ottenere risultati simili.*

## Comprendere i file di configurazione di Dispatcher

>[!TIP]
> I progetti Experience Manager creati dall&#39;[Archetipo Maven progetto AEM](https://github.com/adobe/aem-project-archetype) sono precompilati in questo set di file di configurazione Dispatcher, pertanto non è necessario copiarli dalla cartella Dispatcher Tools Src.

Gli strumenti di Dispatcher forniscono un set di file di configurazione del server web Apache HTTP e di Dispatcher che definiscono il comportamento per tutti gli ambienti, incluso lo sviluppo locale.

Questi file devono essere copiati in un progetto Experience Manager Maven nella cartella `dispatcher/src`, se non esistono nel progetto Experience Manager Maven.

Una descrizione completa dei file di configurazione è disponibile in Strumenti di Dispatcher decompressi come `dispatcher-sdk-x.x.x/docs/Config.html`.

## Convalida configurazioni

Facoltativamente, le configurazioni del server Web Dispatcher e Apache (tramite `httpd -t`) possono essere convalidate utilizzando lo script `validate` (da non confondere con l&#39;eseguibile `validator`). Lo script `validate` consente di eseguire in modo pratico le [tre fasi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) di `validator`.


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

AEM Dispatcher viene eseguito localmente utilizzando Docker sui file di configurazione del server Web Dispatcher e Apache `src`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

L&#39;eseguibile `docker_run_hot_reload` è preferito a `docker_run` in quanto ricarica i file di configurazione man mano che vengono modificati, senza dover terminare e riavviare manualmente `docker_run`. In alternativa, è possibile utilizzare `docker_run`, ma richiede la chiusura e il riavvio manuale di `docker_run` quando i file di configurazione vengono modificati.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

L&#39;eseguibile `docker_run_hot_reload` è preferito a `docker_run` in quanto ricarica i file di configurazione man mano che vengono modificati, senza dover terminare e riavviare manualmente `docker_run`. In alternativa, è possibile utilizzare `docker_run`, ma richiede la chiusura e il riavvio manuale di `docker_run` quando i file di configurazione vengono modificati.

>[!ENDTABS]

È possibile impostare `<aem-publish-host>` su `host.docker.internal`, un nome DNS speciale fornito da Docker nel contenitore che viene risolto nell&#39;IP del computer host. Se `host.docker.internal` non si risolve, vedere la sezione [risoluzione dei problemi](#troubleshooting-host-docker-internal) di seguito.

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

Il servizio di pubblicazione di AEM as a Cloud Service SDK, in esecuzione localmente sulla porta 4503, è disponibile tramite Dispatcher all&#39;indirizzo `http://localhost:8080`.

Per eseguire gli strumenti di Dispatcher nella configurazione Dispatcher di un progetto Experience Manager, seleziona la cartella `dispatcher/src` del progetto.

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


## Registri strumenti Dispatcher

I registri di Dispatcher sono utili durante lo sviluppo locale per capire se e perché le richieste HTTP sono bloccate. È possibile impostare il livello di registro prefissando l&#39;esecuzione di `docker_run` con i parametri dell&#39;ambiente.

I registri degli strumenti di Dispatcher vengono emessi in uscita standard quando viene eseguito `docker_run`.

I parametri utili per il debug di Dispatcher includono:

+ `DISP_LOG_LEVEL=Debug` imposta la registrazione del modulo Dispatcher al livello di debug
   + Valore predefinito: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` imposta la registrazione del modulo di riscrittura del server Web HTTP Apache sul livello Debug
   + Valore predefinito: `Warn`
+ `DISP_RUN_MODE` imposta la &quot;modalità di esecuzione&quot; dell&#39;ambiente Dispatcher, caricando i file di configurazione Dispatcher delle modalità di esecuzione corrispondenti.
   + Impostazione predefinita: `dev`
+ Valori validi: `dev`, `stage` o `prod`

Uno o più parametri possono essere passati a `docker_run`

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

È possibile accedere direttamente ai registri del server web Apache e di AEM Dispatcher nel contenitore Docker:

+ [Accesso ai registri nel contenitore Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copia dei registri Docker nel file system locale](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quando aggiornare gli strumenti Dispatcher{#dispatcher-tools-version}

Le versioni di Dispatcher Tools aumentano meno frequentemente rispetto ad Experience Manager, pertanto gli strumenti di Dispatcher richiedono meno aggiornamenti nell’ambiente di sviluppo locale.

La versione consigliata degli strumenti di Dispatcher è quella fornita in bundle con AEM as a Cloud Service SDK che corrisponde alla versione di Experience Manager as a Cloud Service. La versione di AEM as a Cloud Service è disponibile tramite [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambienti__, per ambiente specificato dall&#39;etichetta __AEM Release__

![Versione Experience Manager](./assets/dispatcher-tools/aem-version.png)

*La versione degli strumenti di Dispatcher non corrisponde alla versione di Experience Manager.*

## Come aggiornare il set di base di configurazioni di Apache e Dispatcher

Il set di base della configurazione di Apache e Dispatcher viene migliorato regolarmente e rilasciato con la versione AEM as a Cloud Service SDK. È consigliabile incorporare i miglioramenti alla configurazione di base nel progetto AEM ed evitare la [convalida locale](#validate-configurations) e gli errori della pipeline Cloud Manager. Aggiornarli utilizzando lo script `update_maven.sh` della cartella `.../dispatcher-sdk-x.x.x/bin`.

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*Questo video utilizza macOS a scopo illustrativo. È possibile utilizzare i comandi Windows/Linux equivalenti per ottenere risultati simili.*


Supponiamo che tu abbia creato un progetto AEM in passato utilizzando [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype); le configurazioni di base di Apache e Dispatcher erano correnti. Utilizzando queste configurazioni di base, le configurazioni specifiche del progetto sono state create riutilizzando e copiando file come `*.vhost`, `*.conf`, `*.farm` e `*.any` dalle cartelle `dispatcher/src/conf.d` e `dispatcher/src/conf.dispatcher.d`. La convalida Dispatcher locale e le pipeline Cloud Manager funzionavano correttamente.

Nel frattempo, le configurazioni di base di Apache e Dispatcher sono state migliorate per vari motivi, come nuove funzioni, correzioni di sicurezza e ottimizzazione. Vengono rilasciati tramite una versione più recente di Dispatcher Tools come parte del rilascio di AEM as a Cloud Service.

Ora, quando si convalidano le configurazioni di Dispatcher specifiche per il progetto rispetto alla versione più recente degli strumenti Dispatcher, si verifica un errore. Per risolvere questo problema, è necessario aggiornare le configurazioni della linea di base utilizzando i passaggi seguenti:

+ Verifica che la convalida non riesca rispetto alla versione più recente di Dispatcher Tools

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Aggiornare i file immutabili utilizzando lo script `update_maven.sh`

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

+ Verificare i file immutabili aggiornati come `dispatcher_vhost.conf`, `default.vhost` e `default.farm` e, se necessario, apportare modifiche rilevanti ai file personalizzati derivati da tali file.

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

`host.docker.internal` è un nome host fornito al Docker che viene risolto nell&#39;host. Secondo docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> A partire da Docker 18.03 si consiglia di connettersi al nome DNS speciale host.docker.internal, che viene risolto nell&#39;indirizzo IP interno utilizzato dall&#39;host

Quando `bin/docker_run src host.docker.internal:4503 8080` restituisce il messaggio __In attesa che host.docker.internal sia disponibile__, allora:

1. Verificare che la versione installata di Docker sia 18.03 o successiva
1. È possibile che sia stato configurato un computer locale che impedisce la registrazione/risoluzione del nome `host.docker.internal`. Utilizza piuttosto l’IP locale.

>[!BEGINTABS]

>[!TAB macOS]

+ Da Terminal, eseguire `ifconfig` e registrare l&#39;indirizzo IP __inet__ dell&#39;host, in genere il dispositivo __en0__.
+ Quindi eseguire `docker_run` utilizzando l&#39;indirizzo IP host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ Dal prompt dei comandi, eseguire `ipconfig` e registrare l&#39;__indirizzo IPv4__ dell&#39;host nel computer host.
+ Quindi, eseguire `docker_run` utilizzando questo indirizzo IP: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ Da Terminal, eseguire `ifconfig` e registrare l&#39;indirizzo IP __inet__ dell&#39;host, in genere il dispositivo __en0__.
+ Quindi eseguire `docker_run` utilizzando l&#39;indirizzo IP host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

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

+ [Scarica AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Scarica Docker](https://www.docker.com/)
+ [Scarica il sito Web di riferimento di AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentazione di Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it)
