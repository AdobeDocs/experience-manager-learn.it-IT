---
title: Impostare strumenti Dispatcher per AEM sviluppo as a Cloud Service
description: AEM strumenti Dispatcher dell’SDK facilita lo sviluppo locale dei progetti Adobe Experience Manager (AEM) semplificando l’installazione, l’esecuzione e la risoluzione dei problemi di Dispatcher localmente.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 3%

---

# Impostare strumenti Dispatcher locali {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Strumenti del Dispatcher locale"
>abstract="Dispatcher è parte integrante dell’architettura Experience Manager complessiva e deve far parte della configurazione dello sviluppo locale. L’SDK AEM as a Cloud Service include la versione consigliata degli strumenti di Dispatcher, che facilita la configurazione, la convalida e la simulazione locale di Dispatcher."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="Dispatcher nel cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html" text="Scaricare AEM SDK as a Cloud Service"

Dispatcher di Adobe Experience Manager (AEM) è un modulo server web Apache HTTP che fornisce una protezione e prestazioni tra il livello CDN e AEM Publish. Dispatcher è parte integrante dell’architettura Experience Manager complessiva e deve far parte della configurazione dello sviluppo locale.

L’SDK AEM as a Cloud Service include la versione consigliata degli strumenti di Dispatcher, che facilita la configurazione, la convalida e la simulazione locale di Dispatcher. Gli strumenti di Dispatcher sono formati da:

+ un set di base di file di configurazione del server web Apache HTTP e del Dispatcher, situato in `.../dispatcher-sdk-x.x.x/src`
+ uno strumento CLI di convalida della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/validate`
+ uno strumento CLI di generazione della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/validator`
+ uno strumento CLI per la distribuzione della configurazione, situato in `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ un’immagine Docker che esegue il server Web Apache HTTP con il modulo Dispatcher

Tieni presente che `~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows, è l&#39;equivalente di `%HOMEPATH%`.

>[!NOTE]
>
> I video in questa pagina sono stati registrati su macOS. Gli utenti Windows possono seguire questa procedura, ma utilizzare i comandi Windows equivalenti di Dispatcher Tools, forniti con ogni video.

## Prerequisiti

1. Gli utenti Windows devono utilizzare Windows 10 Professional (o una versione che supporti Docker)
1. Installa [Experience Manager Publish Quickstart Jar](./aem-runtime.md) sulla macchina di sviluppo locale.
   + Se necessario, installa l&#39;ultima [Sito web di riferimento AEM](https://github.com/adobe/aem-guides-wknd/releases) sul servizio AEM Publish locale. Questo sito web viene utilizzato in questa esercitazione per visualizzare un’istanza di Dispatcher funzionante.
1. Installa e avvia la versione più recente di [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) sulla macchina di sviluppo locale.

## Download degli strumenti di Dispatcher (come parte dell’SDK AEM)

L’SDK AEM as a Cloud Service, o SDK AEM, contiene gli strumenti del Dispatcher utilizzati per eseguire il server Web Apache HTTP con il modulo Dispatcher localmente per lo sviluppo, nonché il Jar QuickStart compatibile.

Se l&#39;SDK as a Cloud Service AEM è già stato scaricato in [imposta il runtime AEM locale](./aem-runtime.md), non è necessario scaricarlo nuovamente.

