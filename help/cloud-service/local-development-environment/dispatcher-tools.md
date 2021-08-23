---
title: Impostare gli strumenti di Dispatcher per lo sviluppo AEM come Cloud Service
description: AEM strumenti Dispatcher dell’SDK facilita lo sviluppo locale dei progetti Adobe Experience Manager (AEM) semplificando l’installazione, l’esecuzione e la risoluzione dei problemi di Dispatcher localmente.
sub-product: fondazione
feature: Dispatcher, strumenti per sviluppatori
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1637'
ht-degree: 2%

---


# Impostare strumenti Dispatcher locali

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Strumenti del Dispatcher locale"
>abstract="Dispatcher è parte integrante dell’architettura Experience Manager complessiva e deve far parte della configurazione dello sviluppo locale. L’SDK di AEM come Cloud Service include la versione consigliata degli strumenti di Dispatcher, che facilita la configurazione, la convalida e la simulazione locale di Dispatcher."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="Dispatcher nel cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html" text="Download di AEM come SDK per Cloud Service"

Dispatcher di Adobe Experience Manager (AEM) è un modulo server web Apache HTTP che fornisce una protezione e prestazioni tra il livello CDN e AEM Publish. Dispatcher è parte integrante dell’architettura Experience Manager complessiva e deve far parte della configurazione dello sviluppo locale.

L’SDK di AEM come Cloud Service include la versione consigliata degli strumenti di Dispatcher, che facilita la configurazione, la convalida e la simulazione locale di Dispatcher. Gli strumenti di Dispatcher sono formati da:

+ un set di base di file di configurazione del server web Apache HTTP e del Dispatcher, situato in `.../dispatcher-sdk-x.x.x/src`
+ uno strumento CLI per la convalida della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+)
+ uno strumento CLI di generazione della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/validator`
+ uno strumento CLI per la distribuzione della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ un’immagine Docker che esegue il server Web Apache HTTP con il modulo Dispatcher

Tenere presente che `~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows, è l’equivalente di `%HOMEPATH%`.

>[!NOTE]
>
> I video in questa pagina sono stati registrati su macOS. Gli utenti Windows possono seguire questa procedura, ma utilizzare i comandi Windows equivalenti di Dispatcher Tools, forniti con ogni video.

## Prerequisiti

1. Gli utenti Windows devono utilizzare Windows 10 Professional
1. Installa [Experience Manager Publish Quickstart Jar](./aem-runtime.md) sul computer di sviluppo locale.
   + Se necessario, installa l&#39;ultimo [sito Web di riferimento AEM](https://github.com/adobe/aem-guides-wknd/releases) sul servizio AEM Publish locale. Questo sito web viene utilizzato in questa esercitazione per visualizzare un’istanza di Dispatcher funzionante.
1. Installa e avvia la versione più recente di [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) nel computer di sviluppo locale.

## Download degli strumenti di Dispatcher (come parte dell’SDK AEM)

Il AEM come SDK di Cloud Service, o SDK AEM, contiene gli strumenti del Dispatcher utilizzati per eseguire il server Web Apache HTTP con il modulo Dispatcher locale per lo sviluppo, nonché il Jar QuickStart compatibile.

Se l&#39;SDK di AEM come Cloud Service è già stato scaricato in [setup the local AEM runtime](./aem-runtime.md), non è necessario scaricarlo nuovamente.

1. Accedi a [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=1) con il tuo Adobe ID
   + È necessario eseguire il provisioning della tua organizzazione Adobe __per AEM come Cloud Service per scaricare l&#39;AEM come Cloud Service SDK.__
1. Fai clic sulla riga dei risultati dell&#39;SDK __AEM più recente__ da scaricare
   + Verifica che AEM strumenti Dispatcher dell’SDK v2.0.29+ sia indicato nella descrizione del download

## Estrarre gli strumenti di Dispatcher dallo zip AEM SDK

>[!TIP]
>
> Gli utenti Windows non possono contenere spazi o caratteri speciali nel percorso della cartella contenente gli strumenti Dispatcher locali. Se nel percorso sono presenti spazi, il `docker_run.cmd` non riuscirà.

La versione di Strumenti Dispatcher è diversa da quella dell’SDK AEM. Assicurati che la versione di Dispatcher Tools sia fornita tramite la versione SDK AEM che corrisponde al AEM come versione di Cloud Service.

