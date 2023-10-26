---
title: Guida alla gerarchia dei siti, alla tassonomia e ai tag
seo-title: AEM Sites Site Hierarchy, Taxonomy, and Tagging Guide
description: Panoramica completa dei metadati, dei tag, della tassonomia e della gerarchia di AEM Sites. Utilizza questa guida per garantire la coerenza della tua strategia dei contenuti e seguire le best practice
seo-description: A full overview of AEM Sites metadata, tagging, taxonomy, and hierarchy. Use this guide to ensure your content strategy is consistent and following best practices
audience: author, marketer
topic: Content Management
feature: Learn From Your Peers
role: Admin, User
jira: KT-14254
level: Beginner, Intermediate
exl-id: c88c3ec7-9060-43e2-a6a2-d47bba6f7cf3
source-git-commit: 3752e22455020b58d23524f7e6a99414e773422d
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 0%

---

# Best practice per tag, tassonomia e metadati: riepilogo di alto livello

I metadati e i tag sono fondamentali per promuovere l’efficienza nell’AEM. Utenti, dirigenti e manager si rendono conto della necessità di una strategia olistica, ma trovano difficile fare progressi. Spesso la conoscenza è isolata tra gli utenti, rendendo difficile la strategia olistica e rendendo ancora più problematici gli adeguamenti.

Qual è la differenza tra metadati e tag? Quali sono gli aspetti aziendali da prendere in considerazione per definire la strategia?

## Qual è lo scopo dei metadati?

I metadati aggiungono struttura ai contenuti meno strutturati.
Esempio: un’immagine di base ha dei pixel. Possiamo chiamarli &quot;dati di base&quot;. Sono i metadati che descrivono il formato, la categoria, i dettagli della licenza e così via.
I metadati vengono utilizzati più di frequente per le risorse. Tuttavia, esiste un numero elevato di casi d’uso per i metadati anche nelle pagine di contenuto o nei frammenti di esperienza.

## Origini dei metadati

Di seguito sono riportate le categorie di cui è possibile generare i metadati:

* Metadati estratti: le informazioni sono già disponibili nel documento, ad esempio nel linguaggio naturale.
* Metadati derivati: le informazioni non sono disponibili nei dati originali, ma possono essere derivate da riferimenti incrociati a conoscenze precedenti.
* Metadati aggiunti manualmente: si tratta di metadati che non rientrano in nessuna delle prime categorie e che devono essere aggiunti manualmente da un utente.

## Tipi di metadati

All&#39;interno delle categorie sopra elencate vi sono quattro tipi principali:

* Metadati tecnici e descrittivi: fornisce informazioni sui dettagli tecnici del contenuto (ad esempio titolo, lingua ecc.)
* Metadati operativi: documenta il ciclo di vita di una risorsa (ovvero approvata, in creativa, per una campagna)
* Metadati amministrativi: lo stato o la condizione di una risorsa all’interno di un’organizzazione (ad esempio informazioni sulla licenza, proprietà)
* Metadati strutturali: consente di categorizzare le risorse o le pagine per agevolare il processo aziendale (si applica alla maggior parte dei tag e delle tassonomie)

## Nomi di cartelle e file

Le cartelle sono un modo naturale per navigare e sfogliare i contenuti in AEM. In che modo le parti interessate interagiranno con l’AEM? Questo determinerà come sono strutturate le cartelle. In genere, la struttura di cartelle è progettata tenendo presente uno o due dei seguenti elementi:

* Navigazione
* Esplorazione
* Categorizzazione
* Controllo degli accessi

Per AEM Sites, la navigazione è fondamentale. Le cartelle vengono utilizzate per controllare l’accesso alle risorse e alle pagine.

Quali livelli di autori avranno bisogno di accedere alle home page? E per quanto riguarda le pagine di prodotti? O campagna? Utilizza le autorizzazioni e la struttura di cartelle per inserire la governance corretta.

## Memorizzazione dei metadati

Esistono tre metodi per memorizzare i metadati:

