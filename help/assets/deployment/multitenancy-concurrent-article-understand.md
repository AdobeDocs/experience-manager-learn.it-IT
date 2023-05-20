---
title: Informazioni sulla multitenancy e sullo sviluppo simultaneo
description: Scopri i vantaggi, le sfide e le tecniche per gestire un’implementazione multi-tenant con Adobe Experience Manager Assets.
feature: Connected Assets
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: c9ee29d4-a8a5-4e61-bc99-498674887da5
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2017'
ht-degree: 0%

---

# Informazioni sulla multitenancy e sullo sviluppo simultaneo {#understanding-multitenancy-and-concurrent-development}

## Introduzione {#introduction}

Quando più team distribuiscono il codice negli stessi ambienti AEM, è consigliabile seguire alcune procedure per garantire che i team possano lavorare nel modo più indipendente possibile, senza dover intervenire sulle attività degli altri team. Anche se non possono mai essere completamente eliminate, queste tecniche ridurranno al minimo le dipendenze tra i team. Affinché un modello di sviluppo simultaneo abbia successo, è fondamentale che vi sia una buona comunicazione tra i team di sviluppo.

Inoltre, quando più team di sviluppo lavorano sullo stesso ambiente AEM, è probabile che ci sia un certo grado di multi-tenancy in gioco. Molto è stato scritto sulle considerazioni pratiche relative al tentativo di supportare più tenant in un ambiente AEM, in particolare sulle sfide affrontate durante la gestione della governance, delle operazioni e dello sviluppo. Questo documento esplora alcune delle sfide tecniche relative all’implementazione dell’AEM in un ambiente multi-tenant, ma molte di queste raccomandazioni saranno valide per qualsiasi organizzazione con più team di sviluppo.

È importante notare fin da subito che, sebbene l’AEM possa supportare più siti e anche più marchi implementati in un unico ambiente, non offre una vera multi-tenancy. Alcune configurazioni di ambiente e risorse di sistema verranno sempre condivise tra tutti i siti distribuiti in un ambiente. Il presente documento fornisce orientamenti per ridurre al minimo l&#39;impatto di queste risorse condivise e offre suggerimenti per semplificare la comunicazione e la collaborazione in questi settori.

## Vantaggi e sfide {#benefits-and-challenges}

L’implementazione di un ambiente multi-tenant comporta diverse sfide.

Comprendono:

* Ulteriore complessità tecnica
* Aumento del sovraccarico di sviluppo
* Dipendenze tra organizzazioni sulla risorsa condivisa
* Maggiore complessità operativa

Nonostante le difficoltà, l&#39;esecuzione di un&#39;applicazione multi-tenant offre vantaggi quali:

* Riduzione dei costi hardware
* Tempi di commercializzazione ridotti per i siti futuri
* Riduzione dei costi di implementazione per i tenant futuri
* Architettura standard e procedure di sviluppo in tutta l&#39;azienda
* Una base di codice comune

Se l’azienda richiede una multi-tenancy effettiva, senza conoscere altri tenant e nessun codice, contenuto o autore comune condiviso, l’unica opzione disponibile è istanze di authoring separate. L’aumento complessivo dello sforzo di sviluppo dovrebbe essere confrontato con i risparmi realizzati sui costi delle infrastrutture e delle licenze per determinare se questo approccio sia il più adatto.

## Tecniche di sviluppo {#development-techniques}

### Gestione delle dipendenze {#managing-dependencies}

Quando gestisci le dipendenze dei progetti Maven, è importante che tutti i team utilizzino sul server la stessa versione di un determinato bundle OSGi. Per illustrare cosa può andare storto quando i progetti Maven vengono gestiti in modo errato, presentiamo un esempio:

Il progetto A dipende dalla versione 1.0 della libreria, foo; foo versione 1.0 è incorporato nella loro distribuzione al server. Il progetto B dipende dalla versione 1.1 della libreria, foo; la versione 1.1 è incorporata nella loro distribuzione.

