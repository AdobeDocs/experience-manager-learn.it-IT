---
title: Imposta AEM esecuzione locale per AEM come sviluppo Cloud Service
description: Imposta il runtime AEM locale utilizzando il AEM come Jar Quickstart dell'SDK Cloud Service.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 1%

---


# Imposta AEM esecuzione locale

Adobe Experience Manager (AEM) può essere eseguito localmente utilizzando il AEM come Jar Quickstart dell&#39;SDK di Cloud Service. Questo consente agli sviluppatori di implementare e testare codice, configurazione e contenuto personalizzati prima di impegnarli nel controllo del codice sorgente e distribuirli in un AEM come ambiente di Cloud Service.

Si noti che `~` viene utilizzato come abbreviazione per la Directory dell&#39;utente. In Windows, questo è l&#39;equivalente di `%HOMEPATH%`.

>[!VIDEO](https://video.tv.adobe.com/v/32551/?quality=12&learn=on)

>[!NOTE]
>
> Questo video mostra come installare ed eseguire un’istanza locale di Adobe Experience Manager in pochi minuti con il quickstart locale dell’SDK AEM. In questo video viene mostrato l&#39;avvio locale dell&#39;AEM SDK facendo doppio clic sul file Jar dell&#39;SDK, ma questo non funzionerà in Java 8 è installato nel computer. In alternativa, puoi avviare il quickstart locale dell’SDK AEM dalla riga di comando utilizzando il `java -jar ...` comando [descritto in questa pagina](#set-up-local-aem-author-service).

## Installare Java

 Experience Manager è un&#39;applicazione Java e richiede quindi Java SDK per supportare lo strumento di sviluppo.

1. [Scarica e installa la versione più recente di Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr cr%3AlastModified&amp;order.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifica che Java 11 SDK sia installato eseguendo il comando:
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Download del AEM come SDK di Cloud Service

Il AEM come SDK di Cloud Service, o SDK AEM, contiene la Jar di avvio rapido utilizzata per eseguire AEM Author e Publish in locale per lo sviluppo, nonché la versione compatibile degli strumenti Dispatcher.

1. Accedete a [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con il vostro Adobe ID 
   + Per scaricare il AEM come Cloud Service SDK, è __necessario__ effettuare il provisioning per l&#39;organizzazione  Adobe come Cloud Service per AEM.
1. Passare alla __AEM come scheda Cloud Service__
1. Ordina per data ____ pubblicazione in ordine __decrescente__
1. Fai clic sulla riga di risultati SDK ____ AEM più recente
1. Rivedete e accettate l&#39;EULA, quindi toccate il pulsante __Scarica__

## Estrarre il file Jar di Quickstart dal file ZIP dell’SDK di AEM

1. Decomprimete il `aem-sdk-XXX.zip` file scaricato
1. Assicurati che il file __license.properties__ sviluppatore  Experience Manager sia disponibile

Tenete presente che gli stessi file Jar e license.properties vengono utilizzati per avviare _sia_ AEM Author Services che Publish Services.

## Configurare il servizio AEM Author locale{#set-up-local-aem-author-service}

Il servizio locale AEM Author Service offre agli sviluppatori un&#39;esperienza locale che gli autori di contenuti e marketing digitali condivideranno per creare e gestire i contenuti.  AEM Author Service è stato progettato sia come ambiente di authoring che come ambiente di anteprima, consentendo di eseguire la maggior parte delle convalide per lo sviluppo di funzioni su di esso, rendendolo un elemento fondamentale del processo di sviluppo locale.

1. Creare la cartella `~/aem-sdk/author`
1. Copiate il file JAR ____ Quickstart in `~/aem-sdk/author` e rinominatelo in `aem-author-p4502.jar`
1. Copiare il file __license.properties__ in  `~/aem-sdk/author`
1. Avviate il servizio AEM Author locale eseguendo le operazioni seguenti dalla riga di comando:
   + `java -jar aem-author-p4502.jar`
      + Immettete la password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare la password predefinita per lo sviluppo locale per ridurre la necessità di riconfigurarla.

   Non *potete* avviare il AEM come Cloud Service Jar QuickStart [facendo doppio clic](#troubleshooting-double-click).
1. Accedete al servizio AEM Author locale all&#39;indirizzo [http://localhost:4502](Http://localhost:4502) in un browser Web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\author
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cp ../license.properties ~/aem-sdk/author
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Configurare il servizio AEM Publish locale

Il servizio AEM Publish Service locale offre agli sviluppatori l&#39;esperienza locale che gli utenti finali del AEM avranno, ad esempio, esplorando il sito Web ospitato sul AEM. Un servizio AEM Publish Service locale è importante in quanto si integra con AEM strumenti [](./dispatcher-tools.md) Dispatcher dell&#39;SDK e consente agli sviluppatori di effettuare test del fumo e ottimizzare l&#39;esperienza finale dell&#39;utente finale.

1. Creare la cartella `~/aem-sdk/publish`
1. Copiate il file JAR ____ Quickstart in `~/aem-sdk/publish` e rinominatelo in `aem-publish-p4503.jar`
1. Copiare il file __license.properties__ in  `~/aem-sdk/publish`
1. Avviate il servizio AEM Publish locale eseguendo le operazioni seguenti dalla riga di comando:
   + `java -jar aem-publish-p4503.jar`
      + Immettete la password amministratore come `admin`. Qualsiasi password amministratore è accettabile, tuttavia si consiglia di utilizzare la password predefinita per lo sviluppo locale per ridurre la necessità di riconfigurarla.

   Non *potete* avviare il AEM come Cloud Service Jar QuickStart [facendo doppio clic](#troubleshooting-double-click).
1. Accedete al servizio AEM Publish Service locale all&#39;indirizzo [http://localhost:4503](Http://localhost:4503) in un browser Web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\publish
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cp ../license.properties ~/aem-sdk/publish
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Modalità di avvio rapido Jar

La denominazione del Jar Quickstart `aem-<tier>_<environment>-p<port number>.jar` specifica come verrà avviato. Una volta AEM iniziato in un livello specifico, autore o pubblicazione, non è possibile modificarlo nel livello alternativo. A tal fine, è necessario eliminare la `crx-Quickstart` cartella generata durante la prima esecuzione e ripetere l&#39;esecuzione di Quickstart Jar. Ambiente e porte possono essere modificati, ma richiedono l&#39;arresto/avvio dell&#39;istanza AEM locale.

La modifica di ambienti `dev``stage` e `prod`di ambienti può essere utile per gli sviluppatori per garantire che le configurazioni specifiche per l&#39;ambiente siano definite correttamente e risolte da AEM. Si consiglia di eseguire lo sviluppo locale principalmente in base alla modalità di esecuzione `dev` dell&#39;ambiente predefinita.

Le permutazioni disponibili sono le seguenti:

+ `aem-author-p4502.jar`
   + Come autore in modalità di esecuzione Dev sulla porta 4502
+ `aem-author_dev-p4502.jar`
   + Come autore in modalità di esecuzione Dev sulla porta 4502 (come `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Come autore in modalità di esecuzione temporanea sulla porta 4502
+ `aem-author_prod-p4502.jar`
   + Come autore in modalità di esecuzione produzione sulla porta 4502
+ `aem-publish-p4503.jar`
   + Come autore in modalità di esecuzione Dev sulla porta 4503
+ `aem-publish_dev-p4503.jar`
   + Come autore in modalità di esecuzione Dev sulla porta 4503 (come `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + Come autore in modalità di esecuzione temporanea sulla porta 4503
+ `aem-publish_prod-p4503.jar`
   + Autore in modalità di esecuzione produzione sulla porta 4503

Si noti che il numero di porta può essere qualsiasi porta disponibile sulla macchina di sviluppo locale, comunque per convenzione:

+ La porta __4502__ è utilizzata per il servizio AEM Author __locale__
+ La porta __4503__ è utilizzata per il servizio AEM Publish __locale__

La modifica di questi parametri potrebbe richiedere modifiche AEM configurazioni SDK

## Arresto di un runtime AEM locale

Per arrestare un runtime AEM locale, AEM Author o Publish Service, aprite la finestra della riga di comando utilizzata per avviare il runtime di AEM e toccate `Ctrl-C`. Attendi AEM spegnimento. Al termine del processo di arresto, sarà disponibile il prompt della riga di comando.

## Quando aggiornare la Jar di Quickstart

Aggiorna l’SDK AEM almeno mensilmente l’ultimo giovedì di ogni mese, o poco dopo, corrispondente alla scadenza del rilascio per AEM come &quot;rilascio di funzionalità&quot; Cloud Service.

>[!WARNING]
>
> L&#39;aggiornamento di Quickstart Jar a una nuova versione richiede la sostituzione dell&#39;intero ambiente di sviluppo locale, con la conseguente perdita di tutto il codice, la configurazione e il contenuto nei repository AEM locali. Verificate che il codice, la configurazione o il contenuto che non deve essere distrutto sia correttamente associato a Git o esportato dall&#39;istanza AEM locale come AEM Packages.

### Come evitare la perdita di contenuto durante l&#39;aggiornamento dell&#39;SDK AEM

L’aggiornamento dell’SDK AEM crea in modo efficace un nuovo runtime AEM, incluso un nuovo repository, il che significa che tutte le modifiche apportate all’archivio AEM SDK precedente vanno perse. Di seguito sono illustrate strategie utili per aiutare a mantenere il contenuto tra AEM aggiornamenti SDK e possono essere utilizzate in modo discreto o concertato:

1. Create un pacchetto di contenuti dedicato a contenere il contenuto &quot;campione&quot; per facilitarne lo sviluppo e mantenetelo in Git. Il contenuto che deve essere mantenuto attraverso AEM aggiornamenti SDK verrà mantenuto in questo pacchetto e ridistribuito dopo l&#39;aggiornamento dell&#39;SDK AEM.
1. Utilizzate [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la `includepaths` direttiva per copiare il contenuto dall&#39;archivio SDK precedente AEM al nuovo archivio SDK AEM.
1. Eseguite il backup di qualsiasi contenuto utilizzando AEM Package Manager e pacchetti di contenuto nell&#39;SDK AEM precedente, quindi reinstallateli nel nuovo SDK AEM.

Ricorda che l&#39;uso dei metodi indicati sopra per mantenere il codice tra AEM aggiornamenti SDK, indica un anti-pattern di sviluppo. Il codice non usa e getta deve essere originato dall’IDE di sviluppo e fluire AEM SDK tramite distribuzioni.

## Risoluzione dei problemi

## Facendo doppio clic sul file Jar Quickstart si verifica un errore{#troubleshooting-double-click}

Quando si fa doppio clic sulla Jar di avvio rapido per avviare l&#39;avvio, viene visualizzato un errore modale che impedisce l&#39;avvio AEM locale.

![Risoluzione dei problemi - Fare doppio clic sul file Jar di Quickstart](./assets/aem-runtime/troubleshooting__double-click.png)

Questo perché AEM come Cloud Service Quickstart Jar non supporta il doppio clic della Jar Quickstart per avviare AEM localmente. È invece necessario eseguire il file Jar da tale riga di comando.

Per avviare il servizio AEM Author, `cd` accedete alla directory contenente il Jar di Quickstart ed eseguite il comando:

`$ java -jar aem-author-p4502.jar`

oppure, per avviare il servizio AEM Publish, `cd` nella directory contenente il Jar di Quickstart ed eseguire il comando:

`$ java -jar aem-author-p4503.jar`

## L&#39;avvio del Jar Quickstart dalla riga di comando interrompe immediatamente{#troubleshooting-java-8}

Quando si avvia il Jar Quickstart dalla riga di comando, il processo si interrompe immediatamente e il servizio AEM non si avvia, con il seguente errore:

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

Questo perché AEM come Cloud Service richiede Java SDK 11 ed è in esecuzione una versione diversa, probabilmente Java 8. Per risolvere questo problema, scaricate e installate [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr cr%3AlastModified&amp;order.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Una volta installato Java SDK 11, verifica che sia la versione attiva eseguendo quanto segue dalla riga di comando.

Una volta installato Java 11 SDK, verifica che sia la versione attiva eseguendo il comando dalla riga di comando:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Risorse aggiuntive

+ [Scarica AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Download Docker](https://www.docker.com/)
+ [Documentazione sul dispatcher di  Experience Manager](https://docs.adobe.com/content/help/it-IT/experience-manager-dispatcher/using/dispatcher.html)
