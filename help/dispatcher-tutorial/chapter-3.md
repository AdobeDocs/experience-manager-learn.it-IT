---
title: Capitolo 3 - Argomenti avanzati della memorizzazione in cache
seo-title: Cache di AEM Dispatcher demistificata - Capitolo 3 - Argomenti avanzati della memorizzazione in cache
description: Il capitolo 3 dell’esercitazione Demystified sulla cache di AEM Dispatcher illustra come superare le limitazioni di cui al capitolo 2.
seo-description: Il capitolo 3 dell’esercitazione Demystified sulla cache di AEM Dispatcher illustra come superare le limitazioni di cui al capitolo 2.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '6187'
ht-degree: 0%

---


# Capitolo 3 - Argomenti avanzati della memorizzazione in cache

*&quot;Ci sono solo due cose difficili in Informatica Scienza: invalidazione della cache e denominazione degli elementi.&quot;*

— PHIL KARLTON

## Panoramica

Questa è la parte 3 di una serie di tre parti per il caching in AEM. Dove le prime due parti si sono concentrate sulla semplice memorizzazione in cache http nel Dispatcher e quali limitazioni ci sono. Questa parte discute alcune idee su come superare questi limiti.

## Memorizzazione in cache in generale

[Il capitolo 1](chapter-1.md) e il  [capitolo 2](chapter-2.md) di questa serie si sono concentrati principalmente sul Dispatcher. Abbiamo spiegato le basi, i limiti e dove è necessario fare determinate scelte.

La complessità e le complessità del caching non sono problemi unici del Dispatcher. Il caching è difficile in generale.

Avere Dispatcher come unico strumento nella cassetta degli attrezzi sarebbe in realtà un reale limite.

In questo capitolo vogliamo ampliare ulteriormente il nostro punto di vista sul caching e sviluppare alcune idee su come superare alcune delle carenze di Dispatcher. Non c&#39;è nessun proiettile d&#39;argento - dovrete fare compromessi nel tuo progetto. Ricordate che con la memorizzazione in cache e l&#39;invalidazione dell&#39;accuratezza viene sempre fornita la complessità, e con la complessità si ha la possibilità di errori.

Sarà necessario effettuare scambi in questi settori,

* Prestazioni e latenza
* Consumo di risorse / Caricamento CPU / Utilizzo del disco
* Precisione / Valuta / Stabilità / Sicurezza
* Semplicità / Complessità / Costo / Mantenibilità / Predicenza dell&#39;errore

Queste dimensioni sono interconnesse in un sistema piuttosto complesso. Non c&#39;è un semplice se-allora-allora-quello. Semplificare un sistema può renderlo più veloce o più lento. Può ridurre i costi di sviluppo, ma aumentare i costi dell&#39;helpdesk, ad esempio se i clienti vedono contenuti scadenti o se si lamentano di un sito web lento. Tutti questi fattori devono essere considerati ed equilibrati l&#39;uno contro l&#39;altro. Ma ormai dovresti già avere una buona idea, che non ci sia una pallottola d&#39;argento o un&#39;unica &quot;best practice&quot; - solo un sacco di pessime pratiche e alcune buone.

## Memorizzazione in cache

### Panoramica

#### Flusso dei dati

La distribuzione di una pagina da un server al browser di un cliente attraversa una moltitudine di sistemi e sottosistemi. Se si guarda attentamente, ci sono una serie di dati di luppolo da prendere dalla sorgente allo scarico, ciascuno dei quali è un potenziale candidato per la memorizzazione in cache.

![Flusso di dati di una tipica applicazione CMS](assets/chapter-3/data-flow-typical-cms-app.png)

*Flusso di dati di una tipica applicazione CMS*

<br> 

Cominciamo il nostro percorso con un pezzo di dati che si trova su un disco rigido e che deve essere visualizzato in un browser.

#### Hardware e sistema operativo

In primo luogo, l&#39;hard disk (HDD) stesso ha una parte di cache integrata nell&#39;hardware. In secondo luogo, il sistema operativo che monta il disco rigido utilizza la memoria libera per memorizzare nella cache i blocchi a cui si accede di frequente per velocizzare l&#39;accesso.

#### Archivio dei contenuti

Il livello successivo è il CRX o Oak - il database dei documenti utilizzato da AEM. CRX e Oak dividono i dati in segmenti che possono essere memorizzati nella cache così come per evitare un accesso più lento all&#39;HDD.

#### Dati di terze parti

La maggior parte delle installazioni web più grandi dispone anche di dati di terze parti; i dati provenienti da un sistema di informazioni sui prodotti, un sistema di gestione delle relazioni con i clienti, un database legacy o qualsiasi altro servizio Web arbitrario. Questi dati non devono essere estratti dall&#39;origine ogni volta che sono necessari, soprattutto no, quando è noto che cambiano non troppo frequentemente. Quindi, può essere memorizzato nella cache, se non è sincronizzato nel database CRX.

#### Livello aziendale - App / Modello

Di solito i tuoi script di modello non eseguono il rendering del contenuto non elaborato proveniente da CRX tramite l’API JCR. Molto probabilmente si dispone di un livello business tra che unisce, calcola e/o trasforma i dati in un oggetto dominio business. Indovina cosa - se queste operazioni sono costose, dovresti considerare la memorizzazione in cache.

#### Frammenti di marcatura

Il modello ora è la base per il rendering del markup per un componente. Perché non memorizzare in cache anche il modello di cui è stato effettuato il rendering?

#### Dispatcher, CDN e altri proxy

