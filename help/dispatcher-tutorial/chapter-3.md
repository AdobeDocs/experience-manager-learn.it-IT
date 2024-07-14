---
title: Capitolo 3 - Argomenti avanzati sulla memorizzazione in cache di Dispatcher
description: Questa è la Parte 3 di una serie in tre parti da memorizzare nella cache in AEM. Le prime due parti si concentrano sul caching http semplice in Dispatcher e sulle limitazioni esistenti. Questa parte illustra alcune idee su come superare queste limitazioni.
feature: Dispatcher
topic: Architecture
role: Architect
level: Intermediate
doc-type: Tutorial
exl-id: 7c7df08d-02a7-4548-96c0-98e27bcbc49b
duration: 1353
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '6172'
ht-degree: 0%

---

# Capitolo 3 - Argomenti avanzati sulla memorizzazione in cache

*&quot;In Computer Science sono disponibili solo due elementi di rilievo: l&#39;annullamento della validità della cache e la denominazione.&quot;*

— PHIL KARLTON

## Panoramica

Questa è la parte 3 di una serie in tre parti dedicata al caching nell&#39;AEM. Le prime due parti si concentrano sul caching http semplice in Dispatcher e sulle limitazioni esistenti. Questa parte illustra alcune idee su come superare queste limitazioni.

## Memorizzazione in cache in generale

