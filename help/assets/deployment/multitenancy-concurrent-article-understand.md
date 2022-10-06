---
title: Multitenancy e sviluppo simultaneo
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

# Multitenancy e sviluppo simultaneo {#understanding-multitenancy-and-concurrent-development}

## Introduzione {#introduction}

Quando più team distribuiscono il codice negli stessi ambienti di AEM, è necessario seguire alcune procedure per garantire che i team possano lavorare nel modo più indipendente possibile, senza utilizzare le dita dei piedi degli altri team. Anche se non possono mai essere eliminati completamente, queste tecniche ridurranno le dipendenze tra i team. Affinché un modello di sviluppo concorrente abbia successo, è fondamentale una buona comunicazione tra i team di sviluppo.

Inoltre, quando più team di sviluppo lavorano sullo stesso ambiente AEM, è probabile che vi sia un certo grado di multi-tenancy in gioco. Molto è stato scritto sulle considerazioni pratiche del tentativo di sostenere più locatari in un ambiente AEM, soprattutto sulle sfide che si affrontano quando si gestisce la governance, le operazioni e lo sviluppo. Questo documento esplora alcune delle problematiche tecniche relative all’implementazione di AEM in un ambiente multi-tenant, ma molte di queste raccomandazioni verranno applicate a qualsiasi organizzazione con più team di sviluppo.

È importante notare in anticipo che, mentre AEM supportare più siti e anche più marchi distribuiti su un unico ambiente, non offre una vera e propria multi-tenancy. Alcune configurazioni di ambiente e risorse di sistema saranno sempre condivise tra tutti i siti distribuiti in un ambiente. Il presente documento fornisce orientamenti per ridurre al minimo l&#39;impatto di tali risorse condivise e offre suggerimenti per razionalizzare la comunicazione e la collaborazione in questi settori.

## Vantaggi e sfide {#benefits-and-challenges}

L’implementazione di un ambiente multi-tenant comporta molte sfide.

Comprendono:

* Complessità tecnica aggiuntiva
* Maggiore sovraccarico per lo sviluppo
* Dipendenze tra organizzazioni sulla risorsa condivisa
* Maggiore complessità operativa

Nonostante le sfide, l’esecuzione di un’applicazione multi-tenant presenta vantaggi quali:

* Riduzione dei costi hardware
* Riduzione dei tempi di commercializzazione per i siti futuri
* Riduzione dei costi di implementazione per i futuri locatari
* Architettura standard e pratiche di sviluppo in tutta l&#39;azienda
* Una base di codice comune

Se l&#39;azienda richiede una vera e propria multi-tenancy, senza alcuna conoscenza degli altri tenant e senza codice condiviso, contenuti o autori comuni, le istanze di authoring separate sono l&#39;unica opzione possibile. L&#39;aumento complessivo dello sforzo di sviluppo va confrontato con il risparmio dei costi di infrastruttura e licenze per determinare se questo approccio è il migliore.

## Tecniche di sviluppo {#development-techniques}

### Gestione delle dipendenze {#managing-dependencies}

Quando gestisci le dipendenze dei progetti Maven, è importante che tutti i team utilizzino la stessa versione di un determinato bundle OSGi sul server. Per illustrare cosa può andare storto quando i progetti Maven sono mal gestiti, vi presentiamo un esempio:

Il progetto A dipende dalla versione 1.0 della libreria, foo; la versione 1.0 di foo è incorporata nella relativa distribuzione al server. Il progetto B dipende dalla versione 1.1 della libreria, foo; la versione 1.1 di foo è incorporata nella relativa implementazione.

Inoltre, supponiamo che in questa libreria sia stata modificata un’API tra le versioni 1.0 e 1.1. A questo punto, uno di questi due progetti non funzionerà più correttamente.

Per risolvere questo problema, si consiglia di fare di tutti i progetti Maven figli di un progetto di reattore principale. Questo progetto di reattore ha due finalità: consente la costruzione e la realizzazione di tutti i progetti insieme, se lo desiderano, e contiene le dichiarazioni di dipendenza per tutti i progetti figlio. Il progetto padre definisce le dipendenze e le loro versioni, mentre i progetti figlio dichiarano solo le dipendenze necessarie, ereditando la versione dal progetto padre.

In questo scenario, se il team che lavora sul Progetto B richiede funzionalità nella versione 1.1 di foo, diventerà rapidamente evidente nell’ambiente di sviluppo che questa modifica interromperà il Progetto A. A questo punto i team possono discutere questa modifica e rendere il Progetto A compatibile con la nuova versione oppure cercare una soluzione alternativa per il Progetto B.

Tieni presente che questo non elimina la necessità che questi team condividano questa dipendenza: evidenzia i problemi in modo rapido e tempestivo, in modo che i team possano discutere qualsiasi rischio e concordare una soluzione.

### Impedire la duplicazione del codice {#preventing-code-duplication-nbsp-br}

Quando lavori su più progetti, è importante garantire che il codice non sia duplicato. La duplicazione del codice aumenta la probabilità di eventi difettosi, il costo delle modifiche al sistema e la rigidità complessiva nella base di codice. Per evitare duplicazioni, reimposta la logica comune nelle librerie riutilizzabili che possono essere utilizzate in più progetti.