1. Decomprimi il file `aem-sdk-xxx.zip` scaricato
1. Estrai gli strumenti del Dispatcher in `~/aem-sdk/dispatcher`
   + Windows: Decomprimere `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (creando le cartelle mancanti in base alle esigenze)
   + macOS / Linux: Esegui lo script shell associato `aem-sdk-dispatcher-tools-x.x.x-unix.sh` per decomprimere gli strumenti di Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Tutti i comandi emessi di seguito presuppongono che la directory di lavoro corrente contenga il contenuto degli strumenti Dispatcher in espansione.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Questo video utilizza macOS a scopo illustrativo. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

## Comprendere i file di configurazione del Dispatcher

>[!TIP]
> I progetti di Experience Manager creati da [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) vengono precompilati da questo set di file di configurazione del Dispatcher, pertanto non è necessario copiarli dalla cartella src di Dispatcher Tools.

Gli strumenti Dispatcher forniscono un set di file di configurazione del server Web Apache HTTP e del Dispatcher che definiscono il comportamento per tutti gli ambienti, incluso lo sviluppo locale.

Questi file devono essere copiati in un progetto Experience Manager Maven nella cartella `dispatcher/src` , se non sono già presenti nel progetto Experience Manager Maven.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*Questo video utilizza macOS a scopo illustrativo. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

Una descrizione completa dei file di configurazione è disponibile in Strumenti Dispatcher decompressi come `dispatcher-sdk-x.x.x/docs/Config.html`.

## Convalidare le configurazioni

Facoltativamente, le configurazioni del Dispatcher e del server Web Apache (tramite `httpd -t`) possono essere convalidate utilizzando lo script `validate` (da non confondere con l&#39;eseguibile `validator`).

+ Utilizzo:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate.sh ./src`

## Eseguire il Dispatcher localmente

Per eseguire il Dispatcher localmente, i file di configurazione del Dispatcher devono essere generati utilizzando lo strumento CLI `validator` di Dispatcher Tools.

+ Utilizzo:
   + Windows: `bin\validator full -d out src`
   + macOS / Linux: `./bin/validator full -d ./out ./src`

Questo comando trasforma le configurazioni in un set di file compatibile con il server Web Apache HTTP del contenitore Docker.

Una volta generate, le configurazioni trasmesse vengono utilizzate per eseguire Dispatcher localmente nel contenitore Docker. È importante assicurarsi che le configurazioni più recenti siano state convalidate utilizzando l&#39;output `validate` __e__ utilizzando l&#39;opzione `-d` della convalida.

+ Utilizzo:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

Il `aem-publish-host` può essere impostato su `host.docker.internal`, un Docker di nome DNS speciale fornisce nel contenitore che viene risolto nell&#39;IP del computer host. Se `host.docker.internal` non viene risolto, consulta la sezione [risoluzione dei problemi](#troubleshooting-host-docker-internal) di seguito.

Ad esempio, per avviare il contenitore Docker Dispatcher utilizzando i file di configurazione predefiniti forniti dagli strumenti Dispatcher:

1. Genera da zero `deployment-folder`, denominato `out` per convenzione, ogni volta che una configurazione cambia:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS / Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Riavvia il contenitore Docker di Dispatcher fornendo il percorso della cartella di distribuzione:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Il servizio Publish dell’SDK di AEM come Cloud Service, eseguito localmente sulla porta 4503, sarà disponibile tramite Dispatcher all’indirizzo `http://localhost:8080`.

Per eseguire gli strumenti di Dispatcher rispetto alla configurazione del Dispatcher di un progetto di Experience Manager, è sufficiente generare il `deployment-folder` utilizzando la cartella `dispatcher/src` del progetto.

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

*Questo video utilizza macOS a scopo illustrativo. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

## Registri degli strumenti di Dispatcher

I registri del Dispatcher sono utili durante lo sviluppo locale per capire se e perché le richieste HTTP sono bloccate. È possibile impostare il livello di registro prefissando l’esecuzione di `docker_run` con i parametri di ambiente.

I registri degli strumenti del Dispatcher vengono emessi all’uscita standard quando viene eseguito `docker_run`.

I parametri utili per il debug di Dispatcher includono:

