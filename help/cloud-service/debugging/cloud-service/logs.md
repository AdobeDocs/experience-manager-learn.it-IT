---
title: Registri
description: I registri fungono da front-line per il debug AEM applicazioni in AEM come Cloud Service, ma dipendono dall’accesso adeguato nell’applicazione AEM distribuita.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
translation-type: tm+mt
source-git-commit: 7fd232d6821f91c342dd04fcdd04b9b505cb7250
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 3%

---


# Debug di AEM come Cloud Service utilizzando i registri

I registri fungono da front-line per il debug AEM applicazioni in AEM come Cloud Service, ma dipendono dall’accesso adeguato nell’applicazione AEM distribuita.

Tutte le attività di registro per il servizio AEM di un determinato ambiente (Author, Publish/Publish Dispatcher) vengono consolidate in un singolo file di registro, anche se contenitori diversi all&#39;interno di tale servizio generano le istruzioni di registro.

Gli ID contenitore vengono forniti in ciascuna istruzione di registro e consentono il filtraggio o il raggruppamento delle istruzioni di registro. Gli ID contenitore sono nel formato di:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Esempio: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## File di registro personalizzati

AEM come Cloud Services non supporta i file di registro personalizzati, ma supporta la registrazione personalizzata.

Affinché i registri Java siano disponibili in AEM come Cloud Service (tramite [Cloud Manager](#cloud-manager) o [CLI](#aio)I/O Adobe), è necessario scrivere le istruzioni di registro personalizzate `error.log`. I registri scritti in registri denominati personalizzati, ad esempio `example.log`, non saranno accessibili da AEM come Cloud Service.

## Registri dei servizi AEM Author e Publish

I servizi AEM Author e Publish forniscono AEM registri del server di runtime:

+ `aemerror` è il registro degli errori Java (disponibile `/crx-quickstart/error.log` nel quickstart locale AEM SDK). Di seguito sono riportati i livelli [di registro](#log-levels) consigliati per i logger di log personalizzati per tipo di ambiente:
   + Sviluppo: `DEBUG`
   + Stadio: `WARN`
   + Produzione: `ERROR`
+ `aemaccess` elenca le richieste HTTP al servizio AEM con i dettagli
+ `aemrequest` elenca le richieste HTTP effettuate AEM servizio e la risposta HTTP corrispondente

## Registri del dispatcher AEM Publish

Solo AEM Publish Dispatcher fornisce i registri Apache dei server Web e dei dispatcher, poiché questi aspetti esistono solo nel livello AEM Publish e non nel livello AEM Author.

+ `httpdaccess` elenca le richieste HTTP effettuate al server Web Apache del servizio AEM.
+ `httperror`  elenca i messaggi di registro provenienti dal server Web Apache e assistenza per il debug dei moduli Apache supportati, ad esempio `mod_rewrite`.
   + Sviluppo: `DEBUG`
   + Stadio: `WARN`
   + Produzione: `ERROR`
+ `aemdispatcher` elenca i messaggi di registro dai moduli Dispatcher, incluso il filtraggio e la trasmissione dai messaggi della cache.
   + Sviluppo: `DEBUG`
   + Stadio: `WARN`
   + Produzione: `ERROR`

## Cloud Manager{#cloud-manager}

 Adobe Cloud Manager consente il download dei registri, di giorno in giorno, tramite l&#39;azione Download Logs di un ambiente.

![Cloud Manager - Registri di download](./assets/logs/download-logs.png)

Questi file di registro possono essere scaricati ed ispezionati tramite qualsiasi strumento di analisi del registro.

##  CLI I/O Adobe con il plug-in Cloud Manager{#aio}

 Adobe Cloud Manager supporta l&#39;accesso AEM come registri di Cloud Service tramite l&#39;interfaccia CLI [I/O del Adobe](https://github.com/adobe/aio-cli) con il plug-in [Cloud Manager per l&#39;interfaccia CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)I/O  Adobe.

Innanzitutto, [configurate l&#39;I/O del Adobe  con il plug-in](../../local-development-environment/development-tools.md#aio-cli)di Cloud Manager.

Assicurati che siano stati identificati l’ID programma e l’ID ambiente pertinenti e utilizza le opzioni [di registro disponibili nell’](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) elenco per elencare le opzioni di registro utilizzate per [ridurre](#aio-cli-tail-logs) o [scaricare](#aio-cli-download-logs) i registri.

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

 CLI I/O di Adobe consente di eseguire i log di coda in tempo reale da AEM come Cloud Service utilizzando il comando [code-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) . La coda è utile per osservare l&#39;attività del registro in tempo reale mentre le azioni vengono eseguite sul AEM come ambiente di Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Altri strumenti della riga di comando, come `grep` possono essere utilizzati insieme `tail-logs` per aiutare a isolare le istruzioni di registro di interesse, ad esempio:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... visualizza solo le istruzioni di registro generate da `com.example.MySlingModel` o che contengono tale stringa.

### Download dei registri{#aio-cli-download-logs}

 CLI I/O di Adobe consente di scaricare i file di registro AEM come Cloud Service utilizzando il comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Questo fornisce lo stesso risultato finale del download dei file di registro dall’interfaccia utente Web di Cloud Manager, con la differenza che il `download-logs` comando consolida i file di registro in giorni diversi, in base al numero di giorni di log richiesti.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Informazioni sui registri

I AEM di login come Cloud Service contengono più contenitori che scrivono le istruzioni di registro. Poiché più istanze AEM scrivono nello stesso file di registro, è importante comprendere come analizzare e ridurre il rumore durante il debug. Per spiegare il motivo, verrà utilizzato il seguente frammento di `aemerror` registro:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Utilizzando gli ID contenitore, il punto dati dopo la data e l’ora, i registri possono essere raggruppati per contenitore, o AEM’istanza all’interno del servizio, semplificando l’analisi e la comprensione dell’esecuzione del codice.

__Contenitore cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Contenitore cm-p12345-e56789-aem-author-abcdefg-222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Livelli di registro consigliati{#log-levels}

 Adobe  linee guida generali sui livelli di log per AEM come ambiente Cloud Service sono:

+ Sviluppo locale (AEM SDK): `DEBUG`
+ Sviluppo: `DEBUG`
+ Stadio: `WARN`
+ Produzione: `ERROR`

L&#39;impostazione del livello di registro più appropriato per ciascun tipo di ambiente è con AEM come Cloud Service, i livelli di registro vengono mantenuti nel codice

+ Le configurazioni del registro Java vengono mantenute nelle configurazioni OSGi
+ Livelli di registro del server Web Apache e del dispatcher nel progetto dispatcher

...quindi, è necessario un intervento per cambiare.

### Visualizzare variabili specifiche per l&#39;ambiente per impostare i livelli di registro Java

Un&#39;alternativa all&#39;impostazione di livelli di registro Java statici noti per ogni ambiente è utilizzare AEM come variabili [specifiche per ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) ambiente per parametrizzare i livelli di registro, consentendo la modifica dinamica dei valori tramite l&#39;interfaccia CLI I/O di [Adobe Cloud Service con il plug](#aio-cli)di Cloud Manager.

Ciò richiede l’aggiornamento delle configurazioni OSGi per utilizzare i segnaposto variabili specifiche per l’ambiente. [I valori](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) predefiniti per i livelli di registro devono essere impostati come da [raccomandazioni](#log-levels)di Adobe. Esempio:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

Questo approccio presenta aspetti negativi che devono essere presi in considerazione:

+ [È consentito](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)un numero limitato di variabili di ambiente e la creazione di una variabile per gestire il livello di registro ne verrà utilizzata una.
+ Le variabili di ambiente possono essere gestite solo a livello di programmazione tramite [CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) I/O di Adobe o le API [HTTP](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)Cloud Manager.
+ Le modifiche alle variabili di ambiente devono essere ripristinate manualmente mediante uno strumento supportato. Se si dimentica di reimpostare un ambiente di traffico elevato, ad esempio Produzione, a un livello di registro meno dettagliato, i registri potrebbero essere danneggiati e le prestazioni AEM impatto.

_Le variabili specifiche per l’ambiente non funzionano per le configurazioni del server Web Apache o del registro del dispatcher, perché non sono configurate tramite la configurazione OSGi._