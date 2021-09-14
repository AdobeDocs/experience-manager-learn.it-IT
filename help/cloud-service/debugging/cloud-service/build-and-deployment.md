---
title: Creare e distribuire
description: Adobe Cloud Manager facilita la creazione del codice e le distribuzioni da AEM come Cloud Service. Possono verificarsi errori durante i passaggi del processo di compilazione, che richiedono un’azione per risolverli. Questa guida illustra gli errori comuni nell’implementazione e le modalità per affrontarli al meglio.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2526'
ht-degree: 0%

---

# Debug di AEM come build e implementazioni di Cloud Service

Adobe Cloud Manager facilita la creazione del codice e le distribuzioni da AEM come Cloud Service. Possono verificarsi errori durante i passaggi del processo di compilazione, che richiedono un’azione per risolverli. Questa guida illustra gli errori comuni nell’implementazione e le modalità per affrontarli al meglio.

![Pipe di generazione gestisci cloud](./assets/build-and-deployment/build-pipeline.png)

## Convalida

Il passaggio di convalida assicura semplicemente la validità delle configurazioni di base di Cloud Manager. Gli errori di convalida comuni includono:

### Lo stato dell&#39;ambiente non è valido

+ __Messaggio di errore:__ lo stato dell&#39;ambiente non è valido.
   ![Lo stato dell&#39;ambiente non è valido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ l’ambiente di destinazione della pipeline è in uno stato di transizione in cui non può accettare nuove build.
+ __Risoluzione:__ attendi che lo stato venga risolto in uno stato di esecuzione (o di aggiornamento disponibile). Se l’ambiente viene eliminato, ricrea l’ambiente o scegli un ambiente diverso in cui generare la build.

### Impossibile trovare l’ambiente associato alla pipeline

+ __Messaggio di errore:__ l’ambiente è contrassegnato come eliminato.
   ![L’ambiente viene contrassegnato come eliminato](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ l’ambiente configurato per l’utilizzo della pipeline è stato eliminato.
Anche se viene ricreato un nuovo ambiente con lo stesso nome, Cloud Manager non riassocia automaticamente la pipeline a tale ambiente con lo stesso nome.
+ __Risoluzione:__ modifica la configurazione della pipeline e seleziona nuovamente l’ambiente in cui distribuire.

### Impossibile trovare il ramo Git associato alla pipeline

+ __Messaggio di errore:__ pipeline non valida: XXXXXX Reason=Branch=xxxx non trovato nell&#39;archivio.
   ![pipeline non valida: XXXXXX Reason=Branch=xxxx non trovato nel repository](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ il ramo Git configurato per l’utilizzo della pipeline è stato eliminato.
+ __Risoluzione:__ ricrea il ramo Git mancante utilizzando lo stesso nome o riconfigura la pipeline in modo da creare da un ramo diverso esistente.

## Build &amp; Unit Testing

![Test di build e unità](./assets/build-and-deployment/build-and-unit-testing.png)

La fase Build and Unit Testing (Generazione e test di unità) esegue una build Maven (`mvn clean package`) del progetto estratto dal ramo Git configurato della pipeline.

Gli errori individuati in questa fase devono essere riproducibili nella creazione locale del progetto, con le seguenti eccezioni:

+ Viene utilizzata una dipendenza Maven non disponibile su [Maven Central](https://search.maven.org/) e l&#39;archivio Maven contenente la dipendenza è:
   + Non raggiungibile da Cloud Manager, ad esempio un archivio Maven interno privato, oppure l’archivio Maven richiede l’autenticazione e sono state fornite le credenziali errate.
   + Non è stato registrato esplicitamente nel `pom.xml` del progetto. Tieni presente che, l’inclusione degli archivi Maven è scoraggiata in quanto aumenta i tempi di creazione.
+ I test di unità non riescono a causa di problemi di temporizzazione. Ciò può verificarsi quando le prove di unità sono sensibili alla tempistica. Un indicatore forte si basa su `.sleep(..)` nel codice di test.
+ L’utilizzo di plug-in Maven non supportati.

## Scansione del codice

![Scansione del codice](./assets/build-and-deployment/code-scanning.png)

La scansione del codice esegue l’analisi del codice statico utilizzando una combinazione di procedure consigliate specifiche per Java e AEM.

La scansione del codice causa un errore di compilazione se nel codice sono presenti vulnerabilità di sicurezza critica. Le violazioni minori possono essere ignorate, ma si consiglia di correggerle. La scansione del codice è imperfetta e può causare [falsi positivi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives).

Per risolvere i problemi di scansione del codice, scarica il rapporto in formato CSV fornito da Cloud Manager tramite il pulsante **Download Details** e controlla tutte le voci.

Per ulteriori dettagli, consulta AEM regole specifiche, consulta la documentazione di Cloud Manager [regole di scansione del codice AEM personalizzate](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Creare immagini

![Creare immagini](./assets/build-and-deployment/build-images.png)

L’immagine Build è responsabile della combinazione degli artefatti di codice generati nel passaggio Build &amp; Unit Testing (Generazione e test di unità) con la versione AEM, per creare un singolo artefatto distribuibile.

Mentre durante il test Build &amp; Unit vengono rilevati eventuali problemi di compilazione e compilazione del codice, possono verificarsi problemi di configurazione o di struttura durante il tentativo di combinare l&#39;artefatto di build personalizzato con la versione AEM.

### Configurazioni OSGi duplicate

Quando più configurazioni OSGi vengono risolte tramite la modalità runmode per l&#39;ambiente di AEM di destinazione, il passaggio Genera immagine non riesce e viene restituito l&#39;errore:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ il pacchetto all del progetto AEM contiene più pacchetti di codice e la stessa configurazione OSGi viene fornita da più di uno dei pacchetti di codice, dando luogo a un conflitto e il passaggio Genera immagine non è in grado di decidere quale deve essere utilizzato, generando in tal modo un errore nella build. Questo non si applica alle configurazioni di fabbrica OSGi, purché abbiano nomi univoci.
+ __Risoluzione:__ esamina tutti i pacchetti di codice (inclusi eventuali pacchetti di codice di terze parti inclusi) che vengono distribuiti come parte dell&#39;applicazione AEM, alla ricerca di configurazioni OSGi duplicate che risolvono, tramite modalità runmode, l&#39;ambiente di destinazione. La guida del messaggio di errore &quot;imposta il flag mergeConfigurations su true&quot; non è possibile in AEM as a Cloud Service e deve essere ignorata.

#### Causa 2

+ __Causa:__ il progetto AEM include erroneamente lo stesso pacchetto di codice due volte, con conseguente duplicazione di qualsiasi configurazione OSGi contenuta in tale pacchetto.
+ __Risoluzione:__ esamina tutti i pacchetti pom.xml di incorporati in tutto il progetto e assicurati che la  `filevault-package-maven-plugin` [](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) configurazione sia impostata su  `<cloudManagerTarget>none</cloudManagerTarget>`.

### Script di reindirizzamento non valido

Gli script di reindirizzamento definiscono il contenuto di base, gli utenti, le ACL, ecc. In AEM come Cloud Service, gli script di reindirizzamento vengono applicati durante la creazione dell’immagine, ma AEM’avvio rapido locale dell’SDK vengono applicati quando viene attivata la configurazione di fabbrica del repoinit OSGi. Per questo motivo, gli script di Repoinit potrebbero avere esito negativo (con la registrazione) sull’avvio rapido locale AEM’SDK e causare un errore nel passaggio Genera immagine, arrestando la distribuzione.

+ __Causa:__ uno script di repoinit non è valido. Questo potrebbe lasciare il tuo archivio in uno stato incompleto come qualsiasi script di reindirizzamento dopo che lo script non riuscito verrà eseguito contro il repository.
+ __Risoluzione:__ esamina l&#39;avvio rapido locale dell&#39;SDK AEM quando la configurazione OSGi dello script di reindirizzamento viene distribuita per determinare se e quali sono gli errori.

### Dipendenza contenuto reindirizzamento non soddisfatta

Gli script di reindirizzamento definiscono il contenuto di base, gli utenti, le ACL, ecc. In AEM’avvio rapido locale dell’SDK, gli script di reindirizzamento vengono applicati quando viene attivata la configurazione di fabbrica OSGi di reindirizzamento, o in altre parole, dopo che l’archivio è attivo e possono aver subito modifiche di contenuto direttamente o tramite pacchetti di contenuto. In AEM come Cloud Service, gli script di reindirizzamento vengono applicati durante la funzione Genera immagine rispetto a un archivio che potrebbe non contenere il contenuto da cui dipende lo script di repoinit.

+ __Causa:__ uno script di reindirizzamento dipende dal contenuto inesistente.
+ __Risoluzione:__ verifica che esista il contenuto da cui dipende lo script di reindirizzamento. Spesso, questo indica un script di reindirizzamento non adeguatamente definito che non contiene direttive che definiscono queste strutture di contenuto mancanti, ma necessarie. Questo può essere riprodotto localmente eliminando AEM, disimpacchettando il Jar e aggiungendo alla cartella di installazione la configurazione OSGi di reindirizzamento contenente lo script di reindirizzamento e avviando AEM. L&#39;errore si presenterà nel file error.log locale dell&#39;SDK AEM.


### La versione dei componenti core dell&#39;applicazione è maggiore della versione distribuita

_Questo problema riguarda solo gli ambienti non di produzione che NON eseguono l&#39;aggiornamento automatico alla versione più recente di AEM._

AEM come Cloud Service include automaticamente la versione più recente dei componenti core in ogni versione di AEM, il che significa che dopo l’aggiornamento automatico o manuale di un AEM come ambiente di Cloud Service, viene distribuita la versione più recente dei componenti core.

È possibile che il passaggio Genera immagine non riesca quando:

+ L’applicazione di distribuzione aggiorna la versione della dipendenza Maven dei componenti core nel progetto `core` (bundle OSGi)
+ L’applicazione di distribuzione viene quindi distribuita in un AEM sandbox (non di produzione) come ambiente di Cloud Service che non è stato aggiornato per utilizzare una versione AEM che contiene la nuova versione dei componenti core.

Per evitare questo errore, ogni volta che è disponibile un aggiornamento del AEM come ambiente di Cloud Service, includi l’aggiornamento come parte della build/distribuzione successiva e assicurati che gli aggiornamenti siano inclusi sempre dopo l’incremento della versione dei componenti core nella base di codice dell’applicazione.

+ __Sintomi:__
il passaggio Genera immagine non riesce con un messaggio di errore che segnala che 
`com.adobe.cq.wcm.core.components...` Impossibile importare i pacchetti in intervalli di versioni specifici dal  `core` progetto.

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __Causa:__  il bundle OSGi dell&#39;applicazione (definito nel  `core` progetto) importa le classi Java dalla dipendenza core dei componenti core, a un livello di versione diverso da quello distribuito per AEM come Cloud Service.
+ __Risoluzione:__
   + Utilizzando Git, ripristina un commit di lavoro esistente prima dell’incremento della versione del componente core. Invia questo commit a un ramo Git di Cloud Manager ed esegui un aggiornamento dell’ambiente da questo ramo. Verrà eseguito l’aggiornamento AEM come Cloud Service all’ultima versione AEM, che includerà la versione successiva dei componenti core. Una volta che l’AEM come Cloud Service viene aggiornato all’ultima versione AEM, che avrà l’ultima versione dei componenti core, ridistribuisci il codice originariamente non riuscito.
   + Per riprodurre questo problema localmente, accertati che la versione dell&#39;SDK AEM sia la stessa versione AEM che l&#39;AEM di un ambiente di Cloud Service sta utilizzando.


### Creare un caso di assistenza per Adobi

Se gli approcci di risoluzione dei problemi sopra descritti non risolvono il problema, crea un caso di assistenza Adobe, tramite:

+ [Adobe Admin Console](https://adminconsole.adobe.com)  > Scheda Supporto > Crea caso

   _Se sei membro di più organizzazioni di Adobe, assicurati che l’organizzazione di Adobe con pipeline non riuscita sia selezionata nel commutatore delle organizzazioni di Adobe prima di creare il caso._

## Distribuisci su

Il passaggio Distribuisci a è responsabile dell’artefatto di codice generato in Genera immagine, avvia i nuovi servizi di authoring e pubblicazione AEM che lo utilizzano e, dopo il successo, rimuove tutti i vecchi servizi di authoring e pubblicazione AEM. Anche in questo passaggio vengono installati e aggiornati pacchetti di contenuto variabile e indici.

Acquisisci familiarità con [AEM come log di Cloud Service](./logs.md) prima di eseguire il debug della distribuzione al passaggio . Il registro `aemerror` contiene informazioni sull&#39;avvio e lo spegnimento dei pod che possono essere pertinenti per distribuire i problemi. Il registro disponibile tramite il pulsante Download Log nel passaggio Distribuzione a di Cloud Manager non è il registro `aemerror` e non contiene informazioni dettagliate relative all’avvio delle applicazioni.

![Distribuisci su](./assets/build-and-deployment/deploy-to.png)

I tre motivi principali per cui il passaggio Distribuisci a potrebbe non riuscire:

### La pipeline di Cloud Manager contiene una vecchia versione AEM

+ __Causa:__ una pipeline di Cloud Manager contiene una versione precedente di AEM rispetto a quella distribuita nell’ambiente di destinazione. Questo può accadere quando una pipeline viene riutilizzata e indirizzata a un nuovo ambiente che esegue una versione successiva di AEM. Questo può essere identificato controllando se la versione AEM dell’ambiente è maggiore della versione AEM della pipeline.
   ![La pipeline di Cloud Manager contiene una vecchia versione AEM](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Risoluzione:__
   + Se nell’ambiente di destinazione è disponibile un aggiornamento, seleziona Aggiorna dalle azioni dell’ambiente, quindi esegui nuovamente la build.
   + Se nell’ambiente di destinazione non è disponibile un aggiornamento, significa che è in esecuzione la versione più recente di AEM. Per risolvere il problema, elimina la pipeline e ricreala.


### Timeout di Cloud Manager

Il codice in esecuzione durante l’avvio del servizio AEM appena implementato richiede così tanto tempo che Cloud Manager riceve un timeout prima del completamento della distribuzione. In questi casi, l’implementazione potrebbe avere successo, anche se lo stato di Cloud Manager segnalava Non riuscito.

+ __Causa:__ il codice personalizzato può eseguire operazioni, come query di grandi dimensioni o traversate di contenuti, attivate all’inizio nel bundle OSGi o nei cicli di vita dei componenti, ritardando significativamente il tempo di avvio del AEM.
+ __Risoluzione:__ esamina l&#39;implementazione del codice in esecuzione all&#39;inizio del ciclo di vita del bundle di OSGi, esamina i  `aemerror` registri dei servizi Author e Publish di AEM al momento dell&#39;errore (ora di accesso GMT) come mostrato dal Cloud Manager e cerca i messaggi di registro che indicano eventuali processi di log in esecuzione personalizzati.

### Codice o configurazione non compatibili

La maggior parte delle violazioni del codice e della configurazione vengono rilevate in precedenza nella build, tuttavia è possibile che il codice personalizzato o la configurazione sia incompatibile con il AEM come Cloud Service e non venga rilevato finché non viene eseguito nel contenitore .

+ __Causa:__ il codice personalizzato può richiamare operazioni lunghe, come query di grandi dimensioni o traversate di contenuti, attivate all’inizio nel bundle OSGi o nei cicli di vita dei componenti, rallentando significativamente il tempo di avvio del AEM.
+ __Risoluzione:__ esamina i  `aemerror` registri dei servizi Author e Publish di AEM intorno all’ora (ora di accesso GMT) dell’errore, come mostrato da Cloud Manager.
   1. Controlla i registri per eventuali ERRORS generati dalle classi Java fornite dall&#39;applicazione personalizzata. Se vengono rilevati problemi, risolvi, invia il push del codice fisso e ricompila la pipeline.
   1. Controlla i registri per eventuali ERRORS segnalati da aspetti di AEM che stai estendendo/interagendo con nell&#39;applicazione personalizzata e analizzali; questi ERRORI potrebbero non essere direttamente attribuiti alle classi Java. Se vengono rilevati problemi, risolvi, invia il push del codice fisso e ricompila la pipeline.

### Inclusione di /var nel pacchetto di contenuti

`/var` è modificabile contenente una varietà di contenuti transitori e di runtime. Inclusione di `/var` in pacchetti di contenuti (ad esempio). `ui.content`) implementata tramite Cloud Manager potrebbe causare il mancato funzionamento del passaggio Distribuzione .

Questo problema è difficile da identificare in quanto non si traduce in un errore nella distribuzione iniziale, solo nelle distribuzioni successive. I sintomi noti includono:

+ La distribuzione iniziale ha esito positivo, indipendentemente dal fatto che il contenuto mutabile, nuovo o modificato, che fa parte della distribuzione, non esista nel servizio AEM Publish.
+ L’attivazione/disattivazione del contenuto in AEM Author è bloccata
+ Le implementazioni successive non riescono nel passaggio Distribuisci a e il passaggio Distribuisci a non riesce dopo circa 60 minuti.

Per convalidare questo problema è la causa del comportamento non riuscito:

1. Determinando che almeno un pacchetto di contenuto fa parte della distribuzione, scrive in `/var`.
1. Verifica che la coda di distribuzione primaria (in grassetto) sia bloccata in:
   + AEM Author > Strumenti > Implementazione > Distribuzione
      ![Coda di distribuzione bloccata](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. In caso di errore nella distribuzione successiva, scarica i registri &quot;Distribuisci a&quot; di Cloud Manager utilizzando il pulsante Download Log (Scarica registro):

   ![Scarica la distribuzione nei registri](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... e verifica che ci siano circa 60 minuti tra le istruzioni di log:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... e ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Tieni presente che questo registro non conterrà questi indicatori sulle distribuzioni iniziali che hanno avuto esito positivo, ma solo sulle distribuzioni successive con errori.

+ __Causa:__ AEM utente del servizio di replica utilizzato per distribuire pacchetti di contenuto al servizio AEM Publish non è in grado di scrivere su  `/var` AEM Publish. Questo causa un errore nella distribuzione del pacchetto di contenuti al servizio AEM Publish.
+ __Risoluzione:__ i seguenti modi per risolvere questi problemi sono elencati in ordine di preferenza:
   1. Se le risorse `/var` non sono necessarie, rimuovi le risorse sotto `/var` dai pacchetti di contenuto distribuiti come parte dell&#39;applicazione.
   2. Se le risorse `/var` sono necessarie, definisci le strutture dei nodi utilizzando [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). Gli script di reindirizzamento possono essere indirizzati a AEM Author, AEM Publish o a entrambi tramite le modalità di esecuzione OSGi.
   3. Se le risorse `/var` sono necessarie solo per AEM autore e non possono essere modellate in modo ragionevole utilizzando [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit), spostale in un pacchetto di contenuti discreti, installato solo su AEM Author da [incorporare](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) nel pacchetto `all` in una cartella di modalità di esecuzione di AEM Author (`<target>/apps/example-packages/content/install.author</target>`).
   4. Fornisci ACL appropriati all&#39;utente del servizio `sling-distribution-importer` come descritto in questo [Adobe KB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Creare un caso di assistenza per Adobi

Se gli approcci di risoluzione dei problemi sopra descritti non risolvono il problema, crea un caso di assistenza Adobe, tramite:

+ [Adobe Admin Console](https://adminconsole.adobe.com)  > Scheda Supporto > Crea caso

   _Se sei membro di più organizzazioni di Adobe, assicurati che l’organizzazione di Adobe con pipeline non riuscita sia selezionata nel commutatore delle organizzazioni di Adobe prima di creare il caso._
