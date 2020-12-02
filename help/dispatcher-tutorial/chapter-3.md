---
title: Capitolo 3 - Argomenti avanzati di memorizzazione nella cache
seo-title: AEM cache del dispatcher demistificato - Capitolo 3 - Argomenti di cache avanzati
description: Il capitolo 3 dell'esercitazione AEM Dispatcher Cache Demystified illustra come superare i limiti di cui al capitolo 2.
seo-description: Il capitolo 3 dell'esercitazione AEM Dispatcher Cache Demystified illustra come superare i limiti di cui al capitolo 2.
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '6187'
ht-degree: 0%

---


# Capitolo 3 - Argomenti avanzati di memorizzazione nella cache

*&quot;Ci sono solo due cose difficili in Informatica Scienza: invalidare la cache e denominare gli elementi.&quot;*

— PHIL KARLTON

## Panoramica

Questa è la parte 3 di una serie di tre parti da memorizzare nella cache in AEM. Dove le prime due parti si concentravano sulla semplice cache http nel Dispatcher e quali limitazioni ci sono. Questa parte illustra alcune idee su come superare questi limiti.

## Caching in generale

[Il capitolo 1 ](chapter-1.md) e il  [capitolo 2 ](chapter-2.md) di questa serie si sono concentrati principalmente sul Dispatcher. Abbiamo spiegato le basi, i limiti e dove è necessario fare determinate scelte.

La complessità della cache e le complessità non sono problemi unici per il dispatcher. Il caching è difficile in generale.

Avere il Dispatcher come unico strumento nella cassetta degli strumenti sarebbe in realtà un limite reale.

In questo capitolo vogliamo ampliare il nostro punto di vista sulla memorizzazione nella cache e sviluppare alcune idee su come superare alcune delle carenze del Dispatcher. Non c&#39;è un proiettile d&#39;argento - si dovrà fare compromessi nel tuo progetto. Ricordate che con la memorizzazione nella cache e l&#39;invalidazione dell&#39;accuratezza sempre arriva la complessità, e con la complessità c&#39;è la possibilità di errori.

Dovrà fare compromessi in questi settori,

* Prestazioni e latenza
* Consumo di risorse / Caricamento CPU / Utilizzo disco
* Precisione / Valuta / Stabilità / Sicurezza
* Semplicità / Complessità / Costo / Manutenzione / Errore-prontezza

Queste dimensioni sono collegate in un sistema piuttosto complesso. Non c&#39;è un semplice se-allora-allora-quello. Semplificando il sistema è possibile velocizzare o rallentare l&#39;attività. Può ridurre i costi di sviluppo, ma aumentare i costi dell&#39;helpdesk, ad esempio se i clienti visualizzano contenuti scadenti o se si lamentano di un sito Web lento. Tutti questi fattori devono essere considerati ed equilibrati l&#39;uno rispetto all&#39;altro. Ma ormai dovreste già avere una buona idea, che non ci sia una pallottola d&#39;argento o una singola &quot;best practice&quot; - solo un sacco di cattive pratiche e qualche buona.

## Cache

### Panoramica

#### Flusso di dati

La distribuzione di una pagina da un server al browser del cliente attraversa una serie di sistemi e sottosistemi. Se si guarda attentamente, c&#39;è un certo numero di dati di luppolo da prendere dalla fonte al drenaggio, ciascuno dei quali è un potenziale candidato per il caching.

![Flusso di dati di una tipica applicazione CMS](assets/chapter-3/data-flow-typical-cms-app.png)

*Flusso di dati di una tipica applicazione CMS*

<br> 

Cominciamo il nostro viaggio con un pezzo di dati che si trova su un disco rigido e che deve essere visualizzato in un browser.

#### Hardware e sistema operativo

In primo luogo, l&#39;hard disk (HDD) stesso dispone di una cache integrata nell&#39;hardware. In secondo luogo, il sistema operativo che monta il disco rigido, utilizza la memoria libera per memorizzare nella cache i blocchi a cui si accede di frequente per velocizzare l&#39;accesso.

#### Archivio dei contenuti

Il livello successivo è il CRX o Oak, il database del documento utilizzato da AEM. CRX e Oak dividono i dati in segmenti che possono essere memorizzati nella cache, così come per evitare un accesso più lento all&#39;HDD.

#### Dati di terze parti

La maggior parte delle installazioni Web più grandi hanno anche dati di terze parti; i dati provenienti da un sistema di informazioni sui prodotti, un sistema di gestione delle relazioni con i clienti, un database legacy o qualsiasi altro servizio Web arbitrario. Questi dati non devono essere estratti dall&#39;origine ogni volta che è necessario - soprattutto non, quando è noto che si modifica non troppo spesso. Pertanto, può essere memorizzato nella cache, se non è sincronizzato nel database CRX.

#### Livello business - App / Modello

Solitamente gli script di modello non eseguono il rendering del contenuto non elaborato proveniente da CRX tramite l&#39;API JCR. È probabile che si disponga di un livello aziendale tra l&#39;unione, il calcolo e/o la trasformazione dei dati in un oggetto dominio aziendale. Indovinate cosa - se queste operazioni sono costose, si dovrebbe prendere in considerazione la memorizzazione nella cache.

#### Frammenti di marcatura

Il modello ora è la base per il rendering della marcatura per un componente. Perché non memorizzare nella cache anche il modello di cui è stato effettuato il rendering?

#### Dispatcher, CDN e altri proxy

