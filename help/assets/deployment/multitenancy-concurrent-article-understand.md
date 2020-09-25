---
title: Multitenenza e sviluppo simultaneo
seo-title: Multitenenza e sviluppo simultaneo
description: 'null'
seo-description: 'null'
uuid: 682093fe-ce55-4ef8-af10-99f7062f8b1b
discoiquuid: 0dfcdf39-7423-459f-8f35-ee5b4b829f2c
feature: connected-assets
topics: authoring, operations, sharing, publishing
audience: all
doc-type: article
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 99f2a8cdfe0b4f5f6f1a149d96affd2a9e8bcf75
workflow-type: tm+mt
source-wordcount: '2009'
ht-degree: 0%

---


# Multitenenza e sviluppo simultaneo {#understanding-multitenancy-and-concurrent-development}

## Introduzione {#introduction}

Quando più team distribuiscono il codice negli stessi ambienti di AEM, esistono delle procedure che dovrebbero seguire per garantire che i team possano lavorare il più in modo indipendente possibile, senza oltrepassare le dita dei piedi degli altri team. Anche se non possono mai essere eliminati completamente, queste tecniche minimizzeranno le dipendenze tra team. Perché un modello di sviluppo concorrente abbia successo, è fondamentale una buona comunicazione tra i team di sviluppo.

Inoltre, quando più team di sviluppo lavorano sullo stesso ambiente AEM, è probabile che vi sia un certo grado di multi-tenancy in fase di riproduzione. Molto è stato scritto sulle considerazioni pratiche del tentativo di sostenere più locatari in un ambiente AEM, soprattutto sulle sfide affrontate nella gestione di governance, operazioni e sviluppo. Questo documento esamina alcune delle problematiche tecniche relative all&#39;implementazione di AEM in un ambiente multi-tenant, ma molte di queste raccomandazioni saranno valide per qualsiasi organizzazione con più team di sviluppo.

È importante notare che, pur AEM supportare più siti e anche più marchi distribuiti in un unico ambiente, non offre una vera e propria multi-tenant. Alcune configurazioni di ambiente e risorse di sistema saranno sempre condivise tra tutti i siti distribuiti in un ambiente. Il presente documento fornisce orientamenti per ridurre al minimo l&#39;impatto di tali risorse condivise e offre suggerimenti per razionalizzare la comunicazione e la collaborazione in questi settori.

## Vantaggi e sfide {#benefits-and-challenges}

L’implementazione di un ambiente multi-tenant comporta molte sfide.

Comprendono:

* Maggiore complessità tecnica
* Costi di sviluppo più elevati
* Dipendenze tra organizzazioni sulla risorsa condivisa
* Maggiore complessità operativa

Nonostante le sfide affrontate, l&#39;esecuzione di un&#39;applicazione multi-tenant presenta dei vantaggi, come:

* Riduzione dei costi hardware
* Riduzione dei tempi di commercializzazione per i siti futuri
* Riduzione dei costi di implementazione per i futuri locatari
* Architettura standard e pratiche di sviluppo in tutta l&#39;azienda
* Codebase comune

Se l&#39;azienda richiede una vera e propria multi-tenant, senza conoscere altri locatari e senza codice condiviso, contenuti o autori comuni, allora le istanze di autori separate sono l&#39;unica opzione possibile. L&#39;aumento complessivo degli sforzi di sviluppo dovrebbe essere confrontato con il risparmio in termini di infrastrutture e costi di licenza per determinare se questo approccio è il più adatto.

## Tecniche di sviluppo {#development-techniques}

### Gestione delle dipendenze {#managing-dependencies}

Quando gestite le dipendenze del progetto Maven, è importante che tutti i team utilizzino la stessa versione di un bundle OSGi specificato sul server. Per illustrare cosa può andare storto quando i progetti Maven sono mal gestiti, presentiamo un esempio:

Il progetto A dipende dalla versione 1.0 della libreria, foo; foo versione 1.0 è incorporato nella relativa distribuzione al server. Il progetto B dipende dalla versione 1.1 della libreria, foo; la versione 1.1 di Foo è incorporata nella relativa implementazione.

Supponiamo inoltre che in questa libreria sia stata modificata un&#39;API tra le versioni 1.0 e 1.1. A questo punto, uno di questi due progetti non funzionerà più correttamente.

Per rispondere a questa preoccupazione, si consiglia di fare di tutti i progetti Maven figli di un progetto di reattore principale. Questo progetto di reattore ha due obiettivi: consente la creazione e la distribuzione di tutti i progetti insieme, se lo desiderano, e contiene le dichiarazioni di dipendenza per tutti i progetti figlio. Il progetto padre definisce le dipendenze e le loro versioni, mentre i progetti secondari dichiarano solo le dipendenze necessarie, ereditando la versione dal progetto principale.

In questo scenario, se il team che lavora sul Progetto B richiede funzionalità nella versione 1.1 di foo, diventerà rapidamente evidente nell&#39;ambiente di sviluppo che questa modifica interromperà il Progetto A. A questo punto i team possono discutere di questa modifica e rendere il Progetto A compatibile con la nuova versione o cercare una soluzione alternativa per il Progetto B.

Questo non elimina la necessità per questi team di condividere questa dipendenza, ma evidenzia i problemi in modo rapido e tempestivo, in modo che i team possano discutere qualsiasi rischio e concordare una soluzione.

### Impedire la duplicazione del codice {#preventing-code-duplication-nbsp-br}

Quando si lavora su più progetti, è importante garantire che il codice non sia duplicato. La duplicazione del codice aumenta la probabilità di occorrenze difettose, il costo delle modifiche al sistema e la rigidità complessiva nella base del codice. Per evitare duplicazioni, è necessario rigenerare la logica comune in librerie riutilizzabili utilizzabili utilizzabili utilizzabili da utilizzare in più progetti.

Per soddisfare questa esigenza, consigliamo lo sviluppo e la manutenzione di un progetto fondamentale su cui tutti i team possono contare e a cui possono contribuire. In tale contesto, è importante garantire che il progetto principale non dipenda, a sua volta, da alcun progetto dei singoli team; questo consente una distribuzione indipendente e allo stesso tempo promuove il riutilizzo del codice.

Alcuni esempi di codice comunemente presenti in un modulo core includono:

* Configurazioni a livello di sistema come:
   * Configurazioni OSGi
   * Filtri servlet
   * Mappature ResourceResolver
   * Trasformatori Sling
   * Gestori di errori (o utilizzare il gestore di pagina di errore ACS AEM Commons1)
   * Servlet di autorizzazione per il caching sensibile alle autorizzazioni
* Classi di utilità
* Logica aziendale di base
* Logica di integrazione di terze parti
* Creazione di sovrapposizioni di interfaccia
* Altre personalizzazioni necessarie per la creazione, ad esempio widget personalizzati
* Avviatori di flussi di lavoro
* Elementi comuni di progettazione utilizzati in più siti

*Architettura del progetto modulare*

Ciò non elimina la necessità che più team dipendano da e possano aggiornare lo stesso set di codici. Creando un progetto di base, abbiamo ridotto la dimensione della base di codice condivisa tra i team, diminuendo ma non eliminando la necessità di risorse condivise.

Per garantire che le modifiche apportate a questo pacchetto di base non interferiscano con la funzionalità del sistema, si consiglia a uno sviluppatore senior o a un team di sviluppatori di mantenere la sorveglianza. Un&#39;opzione consiste nell&#39;avere un unico team che gestisca tutte le modifiche apportate al pacchetto; un altro è far sì che i team inviino richieste pull riviste e unite da tali risorse. È importante che un modello di governance sia progettato e concordato dai team e che gli sviluppatori lo seguano.

## Gestione dell&#39;ambito di distribuzione {#managing-deployment-scope}

