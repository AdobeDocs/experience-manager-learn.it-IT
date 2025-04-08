---
title: Registri
description: I log fungono da prima linea per il debug delle applicazioni AEM in AEM come Cloud Service, ma dipendono da registrazione adeguati nel applicazione AEM distribuito.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: 0363505b426d6e4733c57409e17e9d69f7a567c7
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# Il debug di AEM come Cloud Service l&#39;utilizzo dei registri

I log fungono da prima linea per il debug delle applicazioni AEM in AEM come Cloud Service, ma dipendono da registrazione adeguati nel applicazione AEM distribuito.

Tutte le attività di registro per il servizio AEM di un determinato ambiente (Autore, Publish/Publish Dispatcher) vengono consolidate in un singolo file di registro, lineare se diversi pod all&#39;interno di tale servizio generano le istruzioni di log.

Gli ID dei pod vengono forniti in ogni istruzione di registro e consentono di filtrare o fascicolare le istruzioni di registro. Gli ID pod sono nel formato di:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Esempio: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## File di registro personalizzati

AEM as a Cloud Services non supporta i file di registro personalizzati, ma supporta la registrazione personalizzata.

Affinché i registri Java siano disponibili in AEM as a Cloud Service (tramite [Cloud Manager](#cloud-manager) o [Adobe I/O CLI](#aio)), le istruzioni di registro personalizzate devono essere scritte in `error.log`. I registri scritti in registri denominati personalizzati, ad esempio `example.log`, non saranno accessibili da AEM as a Cloud Service.

I registri possono essere scritti in `error.log` utilizzando una proprietà di configurazione Sling LogManager OSGi nei file `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` dell&#39;applicazione.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Registri del servizio Author e Publish di AEM

I servizi Author e Publish di AEM forniscono i registri del server di runtime di AEM:

+ `aemerror` è il log degli errori Java (trovato in `/crx-quickstart/logs/error.log` nel modulo quickstart locale di AEM SDK). Di seguito sono riportati i [livelli di registro consigliati](#log-levels) per i logger personalizzati per tipo di ambiente:
   + Sviluppo: `DEBUG`
   + Fase: `WARN`
   + Produzione: `ERROR`
+ `aemaccess` elenca le richieste HTTP al servizio AEM con i relativi dettagli
+ `aemrequest` elenca le richieste HTTP effettuate al servizio AEM e la corrispondente risposta HTTP

## Registri Dispatcher di pubblicazione AEM

Solo AEM Publish Dispatcher fornisce il server web Apache e i registri di Dispatcher, in quanto questi aspetti esistono solo nel livello di pubblicazione di AEM e non nel livello di authoring di AEM.

+ `httpdaccess` elenca le richieste HTTP effettuate al server web Apache/Dispatcher del servizio AEM.
+ `httperror`  elenca i messaggi di log dal server Web Apache e aiuta con il debug dei moduli Apache supportati come `mod_rewrite`.
   + Sviluppo: `DEBUG`
   + Palco: `WARN`
   + Produzione: `ERROR`
+ `aemdispatcher` elenca i messaggi di registro provenienti dai moduli di Dispatcher, inclusi il filtraggio e la distribuzione dai messaggi della cache.
   + Sviluppo: `DEBUG`
   + Fase: `WARN`
   + Produzione: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager consente di scaricare i registri ogni giorno tramite l’azione Scarica registri di un ambiente.

![Cloud Manager - Scarica registri](./assets/logs/download-logs.png)

Questi registri possono essere scaricati e ispezionati tramite qualsiasi strumento di analisi dei registri.

## Adobe I/O CLI con plug-in Cloud Manager{#aio}

Adobe Cloud Manager supporta l&#39;accesso ai registri di AEM as a Cloud Service tramite [Adobe I/O CLI](https://github.com/adobe/aio-cli) con il plug-in [Cloud Manager per Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Innanzitutto, [imposta il plug-in Adobe I/O con Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Verificare che siano stati identificati l&#39;ID programma e l&#39;ID ambiente pertinenti e utilizzare [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) per elencare le opzioni di registro utilizzate per [coda](#aio-cli-tail-logs) o [scaricare](#aio-cli-download-logs) registri.

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

### Tronchi di coda{#aio-cli-tail-logs}

Adobe Systems interfaccia a riga di comando I/O consente di coda i log in tempo reale da AEM come Cloud Service utilizzando il [comando coda-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) . La taina è utile per osservare l&#39;attività dei log in tempo reale mentre vengono eseguite azioni sull&#39;ambiente AEM come Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Altri strumenti da riga di comando, come quelli `grep` che possono essere utilizzati insieme `tail-logs` per aiutare a isolare le istruzioni di registro delle interesse, ad esempio:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... visualizza solo le istruzioni di registro generate da `com.example.MySlingModel` o contengono tale stringa.

### Download dei registri{#aio-cli-download-logs}

Adobe I/O CLI consente di scaricare i registri da AEM as a Cloud Service utilizzando il comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Questo fornisce lo stesso risultato finale del download dei registri dall&#39;interfaccia utente Web di Cloud Manager, con la differenza che il comando `download-logs` consolida i registri tra giorni, in base al numero di giorni richiesti.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Informazioni sui registri

I registri in AEM as a Cloud Service contengono più pod che scrivono istruzioni di registro. Poiché più istanze di AEM scrivono nello stesso file di registro, è importante comprendere come analizzare e ridurre i disturbi durante il debug. Per spiegare, viene utilizzato il seguente frammento di registro `aemerror`:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Utilizzando gli ID del pod, il punto dati dopo la data e l’ora, i registri possono essere fascicolati da Pod o dall’istanza di AEM all’interno del servizio, facilitando la tracciatura e la comprensione dell’esecuzione del codice.

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

Le linee guida generali di Adobe sui livelli di registro per ambiente AEM as a Cloud Service consistono nel rispettare le impostazioni di registro predefinite di AEM (con il livello di registro predefinito di `INFO`). Adobe consiglia inoltre di dotare il codice personalizzato di istruzioni di registro, che consentono di eseguirlo con il livello di registro di `INFO`. I livelli di registro vengono mantenuti nel codice

+ Le configurazioni del registro Java vengono mantenute nelle configurazioni OSGi
+ Livelli del server web Apache e del registro Dispatcher nel progetto dispatcher

...e quindi richiedono una distribuzione per cambiare.

### Variabili specifiche dell’ambiente per impostare i livelli di registro Java

Un&#39;alternativa all&#39;impostazione di livelli di registro Java noti statici per ogni ambiente consiste nell&#39;utilizzare le [variabili specifiche dell&#39;ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) di AEM as Cloud Service per parametrizzare i livelli di registro, consentendo la modifica dinamica dei valori tramite [Adobe I/O CLI con plug-in Cloud Manager](#aio-cli).

Ciò richiede l&#39;aggiornamento delle configurazioni registrazione OSGi per utilizzare i segnaposto delle variabili specifiche dell&#39;ambiente. [I valori](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) predefiniti per i livelli di registro devono essere impostati in base ai [consigli Adobe Systems](#log-levels). Ad esempio:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Questo approccio ha degli aspetti negativi che devono essere presi in considerazione account:

+ [È consentito](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables) un numero limitato di variabili di ambiente e la creazione di una variabile per gestire il livello di registro ne utilizzerà una.
+ Le variabili di ambiente possono essere gestite a livello di programmazione tramite [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [Adobe Systems CLI di I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) e [API HTTP](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties) di Cloud Manager.
+ Le modifiche alle variabili di ambiente devono essere reimpostate manualmente da uno strumento supportato. Se si dimentica di reimpostare un ambiente con traffico elevato, come ad esempio quello di produzione, su un livello di registro meno dettagliato, i registri potrebbero essere inondati e avere un impatto sulle prestazioni di AEM.

_Le variabili specifiche dell&#39;ambiente non funzionano per le configurazioni del server web Apache o del registro di Dispatcher, in quanto non sono configurate tramite la configurazione OSGi._
