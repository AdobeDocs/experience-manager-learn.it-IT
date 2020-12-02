---
title: Creazione e implementazione
description: ' Adobe Cloud Manager facilita la creazione del codice e le distribuzioni da AEM come Cloud Service. Potrebbero verificarsi errori durante i passaggi del processo di creazione, che richiedono un''azione per risolverli. Questa guida descrive come comprendere i comuni errori di implementazione e come affrontarli al meglio.'
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
translation-type: tm+mt
source-git-commit: a405cf14d3f71bf51e32e50c828c3216d29aa253
workflow-type: tm+mt
source-wordcount: '2517'
ht-degree: 0%

---


# Debug AEM come build e distribuzioni di Cloud Service

 Adobe Cloud Manager facilita la creazione del codice e le distribuzioni da AEM come Cloud Service. Potrebbero verificarsi errori durante i passaggi del processo di creazione, che richiedono un&#39;azione per risolverli. Questa guida descrive come comprendere i comuni errori di implementazione e come affrontarli al meglio.

![Gestisci pipeline di compilazione cloud](./assets/build-and-deployment/build-pipeline.png)

## Convalida

Il passaggio di convalida assicura semplicemente la validità delle configurazioni di base di Cloud Manager. Errori comuni di convalida:

### Lo stato dell&#39;ambiente non è valido