Poiché i diversi team implementano il codice nello stesso repository, è importante che non sovrascrivano le rispettive modifiche. AEM dispone di un meccanismo per controllare questo problema quando si distribuiscono pacchetti di contenuto, il filtro. file xml. È importante che non vi sia sovrapposizione tra i filtri.  file xml, altrimenti la distribuzione di un team potrebbe eliminare la distribuzione precedente di un altro team. Per illustrare questo punto, consultate i seguenti esempi di file di filtro ben elaborati e problematici:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Se ogni team configura esplicitamente il proprio file di filtro nei siti su cui sta lavorando, ogni team può distribuire i propri componenti, librerie client e strutture del sito in modo indipendente, senza cancellare le modifiche l’uno dall’altro.

Poiché si tratta di un percorso di sistema globale e non specifico a un sito, il servlet seguente deve essere incluso nel progetto principale, in quanto le modifiche apportate qui potrebbero avere un impatto potenziale su qualsiasi team:

/apps/sling/servlet/errorhandler

### Sovrapposizioni {#overlays}

Le sovrapposizioni vengono spesso utilizzate per estendere o sostituire la funzionalità AEM, ma l&#39;utilizzo di una sovrapposizione interessa l&#39;intera applicazione AEM (ovvero, eventuali modifiche alla funzionalità saranno disponibili per tutti i tenant). Ciò sarebbe ulteriormente complicato se gli inquilini avessero requisiti diversi per la sovrapposizione. Idealmente, i gruppi aziendali dovrebbero collaborare per concordare la funzionalità e l&#39;aspetto delle console amministrative di AEM.

Se non si riuscirà a raggiungere un consenso tra le varie unità operative, una possibile soluzione sarebbe semplicemente quella di non utilizzare le sovrapposizioni. Al contrario, create una copia personalizzata della funzionalità ed esponetela tramite un percorso diverso per ciascun tenant. Questo consente a ciascun tenant di avere un&#39;esperienza utente completamente diversa, ma questo approccio aumenta anche i costi di implementazione e successivi interventi di aggiornamento.

### Moduli di avvio per flusso di lavoro {#workflow-launchers}

AEM utilizza gli avviatori di workflow per attivare automaticamente l&#39;esecuzione del flusso di lavoro quando vengono apportate modifiche specifiche nella directory archivio. AEM fornisce diversi avviatori, ad esempio per eseguire processi di generazione delle rappresentazioni e di estrazione dei metadati su risorse nuove e aggiornate. Anche se è possibile lasciare invariati questi lanciatori, in un ambiente multi-tenant, se gli inquilini hanno requisiti di avvio e/o di modello di flusso di lavoro diversi, è probabile che sia necessario creare e mantenere singoli avviatori per ciascun tenant. Questi avviatori dovranno essere configurati per essere eseguiti in base agli aggiornamenti del tenant, lasciando intatto il contenuto di altri tenant. Questo è semplice grazie all&#39;applicazione di strumenti di avvio a percorsi di repository specifici per il tenant.

### URL personalizzati {#vanity-urls}

AEM fornisce funzionalità URL personalizzate che è possibile impostare per ogni pagina. Il problema di questo approccio in uno scenario multi-tenant è che AEM garantire l&#39;univocità tra gli URL personalizzati configurati in questo modo. Se due utenti diversi configurano lo stesso percorso personalizzato per pagine diverse, è possibile che si verifichi un comportamento imprevisto. Per questo motivo, si consiglia di utilizzare le regole mod_rewrite nelle istanze del dispatcher Apache, che consentono un punto centrale di configurazione in concerto con le regole del Resource Resolver in uscita.

### Gruppi di componenti {#component-groups}

Nello sviluppo di componenti e modelli per più gruppi di authoring, è importante utilizzare in modo efficace le proprietà componentGroup e allowPaths. Sfruttando in modo efficace queste funzioni con le progettazioni del sito, possiamo fare in modo che gli autori del marchio A vedano solo i componenti e i modelli creati per il loro sito, mentre gli autori del marchio B ne vedono solo i propri.