* Binario: formato binario correlato alla natura della risorsa (Photoshop, InDesign, PNG, JPG).
* Nodo risorsa: si tratta di metadati sulla risorsa stessa, indipendentemente dal sistema o dal processo utilizzato.
* Posizione esterna: metadati che non si trovano direttamente nella risorsa, ma che possono essere utilizzati come descrittori dello &quot;stato&quot; di una risorsa (ad esempio, un flusso di lavoro che può influenzare una risorsa ma non vi è applicato direttamente)

## Modello metadati

La struttura del modo in cui i metadati vengono acquisiti e formattati è denominata modello di metadati o schema metadati. Questo deve essere concordato prima che le risorse o le pagine vengano acquisite nel sistema.

Un modello di metadati è in genere progettato per soddisfare i seguenti casi d’uso:

* Search &amp; Retrieve (Ricerca e recupero): consente di memorizzare gli aspetti chiave dei contenuti per facilitarne il recupero da parte dell’azienda.
* Riutilizzo: consente di utilizzare le vecchie risorse per il riutilizzo (risparmiando tempo e denaro)
* Gestione delle licenze: tiene traccia della proprietà delle risorse da parte dell’organizzazione (spesso per motivi legali)
* Distribuisci: rende i contenuti disponibili ai consumatori o crea sindacati di risorse per i partner aziendali.
* Archiviazione: i metadati che segnalano la presenza di una risorsa sono obsoleti (è sempre consigliabile aggiungere un contrassegno &quot;archiviato&quot; alla risorsa in modo da non perdere informazioni importanti)/
* Riferimenti incrociati: metadati associativi che acquisiscono la relazione tra due o più risorse (la sintesi dei metadati consente riferimenti incrociati e un’organizzazione coerente del gruppo)
* Naviga: la struttura di cartelle in cui sono memorizzate le risorse (utilizzata per recuperare le informazioni navigando)

*I metadati dell’autore supportano principalmente i processi operativi. La pubblicazione supporta casi di utilizzo di recupero e distribuzione.*

## Utilizzo dei tag come termini predefiniti

Un tag è una parola chiave o un termine assegnato a un&#39;informazione.vAd esempio, invece di immettere &quot;car&quot;, &quot;vehicle&quot;, &quot;automobile&quot;, un sistema di tag consente un solo valore tra cui scegliere, rendendo la ricerca più prevedibile.  I tag normalizzano e semplificano la categorizzazione delle risorse.

*Nota: anche se l’AEM consente l’assegnazione di tag ad hoc, è consigliabile non utilizzarlo, in quanto potrebbe portare a una tassonomia indefinita e complessa.*

Utilizzi comuni dei tag:

* Ricerca per parola chiave: un tag può descrivere l’appartenenza di una risorsa a un determinato gruppo di entità. Ad esempio, un tag &quot;image/subject/car&quot; descrive la risorsa appartiene al set di immagini che mostrano un’auto.
* Relazioni di guida: tutte le risorse che condividono lo stesso tag possono essere considerate connesse. L’assegnazione tag invece del collegamento diretto è particolarmente utile sui siti web che presentano molti contenuti dinamici e connessi.
* Navigazione dell’unità: i tag ordinati nella tassonomia gerarchica possono generare la navigazione, un collegamento o a documenti simili.
I tag dovrebbero essere considerati anche informazioni che collegano vari tipi di dati, in base ai termini aziendali anziché alle proprietà tecniche.

## Applicazioni comuni dei tag

Se utilizzati nell’AEM, i tag possono contribuire a ottenere un’implementazione molto più breve della funzione complessa, ad esempio:

* Ricerca con facet
* Navigazione personalizzata
* Contenuto correlato
* Riferimenti al contenuto
* Ottimizzazione motore di ricerca
* Evidenziazione dei concetti chiave

## Tassonomie

Una tassonomia è un sistema di organizzazione dei tag basato su caratteristiche condivise, che in genere sono strutturate gerarchicamente in base alle esigenze organizzative. La struttura può aiutare a trovare un tag più rapidamente o a imporre una generalizzazione.
Esempio: è necessario suddividere in sottocategorie le immagini d&#39;archivio delle automobili.  La tassonomia potrebbe essere simile alla seguente:

/subject/car/ /subject/car/sportscar /subject/car/sportscar/porsche /subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen ... /subject/car/limousine ...