1. Accedi a [experience.adobe.com/#/download](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=1) con il tuo Adobe ID
   + La tua organizzazione Adobe __deve__ è disponibile il provisioning per AEM as a Cloud Service per scaricare l’SDK as a Cloud Service AEM
1. Fai clic sull&#39;ultima __SDK AEM__ riga dei risultati da scaricare

## Estrarre gli strumenti di Dispatcher dallo zip AEM SDK

>[!TIP]
>
> Gli utenti Windows non possono contenere spazi o caratteri speciali nel percorso della cartella contenente gli strumenti Dispatcher locali. Se nel percorso sono presenti spazi, la `docker_run.cmd` fallirà.

La versione di Strumenti Dispatcher è diversa da quella dell’SDK AEM. Assicurati che la versione di Dispatcher Tools sia fornita tramite la versione SDK AEM che corrisponde alla versione as a Cloud Service AEM.

1. Decomprimi il file scaricato `aem-sdk-xxx.zip` file
1. Estrai gli strumenti del Dispatcher in `~/aem-sdk/dispatcher`
   + Windows: Decomprimi `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (creazione delle cartelle mancanti in base alle esigenze)
   + macOS / Linux: Esegui lo script shell associato `aem-sdk-dispatcher-tools-x.x.x-unix.sh` per decomprimere gli strumenti Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Tutti i comandi emessi di seguito presuppongono che la directory di lavoro corrente contenga il contenuto degli strumenti Dispatcher in espansione.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Questo video utilizza macOS a scopo illustrativo. I comandi Windows/Linux equivalenti possono essere utilizzati per ottenere risultati simili*

## Comprendere i file di configurazione del Dispatcher

>[!TIP]
> Experience Manager di progetti creati dalla [Archetipo AEM progetto Maven](https://github.com/adobe/aem-project-archetype) sono precompilati questo set di file di configurazione di Dispatcher, pertanto non è necessario copiarlo dalla cartella src di Dispatcher Tools.

Gli strumenti Dispatcher forniscono un set di file di configurazione del server Web Apache HTTP e del Dispatcher che definiscono il comportamento per tutti gli ambienti, incluso lo sviluppo locale.

Questi file devono essere copiati in un progetto Experience Manager Maven `dispatcher/src` se non esistono già nel progetto Experience Manager Maven.

Una descrizione completa dei file di configurazione è disponibile negli strumenti Dispatcher decompressi come `dispatcher-sdk-x.x.x/docs/Config.html`.

## Convalidare le configurazioni

Facoltativamente, le configurazioni del Dispatcher e del server web Apache (tramite `httpd -t`) può essere convalidata utilizzando `validate` script (da non confondere con `validator` eseguibile). La `validate` lo script offre un modo pratico di eseguire [3 fasi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode) del `validator`.

+ Utilizzo:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate.sh ./src`

## Eseguire il Dispatcher localmente

AEM Dispatcher viene eseguito localmente utilizzando Docker contro `src` File di configurazione del Dispatcher e del server Web Apache.

+ Utilizzo:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

La `<aem-publish-host>` può essere impostato su `host.docker.internal`, un Docker di nome DNS speciale fornisce nel contenitore che viene risolto nell&#39;IP del computer host. Se `host.docker.internal` non risolve, vedi [risoluzione](#troubleshooting-host-docker-internal) di seguito.

Ad esempio, per avviare il contenitore Docker Dispatcher utilizzando i file di configurazione predefiniti forniti dagli strumenti Dispatcher:

Avvia il contenitore Docker del Dispatcher fornendo il percorso della cartella src di configurazione del Dispatcher:

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS / Linux: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

Il servizio Publish Service dell’SDK di AEM, eseguito localmente sulla porta 4503, sarà disponibile tramite Dispatcher all’indirizzo `http://localhost:8080`.

Per eseguire gli strumenti di Dispatcher rispetto alla configurazione del Dispatcher di un progetto di Experience Manager, fai riferimento al `dispatcher/src` cartella.

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Registri degli strumenti di Dispatcher

I registri del Dispatcher sono utili durante lo sviluppo locale per capire se e perché le richieste HTTP sono bloccate. Il livello di log può essere impostato prefissando l&#39;esecuzione di `docker_run` con parametri di ambiente.

I registri degli strumenti del Dispatcher vengono emessi in modalità standard quando `docker_run` viene eseguito.

I parametri utili per il debug di Dispatcher includono:

+ `DISP_LOG_LEVEL=Debug` imposta la registrazione del modulo Dispatcher a livello di debug
   + Il valore predefinito è: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` imposta la registrazione del modulo di riscrittura del server Web HTTP Apache a livello di debug
   + Il valore predefinito è: `Warn`
+ `DISP_RUN_MODE` imposta la &quot;modalità di esecuzione&quot; dell’ambiente Dispatcher, caricando i file di configurazione del Dispatcher delle modalità di esecuzione corrispondenti.
   + Impostazione predefinita `dev`
+ Valori validi: `dev`, `stage`oppure `prod`

Uno o più parametri possono essere passati a `docker_run`

+ Windows:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

### Accesso ai file di registro

I registri del server web Apache e del Dispatcher AEM sono accessibili direttamente nel contenitore Docker:

+ [Accesso ai registri nel contenitore Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copiare i log Docker nel file system locale](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quando aggiornare gli strumenti di Dispatcher{#dispatcher-tools-version}

Le versioni degli strumenti di Dispatcher incrementano meno frequentemente dell’Experience Manager, pertanto gli strumenti di Dispatcher richiedono meno aggiornamenti nell’ambiente di sviluppo locale.

La versione consigliata degli strumenti di Dispatcher è quella fornita con l’SDK as a Cloud Service AEM che corrisponde alla versione as a Cloud Service dell’Experience Manager. La versione di AEM as a Cloud Service può essere trovata tramite [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambienti__, per ambiente specificato dal __Versione AEM__ etichetta

![Versione Experience Manager](./assets/dispatcher-tools/aem-version.png)

_La versione stessa di Dispatcher Tools non corrisponde alla versione di Experience Manager._

## Risoluzione dei problemi

### docker_run restituisce il messaggio &#39;In attesa che host.docker.internal sia disponibile&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` è un nome host fornito al Docker contenente che viene risolto nell&#39;host. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> A partire da Docker 18.03 il nostro consiglio è di connettersi al nome DNS speciale host.docker.internal, che si risolve all&#39;indirizzo IP interno utilizzato dall&#39;host

Se, quando `bin/docker_run src host.docker.internal:4503 8080` restituisce il messaggio __In attesa che host.docker.internal sia disponibile__, quindi:

1. Verificare che la versione installata di Docker sia 18.03 o successiva
2. È possibile che si disponga di una macchina locale che impedisce la registrazione/risoluzione del `host.docker.internal` nome. Utilizza invece il tuo IP locale.
   + Windows:
      + Dal prompt dei comandi, esegui `ipconfig`e registra il __Indirizzo IPv4__ della macchina host.
      + Quindi, esegui `docker_run` utilizzando questo indirizzo IP:
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS / Linux:
      + Da terminale, esegui `ifconfig` e registra l&#39;host __inet__ Indirizzo IP, in genere __en0__ dispositivo.
      + Quindi esegui `docker_run` utilizzando l&#39;indirizzo IP host:
         `bin/docker_run.sh src <HOST IP>:4503 8080`

#### Esempio di errore

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run non si avvia su Windows{#troubleshooting-windows-compatible}

In esecuzione `docker_run` in Windows può verificarsi il seguente errore, impedendo l’avvio di Dispatcher. Si tratta di un problema segnalato con Dispatcher su Windows e verrà risolto in una versione futura.

#### Esempio di errore

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run src host.docker.internal:4503 8080

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
