---
title: Registri
description: I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM in AEM as a Cloud Service, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 3%

---

# Debug di AEM as a Cloud Service tramite i registri

I registri fungono da strumenti di prima linea per il debug delle applicazioni AEM in AEM as a Cloud Service, ma dipendono dalla registrazione adeguata nell’applicazione AEM implementata.

Tutte le attività di registro per il servizio AEM di un dato ambiente (Author, Publish/Publish Dispatcher) sono consolidate in un unico file di registro, anche se i pod diversi all’interno di tale servizio generano le istruzioni di registro.

Gli ID dei pod vengono forniti in ogni istruzione di registro e consentono di filtrare o fascicolare le istruzioni di registro. Gli ID pod sono nel formato di:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Esempio: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## File di registro personalizzati

I Cloud Service AEM non supportano i file di registro personalizzati, ma la registrazione personalizzata.

Per i registri Java da rendere disponibili in AEM as a Cloud Service (tramite [Cloud Manager](#cloud-manager) o [CLI ADOBE I/O](#aio)), le istruzioni di registro personalizzate devono essere scritte nel `error.log`. Registri scritti in registri denominati personalizzati, ad esempio `example.log`, non sarà accessibile da AEM as a Cloud Service.

I registri possono essere scritti in `error.log` utilizzo di una proprietà di configurazione Sling LogManager OSGi nel file `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` file.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Registri del servizio di authoring e pubblicazione AEM

I servizi di authoring e pubblicazione AEM forniscono i registri del server di runtime AEM:

+ `aemerror` è il registro degli errori Java (disponibile all’indirizzo `/crx-quickstart/logs/error.log` nell’SDK per AEM (avvio rapido locale). Di seguito sono riportati i [livelli di registro consigliati](#log-levels) per i logger personalizzati per tipo di ambiente:
   + Sviluppo: `DEBUG`
   + Ambiente di staging: `WARN`
   + Produzione: `ERROR`
+ `aemaccess` elenca le richieste HTTP al servizio AEM con i relativi dettagli
+ `aemrequest` elenca le richieste HTTP effettuate al servizio AEM e la relativa risposta HTTP

## Registri di Dispatcher per la pubblicazione AEM

Solo il Dispatcher di pubblicazione dell’AEM fornisce il server web Apache e i registri di Dispatcher, in quanto questi aspetti esistono solo nel livello di pubblicazione dell’AEM e non nel livello di creazione dell’AEM.

+ `httpdaccess` elenca le richieste HTTP effettuate al server web Apache/Dispatcher del servizio AEM.
+ `httperror`  elenca i messaggi di registro dal server web Apache e fornisce assistenza per il debug dei moduli Apache supportati, come `mod_rewrite`.
   + Sviluppo: `DEBUG`
   + Ambiente di staging: `WARN`
   + Produzione: `ERROR`
+ `aemdispatcher` elenca i messaggi di registro provenienti dai moduli di Dispatcher, inclusi il filtraggio e la trasmissione dei messaggi dalla cache.
   + Sviluppo: `DEBUG`
   + Ambiente di staging: `WARN`
   + Produzione: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager consente di scaricare i registri, per giorno, tramite l’azione Scarica registri di un ambiente.

![Cloud Manager - Download dei registri](./assets/logs/download-logs.png)

Questi registri possono essere scaricati e ispezionati tramite qualsiasi strumento di analisi dei registri.

## Adobe I/O di CLI con il plug-in Cloud Manager{#aio}

Adobe Cloud Manager supporta l’accesso ai registri AEM as a Cloud Service tramite [CLI ADOBE I/O](https://github.com/adobe/aio-cli) con [Plug-in di Cloud Manager per CLI Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

In primo luogo, [configurare l’Adobe I/O con il plug-in di Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Assicurati di aver identificato l’ID del programma e l’ID dell’ambiente pertinenti e utilizza [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) per elencare le opzioni di registro utilizzate per [tail](#aio-cli-tail-logs) o [scaricare](#aio-cli-download-logs) log.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### Registri di coda{#aio-cli-tail-logs}

Adobe I/O CLI consente di visualizzare i registri in tempo reale da AEM as a Cloud Service utilizzando [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) comando. La coda è utile per osservare l’attività di registro in tempo reale mentre le azioni vengono eseguite nell’ambiente as a Cloud Service dell’AEM.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Altri strumenti per riga di comando, ad esempio `grep` può essere utilizzato insieme a `tail-logs` per isolare le dichiarazioni di registro di interesse, ad esempio:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... visualizza solo le istruzioni di registro generate da `com.example.MySlingModel` o contengono quella stringa.

### Download dei registri{#aio-cli-download-logs}

Adobe I/O CLI consente di scaricare i registri dall’AEM as a Cloud Service utilizzando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Questo fornisce lo stesso risultato finale del download dei registri dall’interfaccia utente web di Cloud Manager, con la differenza che `download-logs` Il comando consolida i registri tra giorni, in base al numero di giorni richiesti per i registri.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Informazioni sui registri

I registri in AEM as a Cloud Service hanno più pod che scrivono istruzioni di registro in essi. Poiché più istanze AEM scrivono nello stesso file di registro, è importante comprendere come analizzare e ridurre i disturbi durante il debug. Per spiegare, le seguenti operazioni `aemerror` viene utilizzato lo snippet di registro:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Utilizzando gli ID del pod, il punto dati dopo la data e l’ora, i registri possono essere fascicolati dal pod o dall’istanza AEM all’interno del servizio, facilitando la tracciatura e la comprensione dell’esecuzione del codice.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Livelli di registro consigliati{#log-levels}

Le linee guida generali di Adobe sui livelli di registro per l’ambiente AEM as a Cloud Service sono:

+ Sviluppo locale (AEM SDK): `DEBUG`
+ Sviluppo: `DEBUG`
+ Ambiente di staging: `WARN`
+ Produzione: `ERROR`

Impostando il livello di registro più appropriato per ogni tipo di ambiente con AEM as a Cloud Service, i livelli di registro vengono mantenuti nel codice

+ Le configurazioni del registro Java vengono mantenute nelle configurazioni OSGi
+ Livelli del server web Apache e del registro di Dispatcher nel progetto dispatcher

...e quindi richiedono una distribuzione per cambiare.

### Variabili specifiche dell’ambiente per impostare i livelli di registro Java

Un’alternativa all’impostazione di livelli di registro Java statici noti per ogni ambiente consiste nell’utilizzare l’AEM Cloud Service come livello di [variabili specifiche per l’ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) per parametrizzare i livelli di log, consentendo la modifica dinamica dei valori tramite [Adobe I/O di CLI con il plug-in Cloud Manager](#aio-cli).

A tal fine, è necessario aggiornare le configurazioni OSGi di registrazione per utilizzare segnaposto di variabili specifici per l’ambiente. [Valori predefiniti](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) per i livelli di registro deve essere impostato come da [Adobe di raccomandazioni](#log-levels). Ad esempio:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Questo approccio presenta aspetti negativi che devono essere presi in considerazione:

+ [È consentito un numero limitato di variabili di ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables), e la creazione di una variabile per gestire il livello di registro ne utilizzerà una.
+ Le variabili di ambiente possono essere gestite a livello di programmazione tramite [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [CLI ADOBE I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), e [API HTTP di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Le modifiche alle variabili di ambiente devono essere reimpostate manualmente da uno strumento supportato. Dimenticare di reimpostare un ambiente con traffico elevato, come quello di produzione, su un livello di registro meno dettagliato può inondare i registri e influire sulle prestazioni dell’AEM.

_Le variabili specifiche dell’ambiente non funzionano per le configurazioni del server web Apache o del registro di Dispatcher, in quanto non sono configurate tramite la configurazione OSGi._
