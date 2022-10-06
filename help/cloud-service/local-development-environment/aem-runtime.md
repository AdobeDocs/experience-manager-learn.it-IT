---
title: Configurare AEM runtime locale per lo sviluppo AEM as a Cloud Service
description: Imposta Local AEM Runtime utilizzando il Jar Quickstart dell'SDK AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1800'
ht-degree: 3%

---

# Configurare AEM Runtime locale {#set-up-local-aem-runtime}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Runtime AEM locale"
>abstract="Adobe Experience Manager (AEM) può essere eseguito localmente utilizzando il Jar Quickstart dell’SDK AEM as a Cloud Service. Questo consente agli sviluppatori di implementare e testare il codice personalizzato, la configurazione e i contenuti prima di passare al controllo del codice sorgente e di distribuirli in un ambiente AEM as a Cloud Service."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=it" text="SDK di AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html" text="Scaricare AEM SDK as a Cloud Service"

Adobe Experience Manager (AEM) può essere eseguito localmente utilizzando il Jar Quickstart dell’SDK AEM as a Cloud Service. Questo consente agli sviluppatori di implementare e testare il codice personalizzato, la configurazione e i contenuti prima di passare al controllo del codice sorgente e di distribuirli in un ambiente AEM as a Cloud Service.

Tieni presente che `~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows, è l&#39;equivalente di `%HOMEPATH%`.

## Installa Java

Experience Manager è un’applicazione Java e quindi richiede l’SDK Java per supportare gli strumenti di sviluppo.

1. [Scarica e installa l&#39;SDK Java più recente 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifica che Java 11 SDK sia installato eseguendo il comando :
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Scaricare l’SDK AEM as a Cloud Service

L’SDK AEM as a Cloud Service, o SDK di AEM, contiene il Jar Quickstart utilizzato per eseguire AEM Author e Publish localmente per lo sviluppo, nonché la versione compatibile degli strumenti di Dispatcher.

1. Accedi a [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con il tuo Adobe ID
   + Tieni presente che la tua organizzazione Adobe __deve__ è stato effettuato il provisioning per AEM as a Cloud Service per scaricare l’SDK as a Cloud Service AEM.
1. Passa a __AEM as a Cloud Service__ scheda
1. Ordina per __Data di pubblicazione__ in __Decrescente__ ordine
1. Fai clic sull&#39;ultima __SDK AEM__ riga risultato
1. Rivedi e accetta l’EULA e tocca il __Scarica__ pulsante

## Estrai il file JAR Quickstart dal file ZIP dell’SDK AEM

1. Decomprimi il file scaricato `aem-sdk-XXX.zip` file

## Configurare il servizio AEM Author locale{#set-up-local-aem-author-service}

Il Servizio locale di authoring di AEM fornisce agli sviluppatori un’esperienza locale che gli autori di contenuti e marketer digitali condivideranno per creare e gestire i contenuti.  AEM Author Service è progettato sia come ambiente di authoring che di anteprima e consente di eseguire la maggior parte delle convalide relative allo sviluppo di funzioni, rendendolo un elemento fondamentale del processo di sviluppo locale.

1. Crea la cartella `~/aem-sdk/author`
1. Copia il __JAR per avvio rapido__ file a  `~/aem-sdk/author` e rinominalo in `aem-author-p4502.jar`
1. Avvia il servizio locale di authoring di AEM eseguendo quanto segue dalla riga di comando:
   + `java -jar aem-author-p4502.jar`
      + Fornisci la password dell’amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l&#39;impostazione predefinita per lo sviluppo locale per ridurre la necessità di riconfigurare.

   You *impossibile* avvia il AEM come Jar Quickstart Cloud Service [facendo doppio clic](#troubleshooting-double-click).
1. Accedi al servizio locale di authoring di AEM all’indirizzo [http://localhost:4502](Http://localhost:4502) in un browser Web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Configurare il servizio AEM Publish locale

Il servizio di pubblicazione AEM locale fornisce agli sviluppatori l’esperienza locale che gli utenti finali del AEM avranno, ad esempio la navigazione nel sito Web ospitato su AEM. Un servizio AEM Publish locale è importante in quanto si integra con AEM SDK [Strumenti di Dispatcher](./dispatcher-tools.md) e consente agli sviluppatori di fumare-test e di ottimizzare l&#39;esperienza finale rivolta all&#39;utente finale.

1. Crea la cartella `~/aem-sdk/publish`
1. Copia il __JAR per avvio rapido__ file a  `~/aem-sdk/publish` e rinominalo in `aem-publish-p4503.jar`
1. Avvia il servizio AEM Publish locale eseguendo quanto segue dalla riga di comando:
   + `java -jar aem-publish-p4503.jar`
      + Fornisci la password dell’amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l&#39;impostazione predefinita per lo sviluppo locale per ridurre la necessità di riconfigurare.

   You *impossibile* avvia il AEM come Jar Quickstart Cloud Service [facendo doppio clic](#troubleshooting-double-click).
1. Accedi al servizio AEM Publish locale all&#39;indirizzo [http://localhost:4503](Http://localhost:4503) in un browser Web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Configurare i servizi AEM locali in modalità prerelease

Il runtime AEM locale può essere avviato in [modalità prerelease](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=it) consente a uno sviluppatore di creare in base alle funzioni della versione successiva di AEM as a Cloud Service. Prerelease viene attivato passando il `-r prerelease` argomento sul primo avvio del runtime di AEM locale. Può essere utilizzato sia con i servizi AEM Author locali che con AEM Publish.

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

## Simula distribuzione dei contenuti {#content-distribution}

In un ambiente di Cloud Service reale il contenuto viene distribuito da Author Service a Publish Service tramite [Distribuzione dei contenuti Sling](https://sling.apache.org/documentation/bundles/content-distribution.html) e la pipeline di Adobe. La [Pipeline di Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) è un microservizio isolato disponibile solo nell’ambiente cloud.

Durante lo sviluppo, può essere opportuno simulare la distribuzione dei contenuti utilizzando il servizio locale Author e Publish . Questo può essere ottenuto abilitando gli agenti di replica legacy.

>[!NOTE]
>
> Gli agenti di replica sono disponibili solo per l’utilizzo nel JAR di Quickstart locale e forniscono solo una simulazione della distribuzione dei contenuti.

1. Accedi a **Autore** assistenza e passa a [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Fai clic su **Agente predefinito (pubblicazione)** per aprire l&#39;agente di replica predefinito.
1. Fai clic su **Modifica** per aprire la configurazione dell&#39;agente.
1. Sotto la **Impostazioni** , aggiorna i campi seguenti:

   + **Abilitato** - verifica true
   + **ID utente agente** - Lascia vuoto questo campo

   ![Configurazione dell&#39;agente di replica - Impostazioni](assets/aem-runtime/settings-config.png)

1. Sotto la **Trasporti** , aggiorna i campi seguenti:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Utente** - `admin`
   + **Password** - `admin`

   ![Configurazione dell&#39;agente di replica - Trasporto](assets/aem-runtime/transport-config.png)

1. Fai clic su **Ok** per salvare la configurazione e abilitare il **Predefinito** Agente di replica.
1. È ora possibile apportare modifiche al contenuto del servizio Author e pubblicarlo nel servizio Publish.

![Pubblica pagina](assets/aem-runtime/publish-page-changes.png)

## Modalità di avvio di Jar Quickstart

Il nome del file JAR di Quickstart, `aem-<tier>_<environment>-p<port number>.jar` specifica come verrà avviato. Una volta AEM come avviato in un livello specifico, autore o pubblicazione, non può essere modificato nel livello alternativo. Per eseguire questa operazione, `crx-Quickstart` la cartella generata durante la prima esecuzione deve essere eliminata e Quickstart Jar deve essere eseguito di nuovo. L’ambiente e le porte possono essere modificate, tuttavia richiedono l’arresto/avvio dell’istanza AEM locale.

Modifica degli ambienti `dev`, `stage` e `prod`, può essere utile per gli sviluppatori per garantire che le configurazioni specifiche per l’ambiente siano definite correttamente e risolte da AEM. Si consiglia di eseguire lo sviluppo locale principalmente contro l&#39;impostazione predefinita `dev` modalità di esecuzione dell&#39;ambiente.

Le permutazioni disponibili sono le seguenti:

| Nome file JAR di Quickstart | Descrizione del modo |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Autore in modalità di esecuzione Dev sulla porta 4502 |
| `aem-author_dev-p4502.jar` | Come autore in modalità di esecuzione Dev sulla porta 4502 (come `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Autore in modalità di esecuzione temporanea sulla porta 4502 |
| `aem-author_prod-p4502.jar` | Autore in modalità di esecuzione produzione sulla porta 4502 |
| `aem-publish-p4503.jar` | Come pubblicazione in modalità di esecuzione Dev sulla porta 4503 |
| `aem-publish_dev-p4503.jar` | Come Pubblica in modalità di esecuzione Dev sulla porta 4503 (come `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Come pubblicare in modalità di esecuzione temporanea sulla porta 4503 |
| `aem-publish_prod-p4503.jar` | Come pubblicazione in modalità di esecuzione produzione sulla porta 4503 |

Tieni presente che il numero di porta può essere qualsiasi porta disponibile sulla macchina di sviluppo locale, tuttavia per convenzione:

+ Porta __4502__ viene utilizzato per __servizio locale di authoring di AEM__
+ Porta __4503__ viene utilizzato per __servizio AEM Publish locale__

La modifica di queste impostazioni potrebbe richiedere modifiche AEM configurazioni SDK

## Arresto di un runtime AEM locale

Per arrestare un runtime di AEM locale, sia il servizio AEM Author che Publish, apri la finestra della riga di comando utilizzata per avviare il runtime di AEM e tocca `Ctrl-C`. Attendi AEM lo spegnimento. Al termine del processo di arresto, è disponibile il prompt della riga di comando.

## Attività di configurazione del runtime di AEM locale opzionali

+ __Variabili dell&#39;ambiente di configurazione OSGi e variabili segrete__ sono [appositamente impostato per il runtime locale AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development), invece di gestirli utilizzando aio CLI.

## Quando aggiornare Quickstart Jar

Aggiorna l&#39;SDK AEM almeno mensilmente l&#39;ultimo giovedì di ogni mese, o poco dopo, che è la frequenza di rilascio per AEM &quot;release di funzioni&quot; as a Cloud Service.

>[!WARNING]
>
> L’aggiornamento del Jar Quickstart a una nuova versione richiede la sostituzione dell’intero ambiente di sviluppo locale, con la conseguente perdita di tutto il codice, la configurazione e il contenuto negli archivi AEM locali. Assicurati che qualsiasi codice, configurazione o contenuto che non deve essere distrutto sia correttamente impegnato in Git o esportato dall&#39;istanza AEM locale come Pacchetti AEM.

### Come evitare la perdita di contenuto durante l&#39;aggiornamento dell&#39;SDK AEM

L’aggiornamento dell’SDK di AEM sta creando in modo efficace un nuovo runtime AEM, incluso un nuovo archivio, il che significa che tutte le modifiche apportate all’archivio di un SDK di AEM precedente andranno perse. Le seguenti sono strategie valide per aiutare a mantenere i contenuti tra gli aggiornamenti dell’SDK AEM e possono essere utilizzate in modo discreto o di concerto:

1. Crea un pacchetto di contenuti dedicato a contenere contenuti di &quot;esempio&quot; per facilitarne lo sviluppo e mantenilo in Git. I contenuti che devono essere mantenuti tramite AEM aggiornamenti SDK verranno memorizzati in questo pacchetto e ridistribuiti dopo l&#39;aggiornamento dell&#39;SDK AEM.
1. Utilizzo [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con `includepaths` , per copiare il contenuto dall&#39;archivio SDK AEM precedente al nuovo archivio SDK AEM.
1. Esegui il backup di qualsiasi contenuto utilizzando AEM Package Manager e i pacchetti di contenuto nell’SDK AEM precedente e reinstallali nel nuovo SDK AEM.

Ricorda che l’utilizzo degli approcci di cui sopra per mantenere il codice tra AEM aggiornamenti dell’SDK indica un pattern di sviluppo non conforme. Il codice non usa e getta deve originarsi nell&#39;IDE di sviluppo e fluire nell&#39;SDK AEM tramite implementazioni.

## Risoluzione dei problemi

### Facendo doppio clic sul file Jar Quickstart si verifica un errore{#troubleshooting-double-click}

Quando fai doppio clic sul Jar Quickstart per avviare, viene visualizzato un modale di errore che impedisce l’avvio locale di AEM.

![Risoluzione dei problemi - Fai doppio clic sul file Jar Quickstart](./assets/aem-runtime/troubleshooting__double-click.png)

Questo perché AEM Jar Quickstart as a Cloud Service non supporta il doppio clic del Jar Quickstart per iniziare a AEM localmente. Devi invece eseguire il file Jar da quella riga di comando.

Per avviare il servizio AEM Author, `cd` nella directory contenente il file JAR di Quickstart ed esegui il comando:

`$ java -jar aem-author-p4502.jar`

o, per avviare il servizio AEM Publish, `cd` nella directory contenente il file JAR di Quickstart ed esegui il comando:

`$ java -jar aem-publish-p4503.jar`

### L&#39;avvio del Jar Quickstart dalla riga di comando interrompe immediatamente{#troubleshooting-java-8}

Quando si avvia il file JAR Quickstart dalla riga di comando, il processo si interrompe immediatamente e il servizio AEM non si avvia, con il seguente errore:

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

Questo perché AEM as a Cloud Service richiede Java SDK 11 e stai eseguendo una versione diversa, molto probabilmente Java 8. Per risolvere il problema, scaricare e installare [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Una volta installato Java SDK 11, verifica che sia la versione attiva eseguendo quanto segue dalla riga di comando.

Una volta installato Java 11 SDK, verifica che sia la versione attiva eseguendo il comando dalla riga di comando:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Risorse aggiuntive

+ [Scaricare AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Download Docker](https://www.docker.com/)
+ [Documentazione di Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it)