### Test {#testing}

Sebbene una buona architettura e canali di comunicazione aperti possano aiutare a prevenire l&#39;introduzione di difetti in aree inaspettate del sito, questi approcci non sono a prova di inganno. Per questo motivo, è importante testare completamente ciò che viene distribuito sulla piattaforma prima di rilasciare qualsiasi cosa nella produzione. Ciò richiede il coordinamento tra i team sui loro cicli di rilascio e rafforza la necessità di una suite di test automatizzati che copra il maggior numero possibile di funzionalità. Inoltre, poiché un sistema sarà condiviso da più team, le prestazioni, la sicurezza e il test del carico diventano più importanti che mai.

## Considerazioni operative {#operational-considerations}

### Risorse condivise {#shared-resources}

AEM viene eseguito all&#39;interno di una singola JVM; tutte le applicazioni AEM distribuite in modo intrinseco condividono risorse, oltre alle risorse già utilizzate nella normale esecuzione di AEM. All&#39;interno dello spazio JVM stesso, non vi sarà alcuna separazione logica dei thread, e saranno condivise anche le risorse limitate disponibili per AEM, come memoria, CPU e i/o disco. Eventuali risorse che consumano locatari influiranno inevitabilmente sugli altri locatari del sistema.

### Spettacolo {#performance}

Se non si seguono AEM best practice, è possibile sviluppare applicazioni che consumano risorse oltre quanto considerato normale. Esempi di questo sono l&#39;attivazione di molte operazioni di flusso di lavoro pesanti (come DAM Update Asset), l&#39;utilizzo di operazioni di push-on-edit MSM su molti nodi o l&#39;utilizzo di costose query JCR per eseguire il rendering del contenuto in tempo reale. Questi avranno inevitabilmente un impatto sulle prestazioni di altre applicazioni tenant.

### Registrazione {#logging}

AEM fornisce interfacce pronte per una configurazione affidabile del logger che può essere utilizzata a nostro vantaggio in scenari di sviluppo condivisi. Specificando i logger separati per ogni marchio, per nome pacchetto, possiamo ottenere un certo grado di separazione del registro. Mentre le operazioni a livello di sistema come la replica e l&#39;autenticazione verranno ancora registrate in una posizione centrale, il codice personalizzato non condiviso può essere registrato separatamente, semplificando le attività di monitoraggio e debug per il team tecnico di ciascun marchio.

### Backup e ripristino {#backup-and-restore}

A causa della natura dell&#39;archivio JCR, i backup tradizionali funzionano in tutto il repository, anziché su un singolo percorso di contenuto. Non è quindi possibile separare facilmente i backup per tenant. Al contrario, il ripristino da un backup comporterà il rollback del contenuto e dei nodi del repository per tutti i tenant del sistema. Sebbene sia possibile eseguire backup di contenuto mirati, utilizzando strumenti come VLT, o per selezionare i contenuti da ripristinare creando un pacchetto in un ambiente separato, questi\
gli approcci non includono facilmente le impostazioni di configurazione o la logica dell&#39;applicazione e possono essere difficili da gestire.

## Considerazioni sulla sicurezza {#security-considerations}

### ACL {#acls}

È ovviamente possibile utilizzare gli elenchi di controllo degli accessi per controllare chi ha accesso per visualizzare, creare ed eliminare contenuti in base ai percorsi dei contenuti, il che richiede la creazione e la gestione di gruppi di utenti. La difficoltà di mantenere gli ACL e i gruppi dipende dall&#39;enfasi posta sull&#39;importanza di garantire che ciascun tenant non abbia alcuna conoscenza degli altri e che le applicazioni distribuite si basino su risorse condivise. Per garantire un&#39;amministrazione efficiente di ACL, utenti e gruppi, si consiglia di avere un gruppo centralizzato con la necessaria sorveglianza per assicurare che tali controlli di accesso e entità si sovrappongano (o non si sovrappongano) in modo da promuovere l&#39;efficienza e la sicurezza.
