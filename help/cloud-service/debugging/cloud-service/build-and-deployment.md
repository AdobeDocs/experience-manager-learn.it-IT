---
title: Build e implementazioni
description: Adobe Cloud Manager facilita la creazione del codice e le distribuzioni in AEM as a Cloud Service. Possono verificarsi errori durante le fasi del processo di compilazione, che richiedono un'azione per risolverli. Questa guida illustra come comprendere gli errori comuni in nell’implementazione e come affrontarli nel modo migliore.
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2523'
ht-degree: 0%

---

# Debug delle build e delle implementazioni as a Cloud Service da AEM

Adobe Cloud Manager facilita la creazione del codice e le distribuzioni in AEM as a Cloud Service. Possono verificarsi errori durante le fasi del processo di compilazione, che richiedono un&#39;azione per risolverli. Questa guida illustra come comprendere gli errori comuni in nell’implementazione e come affrontarli nel modo migliore.

![Pipeline di build per gestione cloud](./assets/build-and-deployment/build-pipeline.png)

## Convalida

Il passaggio di convalida assicura semplicemente la validità delle configurazioni di base di Cloud Manager. Gli errori comuni di convalida includono:

### L’ambiente è in uno stato non valido

+ __Messaggio di errore:__ L’ambiente è in uno stato non valido.
   ![L’ambiente è in uno stato non valido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ L’ambiente di destinazione della pipeline è in uno stato di transizione in cui non può accettare nuove build.
+ __Risoluzione:__ Attendere la risoluzione dello stato in esecuzione (o aggiornamento disponibile). Se l’ambiente viene eliminato, ricrealo o scegli un altro ambiente in cui generare la build.

### Impossibile trovare l’ambiente associato alla pipeline

+ __Messaggio di errore:__ L’ambiente è contrassegnato come eliminato.
   ![L’ambiente è contrassegnato come eliminato](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ L’ambiente per il quale la pipeline è configurata è stato eliminato.
Anche se viene ricreato un nuovo ambiente con lo stesso nome, Cloud Manager non associa automaticamente la pipeline a tale ambiente con lo stesso nome.
+ __Risoluzione:__ Modifica la configurazione della pipeline e riseleziona l’ambiente in cui eseguire la distribuzione.

### Impossibile trovare il ramo Git associato alla pipeline

+ __Messaggio di errore:__ Pipeline non valida: XXXXXX. Reason=Branch=xxxx non trovato nell&#39;archivio.
   ![Pipeline non valida: XXXXXX. Reason=Branch=xxxx non trovato nell’archivio](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ Il ramo Git per il quale la pipeline è configurata è stato eliminato.
+ __Risoluzione:__ Ricrea il ramo Git mancante utilizzando lo stesso nome o riconfigura la pipeline per la generazione da un ramo esistente diverso.

## Test di compilazione e di unità

![Test di compilazione e di unità](./assets/build-and-deployment/build-and-unit-testing.png)

La fase Build e Unit Testing esegue una build Maven (`mvn clean package`) del progetto estratto dal ramo Git configurato della pipeline.

Gli errori identificati in questa fase devono essere riproducibili nella creazione locale del progetto, con le seguenti eccezioni:

+ Una dipendenza Maven non disponibile su [Maven Central](https://search.maven.org/) viene utilizzato e l’archivio Maven contenente la dipendenza è:
   + Non raggiungibile da Cloud Manager, ad esempio un archivio Maven interno privato, o l’archivio Maven richiede l’autenticazione e sono state fornite credenziali errate.
   + Non registrato esplicitamente nel `pom.xml`. Tieni presente che, includere gli archivi Maven è sconsigliato in quanto aumenta i tempi di creazione.
+ Gli unit test non riescono a causa di problemi di tempistica. Ciò può verificarsi quando gli unit test sono sensibili agli intervalli. Un forte indicatore si basa su `.sleep(..)` nel codice del test.
+ Utilizzo di plug-in Maven non supportati.

## Scansione del codice

![Scansione del codice](./assets/build-and-deployment/code-scanning.png)

La scansione del codice esegue l’analisi del codice statico utilizzando una combinazione di best practice specifiche per Java e AEM.

Se nel codice sono presenti vulnerabilità di sicurezza critiche, l’analisi del codice genera un errore di build. Le violazioni meno importanti possono essere ignorate, ma si consiglia di correggerle. La scansione del codice non è perfetta e può causare [falsi positivi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives).

Per risolvere i problemi di scansione del codice, scarica il rapporto in formato CSV fornito da Cloud Manager tramite **Dettagli del download** e rivedere eventuali voci.

Per ulteriori dettagli, consulta Regole specifiche per l’AEM, consulta la documentazione di Cloud Manager. [regole di scansione del codice specifiche per AEM personalizzate](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Genera immagini

![Genera immagini](./assets/build-and-deployment/build-images.png)

L’immagine della build è responsabile della combinazione degli artefatti di codice creati nel passaggio Build e Unit Testing con la versione dell’AEM, per formare un singolo artefatto distribuibile.

Anche se durante la generazione e il testing di unità vengono riscontrati problemi di compilazione e compilazione del codice, possono esserci problemi di configurazione o strutturali identificati quando si tenta di combinare l’artefatto di compilazione personalizzato con la versione dell’AEM.

### Configurazioni OSGi duplicate

Quando più configurazioni OSGi si risolvono tramite la modalità di esecuzione per l’ambiente AEM di destinazione, il passaggio Genera immagine non riesce e viene visualizzato il messaggio di errore:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ Il pacchetto all del progetto AEM contiene più pacchetti di codice e la stessa configurazione OSGi viene fornita da più pacchetti di codice, causando un conflitto che impedisce al passaggio Build Image di decidere quale utilizzare, causando un errore nella build. Questo non si applica alle configurazioni di fabbrica OSGi, purché abbiano nomi univoci.
+ __Risoluzione:__ Esamina tutti i pacchetti di codice (inclusi eventuali pacchetti di codice di terze parti) distribuiti come parte dell’applicazione AEM, cercando configurazioni OSGi duplicate che si risolvono nell’ambiente di destinazione tramite la modalità di esecuzione. La guida del messaggio di errore &quot;imposta il flag mergeConfigurations su true&quot; non è possibile in AEM as a Cloud Service e deve essere ignorata.

#### Causa 2

+ __Causa:__ Il progetto AEM include erroneamente lo stesso pacchetto di codice due volte, con conseguente duplicazione di qualsiasi configurazione OSGi contenuta in tale pacchetto.
+ __Risoluzione:__ Rivedi tutti i pacchetti pom.xml incorporati nel progetto all e assicurati che abbiano `filevault-package-maven-plugin` [configurazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) imposta su `<cloudManagerTarget>none</cloudManagerTarget>`.

### Script di repoinit non valido

Gli script Repoinit definiscono il contenuto della linea di base, gli utenti, gli ACL, ecc. In AEM as a Cloud Service, gli script di repoinit vengono applicati durante la generazione dell’immagine, ma nell’avvio rapido locale dell’SDK dell’AEM vengono applicati quando la configurazione della factory di repoinit OSGi è attivata. Per questo motivo, gli script Repoinit potrebbero non riuscire (con la registrazione) nell’avvio rapido locale dell’SDK dell’AEM, ma il passaggio Build Image non riesce, interrompendo la distribuzione.

+ __Causa:__ Script di repoinit non valido. Questo potrebbe lasciare l’archivio in uno stato incompleto, in quanto eventuali script di repoinit dopo lo script non riuscito non vengono eseguiti nell’archivio.
+ __Risoluzione:__ Controlla l’avvio rapido locale dell’SDK dell’AEM quando viene distribuita la configurazione OSGi dello script di repoinit per determinare se e quali sono gli errori.

### Dipendenza contenuto repoinit non soddisfatta

Gli script Repoinit definiscono il contenuto della linea di base, gli utenti, gli ACL, ecc. Nel quickstart locale dell&#39;SDK dell&#39;AEM, gli script di repoinit vengono applicati quando la configurazione di fabbrica OSGi di repoinit è attivata, o in altre parole, dopo che l&#39;archivio è attivo e potrebbe aver subito modifiche al contenuto direttamente o tramite pacchetti di contenuti. In AEM as a Cloud Service, gli script repoinit vengono applicati durante la generazione dell’immagine in un archivio che potrebbe non contenere contenuto da cui dipende lo script repoinit.

+ __Causa:__ Uno script di repoinit dipende da contenuto inesistente.
+ __Risoluzione:__ Assicurati che esista il contenuto da cui dipende lo script Repoinit. Spesso questo indica una definizione inadeguata degli script di repoinit con direttive mancanti che definiscono queste strutture di contenuto mancanti, ma necessarie. Questa operazione può essere riprodotta localmente eliminando l’AEM, decomprimendo il file JAR e aggiungendo la configurazione OSGi repoinit contenente lo script repoinit alla cartella di installazione e avviando l’AEM. L’errore si presenterà nel file error.log del quickstart locale dell’SDK dell’AEM.


### La versione dei Componenti core dell’applicazione è successiva alla versione implementata

_Questo problema riguarda solo gli ambienti non di produzione che NON eseguono l’aggiornamento automatico all’ultima versione dell’AEM._

AEM as a Cloud Service include automaticamente la versione più recente dei Componenti core in ogni versione AEM, ovvero dopo che a un ambiente AEM as a Cloud Service viene automaticamente o manualmente aggiornata la versione più recente dei Componenti core implementata.

È possibile che il passaggio Genera immagine non riesca quando:

+ L’applicazione che distribuisce aggiorna la versione della dipendenza Maven dei Componenti core in `core` Progetto (bundle OSGi)
+ L’applicazione in distribuzione viene quindi distribuita in un ambiente as a Cloud Service AEM sandbox (non di produzione) che non è stato aggiornato per l’utilizzo di una versione AEM contenente la nuova versione dei Componenti core.

Per evitare questo errore, ogni volta che è disponibile un aggiornamento dell’ambiente as a Cloud Service AEM, includi l’aggiornamento come parte della build/distribuzione successiva e assicurati sempre che gli aggiornamenti vengano inclusi dopo l’incremento della versione dei Componenti core nella base di codice dell’applicazione.

+ __Sintomi:__
Il passaggio Genera immagine non riesce e viene visualizzato un messaggio di ERRORE che segnala che 
`com.adobe.cq.wcm.core.components...` impossibile importare i pacchetti in intervalli di versioni specifici da `core` progetto.

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __Causa:__  Il bundle OSGi dell’applicazione (definito nel `core` project) importa le classi Java dalla dipendenza principale dei Componenti core, a un livello di versione diverso da quello implementato in AEM as a Cloud Service.
+ __Risoluzione:__
   + Utilizzando Git, ripristina un commit di lavoro esistente prima dell’incremento di versione dei Componenti core. Invia questo commit a un ramo Git di Cloud Manager ed esegui un aggiornamento dell’ambiente da questo ramo. Questo aggiornerà AEM as a Cloud Service alla versione più recente dell’AEM, che includerà la versione più recente dei Componenti core. Una volta che l’AEM as a Cloud Service è stato aggiornato alla versione più recente dell’AEM, che avrà la versione più recente dei Componenti core, ridistribuisci il codice che originariamente non riusciva.
   + Per riprodurre questo problema localmente, assicurati che la versione dell’SDK per AEM sia la stessa versione dell’AEM utilizzata dall’ambiente as a Cloud Service AEM.


### Creazione di un caso di supporto Adobe

Se gli approcci di risoluzione dei problemi sopra descritti non risolvono il problema, crea un caso di supporto Adobe tramite:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Scheda Supporto > Crea caso

   _Se sei membro di più organizzazioni di Adobe, accertati che l’organizzazione di Adobe con pipeline non riuscita sia selezionata nel selettore Organizzazioni di Adobe prima di creare il caso._

## Distribuisci in

Il passaggio Distribuisci su è responsabile dell’esecuzione dell’artefatto di codice generato nell’immagine di creazione, avvia i nuovi servizi Author e Publish di AEM e, una volta completato, rimuove tutti i vecchi servizi Author e Publish di AEM. Anche in questo passaggio vengono installati e aggiornati pacchetti e indici di contenuto variabile.

Acquisisci familiarità con [Registri AEM as a Cloud Service](./logs.md) prima di eseguire il debug del passaggio Distribuisci in. Il `aemerror` Il registro contiene informazioni sull’avvio e l’arresto dei pod che possono essere utili per la distribuzione in caso di problemi. Tieni presente che il registro disponibile tramite il pulsante Scarica registro nel passaggio Distribuisci in di Cloud Manager non è il `aemerror` e non contiene informazioni dettagliate relative all’avvio delle applicazioni.

![Distribuisci in](./assets/build-and-deployment/deploy-to.png)

I tre motivi principali per cui la distribuzione al passaggio potrebbe non riuscire:

### La pipeline di Cloud Manager contiene una versione AEM precedente

+ __Causa:__ Una pipeline di Cloud Manager contiene una versione precedente dell’AEM rispetto a quella distribuita nell’ambiente di destinazione. Questo può accadere quando una pipeline viene riutilizzata e puntata a un nuovo ambiente che esegue una versione successiva dell’AEM. Questo può essere identificato controllando per vedere se la versione dell’AEM dell’ambiente è maggiore della versione dell’AEM della pipeline.
   ![La pipeline di Cloud Manager contiene una versione AEM precedente](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Risoluzione:__
   + Se nell&#39;ambiente di destinazione è disponibile un aggiornamento, selezionare Aggiorna dalle azioni dell&#39;ambiente, quindi eseguire nuovamente la build.
   + Se nell’ambiente di destinazione non è disponibile un aggiornamento, significa che è in esecuzione la versione più recente dell’AEM. Per risolvere questo problema, elimina la pipeline e ricreala.


### Timeout di Cloud Manager

Il codice in esecuzione durante l’avvio del servizio AEM appena implementato richiede così tanto tempo che Cloud Manager scade prima che la distribuzione possa essere completata. In questi casi, la distribuzione potrebbe avere esito positivo anche se è stato segnalato lo stato di Cloud Manager Non riuscito.

+ __Causa:__ Il codice personalizzato può eseguire operazioni, come query di grandi dimensioni o attraversamenti di contenuti, attivate in anticipo nel bundle OSGi o nei cicli di vita dei componenti, ritardando notevolmente il tempo di avvio dell’AEM.
+ __Risoluzione:__ Esamina l’implementazione del codice che viene eseguito all’inizio del ciclo di vita del bundle OSGi, quindi controlla `aemerror` registra per i servizi AEM Author e Publish intorno al momento dell’errore (tempo di registrazione in GMT), come mostrato da Cloud Manager, e cerca i messaggi di registro che indicano eventuali processi di esecuzione del registro personalizzati.

### Codice o configurazione non compatibile

La maggior parte del codice e delle violazioni della configurazione vengono rilevate in una fase precedente della build, tuttavia il codice personalizzato o la configurazione possono essere incompatibili con l’AEM as a Cloud Service e non vengono rilevate finché non vengono eseguite nel contenitore.

+ __Causa:__ Il codice personalizzato può richiamare operazioni lunghe, come query di grandi dimensioni o attraversamenti di contenuti, attivate in anticipo nel bundle OSGi o nei cicli di vita dei componenti, ritardando notevolmente il tempo di avvio dell’AEM.
+ __Risoluzione:__ Rivedi `aemerror` registra per i servizi di authoring e pubblicazione di AEM in qualsiasi momento (tempo di registrazione in GMT) dell’errore, come mostrato da Cloud Manager.
   1. Esamina i registri per individuare eventuali ERRORI generati dalle classi Java fornite dall’applicazione personalizzata. Se vengono rilevati problemi, risolvi questi, invia il codice corretto e ricompila la pipeline.
   1. Esamina i registri per individuare eventuali ERRORI segnalati da aspetti dell’AEM che stai estendendo/interagendo con nell’applicazione personalizzata e analizzali; questi ERRORI potrebbero non essere attribuiti direttamente alle classi Java. Se vengono rilevati problemi, risolvi questi, invia il codice corretto e ricompila la pipeline.

### Inclusione di /var nel pacchetto di contenuti

`/var` è mutabile e contiene diversi contenuti transitori e di runtime. Inclusione `/var` in pacchetti di contenuti (ad es. `ui.content`) implementato tramite Cloud Manager potrebbe causare un errore del passaggio di distribuzione.

Questo problema è difficile da identificare in quanto non si verifica un errore nella distribuzione iniziale, ma solo nelle distribuzioni successive. I sintomi più evidenti includono:

+ La distribuzione iniziale ha esito positivo, tuttavia il contenuto mutabile nuovo o modificato, che fa parte della distribuzione, non sembra esistere nel servizio di pubblicazione AEM.
+ L’attivazione/disattivazione del contenuto in AEM Author è bloccata
+ Le distribuzioni successive non riescono nel passaggio di implementazione a, con la distribuzione a un passaggio che non riesce dopo circa 60 minuti.

Per convalidare questo problema, la causa è il comportamento errato:

1. Determinando che almeno un pacchetto di contenuti che fa parte della distribuzione, scrive in `/var`.
1. Verifica che la coda di distribuzione primaria (in grassetto) sia bloccata in:
   + AEM Author > Strumenti > Implementazione > Distribuzione
      ![Coda di distribuzione bloccata](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. In caso di mancata distribuzione successiva, scarica i registri di &quot;Distribuisci in&quot; di Cloud Manager utilizzando il pulsante Scarica registro:

   ![Scarica la distribuzione nei registri](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... e verificare che trascorrano circa 60 minuti tra le istruzioni di registro:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... e ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Tieni presente che questo registro non conterrà questi indicatori sulle distribuzioni iniziali che riportano come riuscite, ma solo sulle distribuzioni successive con errori.

+ __Causa:__ L’utente del servizio di replica AEM utilizzato per distribuire pacchetti di contenuti nel servizio di pubblicazione AEM non può scrivere in `/var` in AEM Publish. In questo modo la distribuzione del pacchetto di contenuti al servizio di pubblicazione AEM non riesce.
+ __Risoluzione:__ Per risolvere questo problema, vengono elencati i seguenti modi in ordine di preferenza:
   1. Se il `/var` risorse non necessarie rimuovere le risorse in `/var` da pacchetti di contenuti distribuiti come parte dell’applicazione.
   2. Se il `/var` risorse sono necessarie, definisci le strutture dei nodi utilizzando [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). Gli script Repoinit possono essere indirizzati ad AEM Author, AEM Publish o a entrambi, tramite le modalità di esecuzione OSGi.
   3. Se il `/var` Le risorse sono necessarie solo per l’autore dell’AEM e non possono essere ragionevolmente modellate utilizzando [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit), spostali in un pacchetto di contenuti discreto, installato solo in AEM Author da [incorporamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=it#embeddeds) in `all` pacchetto in una cartella runmode di AEM Author (`<target>/apps/example-packages/content/install.author</target>`).
   4. Fornisci ACL appropriati al `sling-distribution-importer` utente del servizio come descritto [ADOBE KB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Creazione di un caso di supporto Adobe

Se gli approcci di risoluzione dei problemi sopra descritti non risolvono il problema, crea un caso di supporto Adobe tramite:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Scheda Supporto > Crea caso

   _Se sei membro di più organizzazioni di Adobe, accertati che l’organizzazione di Adobe con pipeline non riuscita sia selezionata nel selettore Organizzazioni di Adobe prima di creare il caso._