Per soddisfare questa esigenza, consigliamo lo sviluppo e la manutenzione di un progetto di base a cui tutti i team possono dipendere e a cui possono contribuire. In tale contesto, è importante garantire che questo progetto fondamentale non dipenda, a sua volta, da nessuno dei progetti dei singoli team; questo consente una distribuzione indipendente e continua a promuovere il riutilizzo del codice.

Alcuni esempi di codice comunemente presenti in un modulo core includono:

* Configurazioni a livello di sistema quali:
   * Configurazioni OSGi
   * Filtri servlet
   * Mappature ResourceResolver
   * Trasformatori Sling
   * Gestori di errori (o utilizzare il gestore di pagina di errore ACS AEM Commons1)
   * Servlet di autorizzazione per la memorizzazione in cache sensibile alle autorizzazioni
* Classi di utilità
* Logica aziendale di base
* Logica dell’integrazione di terze parti
* Sovrapposizioni dell’interfaccia utente di authoring
* Altre personalizzazioni necessarie per l’authoring, ad esempio widget personalizzati
* Lancio dei flussi di lavoro
* Elementi di progettazione comuni utilizzati tra siti diversi

*Architettura dei progetti modulari*

Questo non elimina la necessità che più team dipendano da e possano aggiornare lo stesso set di codice. Creando un progetto di base, abbiamo ridotto le dimensioni della base di codice condivisa tra i team, diminuendo ma senza eliminare la necessità di risorse condivise.

Per garantire che le modifiche apportate a questo pacchetto principale non interrompano le funzionalità del sistema, si consiglia a uno sviluppatore senior o a un team di sviluppatori di mantenere il controllo. Un&#39;opzione consiste nell&#39;avere un unico team che gestisce tutte le modifiche al pacchetto; un altro consiste nell’richiedere ai team di inviare richieste di pull che siano esaminate e unite da queste risorse. È importante che un modello di governance sia progettato e concordato dai team e che gli sviluppatori lo seguano.

## Gestione dell&#39;ambito di distribuzione  {#managing-deployment-scope}

Poiché i diversi team implementano il codice nello stesso archivio, è importante che non sovrascrivano le modifiche reciproche. AEM dispone di un meccanismo per controllare questo quando si distribuiscono i pacchetti di contenuto, il filtro. file xml. È importante che non vi sia sovrapposizione tra i filtri.  file xml, altrimenti la distribuzione di un team potrebbe potenzialmente cancellare la distribuzione precedente di un altro team. Per illustrare questo punto, vedi i seguenti esempi di file di filtro ben elaborati e problematici:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Se ogni team configura in modo esplicito il file di filtro fino al sito o ai siti su cui sta lavorando, ogni team può distribuire i propri componenti, librerie client e progettazioni del sito in modo indipendente, senza cancellare le rispettive modifiche.

Poiché si tratta di un percorso di sistema globale e non è specifico per un sito, il seguente servlet deve essere incluso nel progetto principale, in quanto le modifiche apportate in questo caso potrebbero avere un impatto su qualsiasi team:

/apps/sling/servlet/errorhandler

### Sovrapposizioni {#overlays}

Le sovrapposizioni vengono spesso utilizzate per estendere o sostituire la funzionalità predefinita AEM, ma l’utilizzo di una sovrapposizione influisce sull’intera applicazione AEM (ovvero, eventuali modifiche alla funzionalità sovrapposte vengono rese disponibili per tutti gli tenant). Ciò sarebbe ulteriormente complicato se gli inquilini avessero requisiti diversi per la sovrapposizione. Idealmente, i business group dovrebbero collaborare per concordare la funzionalità e l’aspetto delle console amministrative AEM.

Se non si riuscirà a raggiungere un consenso tra le varie unità operative, una soluzione possibile sarebbe semplicemente quella di non utilizzare le sovrapposizioni. Al contrario, crea una copia personalizzata della funzionalità ed esporla tramite un percorso diverso per ciascun tenant. Questo consente a ogni tenant di avere un’esperienza utente completamente diversa, ma questo approccio aumenta anche i costi di implementazione e di successive attività di aggiornamento.

### Moduli di avvio per flusso di lavoro {#workflow-launchers}

AEM utilizza i moduli di avvio del flusso di lavoro per attivare automaticamente l’esecuzione del flusso di lavoro quando vengono apportate modifiche specifiche nell’archivio. AEM fornisce diversi moduli di avvio pronti all’uso, ad esempio, per eseguire processi di generazione del rendering e estrazione dei metadati su risorse nuove e aggiornate. Anche se è possibile lasciare questi lanciatori così come sono, in un ambiente multi-tenant, se gli inquilini hanno diversi requisiti di lanciatore e/o modello di flusso di lavoro, è probabile che i singoli lanciatori dovranno essere creati e mantenuti per ogni tenant. Questi moduli di avvio dovranno essere configurati per eseguire gli aggiornamenti del tenant lasciando intatto il contenuto di altri tenant. Per farlo, puoi applicare i moduli di avvio a percorsi di archivio specifici per i tenant.