Ora un utente può scegliere se vuole cercare le immagini di cicatrici sportive in generale o una &quot;Porsche&quot; in particolare. Dopotutto, entrambi sono cicatrici sportive.
Best practice: evita le tassonomie flat. Le tassonomie piatte non dispongono dei vantaggi descritti in precedenza e richiedono una manutenzione costante

**Utilizzo di una tassonomia come Thesaurus.**  Quando un utente cerca una parola chiave, il sistema crea una seconda ricerca per tutti i sinonimi trovati.
Inoltre, invece di digitare &quot;car&quot; manualmente, il sistema può fornire un elenco di parole chiave per migliorare la coerenza.

**Utilizzo di una tassonomia come dizionario.** Invece di stampare solo &quot;car&quot;, puoi espandere il singolo tag e utilizzare tutti i sinonimi del tag.

**Più categorie.** A differenza di una gerarchia di cartelle, i tag possono essere utilizzati per esprimere più categorizzazioni contemporaneamente. Una risorsa con tag:

/subject/car/minivan/mercedes /subject/people/family /color/red

## Metadati e tag

Non tutti i metadati devono essere considerati candidati per il sistema di assegnazione tag. I metadati tecnici possono duplicare inutilmente le informazioni. Il candidato migliore per i tag è rappresentato dai metadati aziendali. .I tag sono una buona scelta per applicare vocabolario coerente, ricerca sfaccettata e navigazione.

## Gestione tag

La gestione dei tag offre i vantaggi di un team dedicato di base. I nuovi membri devono apprendere lo scopo e la funzione della tassonomia prima di aggiungere nuovi tag.  Esperti esperti, che fungono da guardiani dei nuovi tag, ridurranno le incoerenze a lungo termine.

## Creazione di tag

Le tassonomie devono essere utilizzate da autori di contenuti e comprese dagli utenti finali. Devono essere create prima del processo di creazione dei contenuti. Eventuali scelte rapide comporteranno un ulteriore sforzo per la gestione e la manutenzione.

## Manutenzione continua

Le cose cambiano e cambiano anche le esigenze dell’elenco di tag. È disponibile un processo di manutenzione audio che riduce la duplicazione.

Assicurati che i collaboratori di contenuto sappiano come possono proporre modifiche e che gli editor o i responsabili di contenuto rivedano regolarmente i termini.

## Best practice con tag e tassonomie

**Standardizzare I Tag.** Crea un glossario che fornisca un vocabolario autorevole. Senza la definizione di standard, la duplicazione dei dati presenterà dei problemi. Inoltre, si consiglia di controllare non solo la tassonomia, ma anche l’utilizzo dei tag.

**Non sovraetichettare.** I tag possono perdere il loro significato se distribuiti troppo frequentemente.Applica tag estranei per un’efficienza ottimale.

**Rivaluta I Tag Nel Tempo.** Ricorda che la terminologia e il contesto di business raramente rimangono statici. Potrebbe essere necessario standardizzare nuovamente e riapplicare i tag.

**Utilizzo di tag avanzati basati sull’intelligenza artificiale.** Applicazione di tag avanzati [vedi collegamento] è una funzionalità di intelligenza artificiale in AEM per ridurre lo sforzo di assegnare tag alle risorse manualmente. L’assegnazione tag avanzati utilizza un’intelligenza artificiale per dedurre informazioni sull’oggetto di un’immagine. Genera tag descrittivi che descrivono il contenuto di un’immagine.

## Qualità e manutenzione dei metadati

Comprendere i requisiti aziendali è un passaggio importante nell’esecuzione di un modello di gestione dei metadati. Senza definizione, le informazioni non possono essere memorizzate. Sarà necessario rivedere periodicamente il modello. Si tratta di un’attività essenziale per il controllo della qualità.

Inoltre, i metadati devono essere acquisiti il prima possibile durante il processo di creazione dei contenuti. Se i metadati non vengono applicati al momento giusto, ci sono poche possibilità di applicarli retroattivamente.

**Utilizzare i metadati** per migliorare la collaborazione: utilizza Adobe Asset Link, Adobe Bridge e il desktop AEM per collegare i processi creativi e utilizzare i metadati per semplificare i flussi di lavoro creativi. L’utilizzo di questi strumenti arricchisce i metadati e l’esperienza utente nel processo creativo.