Inoltre, supponiamo che un’API sia stata modificata in questa libreria tra le versioni 1.0 e 1.1. A questo punto, uno di questi due progetti non funzionerà più correttamente.

Per risolvere questo problema, si consiglia di fare in modo che tutti i progetti Maven siano figli di un progetto di reattore principale. Questo progetto reactor ha due scopi: consente la creazione e la distribuzione di tutti i progetti insieme, se lo si desidera, e contiene le dichiarazioni di dipendenza per tutti i progetti figlio. Il progetto padre definisce le dipendenze e le relative versioni, mentre i progetti figlio dichiarano solo le dipendenze necessarie, ereditando la versione dal progetto padre.

In questo scenario, se il team che lavora al Progetto B richiede funzionalità nella versione 1.1 di foo, diventerà presto evidente nell’ambiente di sviluppo che questa modifica interromperà il Progetto A. A questo punto i team possono discutere di questa modifica e rendere il progetto A compatibile con la nuova versione oppure cercare una soluzione alternativa per il progetto B.

Tieni presente che questo non elimina la necessità per questi team di condividere questa dipendenza, ma si limita a evidenziare i problemi in modo rapido e tempestivo, in modo che i team possano discutere eventuali rischi e concordare una soluzione.

### Impedire la duplicazione del codice {#preventing-code-duplication-nbsp-br}

Quando si lavora su più progetti, è importante assicurarsi che il codice non venga duplicato. La duplicazione del codice aumenta la probabilità di occorrenze di difetti, il costo delle modifiche al sistema e la rigidità generale della base di codice. Per evitare duplicazioni, effettua il refactoring della logica comune in librerie riutilizzabili che possono essere utilizzate in più progetti.

Per supportare questa esigenza, consigliamo lo sviluppo e la manutenzione di un progetto di base da cui tutti i team possano dipendere e a cui possano contribuire. Nel farlo, è importante assicurarsi che questo progetto di base non dipenda, a sua volta, da nessuno dei progetti dei singoli team; questo consente una distribuzione indipendente, promuovendo al contempo il riutilizzo del codice.

Alcuni esempi di codice che comunemente si trovano in un modulo core includono:

* Configurazioni a livello di sistema quali:
   * Configurazioni OSGi
   * Filtri servlet
   * Mappature ResourceResolver
   * Pipeline per trasformatori Sling
   * Gestori errori (o utilizza il Gestore pagina di errore ACS AEM Commons1)
   * Servlet di autorizzazione per il caching sensibile alle autorizzazioni
* Classi di utilità
* Logica aziendale di base
* Logica di integrazione di terze parti
* Sovrapposizioni nell’interfaccia utente di authoring
* Altre personalizzazioni necessarie per l’authoring, ad esempio widget personalizzati
* Moduli di avvio dei flussi di lavoro
* Elementi di progettazione comuni utilizzati nei vari siti

*Architettura di progetto modulare*

Questo non elimina la necessità per più team di dipendere e potenzialmente aggiornare lo stesso set di codice. Creando un progetto di base, abbiamo ridotto la dimensione della base di codice condivisa tra i team, riducendo ma non eliminando la necessità di risorse condivise.

Per garantire che le modifiche apportate a questo pacchetto di base non interrompano la funzionalità del sistema, consigliamo a uno sviluppatore senior o a un team di sviluppatori di mantenere la supervisione. Un’opzione consiste nell’avere un unico team che gestisca tutte le modifiche a questo pacchetto; un’altra consiste nell’inviare ai team le richieste pull che vengono esaminate e unite da queste risorse. È importante che i team progettino e approvino un modello di governance e che gli sviluppatori lo seguano.

## Gestione dell’ambito di implementazione  {#managing-deployment-scope}

