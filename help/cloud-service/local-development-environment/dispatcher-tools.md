---
title: Imposta strumenti del dispatcher per AEM come sviluppo Cloud Service
description: AEM strumenti Dispatcher dell’SDK facilita lo sviluppo locale di progetti Adobe Experience Manager (AEM) semplificando l’installazione, l’esecuzione e la risoluzione dei problemi del dispatcher localmente.
sub-product: foundation
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: 1b4a927a68d24eeb08d0ee244e85519323482910
workflow-type: tm+mt
source-wordcount: '1534'
ht-degree: 2%

---


# Impostazione degli strumenti del dispatcher locale

Il dispatcher di Adobe Experience Manager (AEM) è un modulo server Web Apache HTTP che fornisce un livello di protezione e prestazioni tra il livello CDN e AEM Publish. Dispatcher è parte integrante dell&#39;architettura generale  Experience Manager e dovrebbe essere parte integrante della configurazione di sviluppo locale.

L’AEM come Cloud Service SDK include la versione consigliata di Dispatcher Tools, che semplifica la configurazione, la convalida e la simulazione locale del dispatcher. Gli strumenti Dispatcher sono formati da:

+ un set di base di file di configurazione del server Web Apache HTTP e del dispatcher, situato in `.../dispatcher-sdk-x.x.x/src`
+ uno strumento CLI per la convalida della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+)
+ uno strumento CLI di generazione della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/validator`
+ uno strumento CLI per la distribuzione della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ un&#39;immagine Docker che esegue il server Web Apache HTTP con il modulo Dispatcher

Tenere presente che `~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows, questo è l&#39;equivalente di `%HOMEPATH%`.

>[!NOTE]
>
> I video in questa pagina sono stati registrati su macOS. Gli utenti di Windows possono seguire, ma utilizzare i comandi Windows equivalenti di Dispatcher Tools, forniti con ciascun video.

## Prerequisiti

1. Gli utenti Windows devono utilizzare Windows 10 Professional
1. Installate [ Experience Manager Publish Quickstart Jar](./aem-runtime.md) nel computer di sviluppo locale.
   + Se necessario, installate l&#39;ultimo [sito Web di riferimento AEM](https://github.com/adobe/aem-guides-wknd/releases) nel servizio AEM Publish locale. Questo sito Web viene utilizzato in questa esercitazione per visualizzare un Dispatcher di lavoro.
1. Installate e avviate la versione più recente di [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) nel computer di sviluppo locale.

## Download degli strumenti del dispatcher (come parte dell’SDK AEM)

Il AEM come SDK di Cloud Service, o SDK AEM, contiene gli strumenti Dispatcher utilizzati per eseguire il server Web Apache HTTP con il modulo Dispatcher localmente per lo sviluppo, nonché il Jar QuickStart compatibile.

Se il AEM come SDK di Cloud Service è già stato scaricato in [setup the local AEM runtime](./aem-runtime.md), non è necessario scaricarlo nuovamente.

1. Accedete a [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=list p.offset=0&amp;p.limit=1) con il vostro Adobe ID 
   + È necessario effettuare il provisioning dell&#39;organizzazione  Adobe __AEM__ come Cloud Service per scaricare il AEM come Cloud Service SDK
1. Fai clic sulla riga dei risultati più recente di __AEM SDK__ per il download
   + Verifica che AEM strumenti Dispatcher dell’SDK v2.0.29+ sia presente nella descrizione del download

## Estrarre gli strumenti Dispatcher dallo zip AEM SDK

>[!TIP]
>
> Gli utenti Windows non possono contenere spazi o caratteri speciali nel percorso della cartella contenente gli strumenti Dispatcher locali. Se nel percorso sono presenti degli spazi, la `docker_run.cmd` avrà esito negativo.

La versione di Dispatcher Tools è diversa da quella dell’SDK AEM. Assicurati che la versione di Dispatcher Tools sia fornita tramite la versione SDK AEM che corrisponde al AEM come versione Cloud Service.