Off passa la pagina HTML renderizzata al Dispatcher. Abbiamo già discusso del fatto che lo scopo principale del Dispatcher è quello di memorizzare nella cache le pagine HTML e altre risorse web (nonostante il suo nome). Prima che le risorse raggiungano il browser, potrebbe passare un proxy inverso - che può memorizzare in cache e un CDN - che viene utilizzato anche per la memorizzazione in cache. Il cliente può sedersi in un ufficio, che concede l&#39;accesso web solo tramite un proxy - e quel proxy potrebbe decidere di memorizzare in cache e anche di salvare il traffico.

#### Cache del browser

Ultimo ma non meno importante - anche il browser memorizza in cache. Questa è una risorsa facilmente ignorata. Ma è la cache più vicina e veloce che hai nella catena di memorizzazione in cache. Sfortunatamente - non è condiviso tra gli utenti - ma ancora tra diverse richieste di un utente.

### Dove memorizzare nella cache e perché

Questa è una lunga catena di cache potenziali. E tutti abbiamo affrontato problemi in cui abbiamo visto contenuti obsoleti. Ma tenendo conto di quanti stadi ci sono, è un miracolo che la maggior parte delle volte funzioni.

Ma dove ha senso mettere in cache questa catena? All&#39;inizio? Alla fine? Dappertutto? Dipende... e dipende da un gran numero di fattori. Anche due risorse nello stesso sito web potrebbero desiderare una risposta diversa a quella domanda.

Per darvi un&#39;idea approssimativa di quali fattori potreste prendere in considerazione,

**Tempo di esecuzione** : se gli oggetti hanno un breve tempo di vita intrinseco (i dati sul traffico potrebbero avere una durata inferiore rispetto ai dati meteo), potrebbe non essere utile memorizzarli nella cache.

**Costo di produzione:** quanto costoso (in termini di cicli della CPU e I/O) è la riproduzione e la consegna di un oggetto. Se è economico caching potrebbe non essere necessario.

**Dimensioni** : gli oggetti di grandi dimensioni richiedono la memorizzazione nella cache di più risorse. Questo potrebbe essere un fattore limitante e deve essere equilibrato rispetto al beneficio.

**Frequenza di accesso** : se l’accesso agli oggetti è raro, il caching potrebbe non essere efficace. Semplicemente diventerebbero obsoleti o invalidati prima di poter accedere la seconda volta dalla cache. Tali elementi bloccherebbero semplicemente le risorse di memoria.

**Accesso condiviso** : i dati utilizzati da più entità devono essere memorizzati nella cache più in alto nella catena. In realtà, la catena di caching non è una catena, ma un albero. Un elemento di dati nell’archivio potrebbe essere utilizzato da più di un modello. Questi modelli possono a loro volta essere utilizzati da più script di rendering per generare frammenti HTML. Questi frammenti sono inclusi in più pagine distribuite a più utenti con le relative cache private nel browser. Quindi &quot;condividere&quot; non significa condividere solo tra le persone, ma tra i vari software. Se vuoi trovare una potenziale cache &quot;condivisa&quot;, devi solo rintracciare l’albero alla radice e trovare un antenato comune - è lì che dovresti memorizzare la cache.

**Distribuzione**  geografica: se gli utenti sono distribuiti in tutto il mondo, l’utilizzo di una rete distribuita di cache potrebbe contribuire a ridurre la latenza.

**Larghezza di banda e latenza**  di rete - Parlando di latenza, chi sono i vostri clienti e che tipo di rete stanno utilizzando? Forse i vostri clienti sono clienti mobili in un paese sottosviluppato utilizzando la connessione 3G di smartphone di vecchia generazione? Prendi in considerazione la creazione di oggetti più piccoli e memorizzali nella cache del browser.

Questa lista di gran lunga non è completa, ma pensiamo che l&#39;idea sia già arrivata.

### Regole di base per la memorizzazione in cache in catena

Anche in questo caso, la memorizzazione in cache è difficile. Condividiamo alcune regole di base estratte da progetti precedenti che possono aiutarti a evitare problemi nel tuo progetto.

#### Evitare Il Doppio Caching

Ognuno dei livelli introdotti nell&#39;ultimo capitolo fornisce un certo valore nella catena di caching. Risparmiando i cicli di calcolo o avvicinando i dati al consumatore. Non è sbagliato memorizzare in cache un dato in più fasi della catena, ma dovresti sempre considerare quali sono i vantaggi e i costi della fase successiva. La memorizzazione nella cache di una pagina intera nel sistema di pubblicazione in genere non fornisce alcun vantaggio, come avviene già in Dispatcher.

#### Strategie di invalidazione del mixaggio

Sono disponibili tre strategie di invalidazione di base:

* **TTL, Time to Live:** un oggetto scade dopo un periodo di tempo fisso (ad esempio, &quot;2 ore da ora&quot;)
* **Data di scadenza:** l’oggetto scade a un’ora definita nel futuro (ad esempio, &quot;17:00 del 10 giugno 2019&quot;)
* **Basato su eventi:** l’oggetto viene invalidato esplicitamente da un evento che si è verificato nella piattaforma (ad esempio, quando una pagina viene modificata e attivata)

Ora puoi usare strategie diverse su diversi livelli di cache, ma ce ne sono alcuni &quot;tossici&quot;.

#### Annullamento della validità basato su eventi

![Annullamento della validità basato su eventi puri](assets/chapter-3/event-based-invalidation.png)

*Annullamento della validità basato su eventi puri: Annullare la validità dalla cache interna al livello esterno*

<br> 

L&#39;invalidazione pura basata su eventi è la più semplice da comprendere, la più semplice da ottenere teoricamente corretta e la più accurata.