Poiché i diversi team distribuiscono il codice nello stesso archivio, è importante che non si sovrascrivano reciprocamente le modifiche. L’AEM dispone di un meccanismo per controllare questo fenomeno durante la distribuzione dei pacchetti di contenuti, il filtro. file xml. È importante che non vi sia sovrapposizione tra i filtri.  file xml, altrimenti la distribuzione di un team potrebbe potenzialmente cancellare la precedente distribuzione di un altro team. Per illustrare questo punto, vedi i seguenti esempi di file di filtro ben creati e problematici:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Se ogni team configura in modo esplicito i file di filtro nei siti su cui sta lavorando, ogni team può distribuire i componenti, le librerie client e le progettazioni dei siti in modo indipendente senza cancellare le modifiche reciproche.

Poiché si tratta di un percorso di sistema globale e non specifico di un sito, il seguente servlet deve essere incluso nel progetto di base, in quanto le modifiche apportate qui potrebbero potenzialmente influire su qualsiasi team:

/apps/sling/servlet/errorhandler

### Sovrapposizioni {#overlays}

Le sovrapposizioni vengono spesso utilizzate per estendere o sostituire la funzionalità AEM predefinita, ma l’utilizzo di una sovrapposizione influisce sull’intera applicazione AEM (ovvero, qualsiasi modifica della funzionalità sovrapposta è disponibile per tutti i tenant). Questo sarebbe ancora più complicato se i tenant avessero requisiti diversi per la sovrapposizione. Idealmente, i gruppi di business dovrebbero collaborare per concordare la funzionalità e l&#39;aspetto delle console amministrative dell&#39;AEM.

Se non si riesce a raggiungere un consenso tra le varie unità aziendali, una possibile soluzione sarebbe semplicemente quella di non utilizzare le sovrapposizioni. Al contrario, crea una copia personalizzata della funzionalità e la espone tramite un percorso diverso per ogni tenant. Questo consente a ogni tenant di avere un’esperienza utente completamente diversa, ma questo approccio aumenta anche il costo dell’implementazione e delle successive attività di aggiornamento.

### Moduli di avvio per flusso di lavoro {#workflow-launchers}

L’AEM utilizza i moduli di avvio dei flussi di lavoro per attivare automaticamente l’esecuzione dei flussi di lavoro quando vengono apportate modifiche specificate nell’archivio. AEM fornisce diversi moduli di avvio predefiniti, ad esempio per eseguire la generazione di rendering e i processi di estrazione dei metadati su risorse nuove e aggiornate. Anche se è possibile lasciare invariati questi moduli di avvio, in un ambiente multi-tenant, se i tenant hanno requisiti diversi per quanto riguarda il modulo di avvio e/o il modello di flusso di lavoro, è probabile che sarà necessario creare e gestire singoli moduli di avvio per ciascun tenant. Questi moduli di avvio dovranno essere configurati per l&#39;esecuzione sugli aggiornamenti del tenant e lasciare intatti i contenuti di altri tenant. Questo è possibile applicando moduli di avvio a specifici percorsi dell’archivio specifici per il tenant.

### Gli URL personalizzati {#vanity-urls}

L’AEM fornisce funzionalità per URL personalizzati che possono essere impostati per ogni pagina. Il problema di questo approccio in uno scenario multi-tenant è che l’AEM non garantisce l’univocità tra gli URL personalizzati configurati in questo modo. Se due utenti diversi configurano lo stesso percorso personalizzato per pagine diverse, si può verificare un comportamento imprevisto. Per questo motivo, consigliamo di utilizzare le regole mod_rewrite nelle istanze del dispatcher Apache, che consentono un punto di configurazione centrale insieme alle regole del Resource Resolver in uscita.

### Gruppi di componenti {#component-groups}

Quando si sviluppano componenti e modelli per più gruppi di authoring, è importante utilizzare in modo efficace le proprietà componentGroup e allowedPaths. Sfruttandole in modo efficace con le progettazioni del sito, possiamo garantire che gli autori del Marchio A vedano solo i componenti e i modelli creati per il loro sito, mentre gli autori del Marchio B vedano solo i propri.