1. Decomprimete il file `aem-sdk-xxx.zip` scaricato
1. Rimuovi il pacchetto degli strumenti del dispatcher in `~/aem-sdk/dispatcher`
   + Windows: Decomprimete `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (creando le cartelle mancanti secondo necessità)
   + macOS / Linux: Esegui lo script shell associato `aem-sdk-dispatcher-tools-x.x.x-unix.sh` per rimuovere il pacchetto degli strumenti Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Tenere presente che tutti i comandi riportati di seguito presuppongono che la directory di lavoro corrente contenga il contenuto Espansione di Strumenti Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Questo video utilizza macOS per scopi illustrativi. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

## Informazioni sui file di configurazione del dispatcher

>[!TIP]
>  progetti di Experience Manager creati da [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) vengono precompilati questo set di file di configurazione del Dispatcher, pertanto non è necessario copiarli dalla cartella src di Dispatcher Tools.

Dispatcher Tools fornisce un set di file di configurazione del server Web Apache HTTP e del dispatcher che definiscono il comportamento per tutti gli ambienti, incluso lo sviluppo locale.

Questi file devono essere copiati in un progetto  Experience Manager Maven nella cartella `dispatcher/src`, se non sono già presenti nel progetto  Experience Manager Maven.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*Questo video utilizza macOS per scopi illustrativi. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

Una descrizione completa dei file di configurazione è disponibile in Dispatcher Tools (Strumenti dispatcher) non compressi come `dispatcher-sdk-x.x.x/docs/Config.html`.

## Convalida configurazioni

Facoltativamente, le configurazioni del Dispatcher e del server Web Apache (tramite `httpd -t`) possono essere convalidate utilizzando lo script `validate` (da non confondere con l&#39;eseguibile `validator`).

+ Utilizzo:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate ./src`

## Eseguire il dispatcher localmente

Per eseguire il dispatcher localmente, i file di configurazione del dispatcher devono essere generati utilizzando lo strumento CLI `validator` di Dispatcher Tools.

+ Utilizzo:
   + Windows: `bin\validator full -d out src`
   + macOS / Linux: `./bin/validator full -d ./out ./src`

Questo comando trasforma le configurazioni in un set di file compatibile con il server Web Apache HTTP del contenitore Docker.

Una volta generate, le configurazioni transpeled vengono utilizzate eseguono Dispatcher localmente nel contenitore Docker. È importante verificare che le configurazioni più recenti siano state convalidate utilizzando l&#39;output `validate` __e__ utilizzando l&#39;opzione `-d` del validatore.

+ Utilizzo:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

Il `aem-publish-host` può essere impostato su `host.docker.internal`, un Docker di nomi DNS speciale fornisce nel contenitore che risolve all&#39;IP del computer host. Se `host.docker.internal` non viene risolto, consultare la sezione [risoluzione dei problemi](#troubleshooting-host-docker-internal) riportata di seguito.

Ad esempio, per avviare il contenitore Dispatcher Docker utilizzando i file di configurazione predefiniti forniti dagli strumenti Dispatcher:

1. Generare da zero `deployment-folder`, denominata `out` per convenzione, ogni volta che una configurazione cambia:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS / Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. Avvia nuovamente il contenitore del dispatcher Docker fornendo il percorso della cartella di distribuzione:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Il AEM come servizio di pubblicazione dell&#39;SDK di Cloud Service, eseguito localmente sulla porta 4503, sarà disponibile tramite Dispatcher all&#39;indirizzo `http://localhost:8080`.

Per eseguire Dispatcher Tools rispetto alla configurazione Dispatcher di un progetto di Experience Manager , è sufficiente generare il file `deployment-folder` utilizzando la cartella `dispatcher/src` del progetto.

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*Questo video utilizza macOS per scopi illustrativi. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

## Dispatcher Tools logs

I registri del dispatcher sono utili durante lo sviluppo locale per comprendere se e perché le richieste HTTP sono bloccate. Il livello di registro può essere impostato prefissando l&#39;esecuzione di `docker_run` con i parametri dell&#39;ambiente.

I registri degli strumenti del dispatcher vengono inviati allo standard quando si esegue `docker_run`.

I parametri utili per il debug del dispatcher includono:

