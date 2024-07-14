---
title: Configurare l’SDK AEM locale per lo sviluppo AEM as a Cloud Service
description: Configura il runtime dell’SDK AEM locale utilizzando Quickstart Jar dell’SDK di AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 7%

---

# Configurare l’SDK AEM locale {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Runtime AEM locale"
>abstract="Adobe Experience Manager (AEM) può essere eseguito localmente utilizzando il file Quickstart Jar dell’SDK di AEM as a Cloud Service. Consente agli sviluppatori di distribuire e testare il codice, la configurazione e i contenuti personalizzati prima di passare al controllo del codice sorgente e di distribuirli in un ambiente AEM as a Cloud Service."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=it" text="SDK di AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html" text="Scarica l’SDK di AEM as a Cloud Service"

Adobe Experience Manager (AEM) può essere eseguito localmente utilizzando il file Quickstart Jar dell’SDK di AEM as a Cloud Service. Consente agli sviluppatori di distribuire e testare il codice, la configurazione e i contenuti personalizzati prima di passare al controllo del codice sorgente e di distribuirli in un ambiente AEM as a Cloud Service.

`~` viene utilizzato come abbreviazione per la directory dell&#39;utente. In Windows equivale a `%HOMEPATH%`.

## Installare Java™

Experience Manager è un’applicazione Java™ e pertanto richiede Oracle Java™ SDK per supportare gli strumenti di sviluppo.

