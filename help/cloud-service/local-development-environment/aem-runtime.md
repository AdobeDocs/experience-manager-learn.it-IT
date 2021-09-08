---
title: Configurare Local AEM Runtime per AEM come sviluppo di un Cloud Service
description: Imposta Local AEM Runtime utilizzando il file Jar Quickstart dell'SDK di AEM come Cloud Service.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 2%

---


# Configurare AEM Runtime locale

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Runtime AEM locale"
>abstract="Adobe Experience Manager (AEM) può essere eseguito localmente utilizzando il file Jar Quickstart dell’SDK di AEM as a Cloud Service. Questo consente agli sviluppatori di implementare e testare il codice personalizzato, la configurazione e i contenuti prima di passare al controllo del codice sorgente e di distribuirli in un ambiente di Cloud Service AEM."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="SDK di AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html" text="Download di AEM come SDK per Cloud Service"

Adobe Experience Manager (AEM) può essere eseguito localmente utilizzando il file Jar Quickstart dell’SDK di AEM as a Cloud Service. Questo consente agli sviluppatori di implementare e testare il codice personalizzato, la configurazione e i contenuti prima di passare al controllo del codice sorgente e di distribuirli in un ambiente di Cloud Service AEM.

Tenere presente che `~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows, è l’equivalente di `%HOMEPATH%`.

## Installa Java

Experience Manager è un’applicazione Java e quindi richiede l’SDK Java per supportare gli strumenti di sviluppo.

1. [Scarica e installa l&#39;SDK Java più recente 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifica che Java 11 SDK sia installato eseguendo il comando :
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Scaricare l’AEM come SDK di Cloud Service

L’AEM come SDK di Cloud Service, o SDK AEM, contiene il Jar Quickstart utilizzato per eseguire AEM Author e Publish localmente per lo sviluppo, nonché la versione compatibile degli strumenti di Dispatcher.

1. Accedi a [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con il tuo Adobe ID
   + Tieni presente che l’organizzazione Adobe __deve__ essere fornita per AEM come Cloud Service per scaricare l’AEM come SDK di Cloud Service.
1. Passa alla scheda __AEM come Cloud Service__
1. Ordina per __Data pubblicazione__ nell&#39;ordine __Decrescente__
1. Fai clic sulla riga dei risultati __AEM SDK__ più recente
1. Rivedi e accetta l&#39;EULA e tocca il pulsante __Scarica__

## Estrai il file JAR Quickstart dal file ZIP dell’SDK AEM

1. Decomprimi il file `aem-sdk-XXX.zip` scaricato

## Configurare il servizio AEM Author locale{#set-up-local-aem-author-service}

Il Servizio locale di authoring di AEM fornisce agli sviluppatori un’esperienza locale che gli autori di contenuti e marketer digitali condivideranno per creare e gestire i contenuti.  AEM Author Service è progettato sia come ambiente di authoring che di anteprima e consente di eseguire la maggior parte delle convalide relative allo sviluppo di funzioni, rendendolo un elemento fondamentale del processo di sviluppo locale.

1. Crea la cartella `~/aem-sdk/author`
1. Copia il file __Quickstart JAR__ in `~/aem-sdk/author` e rinominalo in `aem-author-p4502.jar`
1. Avvia il servizio locale di authoring di AEM eseguendo quanto segue dalla riga di comando:
   + `java -jar aem-author-p4502.jar`
      + Immetti la password dell&#39;amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l&#39;impostazione predefinita per lo sviluppo locale per ridurre la necessità di riconfigurare.

   *non è possibile* avviare il AEM come Cloud Service Quickstart Jar [facendo doppio clic](#troubleshooting-double-click).
1. Accedi al servizio locale di authoring di AEM all’indirizzo [http://localhost:4502](Http://localhost:4502) in un browser web

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

Il servizio di pubblicazione AEM locale fornisce agli sviluppatori l’esperienza locale che gli utenti finali del AEM avranno, ad esempio la navigazione nel sito Web ospitato su AEM. Un servizio di pubblicazione AEM locale è importante in quanto si integra con AEM strumenti di Dispatcher dell&#39;SDK [e consente agli sviluppatori di fumare-testare e perfezionare l&#39;esperienza rivolta all&#39;utente finale.](./dispatcher-tools.md)

1. Crea la cartella `~/aem-sdk/publish`
1. Copia il file __Quickstart JAR__ in `~/aem-sdk/publish` e rinominalo in `aem-publish-p4503.jar`
1. Avvia il servizio AEM Publish locale eseguendo quanto segue dalla riga di comando:
   + `java -jar aem-publish-p4503.jar`
      + Immetti la password dell&#39;amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l&#39;impostazione predefinita per lo sviluppo locale per ridurre la necessità di riconfigurare.

   *non è possibile* avviare il AEM come Cloud Service Quickstart Jar [facendo doppio clic](#troubleshooting-double-click).
1. Accedi al servizio di pubblicazione AEM locale all&#39;indirizzo [http://localhost:4503](Http://localhost:4503) in un browser web

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

## Simula distribuzione dei contenuti {#content-distribution}

In un ambiente di Cloud Service reale, il contenuto viene distribuito dal servizio Author al servizio Publish utilizzando [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) e la pipeline Adobe. La [pipeline di Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) è un microservizio isolato disponibile solo nell&#39;ambiente cloud.

Durante lo sviluppo, può essere opportuno simulare la distribuzione dei contenuti utilizzando il servizio locale Author e Publish . Questo può essere ottenuto abilitando gli agenti di replica legacy.

>[!NOTE]
>
> Gli agenti di replica sono disponibili solo per l’utilizzo nel JAR di Quickstart locale e forniscono solo una simulazione della distribuzione dei contenuti.

1. Accedi al servizio **Author** e passa a [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Fare clic su **Agente predefinito (pubblicare)** per aprire l&#39;agente di replica predefinito.
1. Fai clic su **Modifica** per aprire la configurazione dell&#39;agente.
1. Nella scheda **Impostazioni** , aggiorna i campi seguenti:

   + **Abilitato**  - verifica true
   + **ID utente agente** : lasciare vuoto questo campo

   ![Configurazione dell&#39;agente di replica - Impostazioni](assets/aem-runtime/settings-config.png)

1. Nella scheda **Trasporto** , aggiorna i campi seguenti:

   + **URI**  -  `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Utente**  -  `admin`
   + **Password** -  `admin`

   ![Configurazione dell&#39;agente di replica - Trasporto](assets/aem-runtime/transport-config.png)

1. Fai clic su **Ok** per salvare la configurazione e abilitare l&#39;agente di replica **Predefinito**.
1. È ora possibile apportare modifiche al contenuto del servizio Author e pubblicarlo nel servizio Publish.

![Pubblica pagina](assets/aem-runtime/publish-page-changes.png)

## Modalità di avvio di Jar Quickstart

La denominazione del Jar Quickstart `aem-<tier>_<environment>-p<port number>.jar` specifica come verrà avviato. Una volta AEM come avviato in un livello specifico, autore o pubblicazione, non può essere modificato nel livello alternativo. A questo scopo, la cartella `crx-Quickstart` generata durante la prima esecuzione deve essere eliminata e Quickstart Jar deve essere eseguito di nuovo. L’ambiente e le porte possono essere modificate, tuttavia richiedono l’arresto/avvio dell’istanza AEM locale.

La modifica degli ambienti `dev`, `stage` e `prod` può essere utile per gli sviluppatori al fine di garantire che le configurazioni specifiche per l’ambiente siano definite correttamente e risolte da AEM. Si consiglia di eseguire lo sviluppo locale principalmente rispetto alla modalità di esecuzione predefinita dell&#39;ambiente `dev` .

Le permutazioni disponibili sono le seguenti:

+ `aem-author-p4502.jar`
   + Autore in modalità di esecuzione Dev sulla porta 4502
+ `aem-author_dev-p4502.jar`
   + Autore in modalità di esecuzione Dev sulla porta 4502 (come `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Autore in modalità di esecuzione temporanea sulla porta 4502
+ `aem-author_prod-p4502.jar`
   + Autore in modalità di esecuzione produzione sulla porta 4502
+ `aem-publish-p4503.jar`
   + Autore in modalità di esecuzione Dev sulla porta 4503
+ `aem-publish_dev-p4503.jar`
   + Autore in modalità di esecuzione Dev sulla porta 4503 (come `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + Autore in modalità di esecuzione temporanea sulla porta 4503
+ `aem-publish_prod-p4503.jar`
   + Autore in modalità di esecuzione produzione sulla porta 4503

Tieni presente che il numero di porta può essere qualsiasi porta disponibile sulla macchina di sviluppo locale, tuttavia per convenzione:

+ La porta __4502__ viene utilizzata per il __servizio AEM Author locale__
+ La porta __4503__ viene utilizzata per il __servizio di pubblicazione AEM locale__

La modifica di queste impostazioni potrebbe richiedere modifiche AEM configurazioni SDK

## Arresto di un runtime AEM locale

Per arrestare un runtime di AEM locale, sia il servizio AEM Author che Publish, apri la finestra della riga di comando utilizzata per avviare il runtime di AEM e tocca `Ctrl-C`. Attendi AEM lo spegnimento. Al termine del processo di arresto, sarà disponibile il prompt della riga di comando.

## Attività di configurazione del runtime di AEM locale opzionali

+ __Le variabili dell’ambiente di configurazione OSGi e__ le variabili segrete sono  [appositamente impostate per il runtime](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development) locale AEM, anziché gestirle utilizzando aio CLI.

## Quando aggiornare Quickstart Jar

Aggiorna l&#39;SDK AEM almeno mensilmente l&#39;ultimo giovedì di ogni mese, o poco dopo, che è la frequenza di rilascio per AEM come Cloud Service &quot;rilascio di funzioni&quot;.

>[!WARNING]
>
> L’aggiornamento del Jar Quickstart a una nuova versione richiede la sostituzione dell’intero ambiente di sviluppo locale, con la conseguente perdita di tutto il codice, la configurazione e il contenuto negli archivi AEM locali. Assicurati che qualsiasi codice, configurazione o contenuto che non deve essere distrutto sia correttamente impegnato in Git o esportato dall&#39;istanza AEM locale come Pacchetti AEM.

### Come evitare la perdita di contenuto durante l&#39;aggiornamento dell&#39;SDK AEM

L’aggiornamento dell’SDK di AEM sta creando in modo efficace un nuovo runtime AEM, incluso un nuovo archivio, il che significa che tutte le modifiche apportate all’archivio di un SDK di AEM precedente andranno perse. Le seguenti sono strategie valide per aiutare a mantenere i contenuti tra gli aggiornamenti dell’SDK AEM e possono essere utilizzate in modo discreto o di concerto:

1. Crea un pacchetto di contenuti dedicato a contenere contenuti di &quot;esempio&quot; per facilitarne lo sviluppo e mantenilo in Git. I contenuti che devono essere mantenuti tramite AEM aggiornamenti SDK verranno memorizzati in questo pacchetto e ridistribuiti dopo l&#39;aggiornamento dell&#39;SDK AEM.
1. Utilizza [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la direttiva `includepaths` per copiare il contenuto dall&#39;archivio SDK AEM precedente al nuovo archivio SDK AEM.
1. Esegui il backup di qualsiasi contenuto utilizzando AEM Package Manager e i pacchetti di contenuto nell’SDK AEM precedente e reinstallali nel nuovo SDK AEM.

Ricorda che l’utilizzo degli approcci di cui sopra per mantenere il codice tra AEM aggiornamenti dell’SDK indica un pattern di sviluppo non conforme. Il codice non usa e getta deve originarsi nell&#39;IDE di sviluppo e fluire nell&#39;SDK AEM tramite implementazioni.

## Risoluzione dei problemi

## Facendo doppio clic sul file Jar Quickstart si verifica un errore{#troubleshooting-double-click}

Quando fai doppio clic sul Jar Quickstart per avviare, viene visualizzato un modale di errore che impedisce l’avvio locale di AEM.

![Risoluzione dei problemi - Fai doppio clic sul file Jar Quickstart](./assets/aem-runtime/troubleshooting__double-click.png)

Questo perché AEM come Cloud Service Quickstart Jar non supporta il doppio clic del Jar Quickstart per avviare AEM localmente. Devi invece eseguire il file Jar da quella riga di comando.

Per avviare il servizio AEM Author, `cd` nella directory contenente il file Jar Quickstart ed esegui il comando:

`$ java -jar aem-author-p4502.jar`

oppure, per avviare il servizio di pubblicazione AEM, `cd` nella directory contenente il file Jar Quickstart ed esegui il comando:

`$ java -jar aem-publish-p4503.jar`

## L&#39;avvio del Jar Quickstart dalla riga di comando interrompe immediatamente{#troubleshooting-java-8}

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

Questo perché AEM come Cloud Service richiede Java SDK 11 e stai eseguendo una versione diversa, molto probabilmente Java 8. Per risolvere questo problema, scarica e installa [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Una volta installato Java SDK 11, verifica che sia la versione attiva eseguendo quanto segue dalla riga di comando.

Una volta installato Java 11 SDK, verifica che sia la versione attiva eseguendo il comando dalla riga di comando:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Risorse aggiuntive

+ [Scaricare AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Download Docker](https://www.docker.com/)
+ [Documentazione di Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it)