In altre parole, le cache vengono invalidate una alla volta dopo la modifica dell’oggetto.

È sufficiente tenere a mente una regola:

Annulla sempre la validità dall’interno alla cache esterna. Se prima hai invalidato una cache esterna, potrebbe ri-memorizzare in cache il contenuto non aggiornato da una cache interna. Non fare supposizioni a che ora una cache è di nuovo fresca - assicurati. Meglio, attivando l&#39;annullamento della validità della cache esterna _dopo_ l&#39;annullamento della validità di quella interna.

Questa è la teoria. Ma in pratica ci sono un certo numero di gotchas. Gli eventi devono essere distribuiti - potenzialmente su una rete. In pratica, questo rende il sistema di invalidazione più difficile da attuare.

#### Automatico - Guarigione

Con l’annullamento della validità basato su eventi, è necessario disporre di un piano di emergenza. Cosa succede se si perde un evento di annullamento della validità? Una semplice strategia potrebbe essere quella di annullare o rimuovere una volta trascorso un certo periodo di tempo. Quindi - potresti aver perso quell&#39;evento e ora servire contenuti datati. Ma i tuoi oggetti hanno anche un TTL implicito di diverse ore (giorni) solo. Quindi alla fine il sistema si auto-guarisce da solo.

#### Invalidazione pura basata su TTL

![Annullamento della validità basato su TTL non sincronizzato](assets/chapter-3/ttl-based-invalidation.png)

*Annullamento della validità basato su TTL non sincronizzato*

<br> 

Anche questo è uno schema abbastanza comune. Si sovrappongono diversi livelli di cache, ciascuno dei quali ha diritto a servire un oggetto per un certo periodo di tempo.

È facile da implementare. Sfortunatamente, è difficile prevedere l&#39;effettivo ciclo di vita di un dato.

![Tracciamento esterno che prolunga il ciclo di vita di un oggetto interno](assets/chapter-3/outer-cache.png)

*Cache esterna che prolunga l’intervallo di vita di un oggetto interno*

<br> 

Considera l&#39;illustrazione precedente. Ogni livello di caching introduce un TTL di 2 min. Ora - il TTL complessivo deve anche 2 minuti, giusto? Non proprio. Se il livello esterno recupera l&#39;oggetto immediatamente prima che diventi obsoleto, il livello esterno in realtà prolunga il tempo di vita effettivo dell&#39;oggetto. In tal caso, il tempo effettivo può essere compreso tra 2 e 4 minuti. Considera di essere d&#39;accordo con il tuo reparto aziendale, un giorno è tollerabile - e hai quattro strati di cache. Il TTL effettivo su ogni livello non deve essere più lungo di sei ore... aumentando la frequenza di mancati riscontri nella cache...

Non stiamo dicendo che si tratta di un pessimo schema. Dovresti solo conoscerne i limiti. Ed è una strategia facile e bella da usare. Solo se il traffico del tuo sito aumenta, potresti considerare una strategia più precisa.

*Sincronizzazione dell’ora di invalidazione impostando una data specifica*

#### Invalidazione basata su data di scadenza

Se si imposta una data specifica sull&#39;oggetto interno e la si propaga alle cache esterne, si ottiene un tempo di vita effettivo più prevedibile.

