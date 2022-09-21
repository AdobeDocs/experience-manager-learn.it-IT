---
title: Gerarchia del sito, tassonomia e suggerimenti per l’assegnazione tag
description: Gerarchia del sito, tassonomia e suggerimenti per l’assegnazione tag Best practice
hide: true
hidefromtoc: true
source-git-commit: 3eb429039589ae26a81bc6d24f020a77517133e8
workflow-type: tm+mt
source-wordcount: '2053'
ht-degree: 0%

---


# Tecniche consigliate per tag, tassonomia e metadati: Riepilogo di alto livello

I metadati e i tag sono fondamentali per aumentare l’efficienza in AEM. Utenti, leader e dirigenti si rendono conto della necessità di una strategia olistica, ma trovano difficile fare progressi. Spesso la conoscenza viene messa a tacere tra gli utenti, rendendo la strategia olistica difficile e rende gli aggiustamenti ancora più problematici.

Qual è la differenza tra metadati e tag? Quali sono gli aspetti aziendali da considerare quando si guida la strategia?

È possibile trovare un riepilogo più approfondito [qui](https://adobe.sharepoint.com/:w:/r/sites/ACSSuccessServices/_layouts/15/Doc.aspx?sourcedoc=%7BFE5E873A-A3B6-4F40-BF22-A2C9F1269802%7D&amp;file=AEM_TagTaxonomyAndMetadata_BestPractice_en%20(2).docx&amp;action=default&amp;mobileredirect=true).

## Qual è lo scopo dei metadati?

I metadati aggiungono una struttura ai contenuti meno strutturati.
Esempio: Un&#39;immagine di base ha dei pixel. Possiamo chiamare questi &quot;dati di base&quot;. Sono i metadati che descrivono il formato, la categoria, i dettagli delle licenze, ecc.
I metadati vengono utilizzati più di frequente per le risorse. Tuttavia, esistono molti casi d’uso per i metadati nelle pagine di contenuto o nei frammenti di esperienza.

## Origini dei metadati

Di seguito sono riportate le categorie di metadati che possono essere generati:

* Metadati estratti : le informazioni sono già disponibili nel documento, ad esempio nel linguaggio naturale.
* Metadati derivati : le informazioni non sono disponibili nei dati originali ma possono essere derivate facendo riferimento a conoscenze precedenti.
* Metadati aggiunti manualmente : si tratta di metadati che non rientrano in nessuna delle prime categorie e che devono essere aggiunti manualmente da un utente.

## Tipi di metadati

All&#39;interno delle categorie sopra elencate ci sono quattro tipi principali:

* Metadati tecnici e descrittivi: Fornisce informazioni sui dettagli tecnici del contenuto (ad es. testo, lingua, ecc.)
* Metadati operativi: Documenti il ciclo di vita di una risorsa (ad esempio, approvata, in creativa, campagna)
* Metadati amministrativi: Lo stato o lo stato di un’attività all’interno di un’organizzazione (informazioni sulla licenza, proprietà)
* Metadati strutturali: Consente di classificare risorse o pagine per semplificare i processi aziendali (si applica alla maggior parte dei tag e delle tassonomie)

## Nomi di cartelle e file

Le cartelle sono un modo naturale per navigare e navigare nel contenuto di AEM. In che modo interagiranno le parti interessate con AEM? Questo determinerà la struttura delle cartelle. Normalmente struttura cartelle architettata con uno o due dei seguenti elementi:

* Navigazione
* Navigazione
* Categorizzazione
* Controllo dell&#39;accesso

Per AEM Sites, la navigazione è chiave. Le cartelle vengono utilizzate per controllare l’accesso alle risorse e alle pagine.

Quali livelli di autori avranno bisogno dell&#39;accesso alle home page? E le pagine dei prodotti? O la campagna? Utilizza le autorizzazioni e la struttura delle cartelle per implementare la governance corretta.

## Memorizzazione dei metadati

Esistono tre modi per memorizzare i metadati:

* Binario: Il formato binario correlato alla natura della risorsa (Photoshop, InDesign, PNG, JPG).
* Nodo risorsa: Si tratta dei metadati della risorsa stessa, indipendentemente dal sistema o dal processo utilizzato.
* Posizione esterna: Metadati che non si trovano direttamente nella risorsa, ma possono essere utilizzati come descrittore dello &quot;stato&quot; di una risorsa (ad esempio: un flusso di lavoro che può influenzare una risorsa ma non direttamente applicata ad essa)

## Modello metadati

La struttura del modo in cui i metadati vengono acquisiti e formattati è denominata modello di metadati o schema di metadati. Questo deve essere concordato prima che le risorse o le pagine vengano acquisite nel sistema.

Un modello di metadati è in genere progettato per soddisfare i seguenti casi d’uso:

* Ricerca e recupero: Consente di memorizzare aspetti chiave dei contenuti che facilitano il recupero da parte dell&#39;azienda.
* Riutilizza: Aiuta a utilizzare i vecchi asset per il riutilizzo (risparmio di tempo e denaro)
* Gestione licenze: Tiene traccia della proprietà dell’attività da parte dell’organizzazione (spesso per motivi legali)
* Distribuisci: Rendere i contenuti disponibili ai consumatori o divulgare risorse ai partner commerciali.
* Archivia: I metadati che segnalano una risorsa sono obsoleti (best practice per inserire un flag &quot;archiviato&quot; sulla risorsa in modo da non perdere le informazioni vitali)/
* Riferimenti incrociati: Metadati associati che acquisiscono la relazione tra due o più risorse (la sintesi dei metadati consente riferimenti incrociati e un&#39;organizzazione di gruppi coerente)
* Naviga: La struttura delle cartelle in cui vengono memorizzate le risorse (utilizzata per recuperare informazioni tramite navigazione)

*I metadati dell&#39;autore supportano principalmente i processi operativi. La pubblicazione supporta casi d’uso di recupero e distribuzione.*

## Utilizzo dei tag come termini predefiniti

Un tag è una parola chiave o un termine assegnato a un elemento di informazione.vAd esempio, invece di inserire &quot;auto&quot;, &quot;veicolo&quot;, &quot;automobile&quot;, un sistema di tag consente solo un valore tra cui scegliere, rendendo la ricerca più prevedibile.  I tag normalizzano e semplificano la categorizzazione delle risorse.

*Nota: Anche se AEM consente l’assegnazione di tag ad hoc, è buona prassi tenerne conto in quanto potrebbe portare a una tassonomia indefinita e ingombrante.*

Uso comune dei tag:

* Ricerca di parole chiave: Un tag può descrivere che una risorsa appartiene a un determinato gruppo di entità. Ad esempio, un tag &quot;image/subject/car&quot; descrive la risorsa appartiene al set di immagini che mostrano un&#39;auto.
* Relazioni di guida: Tutte le risorse che condividono lo stesso tag possono essere considerate collegate. L’assegnazione tag invece del collegamento diretto è particolarmente utile sui siti web con contenuti molto dinamici e collegati.
* Navigazione dell&#39;unità: I tag ordinati nella tassonomia gerarchica possono generare la navigazione, un collegamento o a documenti simili.
I tag devono anche essere cercati in informazioni che collegano vari tipi di dati in base a termini aziendali anziché a proprietà tecniche.

## Applicazioni comuni dei tag

Quando vengono utilizzati in AEM, i tag possono contribuire a ottenere un’implementazione molto più breve della funzione complessa, ad esempio:

* Ricerca su facet
* Navigazione personalizzata
* Contenuto correlato
* Riferimenti contenuto
* Ottimizzazione dei motori di ricerca
* Evidenziazione dei concetti chiave

## Tassonomie

La tassonomia è un sistema di organizzazione dei tag basato su caratteristiche condivise, solitamente strutturate gerarchicamente in base alle esigenze organizzative. La struttura può aiutare a trovare un tag più velocemente o a imporre una generalizzazione.
Esempio: C&#39;è la necessità di sottoclassificare le immagini stock di auto.  La tassonomia potrebbe avere un aspetto simile al seguente:

/subject/car/ /subject/car/sportscar/sportscar/subject/car/sportscar/porsche/subject/car/sportscar/ferrari ... /subject/car/minivan/subject/car/minivan/mercedes/subject/car/minivan/volkswagen .. /subject/car/limousine ...

Ora un utente può scegliere se vuole cercare immagini di cicatrici sportive in generale o una &quot;Porsche&quot; in particolare. Dopo tutto, entrambi sono cicatrici sportive.
Best practice: Evitare tassonomie piatte. Le tassonomie piane non presentano i vantaggi sopra descritti e richiedono una manutenzione costante

**Utilizzo di una tassonomia come Thesaurus.**  Quando un utente cerca una parola chiave, il sistema crea una seconda ricerca per tutti i sinonimi trovati lì.
Inoltre, invece di digitare manualmente &quot;car&quot;, il sistema può fornire un elenco di parole chiave per migliorare la coerenza.

**Utilizzo di una tassonomia come dizionario.** Invece di stampare semplicemente &quot;auto&quot;, puoi espandere il singolo tag e utilizzare tutti i sinonimi del tag.

**Categorie multiple.** A differenza della gerarchia di cartelle, i tag possono essere utilizzati per esprimere più categorie contemporaneamente. Una risorsa con tag:

/subject/car/minivan/mercedes/subject/people/family/color/red

## Metadati e tag

Non tutti i metadati devono essere considerati come candidati per il sistema di assegnazione tag. I metadati tecnici possono duplicare inutilmente le informazioni. Il miglior candidato per i tag sono i metadati aziendali. .Tags è una buona scelta per applicare vocabolario coerente, ricerca su facet e navigazione.

## Gestione tag

La gestione dei tag offre vantaggi a un team di base dedicato. Prima di aggiungere nuovi tag, i nuovi membri devono imparare lo scopo e la funzione della tassonomia.  Gli esperti stagionati, che fungono da portiere per i nuovi tag, ridurranno le incongruenze a lungo termine.

## Creazione di tag

Le tassonomie devono essere utilizzate dagli autori dei contenuti e comprese dagli utenti finali. Devono essere create prima del processo di creazione dei contenuti. Tutte le scelte rapide si tradurranno in un ulteriore sforzo di gestione e manutenzione.

## Manutenzione continua

Le cose cambiano e anche le esigenze dell’elenco dei tag . È disponibile un processo di manutenzione del suono che ridurrà la duplicazione.

Assicurati che i collaboratori del contenuto sappiano come possono proporre le modifiche e che editor o responsabili del contenuto stiano rivedendo regolarmente i termini.

## Tecniche consigliate per tag e tassonomie

**Assegnazione di tag standard.** Crea glossario che fornisce un vocabolario autorevole. Senza stabilire norme, la duplicazione presenterà problemi. Inoltre, si consiglia di controllare non solo la tassonomia ma anche l’utilizzo dei tag .

**Non sovrascrivere.** I tag possono perdere la loro importanza se distribuiti con troppa frequenza.Pprune tag estranei per un&#39;efficienza ottimale.

**Rivalutare i tag nel tempo.** Ricorda che la terminologia aziendale e il contesto aziendale raramente rimangono statici. Potrebbe essere necessario standardizzare e riapplicare i tag.

**Utilizzo di tag avanzati basati sull’intelligenza artificiale** Assegnazione tag avanzati [link] è una funzionalità di intelligenza artificiale in AEM per ridurre lo sforzo di assegnare manualmente i tag alle risorse. L’assegnazione tag avanzati utilizza un’intelligenza artificiale per dedurre informazioni sull’oggetto di un’immagine. Genera tag descrittivi che descrivono il contenuto di un’immagine.

## Qualità e manutenzione dei metadati

La comprensione dei requisiti aziendali è un passaggio importante nell&#39;esecuzione di un modello di gestione dei metadati. Senza definizione, le informazioni non possono essere memorizzate. Sarà necessario rivedere periodicamente il modello. Si tratta di un&#39;attività di controllo di qualità fondamentale.

Inoltre, i metadati devono essere acquisiti il prima possibile nel processo di creazione dei contenuti. Se i metadati non vengono applicati al momento giusto, le possibilità di applicarli retroattivamente sono scarse.

**Utilizzo dei metadati** per migliorare la collaborazione: Utilizza Adobe Asset Link, Adobe Bridge e AEM Desktop per collegare i processi creativi e utilizzare i metadati per semplificare i flussi di lavoro creativi. L&#39;utilizzo di questi strumenti arricchirà i metadati e l&#39;esperienza utente nel processo creativo.

## Best practice per la gestione dei metadati

* Assegna il team core con un mandato esecutivo forte: Forma un team di base di metadati che ha una conoscenza completa dell&#39;ecosistema aziendale e un forte mandato da parte della gestione dell&#39;organizzazione.
* Definizione della strategia e della governance dei metadati: Una buona strategia per i metadati può aiutare le organizzazioni a spiegare la necessità e i vantaggi dei metadati.  Una strategia consiste in, schemi di metadati, tassonomia, processi aziendali (per la qualità e l’acquisizione dei dati), ruoli e responsabilità e processi di governance. *
* Definire e comunicare un modello di metadati coerente: La strategia e il ragionamento definiti dovrebbero essere ben documentati e comunicati all&#39;interno delle organizzazioni.
* Convenzione di denominazione standard: Crea una convenzione di denominazione dei file coerente e descrittiva per branding, gestione delle informazioni e usabilità migliorati.
* Caratteri sicuri nei nomi dei file: Il nome del file deve poter essere interpretato da tutti i sistemi operativi comuni. È possibile utilizzare caratteri, numeri, umlaut, spazio e sottolineatura in tutta sicurezza. Anche il simbolo meno è sicuro, ma se tagli e incolla, potrebbe assomigliare a un &quot;trattino&quot;.
* Convenzione per la denominazione delle versioni: AEM offre alcune funzionalità per mantenere le versioni precedenti delle risorse. In alcuni casi, potrebbe essere utile mantenere diverse versioni. Tuttavia, assicurati che lo schema di gestione delle versioni sia coerente.

## Metadati organizzativi e descrittivi

Alcune linee guida possono aiutarti a decidere come categorizzare i metadati:

**Descrizione** - Se i dati descrivono la risorsa o il contenuto, devono far parte dei metadati allegati.

**Ricerca** - Se i metadati sono utilizzati nella ricerca, devono essere allegati.

**Esposizione** - Se esponi i metadati su una piattaforma di distribuzione a terzi, fai attenzione a non esporre anche i metadati &quot;interni&quot;.

**Durata** - Più i metadati dovrebbero essere live, più è probabile che siano un buon candidato per i metadata allegati.

**Processi aziendali correlati** - È sicuramente utile avere un ID prodotto permanente come parte dei metadati. Ma la categoria di un articolo in relazione al catalogo del prodotto è un metadati discutibile per la risorsa.

**Organizzazione e trattamento** - Se la natura dei metadati è di natura organizzativa, ad esempio lo stato in un flusso di lavoro di approvazione o la proprietà di un determinato reparto, i metadati esterni devono essere presi in considerazione al momento di allegare i metadati alla risorsa.

*Per creare la strategia, fai le seguenti domande:*

* Che tipo di contenuti e &quot;informazioni aggiuntive&quot; (= metadata) sono necessari per risolvere problemi di business / domande di business / problemi di business?
* Quali sono le variabili, i &quot;campi&quot; nello schema e quali sono i possibili valori? Quali variabili richiedono un input di testo libero, quali possono essere ridotte per tipo (numero, data, booleano, ...), un set di valori fissi (ad esempio paesi) o tag da una determinata tassonomia. Quanti tag sono obbligatori, consentiti?
* Quali problemi tecnici / problemi / domande possono essere risolti con i metadati?
* Come puoi ottenere / creare quel contenuto / metadati? Quanto costerà ottenere/creare tali metadati?
* Quali tipi di metadati sono necessari per un particolare gruppo di utenti?
* Come verranno mantenuti e aggiornati i metadati?
* Chi è responsabile di quale parte del processo?
* Come si può garantire che vengano seguiti i processi aziendali concordati?
* Quali standard dovreste seguire? Dovrebbe adottare e modificare uno standard di settore (Dublin Core, ISO 19115, PRISM, ecc.) o l&#39;organizzazione deve creare i propri standard?
* Dove è documentata la strategia? Come puoi assicurarti che tutte le parti interessate abbiano accesso a tali informazioni? Come puoi essere sicuro che il personale appena integrato aderisca allo standard concordato (ad esempio, visita la formazione prima di accedere?)