Disattivato passa la pagina HTML rappresentata al dispatcher. Abbiamo già discusso del fatto che lo scopo principale del Dispatcher è quello di memorizzare nella cache le pagine HTML e altre risorse Web (nonostante il suo nome). Prima che le risorse raggiungano il browser, può passare un proxy inverso, che può memorizzare nella cache e un CDN, che viene utilizzato anche per il caching. Il client può stare in un ufficio, che concede l&#39;accesso Web solo tramite un proxy - e che il proxy potrebbe decidere di memorizzare anche la cache per risparmiare traffico.

#### Cache browser

Ultimo ma non meno importante: anche il browser cache. Si tratta di una risorsa facilmente trascurata. Ma è la cache più vicina e veloce che si ha nella catena di memorizzazione nella cache. Sfortunatamente, non è condiviso tra utenti, ma tra diverse richieste di un utente.

### Dove memorizzare nella cache e perché

Questa è una lunga catena di cache potenziali. E abbiamo tutti affrontato problemi in cui abbiamo visto contenuti obsoleti. Ma tenendo conto di quanti stadi ci sono, è un miracolo che la maggior parte del tempo stia funzionando.

Ma in quale catena ha senso cache? All&#39;inizio? Alla fine? Ovunque? Dipende... e dipende da un gran numero di fattori. Anche due risorse nello stesso sito web potrebbero desiderare una risposta diversa a quella domanda.

Per darvi un&#39;idea approssimativa dei fattori che potreste prendere in considerazione,

**Tempo di vita** : se gli oggetti presentano un breve tempo di vita intrinseco (i dati del traffico potrebbero avere una durata inferiore a quella dei dati meteorologici), potrebbe non essere utile memorizzarli nella cache.

**Costo di produzione -** Il costo (in termini di cicli di CPU e I/O) della riproduzione e della consegna di un oggetto. Se è economico caching potrebbe non essere necessario.

**Dimensioni** : per gli oggetti di grandi dimensioni è necessario memorizzare nella cache più risorse. Questo potrebbe essere un fattore limitante e deve essere controbilanciato rispetto al beneficio.

**Frequenza**  di accesso: se l&#39;accesso agli oggetti è raro, il caching potrebbe non essere efficace. Non avrebbero potuto essere superati o invalidati prima di poter accedere la seconda volta dalla cache. Tali elementi bloccherebbero semplicemente le risorse di memoria.

**Accesso**  condiviso: i dati utilizzati da più entità devono essere memorizzati nella cache verso l&#39;alto lungo la catena. In realtà, la catena di caching non è una catena, ma un albero. Un dato contenuto nel repository può essere utilizzato da più modelli. Questi modelli possono essere utilizzati a loro volta da più script di rendering per generare frammenti HTML. Questi frammenti sono inclusi in più pagine distribuite a più utenti con le rispettive cache private nel browser. Quindi &quot;condividere&quot; non significa solo condividere tra le persone, ma tra i vari software. Se vuoi trovare una potenziale cache &quot;condivisa&quot;, puoi semplicemente rintracciare la struttura ad albero alla radice e trovare un antenato comune, che è il punto in cui dovresti memorizzare nella cache.

**Distribuzione**  geospaziale - Se gli utenti sono distribuiti in tutto il mondo, l&#39;utilizzo di una rete distribuita di cache potrebbe contribuire a ridurre la latenza.

**Larghezza di banda e latenza**  della rete - Parlando di latenza, chi sono i clienti e che tipo di rete utilizzano? Forse i vostri clienti sono clienti mobili in un paese sottosviluppato utilizzando la connessione 3G degli smartphone di vecchia generazione? Prendete in considerazione la creazione di oggetti più piccoli e memorizzateli nella cache del browser.

Questa lista di gran lunga non è completa, ma pensiamo che l&#39;idea sia già arrivata.

### Regole di base per la memorizzazione nella cache concatenata

Anche in questo caso, la memorizzazione nella cache è difficile. Condividiamo alcune regole di base, che abbiamo tradotto da progetti precedenti che possono aiutarti ad evitare problemi nel tuo progetto.

#### Evitare la doppia memorizzazione

Ciascuno dei livelli introdotti nell’ultimo capitolo fornisce un valore nella catena di memorizzazione nella cache. Salvando i cicli informatici o avvicinando i dati al consumatore. Non è sbagliato memorizzare un dato nella cache in più fasi della catena, ma è sempre necessario considerare quali siano i vantaggi e i costi della fase successiva. La memorizzazione nella cache di una pagina completa nel sistema di pubblicazione in genere non offre alcun vantaggio, come avviene già nel dispatcher.

#### Mixaggio delle strategie di annullamento della convalida

Esistono tre strategie di annullamento della validità:

* **TTL, Time to Live:** un oggetto scade dopo un periodo di tempo fisso (ad esempio, &quot;Tra due ore&quot;)
* **Data di scadenza:** l&#39;oggetto scade all&#39;ora definita nel futuro (ad esempio, &quot;10 giugno 2019, 5:00 PM&quot;)
* **Basato su eventi:** l&#39;oggetto viene invalidato esplicitamente da un evento verificatosi nella piattaforma (ad esempio, quando una pagina viene modificata e attivata)

Ora potete usare diverse strategie su diversi livelli di cache, ma ce ne sono alcuni &quot;tossici&quot;.

#### Annullamento Basato Su Evento

![Annullamento della convalida in base agli eventi puri](assets/chapter-3/event-based-invalidation.png)

*Annullamento della convalida in base agli eventi puri: Annulla validità dalla cache interna al livello esterno*

<br> 

L&#39;invalidazione pura basata sugli eventi è la più semplice da comprendere, la più semplice da ottenere teoricamente corretta e la più accurata.

In altre parole, le cache vengono annullate una per una dopo che l&#39;oggetto è stato modificato.

È sufficiente tenere presente una regola:

Annulla sempre la validità dall&#39;interno alla cache esterna. Se prima avete annullato una cache esterna, potrebbe ricreare il contenuto non aggiornato da una cache interna. Non fare supposizioni a che ora una cache è di nuovo fresca - assicurarsi. Meglio, attivando l&#39;annullamento della validità della cache esterna _dopo_ invalidando quella interna.

Questa è la teoria. Ma in pratica ci sono molti gotcha. Gli eventi devono essere distribuiti - potenzialmente su una rete. In pratica, ciò rende il più difficile sistema di annullamento della validità da attuare.

#### Automatico - Correzione

Con l&#39;annullamento della validità in base all&#39;evento, è necessario disporre di un piano di emergenza. Cosa succede se si perde un evento di annullamento della validità? Una strategia semplice potrebbe essere quella di invalidare o rimuovere dopo un certo periodo di tempo. Quindi, potreste aver perso quell&#39;evento e ora servite contenuti scadenti. Ma i vostri oggetti hanno anche un TTL implicito di diverse ore (giorni) solo. Quindi alla fine il sistema si auto-guarisce.

#### Invalidazione pura basata su TTL

![Annullamento non sincronizzato basato su TTL](assets/chapter-3/ttl-based-invalidation.png)

*Annullamento non sincronizzato basato su TTL*

<br> 

Anche questo è uno schema abbastanza comune. Potete sovrapporre diversi livelli di cache, ciascuno dei quali ha diritto a servire un oggetto per un certo periodo di tempo.

È facile da implementare. Sfortunatamente, è difficile prevedere la durata effettiva di un dato.

![Possibilità di prolungare il ciclo di vita di un oggetto interno](assets/chapter-3/outer-cache.png)

*Cache esterna che prolunga la durata di un oggetto interno*

<br> 

Considerate l&#39;illustrazione qui sopra. Ogni livello di caching introduce un TTL di 2 min. Ora - il TTL complessivo deve anche 2 minuti, giusto? Non proprio. Se lo strato esterno recupera l’oggetto prima che diventi obsoleto, in realtà lo strato esterno allunga il tempo di vita effettivo dell’oggetto. Il tempo effettivo può essere compreso tra 2 e 4 minuti. Considerate di essere d&#39;accordo con il vostro reparto aziendale, un giorno è tollerabile - e avete quattro livelli di cache. Il TTL effettivo su ciascun livello non deve superare le sei ore... aumentando la frequenza di mancati riscontri nella cache...

Non stiamo dicendo che sia un cattivo schema. Dovresti solo conoscerne i limiti. Ed è una strategia semplice e piacevole da iniziare. Solo se il traffico del sito aumenta, potresti considerare una strategia più precisa.

*Sincronizzazione dell&#39;ora di annullamento della convalida impostando una data specifica*

#### Annullamento In Base Alla Data Di Scadenza

Se si imposta una data specifica sull&#39;oggetto interno e la si propaga nelle cache esterne, si ottiene un tempo effettivo più prevedibile.