![Sincronizzazione delle date di scadenza](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronizzazione delle date di scadenza*

<br> 

Tuttavia, non tutte le cache sono in grado di propagare le date. E può diventare brutto, quando la cache esterna aggrega due oggetti interni con date di scadenza diverse.

#### Invalidazione basata su eventi e TTL

![Combinazione di strategie basate su eventi e su TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Combinazione di strategie basate su eventi e su TTL*

<br> 

Uno schema comune in AEM World è anche quello di utilizzare l’annullamento della validità basato su eventi nelle cache interne (ad esempio, nelle cache in memoria in cui gli eventi possono essere elaborati in tempo quasi reale) e nelle cache basate su TTL all’esterno, dove forse non si ha accesso all’annullamento esplicito della validità.

Nel mondo AEM si dispone di una cache in-memory per oggetti aziendali e frammenti HTML nei sistemi Publish, che viene invalidata, quando le risorse sottostanti cambiano e si propaga questo evento change al dispatcher che funziona anche in base agli eventi. Davanti a questo si avrebbe ad esempio una CDN basata su TTL.

Avere un livello di memorizzazione in cache (breve) basata su TTL davanti a un’istanza di Dispatcher potrebbe ammorbidire efficacemente un picco che si verifica solitamente dopo un’annullamento automatico della validità.

#### Miscelazione TTL - e annullamento della validità basato su eventi

![Miscelazione TTL - e annullamento della validità basato su eventi](assets/chapter-3/toxic.png)

*Tossico: Miscelazione TTL - e annullamento della validità basato su eventi*

<br> 

Questa combinazione è tossica. Non inserire mai una cache basata su eventi e TTL dopo una cache basata su scadenza o TTL. Vi ricordate l&#39;effetto di ricaduta che abbiamo avuto nella strategia &quot;pure-TTL&quot;? Lo stesso effetto si può osservare qui. Solo che l’evento di invalidazione della cache esterna già eseguito potrebbe non ripetersi più - mai, Questo può espandere l’intervallo di vita dell’oggetto memorizzato nella cache all’infinito.

![Combinato basato su TTL e su eventi: Passaggio all’infinito](assets/chapter-3/infinity.png)

*Combinato basato su TTL e su eventi: Passaggio all’infinito*

<br> 

## Memorizzazione in cache parziale e memorizzazione in cache in memoria

Per aggiungere livelli di memorizzazione in cache, potete collegarvi allo stadio del processo di rendering. Da ottenere oggetti di trasferimento dati remoti o creare oggetti business locali alla memorizzazione in cache del markup di rendering di un singolo componente. Lasceremo implementazioni concrete a un’esercitazione successiva. Ma forse avete già implementato alcuni di questi livelli di memorizzazione in cache. Quindi il minimo che possiamo fare qui è introdurre i principi base - e gotchas.

### Parole di avvertimento

#### Controllo degli accessi

Le tecniche descritte di seguito sono piuttosto potenti e _devono-avere_ nella toolbox di ogni sviluppatore AEM. Ma non emozionatevi troppo, usateli con saggezza. Memorizzare un oggetto in una cache e condividerlo con altri utenti nelle richieste di follow-up significa in realtà aggirare il controllo degli accessi. Solitamente questo non è un problema sui siti web rivolti al pubblico, ma può esserlo quando un utente deve effettuare il login prima di ottenere l&#39;accesso.

È consigliabile memorizzare il markup HTML di un menu principale dei siti in una cache in memoria per condividerlo tra diverse pagine. In realtà questo è un esempio perfetto per memorizzare un elemento HTML parzialmente renderizzato in quanto la creazione di una navigazione è solitamente costosa in quanto richiede l&#39;attraversamento di molte pagine.

Non condividi la stessa struttura di menu tra tutte le pagine, ma anche con tutti gli utenti, il che lo rende ancora più efficiente. Ma aspetta ... ma forse ci sono alcuni elementi nel menu che sono riservati solo a un certo gruppo di utenti. In tal caso, la memorizzazione in cache può diventare un po&#39; più complessa.

#### Solo oggetti aziendali personalizzati della cache

Se c&#39;è - questo è il consiglio più importante, possiamo darvi:

>[!WARNING]
>
>Solo gli oggetti cache che sono tuoi, che sono immutabili, che hai costruito te stesso, che sono superficiali e non hanno riferimenti in uscita.

Cosa significa?

1. Non si conosce il ciclo di vita previsto degli oggetti di altre persone. Considera di avere un riferimento a un oggetto di richiesta e decidere di memorizzarlo nella cache. Ora la richiesta è terminata e il contenitore servlet vuole riciclare l’oggetto per la successiva richiesta in arrivo. In questo caso qualcun altro sta cambiando il contenuto su cui ritenevi di avere un controllo esclusivo. Non ignorarlo - Abbiamo visto qualcosa di simile accadere in un progetto. I clienti visualizzavano altri dati dei clienti invece dei propri.

2. Finché un oggetto è referenziato da una catena di altri riferimenti, non può essere rimosso dall&#39;heap. Se si mantiene un oggetto apparentemente piccolo nella cache che fa riferimento, ad esempio una rappresentazione di 4 MB di un&#39;immagine si avrà una buona probabilità di ottenere problemi con la perdita di memoria. Le pulci dovrebbero essere basate su riferimenti deboli. Ma - i riferimenti deboli non funzionano come ci si potrebbe aspettare. Questo è il modo migliore assoluto per produrre una perdita di memoria e terminare con un errore di memoria esaurita. E - non sapete quali sono le dimensioni della memoria conservata degli oggetti estranei, giusto?

3. Soprattutto in Sling, è possibile adattare (quasi) ogni oggetto l&#39;uno all&#39;altro. Considera di inserire una risorsa nella cache. La richiesta successiva (con diritti di accesso diversi), recupera la risorsa e la adatta in un resourceResolver o una sessione per accedere ad altre risorse a cui non avrebbe accesso.

4. Anche se crei un sottile &quot;wrapper&quot; intorno a una risorsa da AEM, non devi memorizzarlo nella cache - anche se è personale e immutabile. L&#39;oggetto con wrapping sarebbe un riferimento (che vietiamo prima) e se guardiamo in modo netto, questo crea fondamentalmente gli stessi problemi descritti nell&#39;ultimo elemento.

5. Se desideri memorizzare in cache i tuoi oggetti, crea i tuoi oggetti copiando i dati primitivi nei tuoi oggetti shallo. Potrebbe essere utile collegare i propri oggetti tramite riferimenti, ad esempio per memorizzare nella cache una struttura di oggetti. Va bene, ma solo gli oggetti cache appena creati nella stessa richiesta e nessun oggetto richiesto da un altro punto (anche se si tratta dello spazio nome-nome dell’oggetto). _Copiare_ oggetti è la chiave. Assicurati inoltre di eliminare l’intera struttura degli oggetti collegati in una sola volta ed evitare riferimenti in entrata ed in uscita alla struttura.

6. Sì - e mantenere gli oggetti immutabili. Proprietà private, solo e nessun setter.

Sono molte regole, ma vale la pena seguirle. Anche se siete esperti e super intelligenti e avere tutto sotto controllo. Il giovane collega del tuo progetto si è appena diplomato all&#39;università. Non conosce tutte queste insidie. Se non ci sono insidie, non c&#39;è nulla da evitare. Semplificalo e comprensibile.

### Strumenti e librerie

Questa serie tratta di comprendere i concetti e di consentirti di creare un’architettura che si adatti al meglio al tuo caso d’uso.

Non stiamo promuovendo nessuno strumento in particolare. Ma vi darete indicazioni su come valutarle. Ad esempio, AEM dispone di una semplice cache integrata con un TTL fisso a partire dalla versione 6.0. Utilizzarlo? Probabilmente non al momento della pubblicazione in cui segue una cache basata su eventi nella catena (suggerimento: Dispatcher). Ma potrebbe per una scelta decente per un Autore. C&#39;è anche una cache HTTP da Adobe ACS commons che potrebbe essere utile considerare.

Oppure costruisci il tuo, sulla base di un framework di memorizzazione in cache maturo come [Ehcache](https://www.ehcache.org). Può essere utilizzato per memorizzare nella cache gli oggetti Java e per eseguire il rendering del markup (`String` oggetti).

In alcuni casi semplici potresti anche andare d&#39;accordo con l&#39;utilizzo di mappe hash simultanee - vedrai rapidamente i limiti qui - nello strumento o nelle tue abilità. La concorrenza è difficile da padroneggiare come denominazione e memorizzazione in cache.

#### Riferimenti

* [Cache http di ACS Commons  ](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Framework di memorizzazione in cache di Ehcache](https://www.ehcache.org)

### Termini di base

Non entreremo nella teoria della memorizzazione in cache troppo in profondità, ma ci sentiamo obbligati a fornire un paio di parole-buzz, in modo da avere un buon inizio.

#### Rimozione cache

Abbiamo parlato molto dell&#39;invalidazione e dell&#39;epurazione. _L’_ eliminazione della cache è correlata ai seguenti termini: Dopo che una voce viene sgomberata, non è più disponibile. Ma lo sfratto non avviene quando una voce è obsoleta, ma quando la cache è piena. Gli elementi più recenti o più importanti eliminano quelli più vecchi o meno importanti dalla cache. Quali voci dovrete sacrificare è una decisione caso per caso. Potrebbe essere utile rimuovere quelli più vecchi o quelli che sono stati utilizzati molto raramente o per l&#39;ultimo accesso da molto tempo.

#### Memorizzazione in cache preventiva

Memorizzazione in cache preventiva significa ricreare la voce con contenuto fresco nel momento in cui viene invalidata o considerata obsoleta. Naturalmente - lo si farebbe solo con poche risorse, che si sono sicuri che sono frequentemente e immediatamente accessibili. In caso contrario, sprecherebbe risorse per la creazione di voci di cache che potrebbero non essere mai richieste. Creando in anticipo le voci della cache, puoi ridurre la latenza della prima richiesta a una risorsa dopo l’annullamento della validità della cache.

#### Riscaldamento della cache

Il riscaldamento della cache è strettamente correlato al caching preventivo. Anche se non useresti quel termine per un sistema dal vivo. Ed è meno vincolata del tempo rispetto alla prima. Non eseguire la ri-cache immediatamente dopo l’annullamento della validità, ma riempire gradualmente la cache quando il tempo lo consente.

Ad esempio, togli una gamba Pubblica/Dispatcher dal load balancer per aggiornarla. Prima di reintegrarla, esegui automaticamente la ricerca per indicizzazione delle pagine più visitate per inserirle nuovamente nella cache. Quando la cache è &quot;calda&quot; - riempita adeguatamente si reintegra la gamba nel load balancer.

O forse reintegri la gamba in una volta, ma riduci il traffico verso quella gamba in modo che abbia la possibilità di riscaldarsi le cache per uso regolare.

Oppure si desidera anche memorizzare nella cache alcune pagine a cui si accede meno frequentemente nei momenti in cui il sistema è inattivo per ridurre la latenza quando vi si accede effettivamente da richieste reali.

#### Identità oggetto cache, Payload, Dipendenza di annullamento validità e TTL

In generale, un oggetto memorizzato nella cache o una &quot;voce&quot; ha cinque proprietà principali,

#### Chiave

Questa è l&#39;identità della proprietà tramite la quale si identificano e si creano oggetti. Per recuperare il payload o eliminarlo dalla cache. Il dispatcher, ad esempio, utilizza l’URL di una pagina come chiave. Tieni presente che il dispatcher non utilizza i percorsi delle pagine. Questo non è sufficiente per distinguere diversi rendering. Altre cache possono utilizzare chiavi diverse. Vedremo alcuni esempi più tardi.

#### Valore / Payload

Questo è il cassetto del tesoro dell&#39;oggetto, i dati che volete recuperare. Nel caso del dispatcher sono i contenuti dei file. Ma può anche essere una struttura ad albero di oggetti Java.

#### TTL

Abbiamo già coperto il TTL. Il tempo dopo il quale una voce è considerata datata e non deve più essere consegnata.

#### Dipendenza

Ciò riguarda l’invalidazione basata su eventi. A seconda di quali dati originali dipende l&#39;oggetto? Nella parte I, abbiamo già detto, che un tracciamento delle dipendenze vero e preciso è troppo complesso. Ma con la nostra conoscenza del sistema si possono approssimare le dipendenze con un modello più semplice. Invalidiamo un numero sufficiente di oggetti per eliminare il contenuto obsoleto.. e forse inavvertitamente più di quanto sarebbe necessario. Eppure cerchiamo di tenerci sotto &quot;purge tutto&quot;.

Quali oggetti dipendono da ciò che gli altri sono autentici in ogni singola applicazione. Vi forniremo alcuni esempi su come implementare una strategia di dipendenza in un secondo momento.

### Memorizzazione in cache dei frammenti HTML

![Riutilizzo di un frammento di cui è stato effettuato il rendering su pagine diverse](assets/chapter-3/re-using-rendered-fragment.png)

*Riutilizzo di un frammento di cui è stato effettuato il rendering su pagine diverse*

<br> 

La memorizzazione in cache dei frammenti HTML è uno strumento potente. L’idea è quella di memorizzare nella cache il markup HTML generato da un componente in una cache in-memory. Potreste chiedervi, perché dovrei farlo? Sto comunque memorizzando nella cache l&#39;intero markup della pagina nel dispatcher, incluso il markup di quel componente. Siamo d&#39;accordo. Sì, ma una volta per pagina. Non si condivide il markup tra le pagine.

Immagina di eseguire il rendering di una navigazione sopra ogni pagina. Il markup ha lo stesso aspetto su ogni pagina. Tuttavia, lo esegui di nuovo il rendering per ogni pagina, che non si trova in Dispatcher. E ricorda: Dopo l’annullamento automatico della validità, è necessario eseguire nuovamente il rendering di tutte le pagine. In pratica, esegui lo stesso codice con gli stessi risultati centinaia di volte.

Dalla nostra esperienza, il rendering di una navigazione superiore nidificata è un compito molto costoso. Di solito si attraversa una buona parte della struttura del documento per generare gli elementi di navigazione. Anche se hai solo bisogno del titolo di navigazione e dell’URL, le pagine devono essere caricate in memoria. E qui stanno intasando risorse preziose. Ancora e ancora.

Ma il componente è condiviso tra molte pagine. E condividere qualcosa è un segnale usando una cache. Quindi - quello che si desidera fare è verificare se il componente di navigazione è già stato sottoposto a rendering e memorizzato nella cache e invece di ripetere il rendering emette solo il valore della cache.

Ci sono due meravigliose prodezze di questo regime facilmente mancato:

1. Stai memorizzando nella cache una stringa Java. Una stringa non ha riferimenti in uscita ed è immutabile. Quindi, considerando gli avvertimenti sopra - questo è super sicuro.

2. L&#39;annullamento della validità è anche molto semplice. Ogni volta che qualcosa cambia il tuo sito web, vuoi invalidare questa voce della cache. La ricostruzione è relativamente economica, in quanto deve essere eseguita una sola volta e poi viene riutilizzata da tutte le centinaia di pagine.

Questo è un grande sollievo per i server di pubblicazione.

### Implementazione delle cache dei frammenti

#### Tag personalizzati

Ai vecchi tempi, quando si utilizzava JSP come motore di template, era piuttosto comune usare un tag JSP personalizzato che avvolgeva il codice di rendering dei componenti.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

Il tag personalizzato che acquisirebbe il proprio corpo e lo scriverebbe nella cache o impedirebbe l’esecuzione del proprio corpo e restituirebbe invece il payload della voce di cache.

La &quot;Chiave&quot; è il percorso dei componenti che avrebbe sulla home page. Non utilizziamo il percorso del componente nella pagina corrente, in quanto questo creerebbe una voce di cache per pagina - che contraddirebbe la nostra intenzione di condividere quel componente. Inoltre, non utilizziamo solo il percorso relativo dei componenti (`jcr:conten/mainnavigation`), in quanto ciò impedirebbe l’utilizzo di diversi componenti di navigazione in siti diversi.

&quot;Cache&quot; è un indicatore in cui memorizzare la voce. In genere si dispone di più di una cache in cui archiviare gli elementi. Ognuno di questi potrebbe comportarsi in modo un po&#39; diverso. Quindi, è bene differenziare ciò che è memorizzato - anche se alla fine sono solo stringhe.

&quot;Dipendenza&quot; da cui dipende la voce della cache. La cache di &quot;navigazione principale&quot; potrebbe avere una regola che, se si verifica una modifica sotto il nodo &quot;dipendenza&quot;, la voce corrispondente deve essere eliminata. Pertanto, l’implementazione della cache deve registrarsi come listener di eventi nell’archivio per essere consapevole delle modifiche e quindi applicare le regole specifiche della cache per scoprire cosa deve essere invalidato.

Il precedente era solo un esempio. Puoi anche scegliere di avere una struttura ad albero di cache. Quando il primo livello viene utilizzato per separare i siti (o tenant) e il secondo livello, si dirama in tipi di contenuto (ad esempio, &quot;navigazione principale&quot;), che potrebbero risparmiare l’aggiunta del percorso delle home page come nell’esempio precedente.

A proposito, puoi anche utilizzare questo approccio con componenti basati su HTL più moderni. Avreste quindi un wrapper JSP intorno al vostro script HTL.

#### Filtri dei componenti

Ma in un approccio HTL puro, preferisci creare la cache dei frammenti con un filtro componente Sling. Non l&#39;abbiamo ancora visto allo stato brado, ma questo è l&#39;approccio che adotteremmo su questo problema.

#### Sling Dynamic Include

La cache dei frammenti viene utilizzata se si dispone di una costante (navigazione) nel contesto di un ambiente che cambia (pagine diverse).

Ma puoi avere anche l’opposto, un contesto relativamente costante (una pagina che raramente cambia) e alcuni frammenti in continua evoluzione su quella pagina (ad esempio, un ticker live).

In questo caso, puoi dare una possibilità a [Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html) . In sostanza, si tratta di un filtro di componenti, che ruota intorno al componente dinamico e invece di eseguire il rendering del componente nella pagina crea un riferimento. Questo riferimento può essere una chiamata Ajax , in modo che il componente sia incluso dal browser e quindi la pagina circostante possa essere memorizzata nella cache in modo statico. Oppure - in alternativa - Sling Dynamic Include può generare una direttiva SSI (Server Side Include). Questa direttiva verrebbe eseguita nel server Apache. È inoltre possibile utilizzare le direttive ESI - Edge Side Include se si utilizza Varnish o un CDN che supporta gli script ESI.

![Diagramma di sequenza di una richiesta che utilizza Sling Dynamic Include](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagramma di sequenza di una richiesta che utilizza Sling Dynamic Include*

<br> 

La documentazione SDI dice che dovresti disabilitare la memorizzazione in cache per gli URL che terminano con &quot;*.nocache.html&quot;, il che ha senso - mentre hai a che fare con i componenti dinamici.

Si potrebbe vedere un&#39;altra opzione come utilizzare SDI: Se _non_ disattivi la cache del dispatcher per gli include, il Dispatcher agisce come una cache dei frammenti simile a quella descritta nell’ultimo capitolo: Le pagine e i frammenti di componente vengono memorizzati nella cache del dispatcher in modo uniforme e indipendente e uniti dallo script SSI nel server Apache quando la pagina viene richiesta. In questo modo, puoi implementare componenti condivisi come la navigazione principale (dato che utilizzi sempre lo stesso URL del componente).

Questo dovrebbe funzionare - in teoria. Ma...

Consigliamo di non farlo: Perderesti la possibilità di ignorare la cache per i componenti dinamici reali. SDI è configurato a livello globale e le modifiche apportate per la &quot;cache frammento-poveri&quot; si applicano anche ai componenti dinamici.

Ti consigliamo di studiare attentamente la documentazione SDI. Ci sono alcune altre limitazioni, ma SDI è uno strumento prezioso in alcuni casi.

#### Riferimenti

* [docs.oracle.com - Come scrivere tag JSP personalizzati](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Creazione e utilizzo di filtri dei componenti](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Configurazione di Sling Dynamic Includes in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Memorizzazione in cache del modello

![Memorizzazione in cache basata su modello: Un oggetto business con due rendering diversi](assets/chapter-3/model-based-caching.png)

*Memorizzazione in cache basata su modello: Un oggetto business con due rendering diversi*

<br> 

Rivediamo il caso con la navigazione. Supponevamo che ogni pagina richiedesse lo stesso markup della navigazione.

Ma forse non è così. È possibile eseguire il rendering di markup diversi per l&#39;elemento nella navigazione che rappresenta la _pagina corrente_.

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

Si tratta di due rendering completamente diversi. Tuttavia, l&#39; _oggetto business_ - l&#39;albero di navigazione completo - è lo stesso.  L&#39; _oggetto business_ qui è un grafico a oggetti che rappresenta i nodi nella struttura. Questo grafico può essere facilmente memorizzato in una cache in memoria. Ricorda però che questo grafico non deve contenere alcun oggetto o fare riferimento a qualsiasi oggetto che non hai creato tu stesso, specialmente ora nodi JCR.

#### Memorizzazione in cache nel browser

Abbiamo già toccato l&#39;importanza della memorizzazione in cache nel browser, e ci sono molti buoni tutorial là fuori. Alla fine, per il browser, Dispatcher è solo un server web che segue il protocollo HTTP.

Tuttavia - nonostante la teoria - abbiamo raccolto alcune conoscenze che non abbiamo trovato in nessun altro luogo e che vogliamo condividere.

In sostanza, la memorizzazione in cache del browser può essere utilizzata in due modi diversi,

1. Il browser ha una risorsa memorizzata nella cache di cui conosce la data di scadenza esatta. In questo caso non richiede nuovamente la risorsa.

2. Il browser dispone di una risorsa, ma non è sicuro che sia ancora valida. In tal caso lo chiederebbe al server web (nel nostro caso il Dispatcher). Se la risorsa è stata modificata dopo l’ultima consegna, indicami la risorsa. Se non è cambiato, il server risponde con &quot;304 - non modificato&quot; e solo i metadati sono stati trasmessi.

#### Debug

Se stai ottimizzando le impostazioni del Dispatcher per la memorizzazione in cache del browser, è estremamente utile utilizzare un server proxy desktop tra il browser e il server web. Preferiamo &quot;Charles Web Debugging Proxy&quot; di Karl von Randow.

Utilizzando Charles, puoi leggere le richieste e le risposte, che vengono trasmesse a e dal server. E - puoi imparare molto sul protocollo HTTP. Anche i browser moderni offrono alcune funzionalità di debug, ma le funzioni di un proxy desktop sono senza precedenti. Puoi manipolare i dati trasferiti, limitare la trasmissione, riprodurre richieste singole e molto altro. E l&#39;interfaccia utente è chiaramente organizzata e abbastanza completa.

Il test più basilare è quello di utilizzare il sito web come utente normale - con il proxy in mezzo - e controllare il proxy se il numero delle richieste statiche (a /etc/...) sta diventando più piccolo nel tempo - in quanto queste dovrebbero essere nella cache e non devono più essere richieste.

Abbiamo scoperto che un proxy potrebbe fornire una panoramica più chiara, in quanto la richiesta memorizzata nella cache non viene visualizzata nel registro, mentre alcuni debugger integrati nel browser mostrano ancora queste richieste con &quot;0 ms&quot; o &quot;dal disco&quot;. Il che è corretto e preciso, ma potrebbe offuscare un po&#39; la tua visione.

Puoi quindi eseguire il drill-down e controllare le intestazioni dei file trasferiti per vedere, ad esempio, se le intestazioni http &quot;Expires&quot; sono corrette. Puoi riprodurre le richieste con intestazioni if-modified-since impostate per vedere se il server risponde correttamente con un codice di risposta 304 o 200. Puoi osservare la tempistica delle chiamate asincrone e anche testare in un certo grado le tue ipotesi di sicurezza. Ricorda che ti abbiamo detto di non accettare tutti i selettori che non sono esplicitamente previsti? Qui puoi giocare con l&#39;URL e i parametri e vedere se l&#39;applicazione si comporta bene.

Quando esegui il debug della cache, ti chiediamo solo una cosa:

Non ricaricare le pagine nel browser!

Una &quot;ricarica del browser&quot;, una _ricarica semplice_ e una _ricarica forzata_ (&quot;_shift-reload_&quot;) non è la stessa di una normale richiesta di pagina. Una semplice richiesta di ricarica imposta un’intestazione

```
Cache-Control: max-age=0
```

E un Maiusc-Ricarica (tenendo premuto il tasto &quot;Maiusc&quot; mentre fai clic sul pulsante di ricarica) di solito imposta un&#39;intestazione di richiesta

```
Cache-Control: no-cache
```

Entrambe le intestazioni hanno effetti simili ma leggermente diversi - ma soprattutto, sono completamente diverse da una normale richiesta quando apri un URL dallo slot URL o utilizzando i collegamenti sul sito. La navigazione normale non imposta le intestazioni Cache-Control ma probabilmente un&#39;intestazione if-modified-since.

Quindi, se desideri eseguire il debug del normale comportamento di navigazione, devi fare esattamente questo: _Sfoglia normalmente_. L&#39;utilizzo del pulsante di ricarica del browser è il modo migliore per non visualizzare gli errori di configurazione della cache nella configurazione.

Usa il tuo proxy Charles per vedere di cosa stiamo parlando. Sì - e mentre è aperto - è possibile riprodurre le richieste proprio lì. Non è necessario ricaricare dal browser.

## Test delle prestazioni

Utilizzando un proxy, si ottiene un&#39;idea del comportamento del tempo delle pagine. Ovviamente, questo non è di gran lunga un test di performance.  Un test delle prestazioni richiede un certo numero di client che richiedono le pagine in parallelo.

Un errore comune, che abbiamo visto troppo spesso, è che il test delle prestazioni include solo un numero super ridotto di pagine e queste pagine vengono consegnate solo dalla cache del Dispatcher.

Se si promuove l&#39;applicazione al sistema live, il carico è completamente diverso da quello testato.

Sul sistema live, il pattern di accesso non è costituito da un numero limitato di pagine distribuite equamente utilizzate nei test (home page e poche pagine di contenuto). Il numero di pagine è molto maggiore e le richieste sono distribuite in modo molto diseguale. E, naturalmente, le pagine live non possono essere servite al 100% dalla cache: Alcune richieste di annullamento della validità provenienti dal sistema di pubblicazione annullano automaticamente la validità di una parte considerevole delle risorse preziose.

Ah sì - e quando si sta ricostruendo la cache del Dispatcher, si scoprirà, che anche il sistema di pubblicazione si comporta in modo abbastanza diverso, a seconda che si richiede solo una manciata di pagine - o un numero più grande. Anche se tutte le pagine sono altrettanto complesse, il loro numero gioca un ruolo. Ricordate cosa abbiamo detto sulla memorizzazione in cache in catena? Se richiedi sempre lo stesso numero ridotto di pagine, è probabile che i blocchi corrispondenti con i dati non elaborati si trovino nella cache dei dischi rigidi o che i blocchi siano memorizzati nella cache dal sistema operativo. Inoltre, c&#39;è una buona probabilità che il Repository abbia memorizzato nella cache il segmento corrispondente nella sua memoria principale. Il rendering è quindi molto più veloce rispetto a quando altre pagine si evitano l&#39;un l&#39;altra ora e poi da varie cache.

La memorizzazione in cache è difficile e lo stesso vale per il test di un sistema basato sulla memorizzazione in cache. Quindi, cosa si può fare per avere uno scenario reale più accurato?

Pensiamo che dovresti eseguire più di un test, e dovresti fornire più di un indice di prestazioni come misura della qualità della tua soluzione.

Se disponi già di un sito web esistente, misura il numero di richieste e la relativa modalità di distribuzione. Prova a modellare un test che utilizza una distribuzione simile di richieste. L&#39;aggiunta di un po&#39; di casualità non poteva ferire. Non è necessario simulare un browser che caricherebbe risorse statiche come JS e CSS - quelle non contano davvero. Vengono quindi memorizzati nella cache del browser o del Dispatcher e non aumentano in modo significativo il carico. Ma le immagini a cui si fa riferimento sono importanti. Trovare la loro distribuzione anche in vecchi file di log e modellare un modello di richiesta simile.

Ora esegui un test con il tuo Dispatcher senza memorizzazione in cache. Questo è lo scenario peggiore. Scoprite a quale carico massimo il vostro sistema sta diventando instabile in queste peggiori condizioni. Puoi anche peggiorare la situazione rimuovendo alcune gambe Dispatcher/Publish se lo desideri.

Quindi, esegui lo stesso test con tutte le impostazioni di cache necessarie su &quot;on&quot;. Aumenta lentamente le richieste parallele per scaldare la cache e vedere quanto il sistema può prendere in queste migliori condizioni.

Uno scenario di caso medio consiste nell’eseguire il test con Dispatcher abilitato, ma anche con alcuni invalidamenti in corso. Puoi simulare questo problema toccando gli statfile con un cronjob o inviando le richieste di annullamento della validità a intervalli irregolari al Dispatcher. Non dimenticare di eliminare anche alcune delle risorse invalidate non automatiche di tanto in tanto.

È possibile variare l’ultimo scenario aumentando le richieste di annullamento della validità e aumentando il carico.

È un po&#39; più complesso di un semplice test di carico lineare, ma offre molta più fiducia nella soluzione.

Potresti allontanarti dallo sforzo. Tuttavia, per non parlare dei limiti del sistema, effettua un test nel peggiore dei casi sul sistema Publish con un numero maggiore di pagine (ugualmente distribuite). Assicurati di interpretare correttamente il numero dello scenario migliore e di fornire ai sistemi un&#39;headroom sufficiente.