## Best practice per la gestione dei metadati

* Assegna team di base con mandato esecutivo sicuro: forma un team di base di metadati con una conoscenza completa dell’ecosistema aziendale e un mandato forte da parte della gestione dell’organizzazione.
* Definire la strategia e la governance dei metadati: una buona strategia per i metadati può aiutare le organizzazioni a spiegare la necessità e i vantaggi di tali metadati.  Una strategia è costituita da, schemi di metadati, tassonomia, processi aziendali (per la qualità e l’acquisizione dei dati), ruoli e responsabilità e processi di governance. *
* Definire e comunicare un modello di metadati coerente: la strategia e il ragionamento definiti devono essere ben documentati e comunicati all’interno delle organizzazioni.
* Convenzione di denominazione standard: crea una convenzione di denominazione dei file coerente e descrittiva per migliorare il branding, la gestione delle informazioni e l’usabilità.
* Caratteri sicuri nei nomi dei file: il nome del file deve poter essere interpretato da tutti i sistemi operativi comuni. È possibile utilizzare caratteri, numeri, umlaut, spazi e sottolineature in tutta sicurezza. Anche il simbolo meno è sicuro, ma se si taglia e incolla, potrebbe sembrare un trattino.
* Convenzione di denominazione delle versioni: l’AEM offre alcune funzionalità per mantenere le versioni precedenti delle risorse. In alcuni casi, potrebbe essere utile mantenere diverse versioni. Tuttavia, è necessario assicurarsi che lo schema di versioni sia coerente.

## Metadati organizzativi e descrittivi

Alcune linee guida possono essere utili per decidere come categorizzare i metadati:

**Descrizione** - Se i dati descrivono la risorsa o il contenuto, devono far parte dei metadati allegati.

**Ricerca** - Se i metadati sono utilizzati a fini di ricerca, devono essere allegati.

**Esposizione** - Se stai esponendo i metadati su una piattaforma di distribuzione a terzi, fai attenzione a non esporre anche i metadati &quot;interni&quot;.

**Durata** - Più a lungo i metadati sono conservati, più è probabile che siano adatti per i metadati allegati.

**Processi aziendali correlati** - Come parte dei metadata, è sicuramente utile disporre di un ID prodotto permanente. Tuttavia, la categoria di un articolo in relazione al catalogo dei prodotti è un metadati discutibile per la risorsa.

**Organizzazione ed elaborazione** - Se la natura dei metadati è di tipo organizzativo, ad esempio se si trovano in un flusso di lavoro di approvazione o se sono di proprietà di un determinato reparto, i metadati esterni devono essere presi in considerazione piuttosto che allegarli alla risorsa.

*Per creare la strategia, effettuare le seguenti domande:*

* Che tipo di contenuti e &quot;informazioni aggiuntive&quot; (= metadati) sono necessari per risolvere problemi aziendali/domande aziendali/problemi aziendali?
* Quali sono le variabili, i &quot;campi&quot; nello schema e quali sono i valori possibili? Quali variabili necessitano di un input di testo libero, quali possono essere ristrette per tipo (numero, data, booleano, ...), un set di valori fissi (ad esempio paesi) o tag da una determinata tassonomia. Quanti tag sono necessari, consentiti?
* Quali problemi tecnici/domande possono essere risolti dai metadati?
* Come puoi ottenere/creare tali contenuti/metadati? Quanto costerà ottenere/creare tali metadati?
* Quali tipi di metadati sono necessari per un particolare gruppo di utenti?
* Come verranno mantenuti e aggiornati i metadati?
* Chi è responsabile di quale parte del processo?
* Come si può garantire il rispetto dei processi aziendali concordati?
* Quali standard dovresti seguire? Dovresti adottare e modificare uno standard di settore (Dublin Core, ISO 19115, PRISM, ecc.) oppure l&#39;organizzazione deve creare i propri standard?
* Dove è documentata la strategia? Come puoi assicurarti che tutte le parti interessate abbiano accesso a? Come puoi essere sicuro che il nuovo personale onboarded aderisca allo standard concordato (ad esempio, visita la formazione prima di accedere?)