### Gli URL personalizzati {#vanity-urls}

AEM fornisce funzionalità URL personalizzate che possono essere impostate su base pagina. La preoccupazione di questo approccio in uno scenario multi-tenant è che AEM non garantisca l’univocità tra gli URL personalizzati configurati in questo modo. Se due utenti diversi configurano lo stesso percorso personalizzato per pagine diverse, si può osservare un comportamento imprevisto. Per questo motivo, si consiglia di utilizzare le regole mod_rewrite nelle istanze del dispatcher Apache, che consentono un punto centrale di configurazione insieme alle regole del Resource Resolver in uscita.

### Gruppi di componenti {#component-groups}

Quando si sviluppano componenti e modelli per più gruppi di authoring, è importante utilizzare in modo efficace le proprietà componentGroup e allowedPaths . Sfruttando in modo efficace queste funzioni con le progettazioni del sito, possiamo garantire che gli autori del marchio A vedano solo i componenti e i modelli creati per il sito, mentre gli autori del marchio B ne visualizzano solo i propri.

### Test {#testing}

Sebbene una buona architettura e canali di comunicazione aperti possano contribuire a prevenire l&#39;introduzione di difetti in aree inaspettate del sito, questi approcci non sono a prova di inganno. Per questo motivo, è importante testare completamente ciò che viene implementato sulla piattaforma prima di rilasciare qualsiasi cosa in produzione. Ciò richiede il coordinamento tra i team sui loro cicli di rilascio e rafforza la necessità di una suite di test automatizzati che coprano il maggior numero possibile di funzionalità. Inoltre, poiché un sistema è condiviso da più team, le prestazioni, la sicurezza e i test di carico diventano più importanti che mai.

## Considerazioni operative {#operational-considerations}

### Risorse condivise {#shared-resources}

AEM viene eseguito all&#39;interno di una singola JVM; tutte le applicazioni AEM distribuite in modo intrinseco condividono risorse tra loro, oltre alle risorse già utilizzate nella normale esecuzione di AEM. Nello spazio JVM stesso non esiste una separazione logica dei thread e vengono condivise anche le risorse finite disponibili per AEM, come la memoria, la CPU e l&#39;I/O del disco. Tutte le risorse che consumano tenant influiranno inevitabilmente sugli altri tenant del sistema.

### Spettacolo {#performance}

Se non si seguono AEM best practice, è possibile sviluppare applicazioni che consumano risorse al di là di quanto viene considerato normale. Esempi di questo sono l’attivazione di molte operazioni pesanti del flusso di lavoro (come DAM Update Asset), l’utilizzo di operazioni di push-on-edit MSM su molti nodi o l’utilizzo di query JCR costose per eseguire il rendering del contenuto in tempo reale. Questi avranno inevitabilmente un impatto sulle prestazioni di altre applicazioni tenant.

### Registrazione {#logging}

AEM fornisce interfacce predefinite per una configurazione affidabile del logger che può essere utilizzata a nostro vantaggio in scenari di sviluppo condivisi. Specificando logger separati per ogni marchio, per nome pacchetto, possiamo ottenere un certo grado di separazione dei log. Mentre le operazioni a livello di sistema come la replica e l&#39;autenticazione verranno ancora registrate in una posizione centrale, il codice personalizzato non condiviso può essere registrato separatamente, facilitando le attività di monitoraggio e debug per il team tecnico di ogni marchio.

### Backup e ripristino {#backup-and-restore}

A causa della natura dell&#39;archivio JCR, i backup tradizionali funzionano nell&#39;intero archivio, anziché su un singolo percorso di contenuto. Non è quindi possibile separare facilmente i backup per tenant. Al contrario, il ripristino da un backup comporta il rollback dei contenuti e dei nodi dell&#39;archivio per tutti gli tenant del sistema. Mentre è possibile eseguire backup di contenuti mirati, utilizzando strumenti come VLT, o per selezionare il contenuto da ripristinare creando un pacchetto in un ambiente separato, questi\
gli approcci non includono facilmente le impostazioni di configurazione o la logica dell’applicazione e possono essere ingombranti da gestire.

## Considerazioni sulla sicurezza {#security-considerations}

### ACL {#acls}

È ovviamente possibile utilizzare gli elenchi di controllo accessi (ACL, Access Control List) per controllare chi ha accesso a visualizzare, creare ed eliminare contenuti in base ai percorsi dei contenuti, il che richiede la creazione e la gestione di gruppi di utenti. La difficoltà di mantenere gli ACL e i gruppi dipende dall&#39;enfasi posta sull&#39;importanza di garantire che ogni tenant abbia zero conoscenze degli altri e se le applicazioni distribuite si basano su risorse condivise. Per garantire un&#39;amministrazione efficiente di ACL, utenti e gruppi, si consiglia di disporre di un gruppo centralizzato con la necessaria sorveglianza per garantire che questi controlli di accesso e entità di protezione si sovrappongano (o non si sovrappongano) in un modo che promuova l&#39;efficienza e la sicurezza.