[Il Capitolo 1](chapter-1.md) e il Capitolo 2](chapter-2.md) di questa serie si concentrano principalmente su Dispatcher. [ Abbiamo spiegato le nozioni di base, i limiti e dove è necessario fare alcuni compromessi.

La complessità e le complessità della memorizzazione nella cache non sono problemi specifici di Dispatcher. La memorizzazione in cache è difficile in generale.

Avere Dispatcher come unico strumento nella casella degli strumenti sarebbe in realtà un vero e proprio limite.

In questo capitolo vogliamo ampliare ulteriormente la nostra visione sulla memorizzazione in cache e sviluppare alcune idee su come superare alcune delle carenze di Dispatcher. Non c&#39;è nessun punto d&#39;argento - dovrete fare compromessi nel vostro progetto. Ricorda che la precisione della memorizzazione in cache e dell’annullamento della validità comporta sempre complessità e che, con la complessità, esiste la possibilità di errori.

Dovrai fare dei compromessi in queste aree,

* Prestazioni e latenza
* Consumo risorse / Carico CPU / Utilizzo disco
* Precisione / Valuta / Stabilità / Sicurezza
* Semplicità / Complessità / Costo / Manutenzione / Prontezza all&#39;errore

Queste dimensioni sono interconnesse in un sistema piuttosto complesso. Non c&#39;è un semplice &quot;se-questo-allora-quello&quot;. La semplificazione di un sistema può renderlo più veloce o più lento. Può ridurre i costi di sviluppo, ma aumentare i costi presso l&#39;helpdesk, ad esempio se i clienti vedono contenuti non aggiornati o si lamentano di un sito web lento. Tutti questi fattori devono essere considerati e bilanciati tra loro. Ma a questo punto dovresti già avere una buona idea, che non ci sia un proiettile d&#39;argento o una singola &quot;best practice&quot; - solo un sacco di cattive pratiche e alcune buone.

## Cache concatenata

### Panoramica

#### Flusso di dati

Il recapito di una pagina da un server al browser di un client passa attraverso una moltitudine di sistemi e sottosistemi. Se osservi attentamente, c’è una serie di dati di hop che devono essere raccolti dalla sorgente al drenaggio, ciascuno dei quali è un potenziale candidato per la memorizzazione in cache.

![Flusso di dati di un&#39;applicazione CMS tipica](assets/chapter-3/data-flow-typical-cms-app.png)

*Flusso di dati di un&#39;applicazione CMS tipica*

<br> 

Iniziamo il nostro percorso con dei dati che si trovano su un disco rigido e che devono essere visualizzati in un browser.

#### Hardware e sistema operativo

Innanzitutto, il disco rigido (HDD) stesso ha alcune cache integrate nell&#39;hardware. In secondo luogo, il sistema operativo che monta il disco rigido utilizza la memoria libera per memorizzare nella cache i blocchi a cui si accede di frequente per velocizzare l&#39;accesso.

#### Archivio dei contenuti

Il livello successivo è CRX o Oak, ovvero il database di documenti utilizzato dall&#39;AEM. CRX e Oak dividono i dati in segmenti che possono essere memorizzati nella cache, anche per evitare un accesso più lento all’HDD.

#### Dati di terze parti

La maggior parte delle installazioni Web più grandi dispone anche di dati di terze parti; dati provenienti da un sistema di informazioni di prodotto, un sistema di gestione delle relazioni con i clienti, un database legacy o qualsiasi altro servizio web arbitrario. Non è necessario estrarre questi dati dall’origine ogni volta che sono necessari, soprattutto quando notoriamente non cambiano troppo frequentemente. Può quindi essere memorizzato nella cache, se non è sincronizzato nel database di CRX.

#### Livello aziendale: app/modello

Di solito, gli script di modelli non eseguono il rendering del contenuto non elaborato proveniente da CRX tramite l’API JCR. Probabilmente si dispone di un livello di business nel mezzo che unisce, calcola e/o trasforma i dati in un oggetto del dominio di business. Indovina un po’: se queste operazioni sono costose, dovresti considerare la possibilità di memorizzarle in cache.

#### Frammenti markup

Il modello ora è la base per il rendering del markup di un componente. Perché non memorizzare in cache anche il modello renderizzato?

#### Dispatcher, CDN e altri proxy

Off passa il rendering della pagina HTML a Dispatcher. Abbiamo già discusso del fatto che lo scopo principale di Dispatcher sia quello di memorizzare in cache le pagine HTML e altre risorse web (nonostante il nome). Prima che le risorse raggiungano il browser, è possibile che passi un proxy inverso, che può memorizzare in cache e una rete CDN, utilizzato anche per il caching. Il client può trovarsi in un ufficio, che concede l’accesso web solo tramite un proxy e tale proxy può anche decidere di memorizzare in cache per risparmiare traffico.

#### Cache browser

Ultimo ma non meno importante: anche il browser memorizza in cache. Questa è una risorsa facilmente trascurata. Ma è la cache più vicina e veloce che hai nella catena di caching. Purtroppo non è condiviso tra gli utenti, ma tra diverse richieste di un utente.

### Dove memorizzare in cache e perché

Si tratta di una lunga catena di potenziali cache. E tutti abbiamo dovuto affrontare problemi per i quali abbiamo visto contenuti obsoleti. Ma tenendo conto di quante fasi ci sono, è un miracolo che nella maggior parte dei casi stia funzionando.

Ma in quale punto di quella catena ha senso memorizzare cache? All&#39;inizio? Alla fine? Ovunque? Dipende... e dipende da un numero enorme di fattori. Anche due risorse nello stesso sito web potrebbero desiderare una risposta diversa a tale domanda.

Per darvi un&#39;idea approssimativa di quali fattori potreste prendere in considerazione,

**Durata** - Se gli oggetti hanno una durata breve intrinseca (i dati sul traffico potrebbero avere una durata inferiore ai dati meteo), potrebbe non valere la pena di essere memorizzati nella cache.

**Costo di produzione -** Quanto costoso (in termini di cicli della CPU e I/O) è la riproduzione e la consegna di un oggetto. In caso affermativo, la memorizzazione nella cache potrebbe non essere necessaria.

**Dimensione** - Per gli oggetti di grandi dimensioni sono necessarie più risorse nella cache. Questo potrebbe essere un fattore limitante e deve essere valutato in rapporto al beneficio.

**Frequenza di accesso** - Se si accede raramente agli oggetti, la memorizzazione nella cache potrebbe non essere efficace. Diventerebbero semplicemente obsoleti o verrebbero invalidati prima di poter accedere la seconda volta dalla cache. Tali elementi bloccherebbero semplicemente le risorse di memoria.

**Accesso condiviso** - I dati utilizzati da più entità devono essere memorizzati nella cache più in alto nella catena. In realtà, la catena di memorizzazione in cache non è una catena, ma un albero. Un elemento di dati nell’archivio potrebbe essere utilizzato da più modelli. Questi modelli, a loro volta, possono essere utilizzati da più script di rendering per generare frammenti di HTML. Questi frammenti sono inclusi in più pagine che vengono distribuite a più utenti con le loro cache private nel browser. Quindi &quot;condividere&quot; non significa condividere solo tra le persone, ma tra parti di software. Se desideri trovare una potenziale cache &quot;condivisa&quot;, rintraccia la struttura fino alla radice e individua un predecessore comune: è qui che dovresti memorizzare la cache.

**Distribuzione geospaziale** - Se gli utenti sono distribuiti in tutto il mondo, l&#39;utilizzo di una rete distribuita di cache potrebbe contribuire a ridurre la latenza.

**Larghezza di banda e latenza di rete** - A proposito di latenza, chi sono i tuoi clienti e che tipo di rete utilizzano? Forse i tuoi clienti sono clienti mobili in un paese sottosviluppato che utilizzano la connessione 3G di smartphone di vecchia generazione? Prendi in considerazione la creazione di oggetti più piccoli e memorizzali nella cache del browser.

Questa lista di gran lunga non è esaustiva, ma pensiamo che ormai abbiate capito l&#39;idea.

### Regole di base per il caching collegato

Ancora una volta - la memorizzazione nella cache è difficile. Condividiamo alcune regole di base, che abbiamo estratto da progetti precedenti che possono aiutarti a evitare problemi nel tuo progetto.

#### Evita il Double Caching

Ciascuno dei livelli introdotti nell&#39;ultimo capitolo fornisce un valore nella catena di memorizzazione in cache. Salvando i cicli di elaborazione o avvicinando i dati al consumatore. Non è sbagliato memorizzare in cache una parte di dati in più fasi della catena, ma è sempre necessario considerare i vantaggi e i costi della fase successiva. La memorizzazione in cache di una pagina intera nel sistema Publish di solito non offre alcun vantaggio, come già avviene in Dispatcher.

#### Unione di strategie di invalidazione

Esistono tre strategie di invalidazione di base:

* **TTL, durata:** Un oggetto scade dopo un periodo di tempo fisso (ad esempio, &quot;2 ore da ora&quot;)
* **Data di scadenza:** L&#39;oggetto scade all&#39;ora futura definita (ad esempio, &quot;17:00 del 10 giugno 2019&quot;)
* **Basato su evento:** l&#39;oggetto viene invalidato esplicitamente da un evento che si è verificato nella piattaforma (ad esempio, quando una pagina viene modificata e attivata)

Ora puoi utilizzare strategie diverse su diversi livelli di cache, ma ce ne sono alcune &quot;tossiche&quot;.

#### Annullamento della validità basato su eventi

![Annullamento della validità basato su evento puro](assets/chapter-3/event-based-invalidation.png)

*Annullamento della validità basato su evento puro: annullamento della validità dalla cache interna al livello esterno*

<br> 

L’invalidazione pura basata su eventi è la più semplice da comprendere, la più semplice da ottenere teoricamente corretta e la più accurata.

In breve, le cache vengono invalidate una alla volta dopo che l’oggetto è stato modificato.

Devi solo tenere presente una regola:

Annulla sempre la validità dalla cache interna a quella esterna. Se prima hai invalidato una cache esterna, questa potrebbe rimemorizzare in cache il contenuto non aggiornato proveniente da una cache interna. Non fare più supposizioni sull’ora di aggiornamento di una cache, assicurati che. Meglio, attivando l&#39;invalidazione della cache esterna _dopo_ l&#39;invalidazione di quella interna.

Questa è la teoria. Ma in pratica ci sono un certo numero di errori. Gli eventi devono essere distribuiti, potenzialmente su una rete. In pratica, questo rende il regime di invalidazione più difficile da attuare.

#### Auto - Correzione

Con l’annullamento della validità basato su eventi, è necessario disporre di un piano di emergenza. Cosa succede se si dimentica un evento di invalidazione? Una strategia semplice potrebbe essere quella di invalidare o eliminare dopo un certo periodo di tempo. Quindi, potresti non aver visto quell’evento e ora distribuisci contenuti non aggiornati. Ma gli oggetti hanno anche un TTL implicito di solo diverse ore (giorni). Quindi alla fine il sistema si auto-guarisce da solo.

#### Annullamento della validità basato su TTL puro

![Annullamento della validità basato su TTL non sincronizzato](assets/chapter-3/ttl-based-invalidation.png)

*Annullamento della validità basato su TTL non sincronizzato*

<br> 

Anche questo è un sistema abbastanza comune. Si sovrappongono diversi livelli di cache, ognuno dei quali ha il diritto di servire un oggetto per un certo periodo di tempo.

È facile da implementare. Sfortunatamente, è difficile prevedere l&#39;effettiva durata di vita di un dato.

![Possibilità esterna di prolungare la durata di un oggetto interno](assets/chapter-3/outer-cache.png)

*Cache esterna che prolunga la durata di un oggetto interno*

<br> 

Prendi in considerazione l’illustrazione precedente. Ogni livello di caching introduce un TTL di 2 minuti. Ora - anche il TTL complessivo deve essere di 2 minuti, giusto? Non proprio. Se il livello esterno recupera l&#39;oggetto appena prima che diventi obsoleto, il livello esterno in realtà prolunga il tempo di vita effettivo dell&#39;oggetto. In tal caso, il tempo effettivo di esecuzione può essere compreso tra 2 e 4 minuti. Considera che hai concordato con il tuo reparto aziendale, un giorno è tollerabile - e hai quattro livelli di cache. Il TTL effettivo su ciascun livello non deve superare le sei ore... aumentando la percentuale di mancati riscontri nella cache...

Non stiamo dicendo che sia un cattivo schema. Dovresti solo conoscerne i limiti. Ed è una strategia facile e piacevole con cui iniziare. Solo se il traffico del tuo sito aumenta, potresti prendere in considerazione una strategia più precisa.

*Sincronizzazione dell&#39;ora di invalidazione mediante l&#39;impostazione di una data specifica*

#### Annullamento della validità basato sulla data di scadenza

Si ottiene una durata effettiva più prevedibile, se si imposta una data specifica sull&#39;oggetto interno e la si propaga alle cache esterne.

![Sincronizzazione delle date di scadenza](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronizzazione delle date di scadenza*

<br> 

Tuttavia, non tutte le cache sono in grado di propagare le date. E può diventare brutto, quando la cache esterna aggrega due oggetti interni con date di scadenza diverse.

#### Combinazione dell’annullamento della validità basato su eventi e su TTL

![Unione di strategie basate su eventi e su TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Unione di strategie basate su eventi e su TTL*

<br> 

Un altro schema comune nel mondo dell’AEM è quello di utilizzare l’invalidazione basata sugli eventi nelle cache interne (ad esempio, cache in memoria in cui gli eventi possono essere elaborati quasi in tempo reale) e nelle cache basate su TTL all’esterno, dove forse non si ha accesso all’invalidazione esplicita.

Nel mondo AEM, si avrebbe una cache in memoria per oggetti business e frammenti HTML nei sistemi Publish, che viene invalidata, quando cambiano le risorse sottostanti e si propaga questo evento di modifica al dispatcher, che funziona anche in base agli eventi. Prima di questo avresti, ad esempio, una rete CDN basata su TTL.

Avere un livello di caching basato su TTL (breve) davanti a un Dispatcher potrebbe attenuare efficacemente un picco che solitamente si verifica dopo un’invalidazione automatica.

#### Combinazione di TTL e invalidazione basata su eventi

![Combinazione di TTL - e annullamento della validità basato su eventi](assets/chapter-3/toxic.png)

*Tossico: miscelazione di TTL - e invalidazione basata su eventi*

<br> 

Questa combinazione è tossica. Non inserire mai una cache basata su eventi dopo una cache basata su TTL o Expiry. Ricordate quell&#39;effetto di ricaduta che avevamo nella strategia &quot;pure-TTL&quot;? Lo stesso effetto può essere osservato qui. Solo che l’evento di invalidazione della cache esterna è già accaduto potrebbe non accadere più - mai, Questo può espandere la durata dell’oggetto memorizzato nella cache all’infinito.

![Combinazione basata su TTL ed evento: effetto di ricaduta sull&#39;infinito](assets/chapter-3/infinity.png)

*Combinazione basata su TTL ed evento: effetto di ricaduta sull&#39;infinito*

<br> 

## Memorizzazione nella cache parziale e memorizzazione nella cache in memoria

Potete agganciarvi allo stadio del processo di rendering per aggiungere livelli di memorizzazione nella cache. Dal recupero di oggetti di trasferimento dati remoti alla creazione di oggetti business locali al caching del markup di un singolo componente. Lasceremo le implementazioni concrete a un tutorial successivo. Ma forse hai già implementato tu stesso alcuni di questi livelli di caching. Quindi il minimo che possiamo fare qui è introdurre i principi di base - e gotchas.

### Parole di avvertimento

#### Rispetta controllo di accesso

Le tecniche qui descritte sono piuttosto potenti e un _must-have_ in ogni casella degli strumenti per gli sviluppatori AEM. Ma non emozionatevi troppo, usateli con saggezza. Memorizzare un oggetto in una cache e condividerlo con altri utenti nelle richieste di follow-up significa in realtà eludere il controllo degli accessi. Questo di solito non è un problema sui siti web rivolti al pubblico, ma può esserlo, quando un utente deve effettuare l’accesso prima di ottenerlo.

Considera di memorizzare il markup HTML del menu principale Sites in una cache in memoria per condividerlo tra varie pagine. In realtà si tratta di un esempio perfetto per archiviare HTML parzialmente renderizzati, in quanto la creazione di una navigazione è solitamente costosa in quanto richiede l’attraversamento di molte pagine.

Non stai condividendo la stessa struttura di menu tra tutte le pagine, ma anche con tutti gli utenti, il che la rende ancora più efficiente. Ma aspetta ... forse ci sono alcuni elementi nel menu che sono riservati solo a un certo gruppo di utenti. In tal caso, il caching può diventare un po’ più complesso.

#### Solo oggetti business personalizzati della cache

Se ve ne sono - questo è il consiglio più importante, possiamo darvi:

>[!WARNING]
>
>Memorizza nella cache solo gli oggetti che sono tuoi, che sono immutabili, che sei stato creato, che sono superficiali e non hanno riferimenti in uscita.

Che cosa significa?

1. Non si conosce il ciclo di vita previsto per gli oggetti di altre persone. Considera di visualizzare un riferimento a un oggetto richiesta e decidi di memorizzarlo nella cache. Ora la richiesta è terminata e il contenitore servlet desidera riciclare l’oggetto per la successiva richiesta in ingresso. In questo caso, qualcun altro sta modificando il contenuto che pensavi avesse il controllo esclusivo su di te. Non bisogna dimenticare che in un progetto si è visto qualcosa di simile. I clienti visualizzavano dati di altri clienti invece dei propri.

2. Se un oggetto è referenziato da una catena di altri riferimenti, non può essere rimosso dall’heap. Se si mantiene un oggetto presumibilmente piccolo nella cache che fa riferimento a, supponiamo che una rappresentazione di 4 MB di un&#39;immagine si avrà una buona probabilità di ottenere problemi con la perdita di memoria. Le cache devono essere basate su riferimenti deboli. Ma - i riferimenti deboli non funzionano come ci si potrebbe aspettare. Questo è il modo migliore per produrre una perdita di memoria e terminare in un errore di memoria esaurita. E... non sapete quali siano le dimensioni della memoria trattenuta degli oggetti estranei, giusto?

3. Soprattutto in Sling, puoi adattare (quasi) ogni oggetto l’uno all’altro. Considera di inserire una risorsa nella cache. La richiesta successiva (con diritti di accesso diversi), recupera tale risorsa e la adatta in un resourceResolver o in una sessione per accedere ad altre risorse a cui non avrebbe accesso.

4. Anche se si crea un &quot;wrapper&quot; sottile intorno a una risorsa dell’AEM, non è necessario memorizzarlo in cache, anche se è proprio e immutabile. L’oggetto racchiuso sarebbe un riferimento (che non consentiamo di utilizzare in precedenza) e, se guardiamo in modo nitido, questo fondamentalmente crea gli stessi problemi descritti nell’ultimo elemento.

5. Se desideri memorizzare in cache, crea i tuoi oggetti copiando dati primitivi nei tuoi oggetti shallo. Potrebbe essere necessario creare un collegamento tra i propri oggetti tramite riferimenti, ad esempio per memorizzare in cache una struttura ad albero di oggetti. Va bene, ma memorizza in cache solo gli oggetti appena creati nella stessa richiesta e nessun oggetto richiesto da un’altra posizione (anche se si tratta del namespace dell’oggetto &quot;tuo&quot;). _La chiave è la copia degli oggetti_. Assicurati inoltre di eliminare l’intera struttura di oggetti collegati contemporaneamente ed evitare riferimenti in entrata e in uscita alla struttura.

6. Sì, e mantieni gli oggetti immutabili. Proprietà private, solo e nessun setter.

Ci sono molte regole, ma vale la pena seguirle. Anche se sei esperto e super intelligente e hai tutto sotto controllo. Il giovane collega del tuo progetto si è appena laureato all&#39;università. Lui non sa di tutte queste insidie. Se non ci sono insidie, non c&#39;è niente da evitare. Semplicità e comprensibilità.

### Strumenti e librerie

Questa serie tratta di comprendere i concetti e consentire di creare un’architettura che si adatta al meglio al caso d’uso.

Non stiamo promuovendo alcuno strumento in particolare. Ma fornisci suggerimenti su come valutarli. Ad esempio, l’AEM ha una semplice cache integrata con un TTL fisso a partire dalla versione 6.0. La userai? Probabilmente non in pubblicazione dove segue nella catena una cache basata su eventi (suggerimento: Dispatcher). Ma potrebbe essere una scelta decente per un autore. Esiste anche una cache HTTP per Adobe ACS commons che potrebbe essere utile considerare.

Oppure puoi crearne una tua, in base a un framework di caching maturo come [Ehcache](https://www.ehcache.org). Può essere utilizzato per memorizzare nella cache oggetti Java e markup di cui è stato eseguito il rendering (`String` oggetti).

In alcuni semplici casi, puoi anche andare d’accordo con l’utilizzo di mappe hash simultanee - vedrai rapidamente i limiti qui - sia nello strumento che nelle tue abilità. La concorrenza è difficile da padroneggiare quanto la denominazione e il caching.

#### Riferimenti

* [Cache http Commons ACS](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Framework di caching Ehcache](https://www.ehcache.org)

### Termini di base

Non entreremo nella teoria del caching troppo in profondità qui, ma ci sentiamo obbligati a fornire alcune parole di buzz, in modo da avere un buon inizio di salto.

#### Eliminazione della cache

Abbiamo parlato molto di invalidazione ed eliminazione. _L&#39;eliminazione dalla cache_ è correlata a questi termini: dopo l&#39;eliminazione di una voce, non è più disponibile. Ma lo sfratto non avviene quando una voce non è aggiornata, ma quando la cache è piena. Gli elementi più recenti o &quot;più importanti&quot; spingono fuori dalla cache quelli più vecchi o meno importanti. Quali voci dovrai sacrificare è una decisione caso per caso. Potrebbe essere utile sfrattare i più vecchi o quelli che sono stati utilizzati molto raramente o a cui si è effettuato l’ultimo accesso da molto tempo.

#### Memorizzazione in cache preventiva

La memorizzazione in cache preventiva comporta la ricreazione della voce con contenuto nuovo nel momento in cui viene invalidata o considerata obsoleta. Naturalmente, potresti farlo solo con alcune risorse, che sei sicuro siano accessibili frequentemente e immediatamente. In caso contrario, si sprecherebbero risorse per la creazione di voci cache che potrebbero non essere mai richieste. Creando preventivamente le voci di cache è possibile ridurre la latenza della prima richiesta a una risorsa dopo l’annullamento della validità della cache.

#### Riscaldamento cache

Il riscaldamento della cache è strettamente correlato al caching preventivo. Anche se non usereste quel termine per un sistema live. Ed è meno vincolato nel tempo rispetto al primo. Non si rimemorizza la cache immediatamente dopo l’annullamento della validità, ma si riempie gradualmente la cache quando il tempo lo consente.

Ad esempio, puoi estrarre una gamba Publish/Dispatcher dal load balancer per aggiornarla. Prima di reintegrarla, esegui automaticamente la ricerca per indicizzazione delle pagine a cui si accede più di frequente per riportarle nella cache. Quando la cache è &quot;calda&quot; - riempita adeguatamente si reintegra la gamba nel load balancer.

O forse reintegrare la gamba in una volta sola, ma si limita il traffico a quella gamba in modo che abbia la possibilità di riscaldarla con l&#39;uso regolare.

Oppure, potresti voler memorizzare in cache anche alcune pagine a cui si accede meno di frequente nei momenti in cui il sistema è inattivo, per diminuire la latenza quando si accede effettivamente a tali pagine da richieste reali.

#### Cache Object Identity, Payload, Invalidation Dependency e TTL

In generale, un oggetto o una &quot;voce&quot; memorizzata in cache ha cinque proprietà principali,

#### Chiave

Questa è la proprietà con cui si identifica e si obietta. Per recuperare il payload o per eliminarlo dalla cache. Il dispatcher, ad esempio, utilizza come chiave l’URL di una pagina. Tieni presente che Dispatcher non utilizza i percorsi delle pagine. Questo non è sufficiente per distinguere diversi rendering. Altre cache potrebbero utilizzare chiavi diverse. Vedremo alcuni esempi più tardi.

#### Valore/Payload

Questo è il tesoro dell&#39;oggetto, i dati che vuoi recuperare. Nel caso del dispatcher, si tratta del contenuto dei file. Ma può anche essere una struttura ad oggetti Java.

#### TTL

Abbiamo già coperto il TTL. Il periodo di tempo dopo il quale una voce viene considerata obsoleta e non deve più essere consegnata.

#### Dipendenza

Ciò si riferisce all’annullamento della validità basato su eventi. Su quali dati originali dipende l’oggetto? Nella parte I, abbiamo già detto, che un tracciamento delle dipendenze vero e preciso è troppo complesso. Ma con la nostra conoscenza del sistema è possibile approssimare le dipendenze con un modello più semplice. Vengono invalidati abbastanza oggetti per eliminare contenuti non aggiornati... e forse inavvertitamente più di quanto sarebbe necessario. Ma noi cerchiamo di stare sotto &quot;purge tutto&quot;.

Quali oggetti dipendono dall&#39;autenticità degli altri in ogni singola applicazione. In seguito verranno forniti alcuni esempi su come implementare una strategia di dipendenza.

### Memorizzazione in cache di frammenti di HTML

![Riutilizzo di un frammento sottoposto a rendering in pagine diverse](assets/chapter-3/re-using-rendered-fragment.png)

*Riutilizzo di un frammento sottoposto a rendering in pagine diverse*

<br> 

La memorizzazione in cache dei frammenti di HTML è uno strumento potente. L’idea è quella di memorizzare in cache il markup HTML generato da un componente in una cache in memoria. Potreste chiedere, perché dovrei farlo? Sto comunque memorizzando nella cache il markup dell’intera pagina nel dispatcher, compreso il markup di quel componente. Siamo d&#39;accordo. Sì, ma una volta per pagina. Il markup non viene condiviso tra le pagine.

Immagina di eseguire il rendering di una navigazione sopra ogni pagina. Il markup ha lo stesso aspetto su ogni pagina. Ma lo stai riproducendo più volte per ogni pagina, che non è in Dispatcher. E ricorda: dopo l’annullamento automatico della validità, è necessario eseguire nuovamente il rendering di tutte le pagine. In pratica, si esegue lo stesso codice centinaia di volte con gli stessi risultati.

Dalla nostra esperienza, il rendering di una navigazione superiore nidificata è un’attività molto costosa. In genere si passa in rassegna una buona parte della struttura del documento per generare gli elementi di navigazione. Anche se ti servono solo il titolo della navigazione e l’URL, le pagine devono essere caricate in memoria. E qui intasano risorse preziose. Continuamente.

Tuttavia, il componente è condiviso tra più pagine. E la condivisione di qualcosa è un segno che si usa una cache. Quindi - quello che desideri fare è verificare se il componente Navigazione è già stato renderizzato e memorizzato in cache e invece di eseguire nuovamente il rendering, è sufficiente emettere il valore cache.

Ci sono due meravigliose bellezze di questo schema facilmente perduto:

1. Si sta memorizzando nella cache una stringa Java. Una stringa non dispone di riferimenti in uscita ed è immutabile. Quindi, considerando le avvertenze di cui sopra - questo è super-sicuro.

2. Anche l’invalidazione è estremamente semplice. Ogni volta che cambia qualcosa nel tuo sito web, vuoi annullare la validità di questa voce della cache. La ricostruzione è relativamente economica, in quanto deve essere eseguita una sola volta e poi viene riutilizzata da tutte le centinaia di pagine.

Questo è un grande sollievo per i server Publish.

### Implementazione delle cache dei frammenti

#### Tag personalizzati

In passato, quando si utilizzava JSP come motore di modelli, era piuttosto comune utilizzare un tag JSP personalizzato che racchiudeva il codice di rendering dei componenti.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

Il tag personalizzato che acquisirebbe il corpo e lo scriverebbe nella cache o impedirebbe l’esecuzione del corpo e genererebbe invece il payload della voce cache.

&quot;Key&quot; è il percorso dei componenti che avrebbe nella home page. Non utilizziamo il percorso del componente nella pagina corrente, in quanto questo creerebbe una voce di cache per pagina, il che sarebbe in contraddizione con la nostra intenzione di condividere tale componente. Inoltre, non si utilizza solo il percorso relativo dei componenti (`jcr:conten/mainnavigation`), in quanto ciò impedirebbe l&#39;utilizzo di diversi componenti di navigazione in siti diversi.

&quot;Cache&quot; è un indicatore in cui memorizzare la voce. In genere sono presenti più cache in cui vengono archiviati gli elementi. Ognuna di queste potrebbe comportarsi in modo leggermente diverso. Quindi, è bene differenziare ciò che è memorizzato, anche se alla fine sono solo stringhe.

&quot;Dipendenza&quot;: da questo dipende la voce della cache. La cache di &quot;navigazione principale&quot; potrebbe avere una regola, in base alla quale se si verificano modifiche sotto il nodo &quot;dependency&quot;, la voce corrispondente deve essere eliminata. Quindi: l’implementazione della cache dovrebbe registrarsi come listener di eventi nell’archivio per essere a conoscenza delle modifiche, quindi applicare le regole specifiche per la cache per scoprire cosa deve essere invalidato.

Quello di cui sopra era solo un esempio. Puoi anche scegliere di avere una struttura ad albero delle cache. Se il primo livello viene utilizzato per separare i siti (o tenant) e il secondo livello si suddivide in tipi di contenuto (ad esempio &quot;navigazione principale&quot;), ciò potrebbe non consentire l’aggiunta del percorso delle home page come nell’esempio precedente.

A proposito: puoi utilizzare questo approccio anche con componenti basati su HTL più moderni. Avresti quindi un wrapper JSP intorno allo script HTL.

#### Filtri componenti

Tuttavia, in un approccio HTL puro, è preferibile creare la cache dei frammenti con un filtro del componente Sling. Non l&#39;abbiamo ancora visto allo stato brado, ma questo è l&#39;approccio che adotteremmo sulla questione.

#### Sling Dynamic Include

La cache dei frammenti viene utilizzata se si dispone di qualcosa di costante (la navigazione) nel contesto di un ambiente in evoluzione (pagine diverse).

Ma puoi anche avere l’opposto, un contesto relativamente costante (una pagina che raramente cambia) e alcuni frammenti in continua evoluzione in quella pagina (ad esempio, un ticker live).

In questo caso, puoi dare una possibilità a [Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html). In sostanza, si tratta di un filtro componente che racchiude il componente dinamico e, invece di eseguire il rendering del componente nella pagina, crea un riferimento. Questo riferimento può essere una chiamata Ajax: in modo che il componente sia incluso dal browser e quindi la pagina circostante possa essere memorizzata in cache in modo statico. Oppure, in alternativa, Sling Dynamic Include può generare una direttiva SSI (Server Side Include). Questa direttiva verrebbe eseguita nel server Apache. Puoi anche utilizzare le direttive ESI - Edge Side Include se utilizzi Vernice o una rete CDN che supporta gli script ESI.

![Diagramma di sequenza di una richiesta con inclusione dinamica Sling](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagramma di sequenza di una richiesta con inclusione dinamica Sling*

<br> 

Nella documentazione SDI è necessario disattivare la memorizzazione in cache per gli URL che terminano con &quot;*.nocache.html&quot;, operazione molto utile in quanto si tratta di componenti dinamici.

È possibile visualizzare un&#39;altra opzione per l&#39;utilizzo di SDI: Se _non_ disabilita la cache del dispatcher per le inclusioni, Dispatcher si comporta come una cache dei frammenti simile a quella descritta nell&#39;ultimo capitolo: le pagine e i frammenti dei componenti vengono memorizzati nella cache in modo uniforme e indipendente nel dispatcher e uniti dallo script SSI nel server Apache quando la pagina viene richiesta. In questo modo, puoi implementare componenti condivisi come la navigazione principale (dato che utilizzi sempre lo stesso URL di componente).

Dovrebbe funzionare, in teoria. Ma...

Si consiglia di non farlo: si perderebbe la capacità di bypassare la cache per i componenti dinamici reali. SDI è configurato a livello globale e le modifiche apportate per la &quot;poor-mans-fragment-cache&quot; vengono applicate anche ai componenti dinamici.

Consigliamo di studiare attentamente la documentazione SDI. Ci sono alcune altre limitazioni, ma SDI è uno strumento prezioso in alcuni casi.

#### Riferimenti

* [docs.oracle.com - Come scrivere tag JSP personalizzati](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Creazione e utilizzo di filtri componenti](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Inclusioni dinamiche Sling](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Configurazione delle inclusioni dinamiche Sling in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Memorizzazione in cache del modello

![Memorizzazione in cache basata su modello: un oggetto business con due rendering diversi](assets/chapter-3/model-based-caching.png)

*Memorizzazione in cache basata su modello: un oggetto business con due rendering diversi*

<br> 

Rivediamo il caso con la navigazione di nuovo. Stavamo presupponendo che ogni pagina avrebbe richiesto lo stesso markup della navigazione.

Ma forse non è così. È possibile eseguire il rendering di un markup diverso per l&#39;elemento nella struttura di spostamento che rappresenta la _pagina corrente_.

```
Travel Destinations

<ul class="maninnav">
  <li class="currentPage">Travel Destinations
    <ul>
      <li>Finland
      <li>Canada
      <li>Norway
    </ul>
  <li>News
  <li>About us
<ul>
```

```
News

<ul class="maninnav">
  <li>Travel Destinations
  <li class="currentPage">News
    <ul>
      <li>Winter is coming>
      <li>Calm down in the wild
    </ul>
  <li>About us
<is
```

Questi sono due rendering completamente diversi. Tuttavia, l&#39;_oggetto business_, ovvero la struttura di navigazione completa, è lo stesso.  L&#39;_oggetto business_ qui sarà un object graph che rappresenta i nodi della struttura. Questo grafico può essere facilmente memorizzato in una cache in memoria. Tuttavia, ricorda che questo grafico non deve contenere oggetti o fare riferimento a oggetti che non sono stati creati personalmente, soprattutto ora i nodi JCR.

#### Memorizzazione in cache nel browser

Abbiamo già sottolineato l’importanza di memorizzare in cache il browser e sono disponibili molte esercitazioni valide. Alla fine, per il browser, Dispatcher è solo un server web che segue il protocollo HTTP.

Tuttavia, nonostante la teoria, abbiamo raccolto alcuni frammenti di conoscenza che non abbiamo trovato altrove e che vogliamo condividere.

In sostanza, il caching del browser può essere utilizzato in due modi diversi:

1. Il browser ha una risorsa memorizzata nella cache di cui conosce la data di scadenza esatta. In tal caso, non richiede nuovamente la risorsa.

2. Il browser dispone di una risorsa, ma non è sicuro se sia ancora valido. In tal caso, si rivolgerebbe al server web (il Dispatcher nel nostro caso). Indica la risorsa se è stata modificata dall&#39;ultima consegna. Se non è stato modificato, il server risponde con &quot;304 - non modificato&quot; e sono stati trasmessi solo i metadati.

#### Debugging

Se stai ottimizzando le impostazioni Dispatcher per la memorizzazione nella cache del browser, è estremamente utile utilizzare un server proxy desktop tra il browser e il server web. Preferiamo &quot;Charles Web Debugging Proxy&quot; di Karl von Randow.

Utilizzando Charles, puoi leggere le richieste e le risposte, che vengono trasmesse da e verso il server. E - puoi imparare molto sul protocollo HTTP. I browser moderni offrono anche alcune funzionalità di debug, ma le funzioni di un proxy desktop non hanno precedenti. Puoi manipolare i dati trasferiti, limitare la trasmissione, ripetere singole richieste e molto altro. L&#39;interfaccia utente è strutturata in modo chiaro e abbastanza completa.

Il test più semplice consiste nell’utilizzare il sito web come utente normale, con il proxy in mezzo, e archiviare il proxy se il numero di richieste statiche (a /etc/...) si sta riducendo nel tempo, in quanto dovrebbero trovarsi nella cache e non essere più richieste.

È stato rilevato che un proxy potrebbe fornire una panoramica più chiara, in quanto le richieste memorizzate nella cache non vengono visualizzate nel registro, mentre alcuni debugger incorporati nel browser mostrano ancora queste richieste con &quot;0 ms&quot; o &quot;dal disco&quot;. Il che va bene e accurato, ma potrebbe offuscare un po&#39; la vista.

Puoi quindi approfondire e controllare le intestazioni dei file trasferiti per verificare, ad esempio, se le intestazioni http &quot;Scade&quot; sono corrette. Puoi ripetere le richieste con intestazioni if-modified-Since impostate per verificare se il server risponde correttamente con un codice di risposta 304 o 200. Puoi osservare la tempistica delle chiamate asincrone e testare anche i presupposti di sicurezza in un certo grado. Ti abbiamo detto di non accettare tutti i selettori che non sono esplicitamente previsti? Qui puoi giocare con l’URL e i parametri e vedere se l’applicazione si comporta bene.

Quando esegui il debug della cache, è disponibile una sola cosa che ti chiediamo di non fare:

Non ricaricare le pagine nel browser!

Un &quot;ricaricamento del browser&quot;, un _semplice ricaricamento_ e un _forzato ricaricamento_ (&quot;_spostamento ricaricamento_&quot;) non corrispondono a una normale richiesta di pagina. Una richiesta di ricaricamento semplice imposta un’intestazione

```
Cache-Control: max-age=0
```

E un Maiusc-Ricarica (tenendo premuto il tasto &quot;Maiusc&quot; mentre si fa clic sul pulsante Ricarica) in genere imposta un’intestazione di richiesta

```
Cache-Control: no-cache
```

Entrambe le intestazioni hanno effetti simili ma leggermente diversi, ma soprattutto si differenziano completamente da una normale richiesta quando si apre un URL dallo slot URL o utilizzando i collegamenti sul sito. Con l’esplorazione normale non vengono impostate le intestazioni Cache-Control, ma probabilmente un’intestazione If-modified-Since.

Quindi, se vuoi eseguire il debug del comportamento di navigazione normale, devi fare esattamente questo: _Sfoglia normalmente_. L’utilizzo del pulsante Ricarica del browser è il modo migliore per non visualizzare gli errori di configurazione della cache nella configurazione.

Utilizza il tuo proxy Charles per capire di cosa stiamo parlando. Sì, e finché è aperto, è possibile ripetere le richieste proprio lì. Non è necessario ricaricare dal browser.

## Test delle prestazioni

Utilizzando un proxy, puoi ottenere un’idea del comportamento di temporizzazione delle pagine. Ovviamente, questo non è di gran lunga un test di performance.  Un test delle prestazioni richiederebbe un certo numero di client che richiedono le pagine in parallelo.

Un errore comune, che abbiamo riscontrato troppo spesso, è che il test delle prestazioni include solo un numero estremamente ridotto di pagine, che vengono distribuite solo dalla cache di Dispatcher.

Se promuovi l’applicazione al sistema live, il carico è completamente diverso da quello testato.

Nel sistema live, il pattern di accesso non è quel numero ridotto di pagine equamente distribuite che si hanno nei test (pagina home e poche pagine di contenuto). Il numero di pagine è molto più grande e le richieste sono distribuite in modo molto disomogeneo. E, naturalmente, le pagine live non possono essere servite al 100% dalla cache: esistono richieste di invalidazione provenienti dal sistema Publish che causano l’invalidazione automatica di una porzione enorme delle tue preziose risorse.

Ah sì - e quando si sta ricostruendo la cache di Dispatcher, scoprirete, che il sistema Publish si comporta in modo molto diverso, a seconda se si richiede solo una manciata di pagine - o un numero più grande. Anche se tutte le pagine sono altrettanto complesse, il loro numero ha un ruolo. Ricordate cosa abbiamo detto sul caching concatenato? Se richiedi sempre lo stesso numero ridotto di pagine, è probabile che i blocchi corrispondenti con i dati non elaborati si trovino nella cache dei dischi rigidi o che i blocchi vengano memorizzati nella cache dal sistema operativo. Inoltre, c’è una buona probabilità che l’archivio abbia memorizzato nella cache il segmento corrispondente nella sua memoria principale. Il rendering è quindi notevolmente più rapido rispetto a quando altre pagine venivano eliminate a vicenda di tanto in tanto da diverse cache.

La memorizzazione in cache è difficile, così come il test di un sistema che si basa sulla memorizzazione in cache. Quindi, cosa si può fare per avere uno scenario più accurato nella vita reale?

È consigliabile eseguire più test e fornire più indici di prestazioni per misurare la qualità della soluzione.

Se disponi già di un sito web, misura il numero di richieste e come vengono distribuite. Prova a modellare un test che utilizza una distribuzione simile delle richieste. Aggiungere un po&#39; di casualità non poteva far male. Non è necessario simulare un browser che carichi risorse statiche come JS e CSS, perché in realtà non contano. Alla fine vengono memorizzati nella cache nel browser o nel Dispatcher e non tornano al carico in modo significativo. Ma le immagini di riferimento hanno un impatto reale. Trova la loro distribuzione anche nei vecchi file di registro e modella un modello di richiesta simile.

Ora esegui un test con il Dispatcher che non memorizza nella cache. Quello è lo scenario peggiore. Scoprite a quale picco di carico il vostro sistema sta diventando instabile in queste peggiori condizioni. Se lo desideri, potresti anche peggiorare la situazione togliendo alcune gambe Dispatcher/Publish.

Quindi, esegui lo stesso test con tutte le impostazioni della cache richieste su &quot;on&quot;. Aumenta lentamente le richieste parallele per riscaldare la cache e osserva quanto il sistema può richiedere in queste condizioni di caso migliore.

In media, uno scenario potrebbe essere quello di eseguire il test con Dispatcher abilitato, ma anche con alcuni invalidamenti. Puoi simulare questa situazione toccando gli statfile con un cronjob o inviando le richieste di annullamento della validità a intervalli irregolari al Dispatcher. Non dimenticare di eliminare anche alcune delle risorse non automatiche invalidate di tanto in tanto.

Puoi variare l’ultimo scenario aumentando le richieste di invalidazione e aumentando il carico.

È un po&#39; più complesso di un semplice test di carico lineare, ma offre molta più fiducia nella tua soluzione.

Potresti evitare lo sforzo. Ma almeno condurre un test del caso peggiore sul sistema Publish con un numero maggiore di pagine (equamente distribuite) per vedere i limiti del sistema. Assicurati di interpretare correttamente il numero dello scenario migliore e di eseguire il provisioning dei sistemi con sufficiente spazio di crescita.