![Sincronizzazione delle date di scadenza](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronizzazione delle date di scadenza*

<br> 

Tuttavia, non tutte le cache sono in grado di estendere le date. E può diventare brutto, quando la cache esterna aggrega due oggetti interni con date di scadenza diverse.

#### Invalidazione basata su eventi e TTL

![Combinazione di strategie basate su eventi e TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Combinazione di strategie basate su eventi e TTL*

<br> 

Inoltre, uno schema comune nel mondo AEM è quello di utilizzare l&#39;annullamento della validità basato su eventi nelle cache interne (ad esempio, nelle cache in memoria dove gli eventi possono essere elaborati in tempo quasi reale) e le cache basate su TTL all&#39;esterno, dove forse non si ha accesso all&#39;annullamento della validità esplicito.

Nel AEM mondo è disponibile una cache in memoria per gli oggetti aziendali e i frammenti HTML nei sistemi di pubblicazione, che viene annullata, quando le risorse sottostanti cambiano e si propaga l&#39;evento di modifica al dispatcher che funziona anche in base agli eventi. Di fronte a questo, ad esempio, è disponibile una CDN basata su TTL.

Avere uno strato di cache (breve) basata su TTL davanti a un Dispatcher potrebbe effettivamente ammorbidire un picco che in genere si verifica dopo un&#39;annullamento automatico della validità.

#### Mixaggio TTL e annullamento della validità in base agli eventi

![Mixaggio TTL e annullamento della validità in base agli eventi](assets/chapter-3/toxic.png)

*Tossico: Mixaggio TTL e annullamento della validità in base agli eventi*

<br> 

Questa combinazione è tossica. Non inserire mai e non inserire mai cache basata su eventi dopo che un TTL o una cache basata su scadenza è stata memorizzata nella cache. Ricordate l&#39;effetto di ricaduta che abbiamo avuto nella strategia &quot;pure-TTL&quot;? Lo stesso effetto si può osservare in questo caso. Solo che l&#39;evento di invalidazione della cache esterna è già accaduto potrebbe non ripetersi mai più. Questo può espandere la durata dell&#39;oggetto memorizzato nella cache all&#39;infinito.

![Combinato basato su TTL e basato su eventi: Spill-over all&#39;infinito](assets/chapter-3/infinity.png)

*Combinato basato su TTL e basato su eventi: Spill-over all&#39;infinito*

<br> 

## Memorizzazione parziale nella cache e memorizzazione nella cache

Potete collegarvi allo stadio del processo di rendering per aggiungere livelli di cache. Da ottenere oggetti di trasferimento dati remoti o la creazione di oggetti aziendali locali alla memorizzazione nella cache del markup rappresentato da un singolo componente. In seguito lasceremo implementazioni concrete a un&#39;esercitazione. Ma forse avete già implementato alcuni di questi livelli di cache. Quindi il minimo che possiamo fare qui è introdurre i principi base - e gotchas.

### Parole di avviso

#### Rispetto del controllo degli accessi

Le tecniche descritte qui sono piuttosto potenti e un _must-have_ nella casella degli strumenti di ogni AEM sviluppatore. Ma non emozionatevi troppo, utilizzateli con saggezza. Memorizzando un oggetto nella cache e condividendolo con altri utenti in richieste di follow-up, si evita di controllare l&#39;accesso. Solitamente non si tratta di un problema relativo ai siti Web destinati al pubblico, ma può verificarsi quando un utente deve effettuare il login prima di poter accedere.

È consigliabile memorizzare il codice HTML di un menu principale dei siti in una cache in memoria per condividerlo tra le varie pagine. In realtà questo è un esempio perfetto per memorizzare HTML parzialmente renderizzati come creare una navigazione è in genere costoso, in quanto richiede l&#39;attraversamento di molte pagine.

Non state condividendo la stessa struttura di menu tra tutte le pagine, ma anche con tutti gli utenti, il che lo rende ancora più efficiente. Ma aspetta... ma forse ci sono alcuni elementi nel menu che sono riservati solo a un determinato gruppo di utenti. In questo caso, la memorizzazione nella cache può diventare un po&#39; più complessa.

#### Solo oggetti aziendali personalizzati della cache

Se ci sono - questo è il consiglio più importante, possiamo darvi:

>[!WARNING]
>
>Memorizza nella cache solo gli oggetti che sono tuoi, che sono immutabili, che hai costruito tu stesso, che sono superficiali e non hanno riferimenti in uscita.

Che cosa significa?

1. Non si conosce il ciclo di vita previsto degli oggetti di altre persone. Considerate l&#39;opportunità di visualizzare un riferimento a un oggetto di richiesta e decidere di memorizzarlo nella cache. Ora, la richiesta è terminata e il servlet container vuole riciclare l&#39;oggetto per la richiesta successiva. In questo caso qualcun altro sta modificando il contenuto su cui ritenevate di avere il controllo esclusivo. Non ignorarlo - abbiamo visto qualcosa di simile accadere in un progetto. Il cliente visualizzava i dati di altri clienti invece che i propri.

2. Fintanto che a un oggetto viene fatto riferimento da una catena di altri riferimenti, non è possibile rimuoverlo dall&#39;heap. Se nella cache si conserva un oggetto apparentemente piccolo a cui si fa riferimento, ad esempio una rappresentazione di 4 MB di un&#39;immagine si avrà una buona probabilità di causare problemi con la perdita di memoria. Le pullman dovrebbero essere basate su riferimenti deboli. Ma - i riferimenti deboli non funzionano come potreste aspettarvi. Questo è il modo migliore per produrre una perdita di memoria e terminare con un errore di memoria insufficiente. E - non sapete quanto sia grande la memoria conservata degli oggetti stranieri, giusto?

3. In particolare in Sling, è possibile adattare (quasi) ogni oggetto l&#39;uno all&#39;altro. Considerate l’inserimento di una risorsa nella cache. La richiesta successiva (con diritti di accesso diversi), recupera la risorsa e la adatta a un resourceResolver o a una sessione per accedere ad altre risorse a cui non avrebbe accesso.

4. Anche se si crea un sottile &quot;wrapper&quot; intorno a una risorsa da AEM, non è necessario memorizzarlo nella cache, anche se è il proprio e immutabile. L&#39;oggetto con wrapper sarebbe un riferimento (che non consentiamo prima) e se guardiamo in modo nitido, in pratica crea gli stessi problemi descritti nell&#39;ultimo elemento.

5. Se si desidera memorizzare nella cache gli oggetti, è necessario creare i propri oggetti copiando i dati di base nei propri oggetti shallo. Potrebbe essere utile collegare i propri oggetti tramite riferimenti, ad esempio potrebbe essere necessario memorizzare nella cache una struttura di oggetti. Questo va bene, ma solo gli oggetti cache appena creati nella stessa richiesta, e nessun oggetto richiesto da un&#39;altra parte (anche se si tratta dello spazio nome dell&#39;oggetto &#39;vostro&#39;). _Copia_ degli oggetti: la chiave. Inoltre, accertarsi di rimuovere l&#39;intera struttura di oggetti collegati contemporaneamente ed evitare riferimenti in entrata e in uscita alla struttura.

6. Sì - e mantenere gli oggetti immutabili. Proprietà private, solo e nessun setter.

Sono molte regole, ma vale la pena seguirle. Anche se siete esperti e super intelligenti e avere tutto sotto controllo. Il giovane collega del tuo progetto si è appena laureato all&#39;università. Lui non conosce tutte queste insidie. Se non ci sono trappole, non c&#39;è nulla da evitare. Semplificate e comprensibili.

### Strumenti e librerie

Questa serie riguarda la comprensione dei concetti e la possibilità di creare un&#39;architettura che si adatti al meglio al caso d&#39;uso.

In particolare, non stiamo promuovendo alcuno strumento. Ma date suggerimenti su come valutarli. Ad esempio, AEM ha una semplice cache integrata con un TTL fisso dalla versione 6.0. Lo usi? Probabilmente non è in fase di pubblicazione, se nella catena segue una cache basata su eventi (suggerimento: Dispatcher). Ma potrebbe per una scelta decente per un Autore. Esiste anche una cache HTTP  Adobe ACS commons che potrebbe essere utile considerare.

In alternativa, potete creare un framework di cache maturo come [Ehcache](https://www.ehcache.org). Può essere utilizzato per memorizzare nella cache gli oggetti Java e per eseguire il rendering dei tag (`String` oggetti).

In alcuni casi semplici si potrebbe anche andare avanti con l&#39;uso di hash map simultanee - vedrete velocemente i limiti qui - sia nello strumento che nelle vostre abilità. La concorrenza è difficile da gestire quanto la denominazione e la memorizzazione nella cache.

#### Riferimenti

* [Cache ACS Commons http  ](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Framework di memorizzazione cache Ehcache](https://www.ehcache.org)

### Termini di base

Non entreremo nella teoria della memorizzazione nella cache troppo in profondità, ma ci sentiamo obbligati a fornire qualche segnale, in modo da avere un buon inizio.

#### Rimozione cache

Abbiamo parlato molto di annullamento della validità e di epurazione. _L’eliminazione_ dalla cache è correlata ai seguenti termini: Dopo che una voce viene sgomberata, non è più disponibile. Ma lo sfratto non avviene quando una voce è obsoleta, ma quando la cache è piena. Elementi più recenti o &quot;più importanti&quot; eliminano dalla cache quelli più vecchi o meno importanti. Le voci da sacrificare sono una decisione caso per caso. Potreste voler sfrattare quelli più vecchi o quelli che sono stati utilizzati molto raramente o per l&#39;ultimo accesso da molto tempo.

#### Memorizzazione nella cache preventiva

La memorizzazione preventiva nella cache significa ricreare la voce con contenuto fresco nel momento in cui viene invalidata o considerata obsoleta. Naturalmente - lo fareste solo con poche risorse, che siete sicuri di essere visitati frequentemente e immediatamente. In caso contrario, si sprecano risorse per la creazione di voci della cache che potrebbero non essere mai richieste. Creando le voci della cache in anticipo, potreste ridurre la latenza della prima richiesta a una risorsa dopo l&#39;annullamento della validità della cache.

#### Riscaldamento cache

Il riscaldamento della cache è strettamente correlato al caching preventivo. Anche se non usereste quel termine per un sistema live. Ed è meno limitato del tempo rispetto al primo. Dopo l’annullamento della validità della cache, non si esegue la nuova cache, ma si riempie gradualmente la cache quando il tempo lo consente.

Ad esempio, estraete una gamba Pubblica/Dispatcher dal sistema di bilanciamento del carico per aggiornarla. Prima di reintegrarlo, è possibile eseguire automaticamente la ricerca per indicizzazione delle pagine a cui si accede più di frequente per inserirle nuovamente nella cache. Quando la cache è &quot;calda&quot; - riempita adeguatamente si reintegra la gamba nel sistema di bilanciamento del carico.

O forse si reintegra subito la zampa, ma si grava il traffico verso quella gamba in modo che abbia la possibilità di riscaldarle è caching per uso regolare.

Oppure è possibile memorizzare nella cache alcune pagine a cui si accede meno frequentemente nei periodi in cui il sistema è inattivo per ridurre la latenza quando l&#39;accesso è effettivamente effettuato da richieste reali.

#### Identità oggetto cache, payload, dipendenze di annullamento validità e TTL

In genere, un oggetto memorizzato nella cache o una &quot;voce&quot; ha cinque proprietà principali,

#### Chiave

Questa è l&#39;identità della proprietà tramite la quale si identifica e si crea l&#39;oggetto. Per recuperarne il payload o per eliminarlo dalla cache. Ad esempio, il dispatcher utilizza l’URL di una pagina come chiave. Il dispatcher non utilizza i percorsi delle pagine. Questo non è sufficiente per distinguere diversi rendering. Altre cache possono utilizzare chiavi diverse. Vedremo alcuni esempi più tardi.

#### Valore/Payload

Questo è il tesoro dell&#39;oggetto, i dati che si desidera recuperare. Nel caso del dispatcher sono i contenuti dei file. Ma può anche essere una struttura ad albero di oggetti Java.

#### TTL

Abbiamo già coperto il TTL. L&#39;ora dopo la quale una voce è considerata stantia e non deve più essere consegnata.

#### Dipendenza

Ciò si riferisce all&#39;annullamento della validità in base agli eventi. Quali dati originali dipende da quell&#39;oggetto? Nella Parte I, abbiamo già detto, che un tracciamento delle dipendenze vero e preciso è troppo complesso. Ma con la nostra conoscenza del sistema si possono approssimare le dipendenze con un modello più semplice. Invalidiamo un numero sufficiente di oggetti per eliminare contenuti non aggiornati... e forse inavvertitamente più di quanto sarebbe richiesto. Ma cerchiamo di mantenere sotto &quot;purge tutto&quot;.

Quali oggetti dipendono da ciò che gli altri sono autentici in ciascuna applicazione. Verranno forniti alcuni esempi su come implementare una strategia di dipendenza in un secondo momento.

### Memorizzazione nella cache dei frammenti HTML

![Riutilizzo di un frammento di cui è stato effettuato il rendering su pagine diverse](assets/chapter-3/re-using-rendered-fragment.png)

*Riutilizzo di un frammento di cui è stato effettuato il rendering su pagine diverse*

<br> 

La cache dei frammenti HTML è uno strumento potente. L’idea è quella di memorizzare nella cache il codice HTML generato da un componente in una cache in memoria. Potreste chiedervi, perché dovrei farlo? Il codice di markup dell&#39;intera pagina viene comunque memorizzato nella cache nel dispatcher, inclusa la marcatura del componente. Siamo d&#39;accordo. Sì, ma una volta per pagina. Non si sta condividendo tale markup tra le pagine.

Immaginate di eseguire il rendering di una navigazione sopra ogni pagina. La marcatura ha lo stesso aspetto su ogni pagina. Tuttavia, si esegue il rendering più e più volte per ogni pagina, che non è nel Dispatcher. E ricorda: Dopo l’annullamento della validità automatica, è necessario ripetere il rendering di tutte le pagine. In sostanza, state eseguendo lo stesso codice con gli stessi risultati centinaia di volte.

Dalla nostra esperienza, il rendering di una navigazione superiore nidificata è un compito molto costoso. In genere si passa attraverso una buona parte della struttura del documento per generare gli elementi di navigazione. Anche se è necessario solo il titolo di navigazione e l&#39;URL, le pagine devono essere caricate in memoria. E qui stanno intasando risorse preziose. Ancora e ancora.

Tuttavia, il componente è condiviso tra più pagine. E condividere qualcosa è un segnale usando una cache. Quindi - quello che si desidera fare è verificare se il componente di navigazione è già stato rappresentato e memorizzato nella cache e invece di ripetere il rendering emette solo il valore della cache.

Ci sono due meravigliose piacevolezze di quel regime facilmente mancato:

1. Si sta memorizzando nella cache una stringa Java. Una stringa non ha riferimenti in uscita ed è immutabile. Quindi, considerando gli avvertimenti sopra - questo è super sicuro.

2. L&#39;annullamento della validità è anche super facile. Ogni volta che cambia il sito Web, si desidera annullare la voce della cache. La ricostruzione è relativamente economica, in quanto deve essere eseguita solo una volta e poi viene riutilizzata da tutte le centinaia di pagine.

Si tratta di un notevole miglioramento per i server di pubblicazione.

### Implementazione delle cache dei frammenti

#### Tag personalizzati

Nei vecchi tempi, quando si utilizzava JSP come motore di modellazione, era abbastanza comune utilizzare un tag JSP personalizzato intorno al codice di rendering dei componenti.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

Il tag personalizzato che acquisirebbe il corpo e lo scriverebbe nella cache o ne impedirebbe l&#39;esecuzione del corpo e restituirebbe il payload della voce della cache.

La &quot;Chiave&quot; è il percorso dei componenti che avrebbe sulla home page. Non utilizziamo il percorso del componente sulla pagina corrente, in quanto questo creerebbe una voce cache per pagina, in contrasto con la nostra intenzione di condividere il componente. Inoltre, non utilizziamo solo il percorso relativo dei componenti (`jcr:conten/mainnavigation`), in quanto ciò impedirebbe l&#39;utilizzo di componenti di navigazione diversi in siti diversi.

&quot;Cache&quot; è un indicatore in cui memorizzare la voce. In genere si dispone di più cache in cui vengono archiviati gli elementi. Ognuno dei quali potrebbe comportarsi un po&#39; diverso. Quindi, è bene differenziare ciò che è memorizzato - anche se alla fine sono solo stringhe.

&quot;Dipendenza&quot;: questo è l&#39;elemento da cui dipende la voce della cache. La cache di &quot;navigazione principale&quot; potrebbe avere una regola, che in caso di modifiche sotto il nodo &quot;dipendenza&quot;, la voce corrispondente deve essere eliminata. Pertanto, l&#39;implementazione della cache dovrebbe registrarsi come listener di eventi nell&#39;archivio per essere a conoscenza delle modifiche e quindi applicare le regole specifiche della cache per scoprire cosa deve essere invalidato.

Quanto sopra era solo un esempio. Potete anche scegliere di avere un albero di cache. Dove il primo livello viene utilizzato per separare i siti (o locatari) e il secondo livello, quindi si suddivide in tipi di contenuto (ad esempio, &quot;navigazione principale&quot;), che potrebbero risparmiare l&#39;aggiunta del percorso delle home page come nell&#39;esempio precedente.

A proposito, potete anche usare questo approccio con componenti basati su HTL più moderni. Avreste quindi un wrapper JSP intorno allo script HTL.

#### Filtri per componenti

Tuttavia, con un approccio HTL puro, è preferibile creare la cache dei frammenti con un filtro componente Sling. Non l&#39;abbiamo ancora visto in natura, ma questo è l&#39;approccio che adotteremmo sulla questione.

#### Sling Dynamic Include

La cache dei frammenti viene utilizzata se si dispone di qualcosa di costante (navigazione) nel contesto di un ambiente in continuo cambiamento (pagine diverse).

Tuttavia, è possibile che si verifichi anche l&#39;opposto, un contesto relativamente costante (una pagina che raramente cambia) e alcuni frammenti in continua evoluzione su quella pagina (ad esempio, un ticker attivo).

In questo caso, è possibile assegnare a [Sling Dynamic Include](https://sling.apache.org/documentation/bundles/dynamic-includes.html) una possibilità. In sostanza, si tratta di un filtro di componenti, che ruota attorno al componente dinamico e, invece di eseguire il rendering del componente nella pagina, crea un riferimento. Questo riferimento può essere una chiamata Ajax, in modo che il componente sia incluso nel browser e quindi la pagina circostante possa essere memorizzata nella cache in modo statico. Oppure - in alternativa - Sling Dynamic Include può generare una direttiva SSI (Server Side Include). Questa direttiva verrà eseguita nel server Apache. Potete anche utilizzare direttive ESI - Edge Side Include se utilizzate Varnish o un CDN che supporta gli script ESI.

![Diagramma di sequenza di una richiesta utilizzando Sling Dynamic Include](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagramma di sequenza di una richiesta utilizzando Sling Dynamic Include*

<br> 

La documentazione SDI indica che è necessario disattivare il caching degli URL che terminano con &quot;*.nocache.html&quot;, il che ha un senso - come se si trattasse di componenti dinamici.

È possibile visualizzare un&#39;altra opzione per l&#39;utilizzo di SDI: Se _non_ si disattiva la cache del dispatcher per le includenze, il dispatcher si comporta come una cache dei frammenti simile a quella descritta nell&#39;ultimo capitolo: Le pagine e i frammenti di componente vengono memorizzati nella cache in modo uguale e indipendente nel dispatcher e uniti dallo script SSI nel server Apache quando la pagina viene richiesta. In questo modo, potete implementare componenti condivisi come la navigazione principale (visto che usate sempre lo stesso URL del componente).

Questo dovrebbe funzionare - in teoria. Ma...

Consigliamo di non farlo: Perdereste la possibilità di bypassare la cache per i componenti dinamici reali. L&#39;SDI è configurato a livello globale e le modifiche apportate per la cache dei frammenti scadenti vengono applicate anche ai componenti dinamici.

Vi consigliamo di studiare attentamente la documentazione SDI. Ci sono alcune altre limitazioni, ma SDI è uno strumento prezioso in alcuni casi.

#### Riferimenti

* [docs. oracle.com - Come scrivere tag JSP personalizzati](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Creazione e utilizzo di filtri](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Sling Dynamic Include](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Impostazione di Sling Dynamic Include in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Cache modello

![Memorizzazione nella cache basata su modello: Un oggetto business con due rendering diversi](assets/chapter-3/model-based-caching.png)

*Memorizzazione nella cache basata su modello: Un oggetto business con due rendering diversi*

<br> 

Riesaminiamo il caso con la navigazione. Presupponevamo che ogni pagina richiedesse lo stesso markup della navigazione.

Ma forse non è così. Potrebbe essere necessario eseguire il rendering di marcature diverse per l&#39;elemento nella navigazione che rappresenta la _pagina corrente_.

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

Si tratta di due rendering completamente diversi. Tuttavia, l&#39; _oggetto business_ - la struttura di navigazione completa - è la stessa.  L&#39; _oggetto business_ è un grafico a oggetti che rappresenta i nodi della struttura. Questo grafico può essere facilmente memorizzato in una cache in memoria. Tuttavia, questo grafico non deve contenere alcun oggetto o fare riferimento a oggetti che non si sono creati autonomamente, specialmente ora nodi JCR.

#### Memorizzazione nella cache nel browser

Abbiamo già toccato l&#39;importanza del caching nel browser, e ci sono molti buoni tutorial là fuori. Alla fine, per il browser, il Dispatcher è solo un server Web che segue il protocollo HTTP.

Tuttavia - nonostante la teoria - abbiamo raccolto alcuni frammenti di conoscenza che non abbiamo trovato altrove e che vogliamo condividere.

In sostanza, il caching dei browser può essere utilizzato in due modi diversi,

1. Il browser ha una risorsa memorizzata nella cache di cui conosce la data di scadenza esatta. In questo caso non richiede di nuovo la risorsa.

2. Il browser dispone di una risorsa, ma non è sicuro che sia ancora valida. In tal caso lo chiederà al webserver (il Dispatcher nel nostro caso). Per favore, dammi la risorsa se è stata modificata dopo l&#39;ultima volta che l&#39;hai consegnata. Se non è stato modificato, il server risponde con &quot;304 - non modificato&quot; e solo i metadati sono stati trasmessi.

#### Debug

Se state ottimizzando le impostazioni del dispatcher per il salvataggio nella cache del browser, è estremamente utile utilizzare un server proxy desktop tra il browser e il server Web. Preferiamo &quot;Charles Web Debugging Proxy&quot; di Karl von Randow.

Utilizzando Charles, puoi leggere le richieste e le risposte, che vengono trasmesse a e dal server. E - si può imparare molto sul protocollo HTTP. I browser moderni offrono anche alcune funzionalità di debug, ma le funzioni di un proxy desktop sono senza precedenti. È possibile manipolare i dati trasferiti, accelerare la trasmissione, riprodurre singole richieste e molto altro ancora. L&#39;interfaccia utente è chiaramente organizzata e completa.

Il test più semplice consiste nell&#39;utilizzare il sito Web come utente normale - con il proxy tra - e archiviare il proxy se il numero di richieste statiche (a /etc/...) sta diventando più piccolo nel tempo - in quanto queste dovrebbero essere nella cache e non dovrebbero più essere richieste.

Abbiamo scoperto che un proxy potrebbe fornire una panoramica più chiara, in quanto la richiesta memorizzata nella cache non viene visualizzata nel registro, mentre alcuni debugger integrati nel browser mostrano ancora queste richieste con &quot;0 ms&quot; o &quot;dal disco&quot;. Il che è corretto e preciso, ma potrebbe offuscare un po&#39; la tua vista.

Potete quindi eseguire il drill-down e controllare le intestazioni dei file trasferiti per vedere, ad esempio, se le intestazioni http &quot;Expires&quot; sono corrette. Potete riprodurre le richieste con intestazioni if-modified-from impostate per verificare se il server risponde correttamente con un codice di risposta 304 o 200. È possibile osservare i tempi delle chiamate asincrone e verificare i presupposti di sicurezza in un certo grado. Ricordate che vi è stato detto di non accettare tutti i selettori che non sono esplicitamente previsti? Qui puoi giocare con l&#39;URL e i parametri e vedere se l&#39;applicazione funziona bene.

Quando si esegue il debug della cache, vi viene chiesto di non eseguire una sola operazione:

Non ricaricare le pagine nel browser!

Un &quot;ricarico del browser&quot;, un _semplice-ricaricamento_ e un _reload forzato_ (&quot;_shift-reload_&quot;) non sono uguali a una normale richiesta di pagina. Una semplice richiesta di ricarica imposta un&#39;intestazione

```
Cache-Control: max-age=0
```

Inoltre, se tenete premuto il tasto Maiusc e fate clic sul pulsante di ricarica, in genere viene impostata l’intestazione della richiesta

```
Cache-Control: no-cache
```

Entrambe le intestazioni hanno effetti simili ma leggermente diversi, ma soprattutto differiscono completamente da una normale richiesta quando si apre un URL dallo slot URL o utilizzando i collegamenti sul sito. La navigazione normale non imposta le intestazioni Cache-Control ma probabilmente un&#39;intestazione if-modified-after.

Quindi, se si desidera eseguire il debug del normale comportamento di navigazione, è necessario eseguire esattamente le operazioni seguenti: _Sfogliare normalmente_. L&#39;utilizzo del pulsante di ricarica del browser è il modo migliore per non visualizzare gli errori di configurazione della cache nella configurazione.

Utilizzate il proxy Charles per vedere di cosa stiamo parlando. Sì - e mentre è aperto - è possibile riprodurre le richieste proprio lì. Non è necessario ricaricarsi dal browser.

## Test delle prestazioni

Utilizzando un proxy, si ottiene un&#39;idea del comportamento temporale delle pagine. Ovviamente, non si tratta di un test di performance.  Un test delle prestazioni richiede un certo numero di client che richiedono le pagine in parallelo.

Un errore comune, che abbiamo visto troppo spesso, è che il test delle prestazioni include solo un numero super limitato di pagine e queste pagine vengono distribuite solo dalla cache del Dispatcher.

Se state promuovendo la vostra applicazione al sistema live, il carico è completamente diverso da quello che avete testato.

Nel sistema live, il pattern di accesso non corrisponde a un numero limitato di pagine distribuite allo stesso modo che si trovano nei test (home page e poche pagine di contenuto). Il numero di pagine è molto maggiore e le richieste sono distribuite in modo molto irregolare. E - naturalmente - le pagine live non possono essere servite al 100% dalla cache: Sono presenti richieste di annullamento della validità provenienti dal sistema di pubblicazione che rendono automaticamente invalidante un&#39;enorme parte delle risorse preziose.

Ah sì - e quando si sta ricostruendo la cache del Dispatcher, scoprirete, che anche il sistema di pubblicazione si comporta in modo molto diverso, a seconda che si richiede solo una manciata di pagine - o un numero più grande. Anche se tutte le pagine sono altrettanto complesse, il loro numero ha un ruolo. Ricordate cosa abbiamo detto sulla cache incatenata? Se si richiede sempre lo stesso numero limitato di pagine, le probabilità sono buone, che i blocchi in base ai dati non elaborati si trovino nella cache dei dischi rigidi o che i blocchi siano memorizzati nella cache dal sistema operativo. Inoltre, esiste una buona probabilità che il repository abbia memorizzato nella cache il segmento secondo nella sua memoria principale. Pertanto, il nuovo rendering è notevolmente più veloce rispetto a quando si avevano altre pagine che si spostavano l&#39;una dall&#39;altra ora e poi da diverse cache.

La memorizzazione nella cache è difficile e lo è anche il test di un sistema che si basa sulla memorizzazione nella cache. Quindi, cosa si può fare per avere uno scenario reale più accurato?

Pensiamo che si debba eseguire più test, e si dovrebbe fornire più indice di prestazioni come misura della qualità della soluzione.

Se disponete già di un sito Web esistente, misurate il numero di richieste e la relativa modalità di distribuzione. Provare a modellare un test che utilizza una distribuzione di richieste simile. Aggiungere un po&#39; di casualità non poteva ferire. Non è necessario simulare un browser che caricherebbe risorse statiche come JS e CSS - queste non contano. Vengono memorizzati nella cache del browser o del Dispatcher alla fine e non aumentano in modo significativo il carico. Ma le immagini a cui si fa riferimento sono importanti. Trovare la loro distribuzione anche in vecchi file di registro e modellare un pattern di richiesta simile.

A questo punto, esegui un test con il dispatcher senza che sia necessario memorizzarlo nella cache. Questo è il tuo scenario peggiore. Scoprite a quale picco di carico il vostro sistema sta diventando instabile in queste condizioni peggiori. Potete anche peggiorare la situazione rimuovendo alcune gambe Dispatcher/Publish, se lo desiderate.

Quindi, eseguite lo stesso test con tutte le impostazioni della cache necessarie su &quot;on&quot;. Accelerate le richieste parallele lentamente per riscaldare la cache e vedere quanto il sistema può prendere in queste migliori condizioni.

Uno scenario di caso medio prevede l&#39;esecuzione del test con Dispatcher abilitato, ma anche con alcune invalide. È possibile simulare tale comportamento toccando i file di stato tramite un cronjob o inviando al Dispatcher le richieste di annullamento della validità a intervalli irregolari. Non dimenticare di eliminare anche alcune delle risorse non autoconvalidate di tanto in tanto.

È possibile variare l&#39;ultimo scenario aumentando le richieste di annullamento della validità e aumentando il carico.

È un po&#39; più complesso di un semplice test di carico lineare, ma offre molta più sicurezza nella soluzione.

Potresti allontanarti dallo sforzo. Tuttavia, per visualizzare i limiti del sistema, eseguite un test peggiore sul sistema Publish con un numero maggiore di pagine (distribuite in modo uniforme). Assicuratevi di interpretare correttamente il numero di scenari ottimali e di disporre i sistemi con un numero sufficiente di cuffie.