### Test {#testing}

Anche se una buona architettura e canali di comunicazione aperti possono aiutare a prevenire l&#39;introduzione di difetti in aree impreviste del sito, questi approcci non sono infallibili. Per questo motivo, è importante testare completamente ciò che viene distribuito sulla piattaforma prima di rilasciare qualsiasi elemento in produzione. Ciò richiede il coordinamento tra i team sui rispettivi cicli di rilascio e rafforza la necessità di una suite di test automatizzati che coprano quante più funzionalità possibili. Inoltre, poiché un sistema è condiviso da più team, le prestazioni, la sicurezza e il test di carico diventano più importanti che mai.

## Considerazioni operative {#operational-considerations}

### Risorse condivise {#shared-resources}

L&#39;AEM viene eseguito all&#39;interno di una singola JVM; tutte le applicazioni AEM implementate condividono intrinsecamente le risorse tra loro, oltre alle risorse già utilizzate nella normale esecuzione dell&#39;AEM. All&#39;interno dello spazio JVM stesso, non vi è alcuna separazione logica dei thread e vengono condivise anche le risorse limitate disponibili per l&#39;AEM, come memoria, CPU e I/O del disco. Qualsiasi tenant che consuma risorse influirà inevitabilmente su altri tenant di sistema.

### Prestazioni {#performance}

Se non si seguono le best practice per l’AEM, è possibile sviluppare applicazioni che consumano risorse al di là di quanto è considerato normale. Ad esempio, vengono attivate molte operazioni complesse per il flusso di lavoro (come Aggiorna risorsa DAM), operazioni push-on-modify di MSM su più nodi o query JCR costose per eseguire il rendering del contenuto in tempo reale. Ciò avrà inevitabilmente un impatto sulle prestazioni di altre applicazioni tenant.

### Registrazione {#logging}

L’AEM fornisce interfacce pronte all’uso per una solida configurazione dei logger che può essere utilizzata a nostro vantaggio in scenari di sviluppo condiviso. Specificando logger separati per ogni marchio, in base al nome del pacchetto, possiamo ottenere un certo grado di separazione del registro. Anche se le operazioni a livello di sistema come la replica e l’autenticazione verranno comunque registrate in una posizione centrale, il codice personalizzato non condiviso può essere registrato separatamente, semplificando le attività di monitoraggio e debug per il team tecnico di ciascun marchio.

### Backup e ripristino {#backup-and-restore}

A causa della natura dell’archivio JCR, i backup tradizionali funzionano sull’intero archivio, anziché su un singolo percorso di contenuto. Non è quindi possibile separare facilmente i backup in base al singolo tenant. Al contrario, il ripristino da un backup ripristina il contenuto e i nodi dell’archivio per tutti i tenant sul sistema. Anche se è possibile eseguire backup di contenuto mirati, utilizzando strumenti come VLT, o scegliere contenuti da ripristinare creando un pacchetto in un ambiente separato, questi\
gli approcci non comprendono facilmente le impostazioni di configurazione o la logica dell’applicazione e possono essere difficili da gestire.

## Considerazioni sulla sicurezza {#security-considerations}

### ACL {#acls}

È ovviamente possibile utilizzare gli elenchi di controllo di accesso (ACL, Access Control List) per controllare chi ha accesso alla visualizzazione, alla creazione e all’eliminazione di contenuti basati su percorsi di contenuto, il che richiede la creazione e la gestione di gruppi di utenti. La difficoltà di mantenere gli ACL e i gruppi dipende dall’importanza di garantire che ogni tenant non abbia alcuna conoscenza degli altri e che le applicazioni distribuite si basino su risorse condivise. Per garantire un’amministrazione efficiente di ACL, utenti e gruppi, si consiglia di avere un gruppo centralizzato con la supervisione necessaria per garantire che questi controlli di accesso e utenti/gruppi/ruoli si sovrappongano (o non si sovrappongano) in modo da promuovere l’efficienza e la sicurezza.