+ `DISP_LOG_LEVEL=Debug` imposta la registrazione del modulo del dispatcher a livello di debug
   + Il valore predefinito è: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` imposta la registrazione del modulo di riscrittura del server Web Apache HTTP a livello di debug
   + Il valore predefinito è: `Warn`
+ `DISP_RUN_MODE` imposta la &quot;modalità di esecuzione&quot; dell&#39;ambiente Dispatcher, caricando i file di configurazione del Dispatcher delle modalità di esecuzione corrispondenti.
   + Impostazione predefinita `dev`
+ Valori validi: `dev`, `stage` o `prod`

Uno o più parametri possono essere passati a `docker_run`

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*Questo video utilizza macOS per scopi illustrativi. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

## Quando aggiornare Dispatcher Tools{#dispatcher-tools-version}

Le versioni di Dispatcher Tools incrementano meno frequentemente del Experience Manager , pertanto Dispatcher Tools richiede meno aggiornamenti nell&#39;ambiente di sviluppo locale.

La versione consigliata di Dispatcher Tools è quella fornita con il AEM come SDK di Cloud Service che corrisponde all&#39;Experience Manager di  come versione di Cloud Service. La versione di AEM come Cloud Service si trova tramite [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambienti__, per l&#39;ambiente specificato dal  __AEM__ Release

![Versione Experience Manager ](./assets/dispatcher-tools/aem-version.png)

_La versione di Dispatcher Tools non corrisponderà a quella dell&#39;Experience Manager ._

## Risoluzione dei problemi

### docker_run restituisce &#39;In attesa che host.docker.internal sia disponibile&#39; message{#troubleshooting-host-docker-internal}

`host.docker.internal` è un nome host fornito al Docker che contiene che risolve all&#39;host. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Dal Docker 18.03 in poi la nostra raccomandazione è di connettersi al nome DNS speciale host.docker.internal, che risolve all&#39;indirizzo IP interno utilizzato dall&#39;host

Se, quando `bin/docker_run out host.docker.internal:4503 8080` restituisce il messaggio __In attesa che host.docker.internal sia disponibile__, allora:

1. Verificare che la versione installata di Docker sia 18.03 o successiva
2. È possibile che sia configurato un computer locale che impedisce la registrazione/risoluzione del nome `host.docker.internal`. Utilizzate l&#39;IP locale.
   + Windows:
      + Dal prompt dei comandi, eseguire `ipconfig` e registrare l&#39; __Indirizzo IPv4 dell&#39;host__ del computer host.
      + Quindi eseguire `docker_run` utilizzando questo indirizzo IP:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + Dal terminale, eseguire `ifconfig` e registrare l&#39;indirizzo IP dell&#39;host __inet__, in genere il dispositivo __en0__.
      + Quindi eseguire `docker_run` utilizzando l&#39;indirizzo IP host:
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### Esempio di errore

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run restituisce l&#39;errore &#39;**: Cartella di distribuzione non trovata

Durante l&#39;esecuzione di `docker_run.cmd`, viene visualizzato un errore che riporta l&#39;errore __**: Cartella di distribuzione non trovata:__. Ciò si verifica in genere perché nel percorso sono presenti spazi. Se possibile, rimuovete gli spazi nella cartella oppure spostate la cartella `aem-sdk` in un percorso che non contenga spazi.

Ad esempio, le cartelle utente di Windows sono spesso `<First name> <Last name>`, con uno spazio intermedio. Nell&#39;esempio seguente la cartella `...\My User\...` contiene uno spazio che interrompe l&#39;esecuzione degli strumenti di dispatcher locali `docker_run`. Se gli spazi si trovano in una cartella utente di Windows, non tentare di rinominare la cartella in quanto interromperà Windows, ma spostare la cartella `aem-sdk` in una nuova posizione in cui l&#39;utente dispone delle autorizzazioni per modificarla completamente. Le istruzioni che presuppongono che la cartella `aem-sdk` si trovi nella directory principale dell&#39;utente dovranno essere regolate in base al nuovo percorso.

#### Esempio di errore

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run non viene avviato in Windows{#troubleshooting-windows-compatible}

L&#39;esecuzione di `docker_run` in Windows può causare il seguente errore, impedendo l&#39;avvio del Dispatcher. Si tratta di un problema segnalato con Dispatcher in Windows e verrà risolto in una versione futura.

#### Esempio di errore

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## Risorse aggiuntive

+ [Scarica AEM SDK](https://experience.adobe.com/#/downloads)
+ [ Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Download Docker](https://www.docker.com/)
+ [Scaricate il sito Web di riferimento AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentazione sul dispatcher di  Experience Manager](https://docs.adobe.com/content/help/it-IT/experience-manager-dispatcher/using/dispatcher.html)