+ __Messaggio di errore:__ Lo stato dell&#39;ambiente non è valido.
   ![Lo stato dell&#39;ambiente non è valido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ l&#39;ambiente di destinazione della pipeline è in uno stato transitorio in cui non può accettare nuove build.
+ __Risoluzione:__ attesa che lo stato venga risolto in uno stato di esecuzione (o di aggiornamento disponibile). Se l’ambiente viene eliminato, ricreate l’ambiente o scegliete un ambiente diverso in cui creare l’ambiente.

### Impossibile trovare l&#39;ambiente associato alla pipeline

+ __Messaggio di errore:__ L&#39;ambiente è contrassegnato come eliminato.
   ![L&#39;ambiente è contrassegnato come eliminato](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ l&#39;ambiente configurato per l&#39;utilizzo della pipeline è stato eliminato.
Anche se viene ricreato un nuovo ambiente con lo stesso nome, Cloud Manager non riassocia automaticamente la pipeline a tale ambiente con lo stesso nome.
+ __Risoluzione:__ modificate la configurazione della pipeline e selezionate nuovamente l&#39;ambiente in cui distribuire.

### Impossibile trovare il ramo Git associato alla pipeline

+ __Messaggio di errore:pipeline__ non valida: XXXXXX. Reason=Branch=xxxx non trovato nella directory archivio.
   ![pipeline non valida: XXXXXX. Reason=Branch=xxxx non trovato nella directory archivio](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ il ramo Git configurato per l&#39;utilizzo dalla pipeline è stato eliminato.
+ __Risoluzione:__ ricreate il ramo Git mancante con lo stesso nome oppure riconfigurate la pipeline per creare da un altro ramo esistente.

## Build e unit test

![Creazione e testing di unità](./assets/build-and-deployment/build-and-unit-testing.png)

La fase Build e Unit Testing (Test unità) esegue una build Maven (`mvn clean package`) del progetto estratto dal ramo Git configurato della pipeline.

Gli errori identificati in questa fase devono essere riproducibili nella creazione locale del progetto, con le seguenti eccezioni:

+ Viene utilizzata una dipendenza del cielo non disponibile su [Maven Central](https://search.maven.org/), e il repository Maven contenente la dipendenza è:
   + Non raggiungibile da Cloud Manager, ad esempio un repository interno privato di Maven, oppure l&#39;archivio di Maven richiede l&#39;autenticazione e sono state fornite le credenziali errate.
   + Non registrato esplicitamente in `pom.xml` del progetto. Si noti che, inclusi i repository Maven è scoraggiato man mano che aumenta i tempi di costruzione.
+ I test delle unità non riescono a causa di problemi di tempo. Ciò può verificarsi quando i test di unità sono sensibili ai tempi. Un indicatore forte si basa su `.sleep(..)` nel codice di prova.
+ L&#39;uso di plugin Maven non supportati.

## Analisi del codice

![Analisi del codice](./assets/build-and-deployment/code-scanning.png)

La scansione del codice esegue l&#39;analisi del codice statico utilizzando una combinazione di procedure ottimali Java e AEM specifiche.

La scansione del codice genera un errore di compilazione se nel codice sono presenti vulnerabilità di protezione critica. Le violazioni minori possono essere ignorate, ma si consiglia di correggerle. La scansione del codice è imperfetta e può dare luogo a [falsi positivi](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/understand-test-results.html#dealing-with-false-positives).

Per risolvere i problemi di scansione del codice, scarica il rapporto in formato CSV fornito da Cloud Manager tramite il pulsante **Download Details** e controlla tutte le voci.

Per ulteriori dettagli, consultate AEM regole specifiche, consultate la documentazione di Cloud Manager [regole di scansione del codice AEM personalizzate](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Creare immagini

![Creare immagini](./assets/build-and-deployment/build-images.png)

L&#39;immagine Build è responsabile della combinazione degli artefatti di codice generati nel passaggio Build &amp; Unit Testing con il rilascio AEM, per formare un singolo artefatto distribuibile.

Sebbene durante i test di build e unità siano stati riscontrati problemi di compilazione e compilazione del codice, durante il tentativo di combinare l&#39;artifact di build personalizzata con la release AEM potrebbero verificarsi problemi di configurazione o di struttura.

### Duplicare le configurazioni OSGi

Quando più configurazioni OSGi vengono risolte tramite modalità di esecuzione per l’ambiente AEM di destinazione, il passaggio Genera immagine non riesce con l’errore:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ il pacchetto all del progetto AEM contiene più pacchetti di codice e la stessa configurazione OSGi è fornita da più pacchetti di codice, dando origine a un conflitto, il passaggio Genera immagine non è in grado di decidere quale debba essere utilizzato, dando così esito negativo alla creazione. Questo non si applica alle configurazioni di fabbrica OSGi, purché abbiano nomi univoci.
+ __Risoluzione:__ Esaminate tutti i pacchetti di codice (inclusi eventuali pacchetti di codice di terze parti) distribuiti come parte dell&#39;applicazione AEM, alla ricerca di configurazioni OSGi duplicate che risolvono, tramite modalità di esecuzione, l&#39;ambiente di destinazione. La guida del messaggio di errore &quot;imposta il flag mergeConfigurations su true&quot; non è possibile in AEM come servizio Cloud e deve essere ignorata.

#### Causa 2

+ __Causa:__ il progetto AEM include erroneamente lo stesso pacchetto di codice due volte, con conseguente duplicazione di qualsiasi configurazione OSGi contenuta in tale pacchetto.
+ __Risoluzione:__ Rivedete tutti i pacchetti pom.xml incorporati in tutto il progetto e accertatevi che la  `filevault-package-maven-plugin` [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) configurazione sia impostata su  `<cloudManagerTarget>none</cloudManagerTarget>`.

### Script repoinit non corretto

Gli script di reindirizzamento definiscono il contenuto della linea di base, gli utenti, gli ACL, ecc. AEM come Cloud Service, gli script di reindirizzamento vengono applicati durante la creazione dell&#39;immagine. Tuttavia, AEM&#39;avvio rapido locale dell&#39;SDK vengono applicati quando viene attivata la configurazione della fabbrica di reindirizzamenti OSGi. A causa di questo, gli script Repoinit potrebbero silenziosamente non riuscire (con la registrazione) nel quickstart locale AEM SDK e causare un errore nel passaggio Genera immagine, arrestando la distribuzione.

+ __Causa:__ Uno script di reindirizzamento non è corretto. Questo potrebbe lasciare il repository in uno stato incompleto come qualsiasi script di reindirizzamento dopo che lo script non riuscito verrà eseguito contro il repository.
+ __Risoluzione:__ rivedere il quickstart locale dell&#39;SDK AEM quando la configurazione OSGi dello script di repoinit è distribuita per determinare se e quali sono gli errori.

### Dipendenza contenuto repoinit non soddisfatta

Gli script di reindirizzamento definiscono il contenuto della linea di base, gli utenti, gli ACL, ecc. AEM&#39;avvio rapido locale dell&#39;SDK, gli script di reindirizzamento vengono applicati quando viene attivata la configurazione di fabbrica di reindirizzamento OSGi, o in altre parole, dopo che l&#39;archivio è attivo e potrebbe aver subito modifiche di contenuto direttamente o tramite pacchetti di contenuto. AEM come Cloud Service, gli script di reindirizzamento vengono applicati durante l&#39;operazione Genera immagine rispetto a un repository che potrebbe non contenere contenuto da cui dipende lo script di reindirizzamento.

+ __Causa:__ Uno script di reindirizzamento dipende dal contenuto non esistente.
+ __Risoluzione:__ verificare che il contenuto da cui dipende lo script di reindirizzamento esista. Spesso ciò indica script di repoinit definiti in modo inadeguato, privi di direttive che definiscono le strutture di contenuto mancanti, ma necessarie. Questo può essere riprodotto localmente eliminando AEM, decomprimendo la Jar e aggiungendo alla cartella di installazione la configurazione OSGi di reindirizzamento contenente lo script di reindirizzamento e avviando AEM. L&#39;errore si presenterà nel file error.log locale dell&#39;SDK AEM.


### La versione dei componenti core dell&#39;applicazione è maggiore della versione distribuita

_Questo problema riguarda solo gli ambienti non di produzione che NON eseguono l&#39;aggiornamento automatico alla versione più recente AEM._

AEM come Cloud Service include automaticamente la versione più recente dei componenti core in ogni versione AEM, ovvero dopo che un AEM come ambiente di Cloud Service viene aggiornato automaticamente o manualmente, viene distribuita la versione più recente dei componenti core.

È possibile che il passaggio Genera immagine non riesca quando:

+ L&#39;applicazione di distribuzione aggiorna la versione della dipendenza principale dei componenti nel progetto `core` (bundle OSGi)
+ L&#39;applicazione di distribuzione viene quindi distribuita in un AEM sandbox (non di produzione) come ambiente di Cloud Service che non è stato aggiornato per utilizzare una versione AEM che contiene la nuova versione di Componenti di base.

Per evitare questo errore, ogni volta che è disponibile un aggiornamento del AEM come ambiente di Cloud Service, includete l&#39;aggiornamento come parte della build/implementazione successiva e assicuratevi sempre che gli aggiornamenti siano inclusi dopo l&#39;incremento della versione dei componenti core nella base di codice dell&#39;applicazione.

+ __Sintomi:__
il passaggio Genera immagine non riesce con un errore che segnala che 
`com.adobe.cq.wcm.core.components...` impossibile importare i pacchetti in intervalli di versioni specifici dal  `core` progetto.

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __Causa:__  il bundle OSGi dell&#39;applicazione (definito nel  `core` progetto) importa le classi Java dalla dipendenza di base dei componenti core, a un livello di versione diverso da quello distribuito per AEM come Cloud Service.
+ __Risoluzione:__
   + Utilizzando Git, ripristinare un commit funzionante esistente prima dell’incremento della versione del componente core. Inviate questo commit in un ramo Git di Cloud Manager ed eseguite un aggiornamento dell&#39;ambiente da questo ramo. Questo AEM verrà aggiornato come Cloud Service alla versione più recente AEM, che includerà la versione successiva dei componenti core. Una volta aggiornato l&#39;AEM come Cloud Service alla versione più recente AEM, che avrà la versione più recente dei componenti core, ridistribuite il codice che ha generato l&#39;errore originale.
   + Per riprodurre questo problema localmente, accertati che AEM versione SDK sia la stessa versione AEM release utilizzata dall&#39;AEM di un ambiente di Cloud Service.


### Creare un caso di assistenza per  Adobe

Se i precedenti approcci di risoluzione dei problemi non risolvono il problema, create un caso di assistenza  Adobe tramite:

+ [Adobe Admin Console](https://adminconsole.adobe.com)  > Scheda Assistenza > Crea caso

   _Se siete membri di più organizzazioni di  Adobe, accertatevi che l’organizzazione di Adobi  con condutture non riuscite sia selezionata nel commutatore  organizzazioni Adobe prima di creare il caso._

## Distribuisci su

Il passaggio Implementa è responsabile dell&#39;artifact del codice generato in Genera immagine, avvia i nuovi servizi AEM Author e Publish tramite di esso e, in caso di esito positivo, rimuove tutti i vecchi servizi AEM Author e Publish. Anche in questo passaggio vengono installati e aggiornati pacchetti di contenuto variabile e indici.

Acquisisci familiarità con [AEM come registri di Cloud Service](./logs.md) prima di eseguire il debug dell&#39;operazione di implementazione. Il registro `aemerror` contiene informazioni relative all&#39;avvio e all&#39;arresto dei contenitori che possono essere pertinenti per l&#39;implementazione a problemi. Il registro disponibile tramite il pulsante Scarica registro nel passaggio Distribuisci a di Cloud Manager non è il registro `aemerror` e non contiene informazioni dettagliate relative all&#39;avvio delle applicazioni.

![Distribuisci su](./assets/build-and-deployment/deploy-to.png)

I tre motivi principali per cui il passaggio Distribuisci potrebbe non riuscire:

### La pipeline di Cloud Manager contiene una versione precedente AEM

+ __Causa:__ Una pipeline di Cloud Manager contiene una versione di AEM precedente a quella distribuita nell&#39;ambiente di destinazione. Ciò può accadere quando una pipeline viene riutilizzata e indirizzata a un nuovo ambiente che esegue una versione successiva di AEM. Questo può essere identificato controllando se la versione AEM dell&#39;ambiente è maggiore della versione AEM della pipeline.
   ![La pipeline di Cloud Manager contiene una versione precedente AEM](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Risoluzione:__
   + Se l&#39;ambiente di destinazione dispone di un aggiornamento disponibile, selezionate Aggiorna dalle azioni dell&#39;ambiente, quindi eseguite nuovamente la build.
   + Se nell&#39;ambiente di destinazione non è disponibile un aggiornamento, significa che è in esecuzione la versione più recente di AEM. Per risolvere questo problema, eliminare la pipeline e ricrearla.


### Timeout di Cloud Manager

Il codice in esecuzione durante l&#39;avvio del servizio AEM appena distribuito richiede un tempo tale che Cloud Manager si interrompe prima del completamento della distribuzione. In questi casi, la distribuzione potrebbe avere successo, anche se lo stato di Cloud Manager veniva segnalato come Non riuscito.

+ __Causa: il codice__ personalizzato può eseguire operazioni, come query di grandi dimensioni o traversate di contenuti, attivate in anticipo nel bundle OSGi o nei cicli di vita dei componenti, con un significativo ritardo del tempo di avvio dei AEM.
+ __Risoluzione:__ rivedere l&#39;implementazione del codice in esecuzione nelle prime fasi del ciclo di vita di OSGi Bundle, controllare i  `aemerror` registri dei servizi AEM Author e Publish al momento dell&#39;errore (ora di accesso in GMT) come mostrato da Cloud Manager, e cercare i messaggi di registro che indicano eventuali processi di log in esecuzione personalizzati.

### Codice o configurazione non compatibile

La maggior parte delle violazioni del codice e della configurazione sono rilevate in precedenza nella build, tuttavia è possibile che il codice o la configurazione personalizzata sia incompatibile con il AEM come Cloud Service e non venga rilevato fino all&#39;esecuzione nel contenitore.

+ __Causa: il codice__ personalizzato può richiamare operazioni lunghe, ad esempio query di grandi dimensioni o attraversamenti di contenuti, attivate in anticipo nel bundle OSGi o nei cicli di vita dei componenti e ritardare notevolmente il tempo di avvio del AEM.
+ __Risoluzione:__ esaminare i  `aemerror` registri relativi ai servizi AEM Author e Publish nel momento (ora di accesso in GMT) in cui si è verificato l&#39;errore, come mostrato dal Cloud Manager.
   1. Esaminare i registri per individuare eventuali ERRORS generati dalle classi Java fornite dall&#39;applicazione personalizzata. In caso di problemi, risolvete, inviate il codice fisso e ricreate la pipeline.
   1. Esaminare i registri relativi agli eventuali ERRORS segnalati da aspetti di AEM con i quali si sta estendendo o interagendo nell&#39;applicazione personalizzata e analizzarli; questi ERRORI potrebbero non essere direttamente attribuiti alle classi Java. In caso di problemi, risolvete, inviate il codice fisso e ricreate la pipeline.

### Inclusione di /var nel pacchetto di contenuto

`/var` è modificabile e contiene una varietà di contenuti di runtime transitori. Inclusione di `/var` in pacchetti di contenuti (ad esempio `ui.content`) distribuita tramite Cloud Manager potrebbe causare il fallimento della distribuzione.

Questo problema è difficile da identificare in quanto non si verifica un errore nella distribuzione iniziale, solo nelle distribuzioni successive. I sintomi visibili comprendono:

+ La distribuzione iniziale ha esito positivo, anche se il contenuto nuovo o modificato, che fa parte della distribuzione, non sembra esistere nel servizio AEM Publish.
+ Attivazione/Disattivazione del contenuto in AEM Author bloccata
+ Le distribuzioni successive non riescono nel passaggio Distribuisci a, con errore di distribuzione al passaggio dopo circa 60 minuti.

Per convalidare questo problema è possibile determinare il comportamento non riuscito:

1. Determinando che almeno un pacchetto di contenuto che fa parte della distribuzione, scrive in `/var`.
1. Verifica che la coda di distribuzione primaria (in grassetto) sia bloccata in:
   + AEM Author > Strumenti > Distribuzione > Distribuzione
      ![Coda di distribuzione bloccata](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. Se la distribuzione successiva non riesce, scaricate i registri &quot;Distribuisci a&quot; di Cloud Manager utilizzando il pulsante Scarica registro:

   ![Download della distribuzione nei registri](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... e verificare che tra le istruzioni del registro siano trascorsi circa 60 minuti:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... e ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Questo registro non conterrà questi indicatori sulle distribuzioni iniziali che hanno esito positivo, ma solo sulle distribuzioni con errore successive.

+ __Causa:__ AEM utente del servizio di replica utilizzato per distribuire pacchetti di contenuto al servizio AEM Publish non è in grado di scrivere  `/var` in AEM Publish. Questo causa il fallimento della distribuzione del pacchetto di contenuti al servizio AEM Publish.
+ __Risoluzione:__ I seguenti modi per risolvere questi problemi sono elencati in ordine di preferenza:
   1. Se le risorse `/var` non sono necessarie, rimuovete le risorse in `/var` dai pacchetti di contenuto distribuiti come parte dell&#39;applicazione.
   2. Se le risorse `/var` sono necessarie, definire le strutture dei nodi utilizzando [repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). Gli script di reindirizzamento possono essere indirizzati a AEM Author, AEM Publish o a entrambi tramite le modalità di esecuzione OSGi.
   3. Se le risorse `/var` sono richieste solo per l&#39;autore AEM e non possono essere ragionevolmente modellate utilizzando [repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit), spostatele in un pacchetto di contenuti discreti, che viene installato solo su AEM Author da [incorporare](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) nel pacchetto `all` in una cartella di modalità di esecuzione di AEM Author (`<target>/apps/example-packages/content/install.author</target>`).

### Creare un caso di assistenza per  Adobe

Se i precedenti approcci di risoluzione dei problemi non risolvono il problema, create un caso di assistenza  Adobe tramite:

+ [Adobe Admin Console](https://adminconsole.adobe.com)  > Scheda Assistenza > Crea caso

   _Se siete membri di più organizzazioni di  Adobe, accertatevi che l’organizzazione di Adobi  con condutture non riuscite sia selezionata nel commutatore  organizzazioni Adobe prima di creare il caso._