+ `DISP_LOG_LEVEL=Debug` imposta la registrazione del modulo Dispatcher a livello di debug
   + Il valore predefinito è: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` imposta la registrazione del modulo di riscrittura del server Web HTTP Apache a livello di debug
   + Il valore predefinito è: `Warn`
+ `DISP_RUN_MODE` imposta la &quot;modalità di esecuzione&quot; dell’ambiente Dispatcher, caricando i file di configurazione del Dispatcher delle modalità di esecuzione corrispondenti.
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

*Questo video utilizza macOS a scopo illustrativo. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

### Accesso ai file di registro

I registri del server web Apache e del Dispatcher AEM sono accessibili direttamente nel contenitore Docker:

+ [Accesso ai registri nel contenitore Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copiare i log Docker nel file system locale](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quando aggiornare gli strumenti di Dispatcher{#dispatcher-tools-version}

Le versioni degli strumenti di Dispatcher incrementano meno frequentemente dell’Experience Manager, pertanto gli strumenti di Dispatcher richiedono meno aggiornamenti nell’ambiente di sviluppo locale.

La versione consigliata di Dispatcher Tools è quella fornita con l’SDK di AEM as a Cloud Service che corrisponde alla versione di Experience Manager as a Cloud Service. La versione di AEM come Cloud Service può essere trovata tramite [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambienti__, per ambiente specificato da  __AEM__ Releaselable

![Versione Experience Manager](./assets/dispatcher-tools/aem-version.png)

_La versione stessa di Dispatcher Tools non corrisponde alla versione di Experience Manager._

## Risoluzione dei problemi

### docker_run restituisce il messaggio &#39;In attesa che host.docker.internal sia disponibile&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` è un nome host fornito al Docker contenente che viene risolto nell&#39;host. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> A partire da Docker 18.03 il nostro consiglio è di connettersi al nome DNS speciale host.docker.internal, che si risolve all&#39;indirizzo IP interno utilizzato dall&#39;host

Se, quando `bin/docker_run out host.docker.internal:4503 8080` restituisce il messaggio __In attesa che host.docker.internal sia disponibile__, allora:

1. Verificare che la versione installata di Docker sia 18.03 o successiva
2. È possibile che sia stata impostata una macchina locale che impedisce la registrazione/risoluzione del nome `host.docker.internal`. Utilizza invece il tuo IP locale.
   + Windows:
      + Dal prompt dei comandi, eseguire `ipconfig` e registrare l&#39;indirizzo __IPv4 dell&#39;host__ del computer host.
      + Quindi, esegui `docker_run` utilizzando questo indirizzo IP:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + Dal terminale, esegui `ifconfig` e registra l&#39;indirizzo IP host __inet__, in genere il dispositivo __en0__.
      + Quindi esegui `docker_run` utilizzando l&#39;indirizzo IP host:
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

Quando si esegue `docker_run.cmd`, viene visualizzato un errore che indica __** errore: Cartella di distribuzione non trovata:__. Ciò si verifica in genere perché nel percorso sono presenti spazi. Se possibile, rimuovere gli spazi nella cartella oppure spostare la cartella `aem-sdk` in un percorso che non contenga spazi.

Ad esempio, le cartelle utente di Windows sono spesso `<First name> <Last name>`, con uno spazio intermedio. Nell’esempio seguente la cartella `...\My User\...` contiene uno spazio che interrompe l’esecuzione degli strumenti Dispatcher locali `docker_run`. Se gli spazi si trovano in una cartella utente di Windows, non tentare di rinominare questa cartella in quanto interromperà Windows, ma spostare la cartella `aem-sdk` in una nuova posizione in cui l&#39;utente dispone delle autorizzazioni per la modifica completa. Le istruzioni che presuppongono che la cartella `aem-sdk` si trovi nella home directory dell&#39;utente devono essere regolate in base alla nuova posizione.

#### Esempio di errore

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run non si avvia su Windows{#troubleshooting-windows-compatible}

L’esecuzione di `docker_run` su Windows può causare il seguente errore, impedendo l’avvio di Dispatcher. Si tratta di un problema segnalato con Dispatcher su Windows e verrà risolto in una versione futura.

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

+ [Scaricare AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Download Docker](https://www.docker.com/)
+ [Scarica il sito web di riferimento AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentazione di Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it)