1. [Scarica e installa il più recente Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifica che Oracle Java™ 11 SDK sia installato eseguendo il comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## Scaricare l’SDK di AEM as a Cloud Service

L’SDK per AEM as a Cloud Service, o SDK per AEM, contiene il file Jar Quickstart utilizzato per eseguire AEM Author e Publish localmente per lo sviluppo, nonché la versione compatibile degli strumenti Dispatcher.

1. Accedi a [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con il tuo Adobe ID
   + Tieni presente che per scaricare l&#39;SDK di AEM as a Cloud Service è necessario eseguire il provisioning della tua organizzazione Adobe __1} per AEM as a Cloud Service.__
1. Passa alla scheda __AEM as a Cloud Service__
1. Ordina per __Data pubblicazione__ in ordine __decrescente__
1. Fai clic sulla riga dei risultati __AEM SDK__ più recente
1. Rivedi e accetta il Contratto di licenza con l&#39;utente finale e tocca il pulsante __Scarica__

## Estrarre il file JAR Quickstart dallo zip dell’SDK dell’AEM

1. Decomprimi il file `aem-sdk-XXX.zip` scaricato

## Configurare il servizio Autore AEM locale{#set-up-local-aem-author-service}

Il servizio di authoring AEM locale offre agli sviluppatori un’esperienza locale che gli esperti di marketing digitale e gli autori di contenuti condivideranno per creare e gestire i contenuti.  Il servizio di authoring AEM è progettato sia come ambiente di authoring che come ambiente di anteprima, consentendo di eseguire la maggior parte delle convalide dello sviluppo delle funzioni in base a esso, rendendolo un elemento fondamentale del processo di sviluppo locale.

1. Crea la cartella `~/aem-sdk/author`
1. Copia il file __Quickstart JAR__ in `~/aem-sdk/author` e rinominalo in `aem-author-p4502.jar`
1. Avvia il servizio di authoring AEM locale eseguendo quanto segue dalla riga di comando:
   + `java -jar aem-author-p4502.jar`
      + Specificare la password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l’impostazione predefinita per lo sviluppo locale per ridurre la necessità di riconfigurare.

   *impossibile* avviare l&#39;AEM come JAR Quickstart di Cloud Service [facendo doppio clic](#troubleshooting-double-click).
1. Accedi al servizio di authoring AEM locale all&#39;indirizzo [http://localhost:4502](Http://localhost:4502) in un browser Web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## Configurazione del servizio Publish AEM locale

Il servizio Publish per l’AEM locale offre agli sviluppatori l’esperienza locale di cui dispongono gli utenti finali dell’AEM, ad esempio navigando nel sito web ospitato sull’AEM. Un servizio Publish AEM locale è importante in quanto si integra con gli [strumenti Dispatcher](./dispatcher-tools.md) dell&#39;SDK AEM e consente agli sviluppatori di testare e perfezionare l&#39;esperienza finale per l&#39;utente finale.

1. Crea la cartella `~/aem-sdk/publish`
1. Copia il file __Quickstart JAR__ in `~/aem-sdk/publish` e rinominalo in `aem-publish-p4503.jar`
1. Avviare il servizio Publish AEM locale eseguendo quanto segue dalla riga di comando:
   + `java -jar aem-publish-p4503.jar`
      + Specificare la password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare l’impostazione predefinita per lo sviluppo locale per ridurre la necessità di riconfigurare.

   *impossibile* avviare l&#39;AEM come JAR Quickstart di Cloud Service [facendo doppio clic](#troubleshooting-double-click).
1. Accedere al servizio Publish AEM locale all&#39;indirizzo [http://localhost:4503](http://localhost:4503) in un browser Web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## Configurare i servizi AEM locali in modalità prerelease

Il runtime AEM locale può essere avviato in [modalità prerelease](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=it) per consentire a uno sviluppatore di eseguire la generazione in base alle funzionalità della versione successiva di AEM as a Cloud Service. La versione prerelease è abilitata passando l&#39;argomento `-r prerelease` al primo avvio del runtime AEM locale. Può essere utilizzato sia con i servizi Publish locali di AEM Author che con quelli AEM.


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux®]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## Simula distribuzione contenuto {#content-distribution}

In un ambiente di Cloud Service reale, il contenuto viene distribuito dal servizio di authoring al servizio Publish utilizzando [Distribuzione contenuto Sling](https://sling.apache.org/documentation/bundles/content-distribution.html) e la pipeline di Adobe. La [pipeline Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) è un microservizio isolato disponibile solo nell&#39;ambiente cloud.

Durante lo sviluppo, può essere opportuno simulare la distribuzione dei contenuti utilizzando il servizio Author e Publish locale. Ciò può essere ottenuto abilitando gli agenti di replica legacy.

>[!NOTE]
>
> Gli agenti di replica sono disponibili solo per l’utilizzo nel file JAR Quickstart locale e forniscono solo una simulazione della distribuzione dei contenuti.

1. Accedi al servizio **Author** e passa a [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Fare clic su **Agente predefinito (pubblicazione)** per aprire l&#39;agente di replica predefinito.
1. Fai clic su **Modifica** per aprire la configurazione dell&#39;agente.
1. Nella scheda **Impostazioni**, aggiorna i campi seguenti:

   + **Abilitato** - verifica true
   + **ID utente agente** - Lascia vuoto questo campo

   ![Configurazione agente di replica - Impostazioni](assets/aem-runtime/settings-config.png)

1. Nella scheda **Trasporto**, aggiorna i campi seguenti:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Utente** - `admin`
   + **Password** - `admin`

   ![Configurazione agente di replica - Trasporto](assets/aem-runtime/transport-config.png)

1. Fare clic su **Ok** per salvare la configurazione e abilitare l&#39;agente di replica **Predefinito**.
1. Ora puoi apportare modifiche ai contenuti nel servizio di authoring e pubblicarli nel servizio di Publish.

![Pagina Publish](assets/aem-runtime/publish-page-changes.png)

## Modalità di avvio di Quickstart Jar

La denominazione del file JAR Quickstart `aem-<tier>_<environment>-p<port number>.jar` specifica la modalità di avvio. Una volta avviato in un livello specifico, di authoring o pubblicazione, l’AEM non può essere modificato nel livello alternativo. A questo scopo, la cartella `crx-Quickstart` generata durante la prima esecuzione deve essere eliminata e Quickstart Jar deve essere eseguito nuovamente. L’ambiente e le porte possono essere modificati, ma richiedono l’arresto/avvio dell’istanza AEM locale.

La modifica degli ambienti, `dev`, `stage` e `prod`, può essere utile per gli sviluppatori per garantire che le configurazioni specifiche dell&#39;ambiente siano definite e risolte correttamente dall&#39;AEM. Si consiglia di eseguire lo sviluppo locale principalmente sulla modalità di esecuzione predefinita dell&#39;ambiente `dev`.

Le permutazioni disponibili sono le seguenti:

| Nome file JAR Quickstart | Descrizione modalità |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Come Autore in modalità di esecuzione Dev sulla porta 4502 |
| `aem-author_dev-p4502.jar` | Come Autore in modalità di esecuzione Dev sulla porta 4502 (come `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Autore in modalità di esecuzione Gestione temporanea sulla porta 4502 |
| `aem-author_prod-p4502.jar` | Come Autore in modalità di esecuzione Produzione sulla porta 4502 |
| `aem-publish-p4503.jar` | Come Publish in modalità di esecuzione Dev sulla porta 4503 |
| `aem-publish_dev-p4503.jar` | Come Publish in modalità di esecuzione Dev sulla porta 4503 (come `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Come Publish in modalità di esecuzione staging sulla porta 4503 |
| `aem-publish_prod-p4503.jar` | Come Publish in modalità di esecuzione Produzione sulla porta 4503 |

Il numero di porta può essere una qualsiasi porta disponibile sul computer di sviluppo locale, tuttavia per convenzione:

+ Porta __4502__ utilizzata per il __servizio Autore AEM locale__
+ Porta __4503__ utilizzata per il __servizio Publish AEM locale__

Cambiarli potrebbe richiedere modifiche alle configurazioni dell’SDK dell’AEM

## Arresto di un runtime AEM locale

Per arrestare un runtime AEM locale, sia il servizio di creazione AEM che il servizio di Publish, aprire la finestra della riga di comando utilizzata per avviare il runtime AEM, quindi toccare `Ctrl-C`. Attendere la chiusura dell&#39;AEM. Al termine del processo di arresto, è disponibile il prompt dei comandi.

## Attività di configurazione runtime AEM locali opzionali

+ __Le variabili dell&#39;ambiente di configurazione OSGi e le variabili segrete__ sono [impostate appositamente per il runtime locale dell&#39;AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development), anziché gestirle utilizzando CLI dell&#39;aio.

## Quando aggiornare Quickstart Jar

Aggiorna l’SDK dell’AEM almeno mensilmente l’ultimo giovedì di ogni mese, o poco dopo, cioè la data di rilascio delle &quot;versioni di funzioni&quot; di AEM as a Cloud Service.

>[!WARNING]
>
> L’aggiornamento del file JAR Quickstart a una nuova versione richiede la sostituzione dell’intero ambiente di sviluppo locale, con conseguente perdita di codice, configurazione e contenuto negli archivi AEM locali. Assicurati che qualsiasi codice, configurazione o contenuto che non deve essere eliminato sia salvato in modo sicuro in Git o esportato dall’istanza AEM locale come pacchetti AEM.

### Come evitare la perdita di contenuti durante l’aggiornamento dell’SDK per AEM

L’aggiornamento dell’SDK dell’AEM crea effettivamente un nuovo runtime dell’AEM, incluso un nuovo archivio, il che significa che tutte le modifiche apportate a un precedente archivio dell’SDK dell’AEM vengono perse. Di seguito sono riportate alcune valide strategie per contribuire a mantenere contenuti permanenti tra gli aggiornamenti dell’SDK AEM e possono essere utilizzate in modo discreto o insieme:

1. Crea un pacchetto di contenuti dedicato a contenere contenuti di &quot;esempio&quot; per facilitare lo sviluppo e mantienilo in Git. Tutti i contenuti che devono essere mantenuti tramite gli aggiornamenti dell’SDK AEM verranno mantenuti in questo pacchetto e ridistribuiti dopo l’aggiornamento dell’SDK AEM.
1. Utilizza [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la direttiva `includepaths` per copiare il contenuto dal precedente archivio dell&#39;SDK AEM al nuovo archivio dell&#39;SDK AEM.
1. Esegui il backup di qualsiasi contenuto utilizzando Gestione pacchetti AEM e pacchetti di contenuti nell’SDK AEM precedente e reinstallali nel nuovo SDK AEM.

Ricorda che l’utilizzo degli approcci di cui sopra per mantenere il codice tra gli aggiornamenti dell’SDK AEM indica un anti-pattern di sviluppo. Il codice non monouso deve avere origine nell’IDE di sviluppo e fluire nell’SDK dell’AEM tramite le implementazioni.

## Risoluzione dei problemi

### Se si fa doppio clic sul file Jar Quickstart, viene generato un errore{#troubleshooting-double-click}

Quando si fa doppio clic sul file JAR Quickstart per iniziare, viene visualizzato un messaggio di errore modale che impedisce l’avvio dell’AEM a livello locale.

![Risoluzione dei problemi - Fare doppio clic sul file JAR Quickstart](./assets/aem-runtime/troubleshooting__double-click.png)

Questo perché AEM as a Cloud Service Quickstart Jar non supporta il doppio clic sul file JAR Quickstart per avviare l’AEM a livello locale. È invece necessario eseguire il file Jar da tale riga di comando.

Per avviare il servizio di creazione AEM, `cd` nella directory contenente il file JAR Quickstart ed eseguire il comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

In alternativa, per avviare il servizio Publish AEM, `cd` nella directory contenente il file JAR Quickstart ed eseguire il comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]

### L’avvio del file JAR Quickstart dalla riga di comando si interrompe immediatamente{#troubleshooting-java-8}

Quando si avvia Quickstart Jar dalla riga di comando, il processo si interrompe immediatamente e il servizio AEM non si avvia, con il seguente errore:

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

Questo perché AEM as a Cloud Service richiede Java™ SDK 11 e stai eseguendo una versione diversa, molto probabilmente Java™ 8. Per risolvere il problema, scaricare e installare [Oracle Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).

Una volta installato l’SDK Oracle Java™ 11, verifica che si tratti della versione attiva eseguendo il comando dalla riga di comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

## Risorse aggiuntive

+ [Scarica SDK AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Scarica Docker](https://www.docker.com/)
+ [Documentazione di Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it)
