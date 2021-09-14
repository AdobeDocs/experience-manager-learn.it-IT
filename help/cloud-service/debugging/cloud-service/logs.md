---
title: Registri
description: I registri fungono da guida per il debug AEM applicazioni in AEM come Cloud Service, ma dipendono dalla registrazione adeguata nell’applicazione AEM distribuita.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 3%

---

# Eseguire il debug AEM come Cloud Service utilizzando i registri

I registri fungono da guida per il debug AEM applicazioni in AEM come Cloud Service, ma dipendono dalla registrazione adeguata nell’applicazione AEM distribuita.

Tutte le attività di registro per il servizio AEM di un determinato ambiente (Author, Publish/Publish Dispatcher) vengono consolidate in un singolo file di registro, anche se i diversi pod all’interno di tale servizio generano le istruzioni di registro.

Gli ID del contenitore vengono forniti in ogni istruzione di registro e consentono di filtrare o raggruppare le istruzioni di registro. Gli ID del contenitore sono nel formato di:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Esempio: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## File di registro personalizzati

AEM come Cloud Services non supporta i file di registro personalizzati, ma supporta la registrazione personalizzata.

Affinché i registri Java siano disponibili in AEM come Cloud Service (tramite [Cloud Manager](#cloud-manager) o [Adobe I/O CLI](#aio)), le istruzioni di registro personalizzate devono essere scritte come `error.log`. I registri scritti in registri con nome personalizzato, ad esempio `example.log`, non saranno accessibili da AEM come Cloud Service.

I registri possono essere scritti in `error.log` utilizzando una proprietà di configurazione Sling LogManager OSGi nei file `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` dell&#39;applicazione.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Log del servizio Author e Publish di AEM

I servizi Author e Publish di AEM forniscono AEM log del server di runtime:

+ `aemerror` è il registro degli errori Java (trovato  `/crx-quickstart/logs/error.log` in AEM SDK local quickstart). Di seguito sono riportati i [livelli di registro consigliati](#log-levels) per i logger personalizzati per tipo di ambiente:
   + Sviluppo: `DEBUG`
   + Stadio: `WARN`
   + Produzione: `ERROR`
+ `aemaccess` elenca le richieste HTTP al servizio AEM con i relativi dettagli
+ `aemrequest` elenca le richieste HTTP effettuate al servizio AEM e la relativa risposta HTTP

## Registri di AEM Publish Dispatcher

Solo AEM Publish Dispatcher fornisce i registri del server web Apache e del Dispatcher, poiché questi aspetti esistono solo nel livello di pubblicazione AEM e non nel livello di authoring AEM.

+ `httpdaccess` elenca le richieste HTTP effettuate al server Web/Dispatcher Apache del servizio AEM.
+ `httperror`  elenca i messaggi di registro dal server web Apache e aiuto con il debug dei moduli Apache supportati, come  `mod_rewrite`.
   + Sviluppo: `DEBUG`
   + Stadio: `WARN`
   + Produzione: `ERROR`
+ `aemdispatcher` elenca i messaggi di registro dai moduli di Dispatcher, inclusi i filtri e il servizio dai messaggi di cache.
   + Sviluppo: `DEBUG`
   + Stadio: `WARN`
   + Produzione: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager consente il download dei registri, per giorno, tramite l’azione Download Logs di un ambiente.

![Cloud Manager - Registri di download](./assets/logs/download-logs.png)

Questi registri possono essere scaricati ed ispezionati tramite qualsiasi strumento di analisi dei log.

## Adobe I/O CLI con il plug-in Cloud Manager{#aio}

Adobe Cloud Manager supporta l’accesso a AEM come registri di Cloud Service tramite l’ [CLI di Adobe I/O](https://github.com/adobe/aio-cli) con il plug-in [Cloud Manager per l’Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Innanzitutto, [configura l&#39;Adobe I/O con il plugin Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Assicurati che l&#39;ID del programma e l&#39;ID dell&#39;ambiente pertinenti siano stati identificati e utilizza [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) per elencare le opzioni di registro utilizzate per i registri [tail](#aio-cli-tail-logs) o [download](#aio-cli-download-logs).

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

### Log di coda{#aio-cli-tail-logs}

Adobe I/O CLI fornisce la possibilità di visualizzare i registri in tempo reale da AEM come Cloud Service utilizzando il comando [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name). La coda è utile per guardare l’attività di log in tempo reale mentre le azioni vengono eseguite sul AEM come ambiente di Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Altri strumenti della riga di comando, come `grep`, possono essere utilizzati insieme a `tail-logs` per aiutare a isolare le dichiarazioni di interesse del registro, ad esempio:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... visualizza solo le istruzioni di registro generate da `com.example.MySlingModel` o contengono tale stringa.

### Download dei registri{#aio-cli-download-logs}

Adobe I/O CLI offre la possibilità di scaricare i registri da AEM come Cloud Service utilizzando il comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Questo fornisce lo stesso risultato finale del download dei registri dall’interfaccia utente web di Cloud Manager, con la differenza che il comando `download-logs` consolida i registri in giorni, in base al numero di giorni di log richiesti.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Informazioni sui registri

Gli accessi AEM come Cloud Service hanno più pod che scrivono le istruzioni di log in essi. Poiché più istanze AEM scrivono nello stesso file di registro, è importante comprendere come analizzare e ridurre il rumore durante il debug. Per spiegare, verrà utilizzato il seguente snippet di registro `aemerror`:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Utilizzando gli ID Pod, il punto dati dopo la data e l’ora, i registri possono essere raggruppati per Pod, o AEM’istanza all’interno del servizio, facilitando il tracciamento e la comprensione dell’esecuzione del codice.

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

Le linee guida generali dell&#39;Adobe sui livelli di log per AEM in un ambiente di Cloud Service sono:

+ Sviluppo locale (AEM SDK): `DEBUG`
+ Sviluppo: `DEBUG`
+ Stadio: `WARN`
+ Produzione: `ERROR`

Impostando il livello di log più appropriato per ogni tipo di ambiente con AEM come Cloud Service, i livelli di log vengono mantenuti nel codice

+ Le configurazioni del registro Java vengono mantenute nelle configurazioni OSGi
+ Livelli di registro del server web Apache e del Dispatcher nel progetto del dispatcher

...e quindi, è necessario un intervento per cambiare.

### Variabili specifiche dell&#39;ambiente per impostare i livelli di registro Java

Un&#39;alternativa all&#39;impostazione dei livelli di registro Java statici ben noti per ogni ambiente è quella di utilizzare AEM come variabili specifiche dell&#39;ambiente [del Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) per parametrizzare i livelli di registro, consentendo la modifica dinamica dei valori tramite l&#39; [Adobe I/O CLI con il plug-in Cloud Manager](#aio-cli).

Questo richiede l&#39;aggiornamento delle configurazioni OSGi per utilizzare i segnaposto variabili specifiche dell&#39;ambiente. [I ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) valori predefiniti per i livelli di registro devono essere impostati come da raccomandazioni  [Adobe](#log-levels). Esempio:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

Questo approccio presenta aspetti negativi che devono essere presi in considerazione:

+ [Sono consentite](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables) un numero limitato di variabili di ambiente e la creazione di una variabile per gestire il livello di registro ne utilizzerà una.
+ Le variabili di ambiente possono essere gestite solo a livello di programmazione tramite [API HTTP Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) o [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Le modifiche alle variabili di ambiente devono essere ripristinate manualmente da uno strumento supportato. Se si dimentica di reimpostare un ambiente di traffico elevato, ad esempio Produzione, a un livello di registro meno dettagliato, i registri potrebbero essere invariati e le prestazioni AEM potrebbero essere influenzate.

_Le variabili specifiche per l’ambiente non funzionano per le configurazioni del server web Apache o del registro del Dispatcher, in quanto non sono configurate tramite la configurazione OSGi._
