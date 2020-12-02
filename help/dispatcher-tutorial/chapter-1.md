---
title: Capitolo 1 - Configurazione e download delle esercitazioni
seo-title: Guida introduttiva a AEM Content Services - Capitolo 1 - Configurazione delle esercitazioni
description: Capitolo 1 dell'AEM senza titolo l'impostazione della linea di base per l'istanza AEM per l'esercitazione.
seo-description: Capitolo 1 dell'AEM senza titolo l'impostazione della linea di base per l'istanza AEM per l'esercitazione.
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '17502'
ht-degree: 0%

---


# Capitolo 1 - Concetti, Pattern e Antiformi dei Dispatcher

## Panoramica

Questo capitolo fornisce una breve introduzione alla cronologia e alla meccanica del Dispatcher e spiega in che modo questo influenzi il modo in cui uno sviluppatore AEM progetterebbe i suoi componenti.

## Perché Gli Sviluppatori Dovrebbero Preoccuparsi Dell&#39;Infrastruttura

Il Dispatcher è una parte essenziale della maggior parte - se non di tutte AEM installazioni. Potete trovare molti articoli online che illustrano come configurare il dispatcher, nonché suggerimenti e trucchi.

Questi bit e pezzi di informazioni comunque iniziano sempre a livello molto tecnico - supponendo che si sa già cosa si desidera fare e quindi fornendo solo dettagli su come ottenere ciò che si desidera. Non abbiamo mai trovato alcun documento concettuale che descriva il _cosa e perché è_ quando si tratta di ciò che si può e non si può fare con il dispatcher.

### Antipattern: Dispatcher come ultimo pensiero

Questa mancanza di informazioni di base porta a una serie di anti-modelli Abbiamo visto in una serie di progetti AEM:

1. Poiché il Dispatcher è installato nel server Web Apache, è il lavoro degli dei &quot;Unix&quot; nel progetto per configurarlo. Un &quot;costruttore di Java mortale&quot; non ha bisogno di preoccuparsi con esso.

2. Lo sviluppatore Java deve assicurarsi che il suo codice funzioni... il dispatcher in seguito lo renderà magicamente veloce. Il dispatcher è sempre un retropensiero. Tuttavia, questo non funziona. Uno sviluppatore deve progettare il codice tenendo presente il dispatcher. E ha bisogno di conoscere i suoi concetti base per farlo.

### &quot;Per prima cosa fatelo funzionare - poi renderlo veloce&quot; Non è sempre giusto

Potreste aver sentito il consiglio di programmazione _&quot;Prima di tutto, fatelo funzionare - poi renderlo veloce.&quot;_. Non è del tutto sbagliato. Tuttavia, senza il contesto corretto, si tende ad essere erroneamente interpretati e non applicati correttamente.

Il consiglio dovrebbe impedire allo sviluppatore di ottimizzare prematuramente il codice, che potrebbe non essere mai eseguito, o che viene eseguito così raramente, che un&#39;ottimizzazione non avrebbe un impatto sufficiente a giustificare lo sforzo messo in atto nell&#39;ottimizzazione. Inoltre, l&#39;ottimizzazione potrebbe portare a codici più complessi e quindi introdurre bug. Quindi, se siete sviluppatori, non passate troppo tempo a micro-ottimizzare ogni singola riga di codice. È sufficiente accertarsi di aver scelto le strutture di dati, gli algoritmi e le librerie corrette e attendere l&#39;analisi dei punti di attivazione di un profiler per vedere dove un&#39;ottimizzazione più accurata potrebbe aumentare le prestazioni complessive.

### Decisioni e artefatti architettonici

Tuttavia, il consiglio &quot;Prima di tutto farlo funzionare - poi farlo in fretta&quot; è del tutto sbagliato quando si tratta di decisioni &quot;architettoniche&quot;. Cosa sono le decisioni architettoniche? In parole povere, sono le decisioni che sono costose, difficili e/o impossibili da cambiare in seguito. Ricordate, che &quot;costoso&quot; a volte è lo stesso di &quot;impossibile&quot;.  Ad esempio, quando il progetto è senza budget, è impossibile implementare dei cambiamenti costosi. I cambiamenti infrastrutturali in questa categoria sono il primo tipo di cambiamenti che vengono alla mente della maggior parte delle persone. Ma c&#39;è anche un altro tipo di artefatti &quot;architettonici&quot; che può diventare molto brutto da cambiare:

1. Pezzi di codice nel &quot;centro&quot; di un&#39;applicazione, su cui si basano molti altri pezzi. La modifica di tali dipendenze richiede che tutte le dipendenze vengano modificate e testate contemporaneamente.

2. Artefatti, che sono coinvolti in alcuni scenari asincroni, dipendenti dai tempi in cui l&#39;input - e quindi il comportamento del sistema può variare molto casualmente. Le modifiche possono avere effetti imprevedibili e possono essere difficili da verificare.

3. Modelli software che vengono utilizzati e riutilizzati più e più volte, in tutti i pezzi e parti del sistema. Se il pattern software risulta non ottimale, tutti gli artefatti che utilizzano il pattern devono essere codificati nuovamente.

Ricorda? In cima a questa pagina abbiamo detto che il Dispatcher è una parte essenziale di un&#39;applicazione AEM. L&#39;accesso a un&#39;applicazione Web è molto casuale: gli utenti arrivano e vanno in tempi imprevedibili. Alla fine, tutto il contenuto sarà (o dovrebbe) memorizzato nella cache del dispatcher. Quindi, se avete prestato molta attenzione, potreste aver capito che la memorizzazione nella cache può essere vista come un manufatto &quot;architettonico&quot; e quindi deve essere compresa da tutti i membri del team, sviluppatori e amministratori.

Non stiamo dicendo che uno sviluppatore dovrebbe configurare il Dispatcher. Devono conoscere i concetti - soprattutto i limiti - per essere certi che il codice possa essere utilizzato anche dal Dispatcher.

Il dispatcher non migliora magicamente la velocità del codice. Uno sviluppatore deve creare i propri componenti tenendo presente il dispatcher. Quindi deve sapere come funziona.

## Memorizzazione in cache dei dispatcher - Principi di base

### Dispatcher come Cache Http - Load Balancer

Qual è il Dispatcher e perché si chiama &quot;Dispatcher&quot; in primo luogo?

Il dispatcher è

* Prima di tutto una cache

* Proxy inverso

* Un modulo per il server Web Apache httpd, aggiungendo AEM funzionalità correlate alla versatilità dell&#39;Apache e lavorando senza problemi con tutti gli altri moduli Apache (come SSL o anche SSI include come vedremo più avanti)

Nei primi giorni del web, ci si aspetterebbe qualche centinaio di visitatori in un sito. Configurazione di un dispatcher, &quot;inviato&quot; o bilanciamento del carico di richieste a diversi server di pubblicazione AEM che in genere era sufficiente, ovvero il nome &quot;Dispatcher&quot;. Al giorno d&#39;oggi, tuttavia, questa configurazione non viene più utilizzata molto frequentemente.

Vedremo diversi modi di configurare i dispatcher e i sistemi di pubblicazione più avanti in questo articolo. Cominciamo con alcune nozioni di base sulla cache http.

![Funzionalità di base di una cache del dispatcher](assets/chapter-1/basic-functionality-dispatcher.png)

*Funzionalità di base di una cache del dispatcher*

<br> 

Le basi stesse del dispatcher sono spiegate qui. Il dispatcher è un semplice proxy inverso nella cache con la possibilità di ricevere e creare richieste HTTP. Un normale ciclo di richiesta/risposta è simile al seguente:

1. Un utente richiede una pagina
2. Il Dispatcher controlla se dispone già di una versione di rendering della pagina. Supponiamo che sia la prima richiesta per questa pagina e che il Dispatcher non riesca a trovare una copia nella cache locale.
3. Il dispatcher richiede la pagina dal sistema di pubblicazione
4. Nel sistema di pubblicazione, la pagina viene rappresentata da un modello JSP o HTL
5. La pagina viene restituita al dispatcher
6. Il dispatcher memorizza nella cache la pagina
7. Il Dispatcher restituisce la pagina al browser
8. Se la stessa pagina viene richiesta una seconda volta, può essere servita direttamente dalla cache del dispatcher senza la necessità di eseguire nuovamente il rendering sull’istanza Pubblica. Questo consente di risparmiare tempo di attesa per i cicli utente e CPU sull’istanza Pubblica.

Stavamo parlando di &quot;pagine&quot; nell&#39;ultima sezione. Lo stesso schema si applica anche ad altre risorse come immagini, file CSS, download di PDF e così via.

#### Memorizzazione dei dati nella cache

Il modulo Dispatcher sfrutta le strutture offerte dal server Apache host. Risorse come pagine HTML, download e immagini vengono memorizzate come semplici file nel file system Apache. È così semplice.

Il nome del file viene derivato dall’URL della risorsa richiesta. Se si richiede un file `/foo/bar.html`, questo viene memorizzato, ad esempio, in /`var/cache/docroot/foo/bar.html`.

In linea di principio, se tutti i file sono memorizzati nella cache e quindi memorizzati staticamente nel Dispatcher, è possibile estrarre il plug del sistema Publish e il Dispatcher funge da semplice server Web. Ma questo è solo per illustrare il principio. La vita reale è più complicata. Non è possibile memorizzare tutto nella cache e la cache non è mai completamente &quot;piena&quot;, in quanto il numero di risorse può essere infinito a causa della natura dinamica del processo di rendering. Il modello di un file system statico consente di generare un&#39;immagine approssimativa delle capacità del dispatcher. E aiuta a spiegare i limiti del dispatcher.

#### AEM struttura URL e mappatura file system

Per comprendere meglio il dispatcher, rivisitiamo la struttura di un semplice URL di esempio.  Vediamo l&#39;esempio seguente:

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&amp;otherparameter=value#fragment`

* `http` denota il protocollo

* `domain.com` è il nome del dominio

* `path/to/resource` è il percorso in cui la risorsa viene memorizzata in CRX e successivamente nel file system del server Apache

Da qui, le cose differiscono un po&#39; tra il filesystem AEM e il filesystem Apache.

In AEM,

* `pagename` è l&#39;etichetta delle risorse

* `selectors` sta per una serie di selettori utilizzati in Sling per determinare in che modo viene eseguito il rendering della risorsa. Un URL può avere un numero arbitrario di selettori. Sono separati da un punto. Una sezione di selettori potrebbe essere qualcosa come &quot;french.mobile.fantasia&quot;. I selettori devono contenere solo lettere, cifre e trattini.

* `html` come ultimo dei &quot;selettori&quot; è detta estensione. In AEM/Sling determina anche in parte lo script di rendering.

* `path/suffix.ext` è un&#39;espressione simile a un percorso che può essere un suffisso per l&#39;URL.  Può essere utilizzato AEM script per controllare ulteriormente il rendering di una risorsa. Avremo un&#39;intera sezione su questa parte più avanti. Al momento, dovrebbe bastare sapere che può essere utilizzato come parametro aggiuntivo. I suffissi devono avere un&#39;estensione.

* `?parameter=value&otherparameter=value` è la sezione query dell&#39;URL. Viene utilizzato per trasmettere parametri arbitrari a AEM. Gli URL con parametri non possono essere memorizzati nella cache, pertanto i parametri devono essere limitati ai casi in cui sono assolutamente necessari.

* `#fragment`, la parte del frammento di un URL non viene passata a AEM viene utilizzata solo nel browser; nei framework JavaScript come &quot;parametri di routing&quot; o per passare a una determinata parte della pagina.

In Apache (*fare riferimento al diagramma seguente*),

* `pagename.selectors.html` viene utilizzato come nome file nel file system della cache.

Se l&#39;URL ha un suffisso `path/suffix.ext`,

* `pagename.selectors.html` viene creato come cartella

* `path` una cartella nella  `pagename.selectors.html` cartella

* `suffix.ext` è un file presente nella  `path` cartella. Nota: Se il suffisso non ha un&#39;estensione, il file non viene memorizzato nella cache.

![Layout del file system dopo aver ottenuto gli URL dal dispatcher](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png)

*Layout del file system dopo aver ottenuto gli URL dal dispatcher*

<br> 

#### Limitazioni di base

La mappatura tra un URL, la risorsa e il nome del file è abbastanza semplice.

Potreste aver notato qualche trappola, comunque

1. Gli URL possono diventare molto lunghi. L&#39;aggiunta della parte &quot;percorso&quot; di un `/docroot` nel file system locale potrebbe facilmente superare i limiti di alcuni file system. Eseguire il dispatcher in NTFS su Windows può essere una sfida. Tuttavia, è sicuro con Linux.

2. Gli URL possono contenere caratteri speciali e lettere. In genere non si tratta di un problema per il dispatcher. Tuttavia, tenete presente che l’URL viene interpretato in molte aree dell’applicazione. Molto spesso abbiamo visto comportamenti strani di un&#39;applicazione - solo per scoprire che un pezzo di codice raramente utilizzato (personalizzato) non è stato accuratamente testato per i caratteri speciali. Dovrebbe evitarle se può. E se non si può, pianificare un test approfondito.

3. In CRX, le risorse dispongono di risorse secondarie. Ad esempio, una pagina avrà diverse sottopagine. Non è possibile trovare una corrispondenza in un file system in quanto i file system contengono file o cartelle.

#### Gli URL Senza Estensione Non Vengono Memorizzati Nella Cache

Gli URL devono sempre avere un&#39;estensione. Anche se potete distribuire URL senza estensioni in AEM. Tali URL non verranno memorizzati nella cache del dispatcher.

**Esempi**

`http://domain.com/home.html` è  **memorizzabile nella cache**

`http://domain.com/home` non è  **memorizzabile nella cache**

La stessa regola si applica quando l&#39;URL contiene un suffisso. Per essere possibile memorizzare nella cache il suffisso deve avere un&#39;estensione.

**Esempi**

`http://domain.com/home.html/path/suffix.html` è  **memorizzabile nella cache**

`http://domain.com/home.html/path/suffix` non è  **memorizzabile nella cache**

Potreste chiedervi, cosa succede se la parte risorsa non ha un&#39;estensione, ma il suffisso ne ha una? Beh, in questo caso l&#39;URL non ha alcun suffisso. Guardate l&#39;esempio seguente:

**Esempio**

`http://domain.com/home/path/suffix.ext`

Il percorso `/home/path/suffix` rappresenta la risorsa... pertanto nell&#39;URL non è presente alcun suffisso.

**Conclusione**

Aggiungete sempre estensioni sia al percorso che al suffisso. A volte la gente attenta al SEO sostiene che questo ti sta classificando nei risultati della ricerca. Ma una pagina non memorizzata nella cache sarebbe super lenta e si classificherebbe ulteriormente.

#### Conflitti tra gli URL dei suffissi

Considerate di disporre di due URL validi

`http://domain.com/home.html`

e

`http://domain.com/home.html/suffix.html`

Sono assolutamente validi in AEM. Non si verificherebbero problemi sulla macchina di sviluppo locale (senza un Dispatcher). Probabilmente non si verificherà alcun problema nel test UAT o del carico. Il problema che stiamo affrontando è così sottile che scivola attraverso la maggior parte dei test.  Vi colpirà duramente quando siete in fase di picco e vi sarà limitato il tempo per affrontarlo, probabilmente non hanno accesso al server, né risorse per ripararlo. Ci siamo stati...

Quindi... qual è il problema?

`home.html` in un file system può essere un file o una cartella. Non entrambi nello stesso momento come in AEM.

Se richiedete prima `home.html`, verrà creato come file.

Le richieste successive a `home.html/suffix.html` restituiscono risultati validi, ma poiché il file `home.html` &quot;blocca&quot; la posizione nel file system, `home.html` non può essere creato una seconda volta come cartella e `home.html/suffix.html` non viene quindi memorizzato nella cache.

![Posizione di blocco file nel file system che impedisce la memorizzazione nella cache delle risorse secondarie](assets/chapter-1/file-blocking-position-in-filesystem.png)

*Posizione di blocco file nel file system che impedisce la memorizzazione nella cache delle risorse secondarie*

<br> 

Se lo si fa al contrario, prima di richiedere `home.html/suffix.html`, `suffix.html` viene memorizzato nella cache in una cartella `/home.html` all&#39;inizio. Tuttavia, questa cartella viene eliminata e sostituita da un file `home.html` quando successivamente si richiede `home.html` come risorsa.

![Eliminazione di una struttura di percorsi quando un elemento padre viene recuperato come risorsa](assets/chapter-1/deleting-path-structure.png)

*Eliminazione di una struttura di percorsi quando un elemento padre viene recuperato come risorsa*

<br> 

Pertanto, il risultato di ciò che è memorizzato nella cache è interamente casuale e dipende dall&#39;ordine delle richieste in arrivo. Ciò che rende le cose ancora più complicate, è il fatto che di solito hai più di un dispatcher. Le prestazioni, il tasso di hit della cache e il comportamento possono variare in modo diverso da un Dispatcher all&#39;altro. Per scoprire il motivo per cui il sito Web non risponde è necessario essere sicuri di aver visto il Dispatcher corretto con l&#39;ordine di caching sfortunato. Se si sta guardando il Dispatcher che - per fortuna - ha avuto un modello di richiesta più favorevole, si sarà perso nel cercare di trovare il problema.

#### Evitare il conflitto di URL

È possibile evitare &quot;URL in conflitto&quot;, in cui un nome di cartella e un nome di file &quot;concorrono&quot; per lo stesso percorso nel file system, quando si utilizza un’estensione diversa per la risorsa in presenza di un suffisso.

**Esempio**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Entrambi sono perfettamente cacheable,

![](assets/chapter-1/cacheable.png)

Se scegliete un&#39;estensione dedicata &quot;dir&quot; per una risorsa quando richiedete un suffisso o evitate di usare il suffisso completamente. Ci sono rari casi in cui sono utili. È facile implementare correttamente questi casi.  Come vedremo nel prossimo capitolo, quando parliamo di invalidazione della cache e scaricamento.

#### Richieste non memorizzabili nella cache

Vediamo un breve riepilogo dell&#39;ultimo capitolo più alcune eccezioni. Il dispatcher può memorizzare nella cache un URL se è configurato come memorizzabile nella cache e se si tratta di una richiesta di GET. Non può essere memorizzato nella cache in una delle seguenti eccezioni.

**Richieste in cache**

* La richiesta è configurata per essere memorizzata nella cache nella configurazione Dispatcher
* La richiesta è una richiesta semplice di GET

**Richieste o risposte non raggiungibili**

* Richiesta negata dalla memorizzazione nella cache dalla configurazione (percorso, pattern, tipo MIME)
* Risposte che restituiscono un &quot;Dispatcher: no-cache&quot; header
* Risposta che restituisce &quot;Cache-Control: no-cache|private&quot; header
* Risposta che restituisce un &quot;Pragma: no-cache&quot; header
* Richiesta con parametri di query
* URL senza estensione
* URL con suffisso privo di estensione
* Risposta che restituisce un codice di stato diverso da 200
* Richiesta POST

## Annullamento e cancellazione della cache

### Panoramica

L&#39;ultimo capitolo ha elencato un gran numero di eccezioni, quando il Dispatcher non è in grado di memorizzare nella cache una richiesta. Ma ci sono altre cose da considerare: Solo perché il dispatcher _può_ memorizzare nella cache una richiesta, non significa necessariamente che _deve_.

Il punto è: Caching di solito è facile. Il Dispatcher deve semplicemente memorizzare il risultato di una risposta e restituirlo la volta successiva quando la stessa richiesta viene ricevuta. Destra? Sbagliato!

La parte difficile è l&#39; _annullamento della validità_ o _scaricamento_ della cache. Il Dispatcher deve essere informato quando una risorsa è cambiata e deve essere nuovamente rappresentata.

A prima vista sembra un compito insignificante... ma non lo è. Ulteriori informazioni sono disponibili per scoprire alcune differenze complesse tra risorse singole e semplici e pagine che si basano su una struttura ad elevata combinazione di risorse multiple.

### Risorse semplici e scaricamento

Abbiamo impostato il nostro sistema di AEM per creare dinamicamente una rappresentazione in miniatura per ogni immagine su richiesta con uno speciale selettore &quot;pollice&quot;:

`/content/dam/path/to/image.thumb.png`

E - naturalmente - forniamo un URL per trasmettere l&#39;immagine originale con un URL senza selettore:

`/content/dam/path/to/image.png`

Se scariciamo sia la miniatura che l&#39;immagine originale, finiremo con qualcosa come,

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

nel file system del nostro Dispatcher.

A questo punto, l&#39;utente carica e attiva una nuova versione del file. Infine, una richiesta di annullamento della validità viene inviata dal AEM al Dispatcher,

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

L&#39;annullamento della validità è così semplice: Una semplice richiesta di GET a uno speciale URL &quot;/invalidate&quot; sul dispatcher. Un corpo HTTP non è richiesto, il &quot;payload&quot; è solo l&#39;intestazione &quot;invalidate-path&quot;. Inoltre, il percorso di annullamento della validità nell’intestazione è la risorsa che AEM nota e non il file o i file che il Dispatcher ha memorizzato nella cache. AEM solo le risorse. Le estensioni, i selettori e i suffissi vengono utilizzati in fase di esecuzione quando viene richiesta una risorsa. AEM non esegue alcuna contabilità relativa ai selettori utilizzati in una risorsa, pertanto il percorso della risorsa è tutto ciò che è sicuro quando si attiva una risorsa.

Questo è sufficiente nel nostro caso. Se una risorsa è stata modificata, possiamo presumere che anche tutte le rappresentazioni di tale risorsa siano cambiate. Nel nostro esempio, se l’immagine è cambiata, verrà riprodotta anche una nuova miniatura.

Il Dispatcher può eliminare in modo sicuro la risorsa con tutte le rappresentazioni memorizzate nella cache. Farà qualcosa come,

`$ rm /content/dam/path/to/image.*`

rimozione di `image.png` e `image.thumb.png` e di tutte le altre rappresentazioni corrispondenti a tale pattern.

Molto semplice... purché si utilizzi una sola risorsa per rispondere a una richiesta.

### Riferimenti e contenuti rappresentati

#### Il Problema Del Contenuto Meshed

A differenza delle immagini o di altri file binari caricati in AEM, le pagine HTML non sono animali solitari. Vivono in branchi e sono altamente interconnessi tra loro da collegamenti ipertestuali e riferimenti. Il semplice collegamento è innocuo, ma diventa complicato quando si parla di riferimenti a contenuti. Gli onnipresenti teaser o navigazione superiore sulle pagine sono riferimenti di contenuto.

#### Riferimenti ai contenuti e Perché sono un problema

Vediamo un semplice esempio. Un&#39;agenzia di viaggi ha una pagina web che promuove un viaggio in Canada. Questa promozione è disponibile nella sezione teaser su altre due pagine, sulla pagina &quot;Home&quot; e su una pagina &quot;Inverno Specials&quot;.

Poiché entrambe le pagine visualizzano lo stesso teaser, non sarebbe necessario chiedere all’autore di creare il teaser più volte per ciascuna pagina in cui deve essere visualizzato. Al contrario, la pagina di destinazione &quot;Canada&quot; riserva una sezione nelle proprietà della pagina per fornire le informazioni per il teaser, o meglio per fornire un URL che esegua il rendering del teaser:

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

o

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

Solo su AEM che funziona come charm, ma se si utilizza un Dispatcher nell’istanza Pubblica accade qualcosa di strano.

Immaginate, avete pubblicato il vostro sito web. Il titolo sulla pagina Canada è &quot;Canada&quot;. Quando un visitatore richiede la pagina principale, con un riferimento teaser a tale pagina, il componente della pagina &quot;Canada&quot; esegue un rendering simile a quello di una pagina

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

*nella* home page. La pagina principale è memorizzata dal dispatcher come file .html statico, incluso il teaser ed è in prima pagina nel file.

Ora l&#39;esperto di marketing ha imparato che i titoli dei teaser devono essere fruibili. Così decide di cambiare il titolo da &quot;Canada&quot; a &quot;Visit Canada&quot;, e aggiorna anche l&#39;immagine.

Pubblica la pagina modificata &quot;Canada&quot; e rivede la pagina principale precedentemente pubblicata per visualizzarne le modifiche. Ma non è cambiato niente. Viene comunque visualizzato il vecchio teaser. Lui controlla due volte lo &quot;Special Inverno&quot;. Tale pagina non è mai stata richiesta in precedenza e quindi non è memorizzata nella cache statica del Dispatcher. Per cui, per questa pagina viene eseguito il rendering da Publish e questa pagina contiene ora il nuovo teaser &quot;Visit Canada&quot;.

![Dispatcher memorizzazione del contenuto non aggiornato nella pagina principale](assets/chapter-1/dispatcher-storing-stale-content.png)

*Dispatcher memorizzazione del contenuto non aggiornato nella pagina principale*

<br> 

Cos&#39;è successo? Il Dispatcher memorizza una versione statica di una pagina contenente tutto il contenuto e la marcatura disegnati da altre risorse durante il rendering.

Il Dispatcher, essendo un semplice server Web basato su file system, è veloce ma anche relativamente semplice. Se una risorsa inclusa cambia, non se ne rende conto. Rimane ancorato al contenuto presente durante il rendering della pagina inclusa.

La pagina &quot;Inverno Speciale&quot; non è ancora stata resa, quindi non c&#39;è alcuna versione statica sul Dispatcher e quindi verrà visualizzata con il nuovo teaser in quanto verrà riprodotta di recente su richiesta.

Potreste pensare che il dispatcher seguirà ogni risorsa che toccherà durante il rendering e lo scaricamento di tutte le pagine che hanno utilizzato questa risorsa, quando la risorsa cambia. Tuttavia, il Dispatcher non esegue il rendering delle pagine. Il rendering viene eseguito dal sistema di pubblicazione. Il Dispatcher non sa quali risorse inserire in un file .html di cui è stato eseguito il rendering.

Non ne sei ancora convinto? Potreste pensare che *&quot;ci debba essere un modo per implementare un qualche tipo di tracciamento delle dipendenze&quot;*. Beh, c&#39;è, o più accuratamente lì *era*. Comunicato 3 il bisnonno di AEM ha implementato un tracciatore di dipendenze nella _sessione_ utilizzata per eseguire il rendering di una pagina.

Durante una richiesta, ogni risorsa acquisita tramite questa sessione veniva tracciata come una dipendenza dell’URL di cui era in corso il rendering.

Ma si è scoperto che tenere traccia delle dipendenze era molto costoso. Le persone hanno presto scoperto che il sito Web è più veloce se hanno disattivato completamente la funzione di tracciamento delle dipendenze e si sono affidate al rendering di tutte le pagine HTML dopo che una pagina HTML è stata modificata. Inoltre, anche questo schema non era perfetto - ci sono stati una serie di insidie e eccezioni sul cammino. In alcuni casi, non si utilizzava la sessione predefinita delle richieste per ottenere una risorsa, ma una sessione di amministrazione per ottenere alcune risorse di assistenza per eseguire il rendering di una richiesta. Tali dipendenze solitamente non sono state tracciate e hanno portato a mal di testa e telefonate al team di ops-team chiedendo di cancellare manualmente la cache. Siete stati fortunati se avevano una procedura standard per farlo. Ci sono stati altri gotchas lungo il percorso ma... smettiamo di ricordare. Questo porta indietro al 2005. In ultima analisi, questa funzione è stata disattivata in Communiqué 4 per impostazione predefinita e non è stata reinserita nel successore di CQ5 che poi è diventato AEM.

### Annullamento automatico

#### Quando Lo Scarico Completo È Più Economico Che Il Tracciamento Della Dipendenza

Poiché CQ5 si basa interamente sull’invalidazione, più o meno, dell’intero sito se cambia solo una delle pagine. Questa funzione è denominata &quot;Annullamento automatico&quot;.

Ma ancora - come può essere, che buttare via e restituire centinaia di pagine è più economico che fare un adeguato monitoraggio delle dipendenze e un rendering parziale?

I motivi principali sono due:

1. In un sito Web medio, viene richiesto solo un piccolo sottoinsieme di pagine. Quindi anche, se scartate tutti i contenuti resi, solo poche dozzine saranno effettivamente richieste subito dopo. Il rendering della coda lunga delle pagine può essere distribuito nel tempo, quando vengono effettivamente richiesti. In realtà, il carico sulle pagine di rendering non è così alto come potreste aspettarvi. Naturalmente, ci sono sempre delle eccezioni... discuteremo di alcuni trucchi per gestire in modo uniforme la loady distribuita su siti più grandi con cache di Dispatcher vuoti, più tardi.

2. Tutte le pagine sono comunque collegate dalla navigazione principale. Quasi tutte le pagine alla fine dipendono l&#39;una dall&#39;altra. Questo significa che anche il tracciatore di dipendenze più intelligente scoprirà ciò che già sappiamo: Se una delle pagine cambia, è necessario annullare la validità di tutte le altre.

Non credi? Illustriamo l&#39;ultimo punto.

Lo stesso argomento viene utilizzato nell’ultimo esempio con i teaser che fanno riferimento al contenuto di una pagina remota. Solo ora stiamo usando un esempio più estremo: Navigazione principale con rendering automatico. Come per il teaser, il titolo di navigazione viene disegnato dalla pagina collegata o &quot;remota&quot; come riferimento di contenuto. I titoli di navigazione remoti non sono memorizzati nella pagina attualmente sottoposta a rendering. È opportuno ricordare che la navigazione viene riprodotta su ciascuna pagina del sito Web. Pertanto, il titolo di una pagina viene utilizzato più e più volte su tutte le pagine che dispongono di una navigazione principale. Per modificare un titolo di navigazione, è necessario farlo una sola volta sulla pagina remota, non su ogni pagina che fa riferimento alla pagina.

Nel nostro esempio, la navigazione unisce tutte le pagine utilizzando il &quot;NavTitle&quot; della pagina di destinazione per rappresentare un nome nella navigazione. Il titolo di navigazione per &quot;Islanda&quot; viene disegnato dalla pagina &quot;Islanda&quot; ed eseguito il rendering in ogni pagina con una navigazione principale.

![Navigazione principale inevitabilmente mescolando il contenuto di tutte le pagine, estraendone i &quot;NavTitles&quot;](assets/chapter-1/nav-titles.png)

*Navigazione principale inevitabilmente mescolando il contenuto di tutte le pagine, estraendone i &quot;NavTitles&quot;*

<br> 

Se si modifica NavTitle sulla pagina Islanda da &quot;Islanda&quot; a &quot;Bella Islanda&quot;, il titolo cambia immediatamente su tutte le altre pagine menu principale. Pertanto, le pagine sottoposte a rendering e memorizzate nella cache prima di tale modifica diventano tutte obsolete e devono essere annullate.

#### Modalità di implementazione dell&#39;annullamento automatico della validità: Il file .stat

Se disponete di un grande sito con migliaia di pagine, ci vorrebbe un po&#39; di tempo per scorrere tutte le pagine ed eliminarle fisicamente. Durante tale periodo, il Dispatcher poteva distribuire involontariamente contenuti scadenti. Peggio ancora, potrebbero verificarsi dei conflitti durante l&#39;accesso ai file della cache, è possibile che venga richiesta una pagina mentre viene semplicemente eliminata o che una pagina venga nuovamente eliminata a causa di una seconda annullamento della validità che si verificava dopo un&#39;immediata attivazione successiva. Considerate che casino sarebbe stato. Fortunatamente questo non è quello che succede. Il Dispatcher utilizza un trucco intelligente per evitare che: Invece di eliminare centinaia e migliaia di file, posiziona un file semplice e vuoto nella radice del file system quando un file viene pubblicato e tutti i file dipendenti vengono quindi considerati non validi. Questo file è denominato &quot;file di stato&quot;. Il file di stato è un file vuoto. Ciò che conta sul file di stato è solo la data di creazione.

Tutti i file nel dispatcher con una data di creazione precedente al file di stato sono stati sottoposti a rendering prima dell’ultima attivazione (e l’annullamento della validità) e pertanto sono considerati &quot;non validi&quot;. Sono ancora fisicamente presenti nel file system, ma il Dispatcher li ignora. Sono &quot;stantia&quot;. Ogni volta che viene effettuata una richiesta a una risorsa obsoleta, il dispatcher chiede al sistema AEM di eseguire nuovamente il rendering della pagina. La nuova pagina di cui è stato effettuato il rendering viene memorizzata nel file system, ora con una nuova data di creazione ed è nuovamente fresca.

![La data di creazione del file .stat definisce quale contenuto è obsoleto e quale è fresco](assets/chapter-1/creation-date.png)

*La data di creazione del file .stat definisce quale contenuto è obsoleto e quale è fresco*

<br> 

Potreste chiedervi perché si chiama &quot;.stat&quot;? E non forse &quot;.invalidato&quot;? Beh, si può immaginare, avere quel file nel file system aiuta il Dispatcher a determinare quali risorse potrebbero essere servite *statisticamente*, proprio come da un server Web statico. Non è più necessario eseguire il rendering dinamico di questi file.

La vera natura del nome, tuttavia, è meno metaforica. È derivato dalla chiamata di sistema Unix `stat()`, che restituisce il tempo di modifica di un file (tra le altre proprietà).

#### Mixaggio convalida semplice e automatica

Ma aspetta... prima abbiamo detto che le singole risorse vengono eliminate fisicamente. Ora diciamo, che uno stato più recente li renderebbe praticamente invalidi agli occhi del Dispatcher. Perché poi la cancellazione fisica, prima?

La risposta è semplice. In genere si utilizzano entrambe le strategie in parallelo, ma per diversi tipi di risorse. Risorse binarie, come le immagini, sono indipendenti. Non sono collegati ad altre risorse nel senso che hanno bisogno che le loro informazioni siano rese.

Le pagine HTML, invece, sono altamente interdipendenti. Quindi, si applicherebbe l&#39;auto-annullamento della validità su questi. Questa è l&#39;impostazione predefinita nel dispatcher. Tutti i file appartenenti a una risorsa non convalidata vengono eliminati fisicamente. Inoltre, i file che terminano con &quot;.html&quot; vengono automaticamente invalidati.

Il dispatcher decide in merito all&#39;estensione del file, se applicare o meno lo schema di annullamento automatico della validità.

Le fine dei file per l&#39;annullamento automatico della convalida sono configurabili. In teoria è possibile includere tutte le estensioni per l&#39;annullamento automatico della validità. Ma tenete presente che questo ha un prezzo molto alto. Non vedrete risorse stantio consegnate a malincuore, ma le prestazioni di consegna si riducono notevolmente a causa di un eccesso di annullamento della validità.

Immaginate, ad esempio, di implementare uno schema in cui i PNG e i JPG vengono rappresentati in modo dinamico e dipendono da altre risorse per farlo. Potreste voler ridimensionare le immagini ad alta risoluzione per ridurle a una risoluzione compatibile con il Web. Anche se si è a questo punto si modifica il tasso di compressione. La risoluzione e la frequenza di compressione in questo esempio non sono costanti fisse ma parametri configurabili nel componente che utilizza l’immagine. Ora, se questo parametro viene modificato, è necessario annullare la validità delle immagini.

Nessun problema - abbiamo appena imparato, che possiamo aggiungere immagini all&#39;annullamento automatico e avere sempre le immagini appena sottoposte a rendering ogni volta che cambia qualcosa.

#### Gettare il bambino con l&#39;acqua di bagno

Esatto - ed è un problema enorme. Leggete di nuovo l&#39;ultimo paragrafo. &quot;...immagini appena riprodotte ogni volta che cambia qualcosa.&quot; Come sapete, un buon sito web è cambiato costantemente; aggiungere nuovi contenuti qui, correggere un errore ortografico, modificare un teaser altrove. Ciò significa che tutte le immagini vengono invalidate costantemente e devono essere sottoposte a nuovo rendering. Non sottovalutatelo. Il rendering e il trasferimento dinamico dei dati immagine funziona in millisecondi sul computer di sviluppo locale. Il vostro ambiente di produzione deve farlo un centinaio di volte più spesso - al secondo.

Per essere chiari, i vostri jpgs devono essere riprodotti quando una pagina html cambia e viceversa. Esiste un solo &quot;bucket&quot; di file da annullare automaticamente. Viene spazzolato nel suo insieme. Senza alcun mezzo di scomposizione in ulteriori strutture dettagliate.

Per impostazione predefinita, l’annullamento automatico della validità è limitato a &quot;.html&quot;. L&#39;obiettivo è di mantenere quel secchio il più piccolo possibile. Non buttare via il bambino con l&#39;acqua bagnata semplicemente invalidando tutto - solo per essere sul lato sicuro.

Le risorse autonome devono essere servite sul percorso della risorsa. Questo aiuta molto a annullare la validità. Semplificate, non create schemi di mappatura come &quot;risorsa /a/b/c&quot; servito da &quot;/x/y/z&quot;. I componenti devono funzionare con le impostazioni di annullamento automatico del dispatcher predefinite. Non tentare di riparare un componente progettato in modo errato con l&#39;annullamento della validità in eccesso nel dispatcher.

##### Eccezioni all&#39;annullamento automatico della validità: Annullamento di ResourceOnly

La richiesta di annullamento della validità per il dispatcher viene in genere generata dai sistemi di pubblicazione da un agente di replica.

Se ritenete di disporre di dipendenze affidabili, potete provare a creare un vostro agente di replica invalidante.

Sarebbe un po&#39; oltre questa guida andare nei dettagli, ma vogliamo darvi almeno qualche suggerimento.

1. So davvero cosa stai facendo. Ottenere il diritto di annullamento della validità è davvero difficile. Questo è uno dei motivi per cui l&#39;annullamento automatico della validità è così rigoroso; per evitare la distribuzione di contenuti non aggiornati.

2. Se l&#39;agente invia un&#39;intestazione HTTP `CQ-Action-Scope: ResourceOnly`, questa singola richiesta di annullamento della validità non attiva l&#39;annullamento automatico della convalida. Questo codice ( [https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) può essere un buon punto di partenza per il proprio agente di replica.

3. `ResourceOnly`, impedisce solo l&#39;annullamento automatico della validità. Per eseguire effettivamente la risoluzione delle dipendenze e le invalide necessarie, è necessario attivare le richieste di annullamento della validità. È possibile controllare le regole di eliminazione del pacchetto Dispatcher ([https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)) per ottenere informazioni su come ciò potrebbe effettivamente accadere.

Non si consiglia di creare uno schema di risoluzione delle dipendenze. Ci sono troppi sforzi e pochi guadagni - e come detto prima, c&#39;è troppo che vi sbagliate.

Occorre piuttosto scoprire quali risorse non hanno dipendenze da altre risorse e possono essere invalidate senza l&#39;annullamento automatico della validità. Tuttavia, non è necessario utilizzare un agente di replica personalizzato per tale questione. È sufficiente creare una regola personalizzata nella configurazione del Dispatcher per escludere queste risorse dall&#39;annullamento automatico della convalida.

Abbiamo detto che la navigazione principale o i teaser sono una fonte di dipendenze. Beh, se carichi la navigazione e i teaser in modo asincrono o li includi con uno script SSI in Apache, non avrai quella dipendenza da monitorare. Approfondiremo il caricamento asincrono dei componenti più avanti in questo documento quando parliamo di &quot;Sling Dynamic Include&quot;.

Lo stesso vale per le finestre a comparsa o per il contenuto caricato in una scatola luminosa. Anche questi pezzi raramente hanno navigazioni (ovvero &quot;dipendenze&quot;) e possono essere invalidati come una singola risorsa.

## Creazione di componenti con il dispatcher in Mente

### Applicazione dei meccanismi del dispatcher in un esempio reale

Nell&#39;ultimo capitolo abbiamo spiegato come funziona la meccanica di base del Dispatcher, come funziona in generale e quali sono i limiti.

Ora vogliamo applicare questi meccanismi a un tipo di componenti che molto probabilmente troverete nei requisiti del vostro progetto. Scegliamo il componente deliberatamente per dimostrare i problemi che affronterete anche prima o poi. Non temete - non tutti i componenti hanno bisogno di quella considerazione che presenteremo. Ma se vedete la necessità di costruire un tale componente, siete ben consapevoli delle conseguenze e sapete come gestirle.

### Il Pattern Del Componente Di Spooling (Anti)

#### Componente immagine reattiva

Illustra un pattern comune (o anti-pattern) di un componente con binari interconnessi. Verrà creato un componente &quot;respi&quot; per &quot;responsive-image&quot;. Questo componente deve essere in grado di adattare l’immagine visualizzata al dispositivo su cui è visualizzata. Su computer desktop e tablet mostra la risoluzione completa dell&#39;immagine, sui telefoni una versione più piccola con un ritaglio stretto - o forse anche un motivo completamente diverso (questa è chiamata &quot;direzione artistica&quot; nel mondo reattivo).

Le risorse vengono caricate nell’area DAM di AEM e solo _a cui si fa riferimento_ nel componente immagine reattiva.

Il componente respi si occupa sia del rendering della marcatura che della distribuzione dei dati immagine binari.

Il modo in cui lo implementiamo qui è un modello comune che abbiamo visto in molti progetti e anche uno dei componenti di base AEM è basato su questo modello. Quindi, è molto probabile che voi come sviluppatore potreste adattare quel modello. Ha le sue macchie dolci in termini di incapsulamento, ma richiede molto sforzo per farlo Dispatcher-ready. Discuteremo diverse opzioni per mitigare il problema in un secondo momento.

Chiamiamo il modello usato qui il &quot;Pattern di Spooler&quot;, perché il problema risale ai primi tempi di Communiqué 3, dove c&#39;era un metodo &quot;spool&quot; che poteva essere chiamato su una risorsa per lo streaming dei dati grezzi binari nella risposta.

Il termine originale &quot;spooling&quot; in realtà si riferisce a periferiche offline condivise lente, come le stampanti, per cui non viene applicato correttamente. Ma ci piace comunque il termine perché raramente è così distinguibile nel mondo online. E ogni modello dovrebbe avere comunque un nome distinguibile, giusto? Sta a voi decidere se questo è un modello o un anti-modello.

#### Implementazione

Di seguito è illustrata l’implementazione del componente per immagini reattive:

Il componente ha due parti; la prima parte esegue il rendering del codice HTML dell&#39;immagine, la seconda parte &quot;spool&quot; i dati binari dell&#39;immagine di riferimento. Poiché si tratta di un sito Web moderno con un design reattivo, non stiamo eseguendo il rendering di un semplice tag `<img src"…">`, ma di un set di immagini nel tag `<picture/>`. Per ciascun dispositivo, vengono caricate due diverse immagini in DAM e le viene fatto riferimento dal componente immagine.

Il componente dispone di tre script di rendering (implementati in JSP, HTL o come servlet) ciascuno con un selettore dedicato:

1. `/respi.jsp` - senza selettore per eseguire il rendering della marcatura HTML
2. `/respi.img.java` per eseguire il rendering della versione desktop
3. `/respi.img.mobile.java` per eseguire il rendering della versione mobile.


Il componente viene inserito nei parsys della homepage. La struttura risultante nel CRX è illustrata di seguito.

![Struttura delle risorse dell&#39;immagine reattiva nel CRX](assets/chapter-1/responsive-image-crx.png)

*Struttura delle risorse dell&#39;immagine reattiva nel CRX*

<br> 

Viene eseguito il rendering della marcatura dei componenti in questo modo,

```plain
  #GET /content/home.html

  <html>

  …

  <div class="responsive-image>

  <picture>
    <source src="/content/home/jcr:content/par/respi.img.mobile.jpg" …/>
    <source src="/content/home/jcr:content/par/respi.img.jpg …/>

    …

  </picture>
  </div>
  …
```

e... abbiamo finito con il nostro bene incapsulato componente.

#### Componente immagine reattiva in azione

Ora un utente richiede la pagina e le risorse tramite il dispatcher. Questo determina la creazione di file nel file system Dispatcher come illustrato di seguito,

![Struttura in cache del componente immagine reattiva racchiuso](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Struttura in cache del componente immagine reattiva racchiuso*

<br> 

Considerate l&#39;opportunità di caricare e attivare una nuova versione delle due immagini del fiore in DAM. AEM invierà una richiesta di annullamento della validità

`/content/dam/flower.jpg`

e

`/content/dam/flower-mobile.jpg`

al Dispatcher. Queste richieste sono però vane. I contenuti sono stati memorizzati nella cache come file sotto la sottostruttura del componente. Questi file sono ora scadenti ma vengono comunque serviti su richiesta.

![Mancata corrispondenza della struttura che porta a contenuti scadenti](assets/chapter-1/structure-mismatch.png)

*Mancata corrispondenza della struttura che porta a contenuti scadenti*

<br> 

C&#39;è un altro motivo di riserva in questo approccio. Si consideri di utilizzare lo stesso fiore.jpg su più pagine. Quindi la stessa risorsa sarà memorizzata nella cache in più URL o file,

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Ogni volta che viene richiesta una pagina nuova e non memorizzata nella cache, le risorse vengono recuperate da AEM con URL diversi. Nessuna cache del dispatcher e nessuna cache del browser possono velocizzare la consegna.

#### Dove brilla il pattern di spooler

Esiste un&#39;eccezione naturale, in cui questo pattern anche nella sua forma semplice è utile: Se il binario è memorizzato nel componente stesso e non nel DAM. Questa funzione è tuttavia utile solo per le immagini utilizzate una volta sul sito Web, e se non si memorizzano le risorse in DAM è difficile gestire le risorse. È sufficiente immaginare che la licenza di utilizzo per una particolare risorsa venga esaurita. Come puoi scoprire quali componenti hai usato la risorsa?

Vedete? L’espressione &quot;M&quot; in DAM significa &quot;Management&quot;, come in Digital Asset Management. Non volete togliere quella caratteristica.

#### Conclusione

Dal punto di vista di uno sviluppatore AEM il modello sembrava super elegante. Ma con il Dispatcher preso in considerazione, potreste essere d&#39;accordo, che l&#39;approccio ingenuo potrebbe non essere sufficiente.

Lasciamo a voi decidere se per il momento si tratta di un modello o di un anti-modello. E forse avete già qualche buona idea in mente su come mitigare i problemi sopra descritti? Bene. Sarete quindi lieti di vedere come altri progetti hanno risolto questi problemi.

### Risoluzione di problemi comuni del dispatcher

#### Panoramica

Parliamo di come potrebbe essere stato implementato un po&#39; più a portata di cache. Ci sono diverse opzioni. A volte non è possibile scegliere la soluzione migliore. Forse si entra in un progetto già in esecuzione e si dispone di un budget limitato per risolvere il &quot;problema della cache&quot; a portata di mano e non abbastanza per fare un vero e proprio refactoring. Oppure si ha un problema, ovvero più complesso del componente Immagine di esempio.

I principi e le avvertenze sono delineati nelle sezioni seguenti.

Di nuovo, questo si basa sull&#39;esperienza reale. Abbiamo già visto tutti questi schemi allo stato brado, quindi non è un esercizio accademico. Questo è il motivo per cui vi stiamo mostrando alcuni anti-modelli, così avete la possibilità di imparare dagli errori che altri hanno già fatto.

#### Cache killer

>[!WARNING]
>
>Questo è un anti-modello. Non la usi. Mai.

Hai mai visto parametri di query come `?ck=398547283745`? Sono chiamati cache-killer (&quot;ck&quot;). L&#39;idea è che se aggiungete un parametro di query la risorsa non verrà inserita nella cache. Inoltre, se aggiungete un numero casuale come valore del parametro (come &quot;398547283745&quot;) l’URL diventa univoco e accertatevi che nessun’altra cache tra il sistema AEM e lo schermo sia in grado di memorizzare nella cache. Solitamente, i sospetti intermedi sono una cache &quot;Varnish&quot; davanti al Dispatcher, una CDN o persino la cache del browser. Ancora: Non farlo. Desiderate che le risorse siano memorizzate nella cache il più a lungo possibile. La cache è tua amica. Non uccidere gli amici.

#### Annullamento automatico

>[!WARNING]
>
>Questo è un anti-modello. Evitate di utilizzarlo per le risorse digitali. Cercare di mantenere la configurazione predefinita del dispatcher, che > è l&#39;annullamento automatico della validità per i file &quot;.html&quot;, solo

A breve termine, potete aggiungere &quot;.jpg&quot; e &quot;.png&quot; alla configurazione di annullamento automatico della convalida nel dispatcher. Ciò significa che, ogni volta che si verifica un&#39;annullamento della validità, è necessario ripetere il rendering di tutti i formati &quot;.jpg&quot;, &quot;.png&quot; e &quot;.html&quot;.

Questo modello è super facile implementato se i proprietari aziendali lamentano di non vedere i loro cambiamenti materializzarsi sul sito live abbastanza velocemente. Ma questo può offrirvi solo un po&#39; di tempo per trovare una soluzione più sofisticata.

Assicuratevi di comprendere l&#39;impatto delle grandi prestazioni. Questo rallenterà notevolmente il sito Web e potrebbe persino influire sulla stabilità - se il sito è un sito web ad alto carico con frequenti modifiche - come un portale di notizie.

#### impronta URL

L’impronta digitale di un URL è simile a un elemento chiave della cache. Ma non lo è. Non è un numero casuale, ma un valore che caratterizza il contenuto della risorsa. Può trattarsi di un hash del contenuto della risorsa o, ancora più semplice, di una marca temporale quando la risorsa è stata caricata, modificata o aggiornata.

Una marca temporale Unix è sufficiente per un&#39;implementazione nel mondo reale. Per una migliore leggibilità, questa esercitazione utilizza un formato più leggibile: `2018 31.12 23:59 or fp-2018-31-12-23-59`.

L&#39;impronta digitale non deve essere utilizzata come parametro di query, come URL con parametri di query   non può essere memorizzato nella cache. È possibile utilizzare un selettore o il suffisso per l&#39;impronta digitale.

Supponiamo che il file `/content/dam/flower.jpg` abbia una `jcr:lastModified` data del 31 dicembre 2018, 23:59. L&#39;URL con l&#39;impronta digitale è `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg`.

Questo URL rimane stabile, a condizione che il file della risorsa di riferimento (`flower.jpg`) non venga modificato. Può quindi essere memorizzato nella cache per un periodo di tempo indefinito e non è un caching killer.

Nota: questo URL deve essere creato e servito dal componente immagine reattivo. Non è una funzionalità standard AEM.

Questo è il concetto di base. Ci sono però alcuni dettagli che potrebbero essere facilmente ignorati.

Nel nostro esempio, il componente è stato rappresentato e memorizzato nella cache alle 23:59. Ora l&#39;immagine è stata modificata diciamo alle 00:00.  Il componente _genera un nuovo URL con impronta digitale nella marcatura._

Potreste pensare che _dovrebbe_... ma non lo è. Poiché solo il binario dell’immagine è stato modificato e la pagina inclusa non è stata toccata, non è necessario ripetere il rendering del codice HTML. Quindi, il Dispatcher serve la pagina con la vecchia impronta digitale, e quindi la vecchia versione dell&#39;immagine.

![Il componente Immagine è più recente dell’immagine di riferimento e non viene riprodotta alcuna impronta digitale.](assets/chapter-1/recent-image-component.png)

*Il componente Immagine è più recente dell’immagine di riferimento e non viene riprodotta alcuna impronta digitale.*

<br> 

Ora, se riattivate la pagina principale (o qualsiasi altra pagina del sito) il file di stato viene aggiornato, il dispatcher considererà il file home.html obsoleto ed eseguirà nuovamente il rendering con una nuova impronta digitale nel componente immagine.

Ma non abbiamo attivato la pagina principale, giusto? E perché dovremmo attivare una pagina che comunque non abbiamo toccato? Inoltre, forse non disponiamo di diritti sufficienti per attivare le pagine o il flusso di lavoro di approvazione richiede molto tempo, e non possiamo farlo con un preavviso limitato. Quindi - cosa fare?

#### Lo strumento dell&#39;amministratore pigro - Diminuire i livelli dei file statici

>[!WARNING]
>
>Questo è un anti-modello. Utilizzatelo solo a breve termine per acquistare un po&#39; di tempo e trovare una soluzione più sofisticata.

L&#39;amministratore pigro in genere &quot;_imposta l&#39;annullamento automatico della convalida su jpgs e il livello dello stato su zero - che aiuta sempre con problemi di caching di tutti i tipi_.&quot; Questo consiglio è disponibile nei forum tecnici e aiuta a risolvere il problema di annullamento della validità.

Finora non abbiamo discusso a livello di file di stato. In sostanza, l&#39;annullamento automatico della convalida funziona solo per i file della stessa struttura secondaria. Tuttavia, il problema è che le pagine e le risorse in genere non vivono nella stessa struttura ad albero secondaria. Le pagine si trovano sotto `/content/mysite`, mentre le risorse vivono sotto `/content/dam`.

Il &quot;livello statfile&quot; definisce la posizione in cui si trovano i nodi radice della sottostruttura. Nell&#39;esempio sopra il livello sarebbe &quot;2&quot; (1=/content, 2=/mysite,dam)

L&#39;idea di &quot;ridurre&quot; il livello del file di stato a 0 consiste fondamentalmente nel definire l&#39;intera struttura /content come una e unica sottostruttura ad albero per rendere le pagine e le risorse live nello stesso dominio di annullamento automatico. Quindi avremmo solo su un grande albero a livello (al docroot &quot;/&quot;). Ma in questo modo si annullano automaticamente tutti i siti sul server ogni volta che viene pubblicato qualcosa, anche su siti completamente indipendenti. Fidati di noi: Si tratta di una cattiva idea a lungo termine, in quanto si riduce notevolmente il tasso di hit complessivo della cache. Tutto quello che si può fare è sperare che i server AEM abbiano abbastanza potenza di fuoco da funzionare senza cache.

I vantaggi di livelli di stato più profondi saranno evidenti anche in un secondo momento.

#### Implementazione di un agente di annullamento validità personalizzato

In ogni caso, è necessario informare il Dispatcher in qualche modo, per annullare la validità delle pagine HTML se viene modificato un &quot;.jpg&quot; o un &quot;.png&quot; per consentire il rendering con un nuovo URL.

Ciò che abbiamo visto nei progetti è, per esempio, agenti di replica speciali nel sistema di pubblicazione che inviano richieste di annullamento della validità per un sito ogni volta che viene pubblicata un&#39;immagine del sito.

In questo caso è molto utile se puoi derivare il percorso del sito dal percorso della risorsa assegnando una convenzione di denominazione.

In generale, è consigliabile associare i siti e i percorsi delle risorse come segue:

**Esempio**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

In questo modo il vostro agente di cancellazione del dispatcher personalizzato potrebbe facilmente inviare e annullare la richiesta a /content/site-a quando incontra una modifica in `/content/dam/site-a`.

In realtà, non importa quale percorso dici al Dispatcher di annullare la validità - purché si trovi nello stesso sito, nello stesso &quot;sottoalbero&quot;. Non è nemmeno necessario utilizzare un percorso di risorse reale. Può anche essere &quot;virtuale&quot;:

`GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy`

![](assets/chapter-1/resource-path.png)

1. Un listener nel sistema di pubblicazione viene attivato quando un file nel DAM cambia

2. Il listener invia una richiesta di annullamento della validità al Dispatcher. A causa dell&#39;annullamento della validità automatica, il percorso inviato per l&#39;annullamento automatico non ha importanza, a meno che non si trovi nella home page del sito - o sia più preciso a livello di file di stato dei siti.

3. Il file di stato viene aggiornato.

4. La volta successiva, la pagina principale viene richiesta, viene riprodotta. La nuova impronta digitale/ data viene rilevata dalla proprietà lastModified dell&#39;immagine come selettore aggiuntivo

5. Questo crea implicitamente un riferimento a una nuova immagine

6. Se l&#39;immagine è effettivamente richiesta, viene creata e memorizzata una nuova rappresentazione nel Dispatcher


#### La necessità di pulizia

Phew. Completato. Urrà!

Beh... non ancora del tutto.

Il percorso,

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

non si riferisce ad alcuna delle risorse invalidate. Ricorda? Abbiamo annullato solo una risorsa fittizia e ci siamo affidati all&#39;annullamento automatico della convalida per considerare &quot;home&quot; non valida. L&#39;immagine stessa potrebbe non essere mai eliminata _fisicamente_. Quindi, la cache crescerà e crescerà. Quando le immagini vengono modificate e attivate, ottengono nuovi nomi file nel file system del Dispatcher.

Ci sono tre problemi con non eliminare fisicamente i file memorizzati nella cache e mantenerli indefinitamente:

1. Si sta sprecando la capacità di stoccaggio - ovviamente. Concesso - lo stoccaggio è diventato più economico e meno costoso negli ultimi anni. Negli ultimi anni sono cresciute anche le risoluzioni delle immagini e le dimensioni dei file - con l&#39;avvento di display retina che hanno fame di immagini nitide.

2. Anche se i dischi rigidi sono diventati più economici, lo &quot;storage&quot; potrebbe non essere diventato più economico. Abbiamo visto una tendenza a non avere (a buon mercato) lo storage HDD in metallo nudo, ma noleggio lo storage virtuale su un NAS da parte del provider del centro dati. Questo tipo di storage è un po&#39; più affidabile e scalabile, ma anche un po&#39; più costoso. Potrebbe non voler sprecarlo conservando immondizia obsoleta. Questo non riguarda solo lo storage principale, ma anche i backup. Se si dispone di una soluzione di backup out-of-the-box, potrebbe non essere possibile escludere le directory della cache. Alla fine si sta anche effettuando il backup dei dati indesiderati.

3. Ancora peggio: Potreste aver acquistato licenze d’uso per determinate immagini solo per un periodo limitato, purché necessarie. Ora, se si conserva ancora l&#39;immagine dopo la scadenza di una licenza, ciò potrebbe essere visto come una violazione del copyright. Potreste non utilizzare più l&#39;immagine nelle vostre pagine Web - ma Google ancora li troverà.

Quindi, alla fine, troverete un po&#39; di pulizia cronaca per pulire tutti i file più vecchi di... diciamo una settimana per tenere sotto controllo questo tipo di lettiera.

#### Abuso delle impronte digitali URL per attacchi di tipo Denial of Service

Ma aspettate, c&#39;è un altro difetto in questa soluzione:

Stiamo abusando di un selettore come parametro: fp-2018-31-12-23-59 viene generato dinamicamente come una sorta di &quot;cache-killer&quot;. Ma forse un ragazzino annoiato (o un crawler del motore di ricerca che è impazzito) inizia a chiedere le pagine:

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Ogni richiesta bypasserà il dispatcher, causando il caricamento su un&#39;istanza Pubblica. E, peggio ancora, creare un file conforme sul Dispatcher.

Quindi... invece di usare semplicemente l&#39;impronta digitale come semplice caching-killer, si dovrebbe controllare la data jcr:lastModified dell&#39;immagine e restituire un 404 se non è la data prevista. Ci vogliono un po&#39; di tempo e di cicli CPU sul sistema di pubblicazione... che è ciò che si vuole evitare in primo luogo.

#### Verifica delle impronte digitali URL nelle release ad alta frequenza

Potete utilizzare lo schema di impronte digitali non solo per le risorse provenienti da DAM, ma anche per i file JS e CSS e le relative risorse.

[Clientlibsis versione ](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) di un modulo che utilizza questo approccio.

Ma qui si potrebbe affrontare un altro avvertimento con impronte digitali URL: Collega l’URL al contenuto. Non potete modificare il contenuto senza modificare anche l’URL (ovvero, aggiornare la data di modifica). Questo è ciò per cui le impronte digitali sono progettate in primo luogo. Tuttavia, tenete presente che state implementando una nuova versione, con nuovi file CSS e JS e quindi nuovi URL con nuove impronte digitali. Tutte le pagine HTML contengono ancora riferimenti ai vecchi URL con impronta digitale. Per garantire il corretto funzionamento della nuova release, è quindi necessario annullare la validità di tutte le pagine HTML contemporaneamente per forzare un nuovo rendering con riferimenti ai file appena acquisiti. Se disponete di più siti che dipendono dalle stesse librerie, il rendering può essere notevole e in questo caso non potete sfruttare il `statfiles`. Preparatevi quindi a visualizzare i picchi di carico sui sistemi di pubblicazione dopo l’implementazione. Potreste considerare una distribuzione blu-verde con il riscaldamento della cache o forse una cache basata su TTL davanti al Dispatcher ... le possibilità sono infinite.

#### Breve interruzione

Wow - Ci sono molti dettagli da considerare, giusto? E rifiuta di essere compreso, testato e sottoposto a debug con facilità. E tutto per una soluzione apparentemente elegante. Certo, è elegante, ma solo da una prospettiva AEM. Insieme al Dispatcher diventa brutto.

Tuttavia, non risolve un problema di base: se un&#39;immagine viene utilizzata più volte su pagine diverse, verrà memorizzata nella cache sotto di esse. Non c&#39;è molta sinergia nella cache.

In generale, l&#39;impronta digitale degli URL è uno strumento utile nel toolkit, ma è necessario applicarla con attenzione, perché può causare nuovi problemi mentre risolve solo alcuni problemi esistenti.

Quindi... era un capitolo lungo. Ma abbiamo visto questo schema così spesso, che ci è sembrato necessario darvi un quadro completo con tutti i pro e i contro. Le impronte digitali URL risolvono alcuni dei problemi intrinseci nel Pattern spooler, ma lo sforzo di implementazione è piuttosto alto e si deve considerare anche altre - più facili - soluzioni. Consigliamo di verificare sempre se potete basare gli URL sui percorsi delle risorse serviti e non disponete di un componente intermedio. Ci arriveremo al prossimo capitolo.

##### Risoluzione dipendenza runtime

Risoluzione dipendenza runtime è un concetto che abbiamo preso in considerazione in un progetto. Ma pensarlo è diventato piuttosto complesso e abbiamo deciso di non applicarlo.

Ecco l&#39;idea di base:

Il Dispatcher non è a conoscenza delle dipendenze delle risorse. Sono solo un mucchio di file singoli con una piccola semantica.

AEM anche poche informazioni sulle dipendenze. Manca di una semantica corretta o di un &quot;tracciatore di dipendenze&quot;.

AEM è a conoscenza di alcuni riferimenti. Utilizza questa conoscenza per avvisarti quando tenti di eliminare o spostare una pagina o una risorsa a cui fai riferimento. In questo modo viene eseguita una query di ricerca interna quando si elimina una risorsa. I riferimenti ai contenuti hanno un modulo molto particolare. Si tratta di espressioni di percorso che iniziano con &quot;/content&quot;. In questo modo, possono essere facilmente indicizzati full-text e interrogati quando necessario.

Nel nostro caso, avremmo bisogno di un agente di replica personalizzato nel sistema di pubblicazione, che attivi la ricerca di un percorso specifico quando tale percorso è cambiato.

Diciamo

`/content/dam/flower.jpg`

È stato modificato in Pubblica. L&#39;agente avvia una ricerca per &quot;/content/dam/flower.jpg&quot; e trova tutte le pagine che fanno riferimento a tali immagini.

Potrebbe quindi inviare al Dispatcher una serie di richieste di annullamento della validità. Una per ogni pagina contenente la risorsa.

In teoria, dovrebbe funzionare. Ma solo per dipendenze di primo livello. Non è consigliabile applicare lo schema per dipendenze con più livelli, ad esempio quando si utilizza l&#39;immagine su un frammento esperienza utilizzato in una pagina. In realtà, crediamo che l&#39;approccio sia troppo complesso - e che ci possano essere problemi in fase di esecuzione. E generalmente il miglior consiglio è di non fare informatica costosa nei gestori di eventi. E soprattutto la ricerca può diventare molto costosa.

##### Conclusione

Ci auguriamo che abbiamo discusso abbastanza a fondo del modello Spooler per aiutarti a decidere quando usarlo e non usarlo nella tua implementazione.

## Evitare i problemi del dispatcher

### URL basati su risorse

Un modo molto più elegante per risolvere il problema della dipendenza è di non avere dipendenze. Evitare dipendenze artificiali che si verificano quando si utilizza una risorsa per eseguire semplicemente il proxy di un&#39;altra - come abbiamo fatto nell&#39;ultimo esempio. Cercate di vedere le risorse come entità &quot;solitarie&quot; il più spesso possibile.

Il nostro esempio è facilmente risolto:

![Spooling dell’immagine con un servlet associato all’immagine, non al componente.](assets/chapter-1/spooling-image.png)

*Spooling dell’immagine con un servlet associato all’immagine, non al componente.*

<br> 

Utilizziamo i percorsi di risorse originali per eseguire il rendering dei dati. Per eseguire il rendering dell’immagine originale così com’è, è sufficiente utilizzare AEM renderer predefinito per le risorse.

Se è necessario eseguire un&#39;elaborazione speciale per un componente specifico, registreremmo un servlet dedicato su quel percorso e un selettore per eseguire la trasformazione per conto del componente. L&#39;abbiamo fatto in questo caso in modo esemplare con il &quot;.respi&quot;. selettore. È opportuno tenere traccia dei nomi dei selettori utilizzati nello spazio URL globale (ad esempio `/content/dam`) e avere una buona convenzione di denominazione per evitare conflitti di denominazione.

A proposito, non vediamo alcun problema con la coerenza del codice. Il servlet può essere definito nello stesso pacchetto Java del modello di sling dei componenti.

È inoltre possibile utilizzare selettori aggiuntivi nello spazio globale, come

`/content/dam/flower.respi.thumbnail.jpg`

Facile, vero? Allora perché la gente inventa schemi complicati come lo Spooler?

Beh, potremmo risolvere il problema evitando il riferimento interno al contenuto perché il componente esterno aggiungeva poco valore o informazioni al rendering della risorsa interna, che potesse essere facilmente codificato in set di selettori statici che controllano la rappresentazione di una risorsa solitaria.

Tuttavia, esiste una classe di casi che non è possibile risolvere facilmente con un URL basato su risorse. Questo tipo di caso viene chiamato &quot;Parameter Injection Components&quot; e ne discutiamo nel prossimo capitolo.

### Componenti di inserimento parametri

#### Panoramica

Lo spooler nell&#39;ultimo capitolo era solo un sottile wrapper intorno a una risorsa. Ha causato più problemi che aiutare a risolvere il problema.

Potremmo facilmente sostituire il wrapping con un semplice selettore e aggiungere un servlet adatto per soddisfare tali richieste.

Ma se il componente &quot;respi&quot; fosse più di un semplice proxy. Cosa succede se il componente contribuisce effettivamente al rendering del componente?

Introduciamo una piccola estensione del nostro componente &quot;respi&quot;, che è un po &#39;un cambio di gioco. Di nuovo, introdurremo prima alcune soluzioni ingenue per affrontare le nuove sfide e mostrare dove sono carenti.

#### Il componente Respi2

Il componente respi2 è un componente che visualizza un’immagine reattiva, così come il componente respi. Ma ha un leggero add-on,

![Struttura CRX: respi2, componente, aggiunta di una proprietà di qualità al recapito](assets/chapter-1/respi2.png)

*Struttura CRX: respi2, componente, aggiunta di una proprietà di qualità al recapito*

<br> 

Le immagini sono jpegs e jpegs può essere compresso. Quando comprimete un’immagine jpeg, potete commercializzare la qualità per le dimensioni del file. La compressione è definita come parametro numerico di &quot;qualità&quot; compreso tra &quot;1&quot; e &quot;100&quot;. &quot;1&quot; significa &quot;piccola ma di scarsa qualità&quot;, &quot;100&quot; significa &quot;di ottima qualità ma file grandi&quot;. Qual è quindi il valore perfetto?

Come in tutte le cose IT, la risposta è: &quot;Dipende.&quot;

Qui dipende dal motivo. I motivi con bordi ad alto contrasto come i motivi, come testo scritto, foto di edifici, illustrazioni, schizzi o foto di scatole di prodotti (con contorni netti e testo scritto sopra) rientrano in genere in quella categoria. I motivi con transizioni di colore e contrasto più morbide come paesaggi o ritratti possono essere compressi un po&#39; di più senza perdita di qualità visibile. Le fotografie della natura di solito rientrano in quella categoria.

Inoltre, a seconda di dove viene utilizzata l’immagine, potrebbe essere utile usare un parametro diverso. Una piccola miniatura in un teaser potrebbe presentare una compressione migliore rispetto alla stessa immagine utilizzata in un banner eroe a tutto schermo. Questo significa che il parametro quality non è innato sull&#39;immagine, ma sull&#39;immagine e sul contesto. E al gusto dell&#39;autore.

In breve: Non c&#39;è un&#39;impostazione perfetta per tutte le immagini. Non c&#39;è una taglia unica per tutti. È meglio che decida l&#39;autore. Modificherà il parametro &quot;quality&quot; come proprietà del componente finché non sarà soddisfatto della qualità e non andrà oltre a non sacrificare la larghezza di banda.

Ora abbiamo un file binario in DAM e un componente, che fornisce una proprietà di qualità. Che aspetto dovrebbe avere l’URL? Quale componente è responsabile dello spooling?

#### Approccio ingenuo 1: Passa proprietà come parametri di query

>[!WARNING]
>
>Questo è un anti-modello. Non la usi.

Nell’ultimo capitolo, l’URL dell’immagine rappresentato dal componente era simile al seguente:

`/content/dam/flower.respi.jpg`

Manca solo il valore della qualità. Il componente sa quale proprietà viene immessa dall’autore... Può essere facilmente passata al servlet di rendering immagine come parametro di query quando viene eseguito il rendering della marcatura, come `flower.respi2.jpg?quality=60`:

```plain
  <div class="respi2">
  <picture>
    <source src="/content/dam/flower.respi2.jpg?quality=60" …/>
    …
  </picture>
  </div>
  …
```

Questa è una cattiva idea. Ricorda? Le richieste con parametri di query non sono memorizzabili nella cache.

#### Approccio ingenuo 2: Passa informazioni aggiuntive come selettore

>[!WARNING]
>
>Questo potrebbe diventare un anti-modello. La usi con attenzione.

![Passaggio delle proprietà del componente come selettori](assets/chapter-1/passing-component-properties.png)

*Passaggio delle proprietà del componente come selettori*

<br> 

Si tratta di una leggera variazione dell’ultimo URL. Solo questa volta si utilizza un selettore per passare la proprietà al servlet, in modo che il risultato sia memorizzabile nella cache:

`/content/dam/flower.respi.q-60.jpg`

Questo è molto meglio, ma ricordate quel brutto ragazzo di script dell&#39;ultimo capitolo che cerca questi schemi? Egli vedrebbe quanto può arrivare con un ciclo di valori:

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

Anche in questo caso, la cache non viene superata e viene creato un carico nel sistema di pubblicazione. Potrebbe essere una cattiva idea. È possibile attenuare questa situazione filtrando solo un piccolo sottoinsieme di parametri. Consentire solo `q-20, q-40, q-60, q-80, q-100`.

#### Filtro delle richieste non valide quando si utilizzano i selettori

Ridurre il numero di selettori è stato un buon inizio. Di regola, è necessario limitare sempre il numero di parametri validi a un minimo assoluto. In questo caso, potete anche utilizzare un Web Application Firewall esterno a AEM utilizzando un set statico di filtri senza conoscere a fondo il sistema di AEM sottostante per proteggere i sistemi:

`Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
\.respi\.q-(20|40|60|80|100)\.jpg`

Se non si dispone di un firewall per applicazioni Web, è necessario filtrare nel Dispatcher o nel AEM stesso. Se lo fai in AEM, assicurati che

1. Il filtro è implementato in modo super efficiente, senza accedere troppo al CRX e sprecare memoria e tempo.

2. Il filtro risponde a un messaggio di errore &quot;404 - Non trovato&quot;

Sottolineiamo ancora l&#39;ultimo punto. La conversazione HTTP avrà l’aspetto seguente:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

Sono state inoltre visualizzate implementazioni che filtravano parametri non validi ma restituivano un rendering di fallback valido quando viene utilizzato un parametro non valido. Supponiamo che consentiamo solo parametri da 20 a 100. I valori intermedi sono mappati su quelli validi. Quindi,

`q-41, q-42, q-43, …`

risponderebbe sempre alla stessa immagine di q-40:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Questo approccio non aiuta per niente. Queste richieste sono in realtà richieste valide.  Utilizzano la potenza di elaborazione e occupano spazio nella directory della cache del Dispatcher.

Meglio è restituire un `301 – Moved permanently`:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

Qui AEM il browser. &quot;Non ho `q-41`. Ma ehi - mi puoi chiedere di `q-40` &quot;.

Questo aggiunge un ulteriore ciclo di richiesta-risposta alla conversazione, che rappresenta un po&#39; di sovraccarico, ma è più economico rispetto all&#39;elaborazione completa su `q-41`. È inoltre possibile sfruttare il file già memorizzato nella cache in `q-40`. Dovete capire, però, che 302 risposte non sono memorizzate nella cache del Dispatcher, stiamo parlando di logica che viene eseguita nel AEM. Ancora e ancora. Quindi è meglio renderlo sottile e veloce.

Personalmente ci piace che il 404 risponda di più. Rende estremamente ovvio ciò che sta accadendo. Inoltre, aiuta a rilevare gli errori nel sito Web quando si analizzano i file di registro. 301 s può essere previsto, dove 404 sempre dovrebbe essere analizzato ed eliminato.

## Sicurezza - Escursione

### Richieste di filtro

#### Dove applicare il filtro

Alla fine dell&#39;ultimo capitolo abbiamo sottolineato la necessità di filtrare il traffico in entrata per i selettori noti. Questo lascia la domanda: Dove posso filtrare le richieste?

Beh, dipende. Prima meglio è.

#### Firewall applicazione Web

Se si dispone di un dispositivo Web Application Firewall o di un dispositivo &quot;WAF&quot; progettato per Web Security, è assolutamente necessario sfruttare tali funzionalità. Ma si potrebbe scoprire che il WAF è gestito da persone che hanno solo una conoscenza limitata dell&#39;applicazione di contenuto e che filtrano richieste valide o lasciano passare troppe richieste dannose. Forse scoprirete che le persone che gestiscono il WAF sono assegnate a un dipartimento diverso con diversi turni e programmi di rilascio, la comunicazione potrebbe non essere così stretta come con i vostri colleghi diretti e non sempre si ottengono i cambiamenti nel tempo, il che significa che in definitiva il vostro sviluppo e la velocità del contenuto soffrono.

Potreste ritrovarvi con alcune regole generali o anche con un  inserii nell&#39;elenco Bloccati, che la sensazione istintiva dice, potrebbe essere inasprito.

#### Dispatcher e filtro pubblicazione

Il passaggio successivo consiste nell’aggiungere regole di filtro URL nel core Apache e/o nel dispatcher.

Qui potete accedere solo agli URL. I filtri basati su pattern sono limitati. Se è necessario impostare un filtro più basato sui contenuti (ad esempio, per consentire i file solo con un indicatore di data e ora corretto) o se si desidera che alcuni dei filtri controllati dall&#39;autore siano impostati, alla fine si scriverà qualcosa come un filtro servlet personalizzato.

#### Monitoraggio e debug

In pratica avrete un po&#39; di sicurezza su ogni livello. Ma si prega di assicurarsi di avere i mezzi per scoprire a quale livello una richiesta viene filtrata. Assicuratevi di avere accesso diretto al sistema di pubblicazione, al dispatcher e ai file di registro nel WAF per scoprire quale filtro nella catena blocca le richieste.

### Selettori e Proliferazione selettrice

L&#39;approccio che utilizza &quot;selector-parameters&quot; nell&#39;ultimo capitolo è rapido e semplice e può accelerare il tempo di sviluppo dei nuovi componenti, ma ha dei limiti.

L&#39;impostazione di una proprietà &quot;quality&quot; è solo un semplice esempio. Ma diciamo che il servlet si aspetta anche parametri per la &quot;larghezza&quot; per essere più versatili.

È possibile ridurre il numero di URL validi riducendo il numero di possibili valori del selettore. Potete eseguire le stesse operazioni anche con la larghezza:

qualità = q-20, q-40, q-60, q-80, q-100

width = w-100, w-200, w-400, w-800, w-1000, w-1200

Tutte le combinazioni sono ora URL validi:

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Ora sono già disponibili 5x6=30 URL validi per una risorsa. Ogni proprietà aggiuntiva aggiunge alla complessità. E ci potrebbero essere proprietà che non possono essere ridotte a un ragionevole ammontare di valori.

Anche questo approccio ha dei limiti.

#### Esposizione involontaria di un&#39;API

Cosa sta succedendo qui? Se guardiamo con attenzione, vediamo che stiamo gradualmente passando da un sito Web statico ad un sito altamente dinamico. Inoltre, stiamo inavvertitamente visualizzando un&#39;API di rendering delle immagini nel browser del cliente, che in realtà era destinata solo agli autori.

L’impostazione della qualità e delle dimensioni di un’immagine deve essere effettuata dall’autore che modifica la pagina. Avere le stesse funzionalità esposte da un servlet potrebbe essere visto come una funzionalità o come vettore per un attacco Denial of Service. Ciò che è in realtà, dipende dal contesto. Quanto è critico il sito Web? Qual è il carico sui server? Quanta cuffia rimane? Quanto budget hai per l&#39;implementazione? Devi bilanciare questi fattori. Devi essere consapevole dei pro e dei contro.

## Il modello spooler - Rivisitato e riabilitato

### Come lo spooler evita di esporre l&#39;API

Abbiamo screditato il modello Spooler nell&#39;ultimo capitolo. È ora di riabilitarla.

![](assets/chapter-1/spooler-pattern.png)

Il Pattern spooler impedisce il problema relativo all&#39;esposizione di un&#39;API di cui abbiamo discusso nell&#39;ultimo capitolo. Le proprietà vengono memorizzate e incapsulate nel componente. Per accedere a queste proprietà è sufficiente il percorso del componente. Non è necessario utilizzare l’URL come veicolo per trasmettere i parametri tra markup e rendering binario:

1. Il client esegue il rendering del codice HTML quando il componente viene richiesto all&#39;interno del loop di richieste principale

2. Il percorso del componente funge da riferimento di sfondo dalla marcatura al componente

3. Il browser utilizza questo riferimento per richiedere il binario

4. Quando la richiesta colpisce il componente, abbiamo tutte le proprietà in mano per ridimensionare, comprimere e spool i dati binari

5. L’immagine viene trasmessa tramite il componente al browser client

Il Pattern di Spooler non è poi così male, ecco perché è così popolare. Se solo dove non è così ingombrante quando si tratta di invalidazione cache...

### Lo spooler invertito - il meglio di entrambi i mondi?

Questo ci porta alla domanda. Perché non possiamo ottenere il meglio da entrambi i mondi? Buona incapsulazione del pattern spooler e delle proprietà di memorizzazione nella cache di un URL basato su risorse?

Dobbiamo ammettere che non l&#39;abbiamo visto in un vero progetto live. Ma osiamo comunque un piccolo esperimento mentale - come punto di partenza per la vostra soluzione.

Questo pattern verrà denominato _Spooler invertito_. Lo spooler invertito deve essere basato sulla risorsa delle immagini, per avere tutte le proprietà di annullamento validità della cache.

Ma non deve esporre alcun parametro. Tutte le proprietà devono essere racchiuse nel componente. Ma possiamo esporre il percorso dei componenti - come un riferimento opaco alle proprietà.

che porta a un URL nel modulo:

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` è il percorso della risorsa dell&#39;immagine

`.respi3` è un selettore per selezionare il servlet corretto per distribuire l&#39;immagine

`.content-mysite-home-jcrcontent-par-respi` è un selettore aggiuntivo. Codifica il percorso del componente che memorizza la proprietà necessaria per la trasformazione dell’immagine. I selettori sono limitati a un intervallo di caratteri inferiore a quello dei tracciati. Lo schema di codifica qui è esemplare. Sostituisce &quot;/&quot; con &quot;-&quot;. Non si tiene conto, che il percorso stesso può contenere anche &quot;-&quot;. Uno schema di codifica più sofisticato sarebbe consigliato in un esempio reale. Base64 deve essere ok. Ma rende il debug un po&#39; più difficile.

`.jpg` è il suffisso dei file

### Conclusione

Wow... la discussione dello spooler è diventata sempre più complicata del previsto. Ti dobbiamo una scusa. Ma abbiamo sentito che è necessario presentarvi una moltitudine di aspetti - buoni e cattivi - in modo da poter sviluppare qualche intuizione su cosa funziona bene nel Dispatcher-land e cosa no.

## Livello file di stato e file di stato

### Nozioni di base

#### Introduzione

Abbiamo già accennato brevemente al _file di stato_ prima. È correlato all&#39;annullamento automatico della validità:

Tutti i file della cache nel file system del dispatcher configurati per l&#39;annullamento automatico della convalida vengono considerati non validi se la data dell&#39;ultima modifica è precedente alla data dell&#39;ultima `statfile's` modifica.

>[!NOTE]
>
>La data dell&#39;ultima modifica di cui stiamo parlando è il file memorizzato nella cache la data in cui il file è stato richiesto dal browser del client e creato nel file system. Non è la data `jcr:lastModified` della risorsa.

La data dell&#39;ultima modifica del file di stato (`.stat`) è la data in cui la richiesta di annullamento della validità AEM è stata ricevuta sul dispatcher.

Se hai più di un Dispatcher, questo può portare a strani effetti. Il browser può disporre di una versione più recente di un dispatcher (se si dispone di più Dispatcher). Oppure, un Dispatcher potrebbe ritenere che la versione del browser rilasciata dall&#39;altro Dispatcher sia obsoleta e che invii inutilmente una nuova copia. Questi effetti non hanno un impatto significativo sulle prestazioni o sui requisiti funzionali. E si livelleranno nel tempo, quando il browser avrà la versione più recente. Tuttavia, può essere un po&#39; confuso quando si ottimizza e si esegue il debug del comportamento di memorizzazione nella cache del browser. Quindi siate avvertiti.

#### Impostazione dei domini di annullamento della validità con /statfileslevel

Quando abbiamo introdotto l&#39;annullamento automatico e il file di stato abbiamo detto, che *tutti i file* sono considerati non validi in caso di modifiche e che tutti i file sono comunque interdipendenti.

Non è abbastanza preciso. Solitamente, tutti i file che condividono una radice di navigazione principale comune sono interdipendenti. Ma un&#39;istanza AEM può ospitare un certo numero di siti web - *indipendenti*. Non condividere una navigazione comune - in realtà, non condividere nulla.

Non sarebbe uno spreco per invalidare il sito B perché c&#39;è una modifica nel sito A? Sì, lo è. E non deve essere così.

Il Dispatcher fornisce un metodo semplice per separare i siti: Il `statfiles-level`.

È un numero che definisce da quale livello nel file system on, due sottoalberi sono considerati &quot;indipendenti&quot;.

Esaminiamo il caso predefinito in cui il livello statfileslevel è 0.

![/statfileslivello &quot;0&quot;: Il simbolo_  _.stat_ _viene creato nel docroot. Il dominio di annullamento della validità si estende per l&#39;intera installazione, compresi tutti i siti](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":` Il  `.stat` file viene creato nel docroot. Il dominio di annullamento della validità si estende per l&#39;intera installazione, compresi tutti i siti.

Qualunque file sia stato invalidato, il file `.stat` nella parte superiore del docroot dei dispatcher viene sempre aggiornato. Pertanto, se si annulla la validità di `/content/site-b/home`, anche tutti i file in `/content/site-a` vengono invalidati, poiché sono ora più vecchi del file `.stat` nel docroot. Chiaramente non ciò di cui hai bisogno, quando si invalida `site-b`.

In questo esempio si preferisce impostare `statfileslevel` su `1`.

Ora, se pubblicate e quindi invalidate `/content/site-b/home` o qualsiasi altra risorsa sotto `/content/site-b`, il file `.stat` viene creato in `/content/site-b/`.

Il contenuto inferiore a `/content/site-a/` non viene interessato. Questo contenuto viene confrontato con un file `.stat` in `/content/site-a/`. Sono stati creati due domini di annullamento della validità separati.

![Un livello statfileslevel &quot;1&quot; crea diversi domini di annullamento della validità](assets/chapter-1/statfiles-level-1.png)

*Un livello statfileslevel &quot;1&quot; crea diversi domini di annullamento della validità*

<br> 

Le installazioni di grandi dimensioni sono in genere strutturate in modo più complesso e profondo. Uno schema comune è quello di strutturare i siti per marchio, paese e lingua. In tal caso, potete impostare lo stato livello ancor più alto. _1_ creerebbe domini di annullamento della validità per marchio,  _2_ per paese e  _3_ per lingua.

### Necessità di una struttura del sito omogenea

Il livello statfileslevel viene applicato allo stesso modo a tutti i siti nella configurazione. È pertanto necessario che tutti i siti seguano la stessa struttura e inizino allo stesso livello.

Considerate di avere alcuni marchi nel vostro portafoglio che sono venduti solo su pochi piccoli mercati mentre altri sono venduti in tutto il mondo. I piccoli mercati hanno una sola lingua locale mentre sul mercato globale ci sono paesi in cui si parla più di una lingua:

```plain
  /content/tiny-local-brand/finland/home
  /content/tiny-local-brand/finland/products
  /content/tiny-local-brand/finland/about
                              ^
                          /statfileslevel "2"
  …

  /content/tiny-local-brand/norway
  …

  /content/shiny-global-brand/canada/en
  /content/shiny-global-brand/canada/fr
  /content/shiny-global-brand/switzerland/fr
  /content/shiny-global-brand/switzerland/de
  /content/shiny-global-brand/switzerland/it
                                          ^
                                /statfileslevel "3"
  ..
```

Il primo richiederebbe un `statfileslevel` di _2_, mentre il secondo richiede _3_.

Non è una situazione ideale. Se si imposta su _3_, l&#39;annullamento automatico non funzionerebbe all&#39;interno dei siti più piccoli tra i rami secondari `/home`, `/products` e `/about`.

Impostandolo su _2_ si indica che nei siti più grandi si dichiara `/canada/en` e `/canada/fr` dipendente, che potrebbe non essere. Pertanto ogni annullamento della validità in `/en` invalida anche `/fr`. Ciò determinerà una leggera riduzione del tasso di hit della cache, ma è comunque migliore della distribuzione di contenuto non aggiornato nella cache.

La soluzione migliore è di rendere le radici di tutti i siti ugualmente profonde:

```
/content/tiny-local-brand/finland/fi/home
/content/tiny-local-brand/finland/fi/products
/content/tiny-local-brand/finland/fi/about
…
/content/tiny-local-brand/norway/no/home
                                 ^
                        /statfileslevel "3"
```

### Collegamento tra siti

Qual è il livello giusto? Dipende dal numero di dipendenze tra i siti. Le inclusione risolte per il rendering di una pagina sono considerate &quot;dipendenze complesse&quot;. Abbiamo dimostrato tale _inclusione_ quando abbiamo introdotto il componente _Teaser_ all&#39;inizio di questa guida.

_I_ collegamenti ipertestuali sono una forma più morbida di dipendenze. È molto probabile, che si farà collegamenti ipertestuali all&#39;interno di un sito web... e non è improbabile che si disponga di collegamenti tra i siti Web. I collegamenti ipertestuali semplici in genere non creano dipendenze tra siti Web. Pensate solo a un collegamento esterno che avete impostato dal vostro sito a Facebook... Non dovreste eseguire il rendering della pagina se qualcosa cambia su Facebook e viceversa, giusto?

Una dipendenza si verifica quando si legge il contenuto dalla risorsa collegata (ad esempio, il titolo di navigazione). Tali dipendenze possono essere evitate se fate affidamento solo ai titoli di navigazione inseriti localmente e non li tracciate dalla pagina di destinazione (come fareste con i collegamenti esterni).

#### Dipendenza Imprevista

Tuttavia, potrebbe essere presente una parte della configurazione in cui i siti, presumibilmente indipendenti, si riuniscono. Vediamo uno scenario reale che abbiamo visto in uno dei nostri progetti.

Il cliente aveva una struttura del sito come quella illustrata nell&#39;ultimo capitolo:

```
/content/brand/country/language
```

Esempio,

```
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de

/content/shiny-brand/france/fr

/content/shiny-brand/germany/de
```

Ogni paese aveva il proprio dominio,

```
www.shiny-brand.ch

www.shiny-brand.fr

www.shiny-brand.de
```

Non c&#39;erano collegamenti navigabili tra i siti della lingua e nessuna inclusione apparente, quindi abbiamo impostato lo stato livello su 3.

Tutti i siti servivano in pratica lo stesso contenuto. L&#39;unica differenza importante era la lingua.

I motori di ricerca come Google considerano l&#39;idea di avere lo stesso contenuto su URL diversi &quot;ingannevole&quot;. Un utente potrebbe desiderare di passare alla classifica superiore o elencare più spesso, creando farm che eseguono contenuto identico. I motori di ricerca riconoscono questi tentativi e classificano le pagine in basso che semplicemente riciclano il contenuto.

È possibile evitare di classificare il sistema rendendolo trasparente, di avere in realtà più pagine con lo stesso contenuto e di non tentare di &quot;giocare&quot; il sistema (vedere [&quot;Tell Google about localized version of your page&quot;](https://support.google.com/webmasters/answer/189077?hl=en)) impostando `<link rel="alternate">` tag su ogni pagina correlata nella sezione di intestazione di ogni pagina:

```
# URL: www.shiny-brand.fr/fr/home/produits.html

<head>

  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch" 
        href="http://www.shiny-brand.ch/de/home/produkte.html">
  <link rel="alternate" 
        hreflang="de-de" 
        href="http://www.shiny-brand.de/de/home/produkte.html">

</head>

----

# URL www.shiny-brand.de/de/home/produkte.html

<head>

  <link rel="alternate" 
        hreflang="fr-fr" 
        href="http://www.shiny-brand.fr/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch"
         href="http://www.shiny-brand.ch/de/home/produits.html">

</head>
```

![Interlinking all](assets/chapter-1/inter-linking-all.png)

*Interlinking all*

<br> 

Alcuni esperti SEO sostengono addirittura che ciò potrebbe trasferire la reputazione o il &quot;succo di collegamento&quot; da un sito web di alto livello in una lingua allo stesso sito web in una lingua diversa.

Questo schema ha creato non solo una serie di collegamenti, ma anche alcuni problemi. Il numero di collegamenti necessari per _p_ nelle _n_ lingue è _p x (n<sup>2</sup>-n)_: Ogni pagina si collega a un&#39;altra pagina (_n x n_) tranne se stessa (_-n_). Questo schema viene applicato a ogni pagina. Se disponiamo di un sito in 4 lingue con 20 pagine, ciascuno equivale a _240_ collegamenti.

In primo luogo, non è necessario che un editor mantenga manualmente questi collegamenti, che devono essere generati automaticamente dal sistema.

Secondo, dovrebbero essere precisi. Ogni volta che il sistema rileva un nuovo &quot;relativo&quot;, si desidera collegarlo da tutte le altre pagine con lo stesso contenuto (ma in una lingua diversa).

Nel nostro progetto, nuove pagine relative sono saltate fuori frequentemente. Ma non si materializzarono come collegamenti &quot;alternativi&quot;. Ad esempio, quando la pagina `de-de/produkte` è stata pubblicata sul sito Web tedesco, non era immediatamente visibile sugli altri siti.

Il motivo era che nella nostra configurazione i siti dovevano essere indipendenti. Una modifica apportata al sito tedesco non ha quindi provocato l&#39;annullamento della validità del sito web francese.

Conoscete già una soluzione per risolvere il problema. È sufficiente ridurre il livello statfileslevel a 2 per ampliare il dominio di annullamento della validità. Naturalmente, questo diminuisce anche il tasso di hit della cache, soprattutto quando le pubblicazioni, e quindi si verificano più spesso inconvalide.

Nel nostro caso era ancora più complicato:

Anche se avevamo lo stesso contenuto, i nomi reali non erano diversi in ogni paese.

`shiny-brand` è stato chiamato  `marque-brillant` in Francia e  `blitzmarke` in Germania:

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Ciò avrebbe significato impostare il livello `statfiles` su 1, il che avrebbe comportato un dominio di annullamento della validità troppo grande.

La ristrutturazione del sito avrebbe risolto tale problema. Unione di tutti i marchi in un&#39;unica radice comune. Ma allora non avevamo la capacità, e questo ci avrebbe dato solo un livello 2.

Abbiamo deciso di mantenere il livello 3 e pagato il prezzo di non avere sempre i link &quot;alternativi&quot; aggiornati. Per attenuare il problema, abbiamo avuto un lavoro di cron &quot;più facile&quot; in esecuzione sul Dispatcher che avrebbe ripulito i file più vecchi di 1 settimana comunque. Alla fine, tutte le pagine venivano riprodotte comunque, a un certo punto. Ma questo è un compromesso che deve essere deciso individualmente in ogni progetto.

## Conclusione

Abbiamo trattato alcuni principi di base sul funzionamento del Dispatcher in generale e abbiamo fornito alcuni esempi in cui potreste dover compiere un po&#39; più di sforzo di implementazione per ottenere il risultato giusto e dove potreste voler fare delle scelte.

Non sono stati forniti dettagli sulla configurazione del Dispatcher. Volevamo che tu capissi prima i concetti di base e i problemi, senza perderti troppo presto nella console. E - il lavoro di configurazione effettivo è ben documentato - se si comprendono i concetti di base, è necessario sapere per quali scopi vengono utilizzati i vari switch.

## Dispatcher Tips and Tricks

Concluderemo la prima parte di questo libro con una raccolta casuale di suggerimenti e trucchi che potrebbero essere utili in una situazione o nell&#39;altra. Come abbiamo già fatto, non presentiamo la soluzione, ma l&#39;idea per poter comprendere l&#39;idea e il concetto e il collegamento agli articoli che descrivono la configurazione effettiva in modo più dettagliato.

### Correzione tempo annullamento convalida

Se installate AEM Author e Publish out, la topologia è un po&#39; strana. L’autore invia i contenuti ai sistemi di pubblicazione e la richiesta di annullamento della validità ai dispatcher contemporaneamente. Poiché entrambi i sistemi Publish e Dispatcher sono separati dall&#39;Autore dalle code, i tempi possono risultare un po&#39; infelici. Il dispatcher può ricevere la richiesta di annullamento della validità dall&#39;autore prima che il contenuto venga aggiornato nel sistema di pubblicazione.

Se nel frattempo un client richiede quel contenuto, il dispatcher richiederà e archivierà il contenuto non aggiornato.

Una configurazione più affidabile consiste nell&#39;inviare la richiesta di annullamento della validità dai sistemi di pubblicazione _dopo che_ hanno ricevuto il contenuto. L&#39;articolo &quot;[Invalidazione della cache del dispatcher da un&#39;istanza di pubblicazione](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)&quot; descrive i dettagli.

**Riferimenti**

[helpx.adobe.com - Annullamento della validità della cache del dispatcher da un&#39;istanza di pubblicazione](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)

### Memorizzazione nella cache delle intestazioni HTTP

Ai vecchi tempi, il Dispatcher stava semplicemente memorizzando file semplici nel file system. Se avevate bisogno di intestazioni HTTP da consegnare al cliente, lo avete fatto configurando Apache in base alle poche informazioni ottenute dal file o dalla posizione. Ciò era particolarmente fastidioso quando si implementava un&#39;applicazione Web in AEM che si basava fortemente sulle intestazioni HTTP. Tutto funzionava correttamente nell&#39;istanza AEM-only, ma non quando si utilizzava un Dispatcher.

Di solito si iniziava ad applicare di nuovo le intestazioni mancanti alle risorse nel server Apache con `mod_headers` utilizzando le informazioni derivate dal percorso e dal suffisso delle risorse. Ma non è sempre stato sufficiente.

Particolarmente fastidioso è stato il fatto che anche con il Dispatcher la prima risposta _non memorizzata nella cache_ al browser proveniva dal sistema Publish con una serie completa di intestazioni, mentre le risposte successive venivano generate dal Dispatcher con un set limitato di intestazioni.

A partire dal dispatcher 4.1.11, il dispatcher può memorizzare le intestazioni generate dai sistemi di pubblicazione.

In questo modo si evita di duplicare la logica di intestazione nel dispatcher e si libera la piena potenza espressiva di HTTP e AEM.

**Riferimenti**

* [helpx.adobe.com - Memorizzazione delle intestazioni di risposta nella cache](https://helpx.adobe.com/experience-manager/kb/dispatcher-cache-response-headers.html)

### Singole eccezioni di memorizzazione nella cache

Potrebbe essere utile memorizzare nella cache tutte le pagine e le immagini in generale, ma in alcune circostanze fare un&#39;eccezione. Ad esempio, desiderate memorizzare le immagini PNG nella cache, ma non le immagini PNG che visualizzano un captcha (che può essere modificato per ogni richiesta). Il Dispatcher potrebbe non riconoscere un captcha come captcha... ma AEM certamente. Può chiedere al Dispatcher di non memorizzare nella cache una richiesta inviando un&#39;intestazione conforme con la risposta:

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Control e Pragma sono intestazioni HTTP ufficiali, propagate e interpretate da livelli di memorizzazione nella cache superiori, ad esempio una CDN. L&#39;intestazione `Dispatcher` è solo un suggerimento per il Dispatcher a non memorizzare nella cache. Può essere utilizzato per indicare al Dispatcher di non memorizzare nella cache, consentendo comunque ai livelli superiori di memorizzazione nella cache di farlo. In realtà, è difficile trovare un caso in cui possa essere utile. Ma siamo sicuri che ce ne siano alcuni, da qualche parte.

**Riferimenti**

* [Dispatcher - Nessuna cache](https://helpx.adobe.com/experience-manager/kb/DispatcherNoCache.html)

### Cache browser

La risposta HTTP più rapida è la risposta data dal browser stesso. Se la richiesta e la risposta non devono essere inviate su una rete a un server Web con un carico elevato.

È possibile aiutare il browser a decidere quando chiedere al server una nuova versione del file impostando una data di scadenza su una risorsa.

In genere, lo si fa statisticamente utilizzando l&#39;intestazione di Apache `mod_expires` o memorizzando l&#39;intestazione Cache-Control e Scade che provengono da AEM se è necessario un controllo più individuale.

Un documento memorizzato nella cache del browser può presentare tre livelli di aggiornamento.

1. _Garantito fresco_  - Il browser può utilizzare il documento memorizzato nella cache.

2. _Potenzialmente obsoleto_ : il browser dovrebbe chiedere al server se il documento memorizzato nella cache è ancora aggiornato,

3. _Stale_ : il browser deve richiedere al server una nuova versione.

Il primo è garantito dalla data di scadenza impostata dal server. Se una risorsa non è scaduta, non è necessario chiedere nuovamente al server.

Se il documento ha raggiunto la data di scadenza, può essere ancora fresco. La data di scadenza viene impostata al momento della consegna del documento. Ma spesso non si conosce in anticipo quando sono disponibili nuovi contenuti, quindi si tratta solo di una stima prudente.

Per determinare se il documento nella cache del browser rimane lo stesso che verrà distribuito su una nuova richiesta, il browser può utilizzare la data `Last-Modified` del documento. Il browser chiede al server:

&quot;_Ho una versione a partire dal 10 giugno... Ho bisogno di un aggiornamento?_&quot; E il server potrebbe rispondere con

&quot;_304 - La versione in uso è ancora aggiornata_&quot; senza la ritrasmissione della risorsa, oppure il server potrebbe rispondere con

&quot;_200 - ecco una versione più recente_&quot; nell&#39;intestazione HTTP e il contenuto effettivo più recente nel corpo HTTP.

Per far funzionare la seconda parte, accertatevi di trasmettere la data `Last-Modified` al browser in modo che abbia un punto di riferimento per richiedere gli aggiornamenti.

Abbiamo spiegato in precedenza che, quando la data `Last-Modified` viene generata dal dispatcher, può variare tra diverse richieste perché il file memorizzato nella cache - e la relativa data - viene generato quando il file viene richiesto dal browser. Un&#39;alternativa potrebbe essere l&#39;utilizzo di &quot;e-tag&quot;, ossia numeri che identificano il contenuto effettivo (ad esempio generando un codice hash) invece di una data.

&quot;[Supporto Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)&quot; dal _pacchetto ACS Commons_ utilizza questo approccio. Tuttavia, questo viene fornito con un prezzo: Poiché il tag E deve essere inviato come intestazione, ma il calcolo del codice hash richiede la lettura completa della risposta, la risposta deve essere memorizzata completamente nella memoria principale prima di poterla distribuire. Questo può avere un impatto negativo sulla latenza quando il sito Web ha più probabilità di disporre di risorse non memorizzate nella cache e ovviamente è necessario tenere d&#39;occhio la memoria utilizzata dal sistema AEM.

Se utilizzi impronte digitali URL, puoi impostare date di scadenza molto lunghe. È possibile memorizzare le risorse stampate nella cache per sempre nel browser. Una nuova versione viene contrassegnata con un nuovo URL e le versioni precedenti non devono mai essere aggiornate.

Abbiamo usato le impronte digitali degli URL quando abbiamo introdotto il pattern di spooler. I file statici provenienti da `/etc/design` (CSS, JS) vengono modificati raramente, e quindi anche buoni candidati da usare come impronte digitali.

Per i file regolari, in genere viene impostato uno schema fisso, come il controllo HTML ogni 30 minuti, le immagini ogni 4 ore e così via.

La memorizzazione nella cache del browser è estremamente utile per il sistema Author. Desiderate memorizzare nella cache tutto il possibile nel browser per migliorare l&#39;esperienza di modifica. Sfortunatamente, le risorse più costose, le pagine HTML non possono essere memorizzate nella cache... dovrebbero essere modificate frequentemente dall&#39;autore.

Le librerie di graniti, che compongono AEM&#39;interfaccia utente, possono essere memorizzate nella cache per un certo periodo di tempo. Potete anche memorizzare nella cache i file statici dei siti (font, CSS e JavaScript) nel browser. Anche le immagini in `/content/dam` in genere possono essere memorizzate nella cache per circa 15 minuti, in quanto non vengono modificate spesso quanto il testo copiato sulle pagine. Le immagini non vengono modificate in AEM in modo interattivo. Vengono prima modificati e approvati, prima di essere caricati in AEM. Pertanto, è possibile supporre che le modifiche non vengano apportate con la stessa frequenza del testo.

Memorizzazione nella cache dei file dell&#39;interfaccia utente, i file libreria dei siti e le immagini possono velocizzare notevolmente il ricaricamento delle pagine in modalità di modifica.



**Riferimenti**

*[sviluppatore.mozilla.org - Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

* [apache.org - Scadenza Mod](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [ACS Commons - Supporto Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### Troncamento degli URL

Le risorse sono memorizzate in

`/content/brand/country/language/…`

Ma naturalmente, questo non è l&#39;URL che si desidera presentare al cliente. Per motivi estetici, di leggibilità e di SEO è possibile troncare la parte già rappresentata nel nome del dominio.

Se disponete di un dominio

`www.shiny-brand.fi`

di solito non c&#39;è bisogno di mettere il marchio e il paese nel percorso. Invece di

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

vorresti avere,

`www.shiny-brand.fi/home.html`

Dovete implementare la mappatura su AEM, perché AEM deve sapere come eseguire il rendering dei collegamenti in base a tale formato troncato.

Ma non fate affidamento solo su AEM. In caso contrario, nella directory principale della cache si verificherebbero percorsi come `/home.html`. Ora, quella è la &quot;casa&quot; per il sito finlandese o tedesco o canadese? E se nel Dispatcher è presente un file `/home.html`, come può il Dispatcher sapere che questo deve essere invalidato quando viene inoltrata una richiesta di annullamento della validità per `/content/brand/fi/fi/home`.

Abbiamo visto un progetto con docroots separati per ogni dominio. Era un incubo da curare e mantenere - e in realtà non l&#39;abbiamo mai visto funzionare senza problemi.

Potremmo risolvere i problemi ristrutturando la cache. Avevamo un singolo docroot per tutti i domini, e le richieste di annullamento della validità potevano essere gestite 1:1 come tutti i file sul server iniziavano con `/content`.

Anche la parte troncante era molto facile.  AEM generato collegamenti troncati a causa di una configurazione conforme in `/etc/map`.

Ora, quando una richiesta `/home.html` colpisce il Dispatcher, la prima cosa che si verifica è l&#39;applicazione di una regola di riscrittura che espande internamente il percorso.

Tale regola è stata impostata in modo statico in ogni configurazione host. In poche parole, le regole erano così,

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

Nel file system ora sono presenti percorsi `/content` semplici basati su file, che si trovano anche su Author e Publish, il che ha aiutato molto il debug. Per non parlare della corretta invalidazione, che non era più un problema.

Nota: è stato eseguito solo per gli URL &quot;visibili&quot;, gli URL visualizzati nello slot URL del browser. Gli URL per le immagini, ad esempio, erano ancora URL &quot;/content&quot; puri. Riteniamo che abbellire l&#39;URL &quot;principale&quot; sia sufficiente in termini di ottimizzazione del motore di ricerca.

Avere un docroot comune aveva anche un&#39;altra caratteristica bella. Quando qualcosa è andato storto nel Dispatcher, potremmo ripulire l&#39;intera cache eseguendo,

`rm -rf /cache/dispatcher/*`

(una cosa che potrebbe non essere utile fare a picchi di carico elevati).

**Riferimenti**

* [apache.org - Mod Rewrite](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [helpx.adobe.com - Mappatura delle risorse](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/resource-mapping.html)

### Gestione degli errori

Nelle classi AEM viene illustrato come programmare un gestore di errori in Sling. Non è così diverso da scrivere un modello normale. Scrivi semplicemente un modello in JSP o HTL, giusto?

Sì, ma questa è solo la parte AEM. Ricorda: il dispatcher non memorizza nella cache le risposte `404 – not found` o `500 – internal server error`.

Se esegui il rendering dinamico di queste pagine su ciascuna richiesta (non riuscita), avrai un carico eccessivo inutile sui sistemi di pubblicazione.

Ciò che abbiamo trovato utile è non eseguire il rendering della pagina di errore completa quando si verifica un errore, ma solo una versione super semplificata e piccola - anche statica di quella pagina, senza ornamenti o logica.

Questo ovviamente non è quello che il cliente ha visto. Nel Dispatcher, abbiamo registrato `ErrorDocuments` come segue:

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Ora il sistema AEM poteva semplicemente avvisare il Dispatcher che qualcosa non andava, e il Dispatcher poteva fornire una versione brillante e bella del documento di errore.

A questo proposito si dovrebbero sottolineare due punti.

Innanzitutto, la `error-404.html` è sempre la stessa pagina. Quindi, non c&#39;è nessun messaggio individuale come &quot;La ricerca di &quot;_produkten_&quot; non ha dato un risultato&quot;. Potremmo conviverci facilmente.

Secondo... beh, se vediamo un errore interno del server - o peggio ancora un&#39;interruzione del sistema AEM, non c&#39;è modo di chiedere AEM di eseguire una pagina di errore, giusto? Anche la richiesta successiva necessaria, come definita nella direttiva `ErrorDocument`, non è riuscita. Abbiamo risolto il problema eseguendo un processo cron che periodicamente estraeva le pagine di errore dalle relative posizioni definite tramite `wget` e le archiviava in percorsi di file statici definiti nella direttiva `ErrorDocuments`.

**Riferimenti**

* [apache.org - Documenti di errore personalizzati](https://httpd.apache.org/docs/2.4/custom-error.html)

### Memorizzazione del contenuto protetto nella cache

Il dispatcher non verifica le autorizzazioni quando distribuisce una risorsa per impostazione predefinita. È implementato in questo modo di proposito - per velocizzare il vostro sito web pubblico. Se si desidera proteggere alcune risorse tramite un login si dispone sostanzialmente di tre opzioni,

1. Protect la risorsa prima che la richiesta raggiunga la cache, ad esempio tramite un gateway SSO (Single Sign On) davanti al dispatcher o come modulo nel server Apache

2. Escludere le risorse sensibili dalla cache e quindi distribuirle sempre live dal sistema di pubblicazione.

3. Utilizzare il caching sensibile alle autorizzazioni nel dispatcher

E naturalmente, potete applicare la vostra combinazione di tutti e tre gli approcci.

**Opzione 1**. Un gateway &quot;SSO&quot; potrebbe comunque essere imposto dalla tua organizzazione. Se lo schema di accesso è molto grezzo, potrebbe non essere necessario disporre di informazioni da AEM per decidere se concedere o negare l&#39;accesso a una risorsa.

>[!NOTE]
>
>Questo pattern richiede un _Gateway_ che _intercetta_ ogni richiesta ed esegue l&#39;effettiva _autorizzazione_, concedendo o negando richieste al Dispatcher. Se il sistema SSO è un _autenticatore_, questo stabilisce solo l&#39;identità di un utente che devi implementare l&#39;opzione 3. Se leggete termini come &quot;SAML&quot; o &quot;OAauth&quot; nel manuale del sistema SSO, questo è un indicatore forte che è necessario implementare l&#39;opzione 3.


**Opzione 2**. &quot;Not caching&quot; generalmente è una cattiva idea. In questo caso, accertatevi che la quantità di traffico e il numero di risorse sensibili escluse siano limitati. In alternativa, accertatevi che nel sistema di pubblicazione sia installata una cache in memoria, che i sistemi di pubblicazione siano in grado di gestire il carico risultante - più su quello indicato nella parte III della serie.

**Opzione 3**. &quot;Il caching sensibile alle autorizzazioni&quot; è un approccio interessante. Il dispatcher sta memorizzando nella cache una risorsa, ma prima di distribuirla chiede al sistema AEM se può farlo. Questo crea una richiesta aggiuntiva dal dispatcher alla pubblicazione, ma in genere impedisce al sistema di pubblicazione di eseguire nuovamente il rendering di una pagina se è già memorizzata nella cache. Tuttavia, questo approccio richiede un&#39;implementazione personalizzata. Per informazioni dettagliate, consultate l&#39;articolo [Caching sensibile alle autorizzazioni](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html).

**Riferimenti**

* [helpx.adobe.com - Caching sensibile alle autorizzazioni](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html)

### Impostazione del periodo di tolleranza

Se si sta spesso invalidando in breve tempo - ad esempio, per l&#39;attivazione di una struttura ad albero o semplicemente per necessità di mantenere il contenuto aggiornato, potrebbe accadere che si stia costantemente svuotando la cache e che i visitatori raggiungano quasi sempre una cache vuota.

Il diagramma seguente illustra i tempi possibili per accedere a una singola pagina.  Il problema ovviamente peggiora quando il numero di pagine diverse richieste aumenta.

![Frequenti attivazioni che generano una cache non valida per la maggior parte del tempo](assets/chapter-1/frequent-activations.png)

*Frequenti attivazioni che generano una cache non valida per la maggior parte del tempo*

<br> 

Per attenuare il problema di questo &quot;temporale di annullamento della validità della cache&quot;, come talvolta viene chiamato, è possibile essere meno rigorosi riguardo l&#39;interpretazione `statfile`.

È possibile impostare il dispatcher in modo che utilizzi `grace period` per l&#39;annullamento automatico della validità. Ciò comporterebbe l&#39;aggiunta di un ulteriore tempo alla data di modifica `statfiles`.

Ad esempio, il `statfile` ha un tempo di modifica di oggi 12:00 e il `gracePeriod` è impostato su 2 minuti. Tutti i file auto-invalidati saranno considerati validi alle 12:01 e alle 12:02. Saranno riprodotti dopo le 12:02.

La configurazione di riferimento propone un `gracePeriod` di due minuti per una buona ragione. Potreste pensare &quot;Due minuti? È quasi niente. Posso facilmente aspettare 10 minuti prima che il contenuto venga visualizzato...&quot;.  Potreste essere tentati di impostare un periodo più lungo, ad esempio 10 minuti, partendo dal presupposto che il contenuto venga visualizzato almeno dopo questi 10 minuti.

>[!WARNING]
>
>Non è così che funziona `gracePeriod`. Il periodo di tolleranza è _not_ il periodo di tempo dopo il quale si garantisce che un documento venga invalidato, ma non si verifica alcuna annullamento della validità. Ogni successiva annullamento della validità che rientra in questo frame _prolunga_ l&#39;intervallo di tempo, che può essere indefinitamente lungo.

Illustra come `gracePeriod` funziona effettivamente con un esempio:

Supponiamo che tu stia gestendo un sito multimediale e che il personale addetto all&#39;editing fornisca regolarmente aggiornamenti dei contenuti ogni 5 minuti. Considera di impostare il periodo di tolleranza su 5 minuti.

Cominceremo con un rapido esempio alle 12:00.

12:00 - Il file di stato è impostato su 12:00. Tutti i file memorizzati nella cache sono considerati validi fino alle 12:05.

12:01 - Si verifica un&#39;annullamento della validità. Questo proroga il tempo grate a 12:06

12:05 - Un altro redattore pubblica il suo articolo - prolungando il tempo di grazia di un altro periodo di grazia a 12:10.

E così via... il contenuto non viene mai invalidato. Ogni annullamento della validità *entro* il periodo di tolleranza prolunga efficacemente il tempo di tolleranza. Il `gracePeriod` è progettato per resistere alla tempesta di invalidazione... ma è necessario uscire sotto la pioggia alla fine... quindi, tenere il `gracePeriod` considerevolmente breve per evitare di nascondersi nel rifugio per sempre.

#### Un periodo di tolleranza deterministico

Vorremmo introdurre un&#39;altra idea su come superare una tempesta di invalidazione. È solo un&#39;idea. Non l&#39;abbiamo provato in produzione, ma abbiamo trovato il concetto abbastanza interessante da condividere l&#39;idea con voi.

Se l&#39;intervallo di replica regolare è inferiore a `gracePeriod`, la lunghezza di `gracePeriod` può risultare imprevista.

L&#39;idea alternativa è la seguente: Annulla validità solo in intervalli di tempo fissi. Il tempo nel mezzo significa sempre servire contenuti scadenti. L&#39;invalidazione si verificherà alla fine, ma una serie di invalide vengono raccolte in un&#39;unica invalidazione &quot;collettiva&quot;, in modo che il Dispatcher abbia la possibilità di servire alcuni contenuti memorizzati nella cache nel frattempo e dare al sistema di pubblicazione un po&#39; d&#39;aria da respirare.

L&#39;implementazione avrà l&#39;aspetto seguente:

È possibile utilizzare uno &quot;script di annullamento della validità personalizzato&quot; (vedere il riferimento), che verrebbe eseguito dopo l&#39;annullamento della validità. Questo script leggeva la data dell&#39;ultima modifica `statfile's` e la arrotondava all&#39;arresto successivo dell&#39;intervallo. Il comando Unix shell `touch --time` consente di specificare un&#39;ora.

Ad esempio, se imposti il periodo di tolleranza su 30 secondi, il Dispatcher arrotonda la data dell&#39;ultima modifica del file di stato ai 30 secondi successivi. Le richieste di annullamento della validità che si verificano tra un secondo e l&#39;altro devono essere impostate per 30 secondi.

![Se si posticipa l&#39;annullamento della validità a 30 secondi, la frequenza degli hit aumenta.](assets/chapter-1/postponing-the-invalidation.png)

*Se si posticipa l&#39;annullamento della validità a 30 secondi, la frequenza degli hit aumenta.*

<br> 

Gli hit della cache che si verificano tra la richiesta di annullamento della validità e lo slot successivo di 30 sec. arrotondati vengono considerati obsoleti; Si è verificato un aggiornamento sulla pubblicazione, ma il dispatcher continua a distribuire contenuti obsoleti.

Questo approccio potrebbe aiutare a definire periodi di tolleranza più lunghi senza dover temere che le richieste successive prolunghino il periodo in modo indeterminato. Anche se come abbiamo già detto - è solo un&#39;idea e non abbiamo avuto la possibilità di testarla.

**Riferimenti**

[helpx.adobe.com - Configurazione del dispatcher](https://helpx.adobe.com/it/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Recupero automatico

Il sito ha un pattern di accesso molto particolare. Il traffico in entrata è elevato e la maggior parte del traffico è concentrata su una piccola parte delle pagine. La pagina principale, le pagine di destinazione della campagna e le pagine più dettagliate sui prodotti ricevono il 90% del traffico. Oppure, se gestite un nuovo sito, gli articoli più recenti hanno un numero di traffico più elevato rispetto a quelli precedenti.

Queste pagine sono molto probabilmente memorizzate nella cache del dispatcher, in quanto vengono richieste con tale frequenza.

Una richiesta di annullamento della validità arbitraria viene inviata al dispatcher, causando l&#39;annullamento della validità di tutte le pagine, inclusa quella più popolare.

Successivamente, poiché queste pagine sono così popolari, ci sono nuove richieste in entrata da browser diversi. Prendiamo ad esempio la pagina principale.

Poiché la cache non è valida, tutte le richieste inviate alla home page contemporaneamente vengono inoltrate al sistema di pubblicazione generando un carico elevato.

![Richieste parallele alla stessa risorsa su una cache vuota: Le richieste vengono inoltrate a Pubblica](assets/chapter-1/parallel-requests.png)

*Richieste parallele alla stessa risorsa su una cache vuota: Le richieste vengono inoltrate a Pubblica*

Con il recupero automatico è possibile attenuare in una certa misura questo problema. La maggior parte delle pagine invalidate vengono ancora archiviate fisicamente nel dispatcher dopo l&#39;annullamento della validità automatica. Sono solo _considerati_ scadenti. _Con_ Recupero automatico si intende che queste pagine non vengono ancora distribuite per alcuni secondi, mentre viene avviata  _una_ singola richiesta al sistema di pubblicazione per recuperare nuovamente il contenuto non aggiornato:

![Distribuzione di contenuti obsoleti durante il recupero in background](assets/chapter-1/fetching-background.png)

*Distribuzione di contenuti obsoleti durante il recupero in background*

<br> 

Per abilitare il recupero, è necessario indicare al dispatcher quali risorse recuperare dopo un&#39;annullamento della validità automatica. Ricorda che qualsiasi pagina attivata invalida automaticamente anche tutte le altre pagine, incluse quelle più popolari.

Re-fetching in realtà significa dire al Dispatcher in ogni (!) richiesta di annullamento della validità che si desidera recuperare i più popolari e che sono quelli più popolari.

Questo risultato si ottiene inserendo un elenco di URL di risorse (URL effettivi, non solo percorsi) nel corpo delle richieste di annullamento della validità:

```
POST /dispatcher/invalidate.cache HTTP/1.1

CQ-Action: Activate
CQ-Handle: /content/my-brand/home/path/to/some/resource
Content-Type: Text/Plain
Content-Length: 207

/content/my-brand/home.html
/content/my-brand/campaigns/landing-page-1.html
/content/my-brand/campaigns/landing-page-2.html
/content/my-brand/products/product-1.html
/content/my-brand/products/product-2.html
```

Quando il dispatcher visualizza tale richiesta, attiva l&#39;annullamento della validità automatica come di consueto e mette immediatamente in coda le richieste per recuperare nuovo contenuto dal sistema di pubblicazione.

Poiché ora utilizziamo un corpo di richiesta, dobbiamo anche impostare il tipo di contenuto e la lunghezza del contenuto in base allo standard HTTP.

Il Dispatcher contrassegna anche gli URL in base ai quali è possibile inviare direttamente tali risorse anche se sono considerati non validi dall&#39;annullamento automatico della convalida.

Tutti gli URL elencati vengono richiesti uno per uno. Non è quindi necessario creare un carico troppo elevato sui sistemi di pubblicazione. Ma non vorreste neanche inserire troppi URL in quell&#39;elenco. Alla fine, la coda deve essere elaborata in un periodo di tempo limitato per non distribuire contenuti non aggiornati per troppo tempo. È sufficiente includere le 10 pagine a cui si accede più frequentemente.

Se si esamina la directory della cache del Dispatcher, verranno visualizzati i file temporanei contrassegnati con marche temporali. Si tratta dei file attualmente caricati in background.

**Riferimenti**

[helpx.adobe.com - Annullamento della validità delle pagine memorizzate nella cache da AEM](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

### Protezione del sistema di pubblicazione

Il dispatcher offre un po&#39; di protezione in più proteggendo il sistema di pubblicazione dalle richieste destinate esclusivamente a scopi di manutenzione. Ad esempio, non si desidera esporre gli URL `/crx/de` o `/system/console` al pubblico.

La presenza di un firewall per applicazioni Web (WAF) installato nel sistema non comporta alcun danno. Ma questo aggiunge un numero significativo al vostro budget e non tutti i progetti sono in una situazione in cui possono permettersi e - per non dimenticare - gestire e mantenere un WAF.

Quello che vediamo spesso, è una serie di regole di riscrittura Apache nella configurazione Dispatcher che impediscono l&#39;accesso alle risorse più vulnerabili.

Ma si potrebbe anche considerare un approccio diverso:

Secondo la configurazione Dispatcher, il modulo Dispatcher è associato a una determinata directory:

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Ma perché legare il gestore all&#39;intero docroot, quando si deve filtrare in seguito?

È innanzitutto possibile restringere il binding del gestore. `SetHandler` è sufficiente collegare un gestore a una directory, è possibile eseguire il binding del gestore a un URL o a un pattern di URL:

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

In questo caso, non dimenticare di associare sempre il gestore di dispatcher all&#39;URL di annullamento validità del dispatcher. In caso contrario non sarà possibile inviare le richieste di annullamento della validità dal AEM al dispatcher.

Un&#39;altra alternativa per utilizzare il dispatcher come filtro è impostare le direttive filtro in `dispatcher.any`

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

Non stiamo imponendo l&#39;uso di una direttiva rispetto all&#39;altra, ma raccomandiamo una combinazione adeguata di tutte le direttive.

Tuttavia, proponiamo che si consideri la riduzione dello spazio URL il prima possibile nella catena, per quanto necessario, e lo si faccia nel modo più semplice possibile. Tenete sempre presente che queste tecniche non sostituiscono un WAF su siti Web altamente sensibili. Alcune persone chiamano queste tecniche &quot;Firewall dell&#39;uomo povero&quot; - per un motivo.

**Riferimenti**

[apache.org - direttiva candele](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[helpx.adobe.com - Configurazione dell&#39;accesso al filtro dei contenuti](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#ConfiguringAccesstoContentfilter)

### Filtrare utilizzando espressioni regolari e globali

Nei primi giorni era possibile utilizzare solo i &quot;globs&quot; - segnaposto semplici per definire i filtri nella configurazione del Dispatcher.

Fortunatamente ciò è cambiato nelle versioni successive del Dispatcher. Ora è possibile utilizzare anche espressioni regolari POSIX e accedere a varie parti di una richiesta per definire un filtro. Per qualcuno che ha appena iniziato con il Dispatcher che potrebbe essere dato per scontato. Ma se siete abituati ad avere globi, è una sorta di sorpresa e facilmente si può ignorare. Oltre alla sintassi di globi e regessi è semplicemente troppo simile. Confrontiamo due versioni che fanno lo stesso:

```
# Version A

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }

# Version B

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url '/content.\*'  }
```

Vedete la differenza?

La versione B utilizza virgolette singole `'` per contrassegnare un _pattern di espressione regolare_. &quot;Qualsiasi carattere&quot; è espresso utilizzando `.*`.

_I pattern_ di sovrapposizione, al contrario, utilizzano virgolette doppie  `"` e possono essere utilizzati solo segnaposto semplici come  `*`.

Se conosci questa differenza, è insignificante - ma se non lo è, puoi facilmente combinare le citazioni e passare un pomeriggio soleggiato per il debug della configurazione. Ora è avvisata.

&quot;Riconosco `'/url'` nella configurazione ... Ma che cos&#39;è questo `'/glob'` nel filtro si può chiedere?

Tale direttiva rappresenta l&#39;intera stringa di richiesta, incluso il metodo e il percorso. Potrebbe essere

`"GET /content/foo/bar.html HTTP/1.1"`

questa è la stringa con cui verrà confrontato il pattern. I principianti tendono a dimenticare la prima parte, la `method` (GET, POST, ...). Quindi, uno schema

`/0002  { /glob "/content/\*" /type "allow" }`

Verrebbe sempre un errore in quanto &quot;/content&quot; non corrisponde a &quot;GET ..&quot; della richiesta.

Quindi quando si vuole usare Globs,

`/0002  { /glob "GET /content/\*" /type "allow" }`

sarebbe corretto.

Per una regola di negazione iniziale, come

`/0001  { /glob "\*" /type "deny" }`

questo va bene. Ma per le successive autorizzazioni, è meglio e più chiaro più espressivo e più sicuro utilizzare le singole parti di una richiesta:

```
/method
/url
/path
/selector
/extension
/suffix
```

Tipo:

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Notate che potete combinare espressioni regex e div nella regola.

Un&#39;ultima parola sui &quot;numeri di riga&quot; come `/005` davanti a ogni definizione,

Non hanno alcun significato! Potete scegliere denominatori arbitrari per le regole. L&#39;uso dei numeri non richiede molto sforzo per pensare a uno schema, ma tenete presente che l&#39;ordine conta.

Se hai centinaia di regole come questa:

```
/001
/002
/003
…
/100
…
```

e si desidera inserire uno tra /001 e /002 che cosa succede con i numeri successivi? Sta aumentando il loro numero? Inserite numeri intermedi?

```
/001
/001a
/002
/003
…
/100
…
```

O cosa succede se cambiate per riordinare /003 e /001 cambierete i nomi e le loro identità o siete voi

```
/003
/002
/001
…
/100
…
```

La numerazione, pur sembrando una scelta semplice, raggiunge i limiti a lungo termine. Ad essere onesti, scegliere numeri come identificatori è comunque un cattivo stile di programmazione.

Vorremmo proporre un approccio diverso: Molto probabilmente non verranno visualizzati identificatori significativi per ogni singola regola filtro. Ma probabilmente servono a uno scopo più grande, in modo che possano essere raggruppati in qualche modo a seconda di quello scopo. Ad esempio, &quot;configurazione di base&quot;, &quot;eccezioni specifiche dell&#39;applicazione&quot;, &quot;eccezioni globali&quot; e &quot;protezione&quot;.

Potete quindi assegnare un nome e raggruppare le regole di conseguenza e fornire al lettore della configurazione (il vostro caro collega), un orientamento nel file:

```plain
  # basic setup:

  /filter {

    # basic setup

    /basic_01  { /glob "\*"             /type "deny"  }
    /basic_02  { /glob "/content/\*"    /type "allow" }
    /basic_03  { /glob "/etc/design/\*" /type "allow" }

    /basic_04  { /extension '(json|xml)'  /type "deny"  }
    …


    # login

    /login_01 { /glob "/api/myapp/login/\*" /type "allow" }
    /login_02 { … }

    # global exceptions

    /global_01 { /method "POST" /url '.\*contact-form.html' }
```


Molto probabilmente aggiungerete una nuova regola a uno dei gruppi, o forse anche create un nuovo gruppo. In tal caso, il numero di elementi da rinominare/rinominare è limitato a tale gruppo.

>[!WARNING]
>
>Configurazioni più sofisticate suddividono le regole di filtro in un certo numero di file, inclusi dal file di configurazione principale `dispatcher.any`. Un nuovo file, tuttavia, non introduce un nuovo spazio dei nomi. Quindi se avete una regola &quot;001&quot; in un file e &quot;001&quot; in un altro otterrete un errore. Ancora più ragione per arrivare con nomi semanticamente forti.

**Riferimenti**

[helpx.adobe.com - Progettazione di pattern per le proprietà dell&#39;elemento](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#DesigningPatternsforglobProperties)

### Specifiche del protocollo

L&#39;ultimo suggerimento non è un vero suggerimento, ma abbiamo sentito che valeva la pena condividerlo con voi comunque.

AEM e il dispatcher nella maggior parte dei casi lavorano out-of-the-box. Pertanto, non troverai una specifica completa del protocollo Dispatcher sul protocollo di annullamento della validità per creare la tua applicazione in cima. Le informazioni sono pubbliche, ma un po&#39; sparse su una serie di risorse.

Cerchiamo di colmare il vuoto in qualche modo. Esempio di una richiesta di annullamento della validità:

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: <action>
CQ-Handle: <path-pattern>
[CQ-Action-Scope]
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]

<newline>

<refetch-url-1>
<refetch-url-2>

…

<refetch-url-n>
```

`POST /dispatcher/invalidate.cache HTTP/1.1` - La prima riga è l&#39;URL dell&#39;endpoint di controllo del dispatcher e probabilmente non lo si modificherà.

`CQ-Action: <action>` - Cosa dovrebbe accadere. `<action>` è:

* `Activate:` elimina  `/path-pattern.*`
* `Deactive:` delete  `/path-pattern.*`
AND delete  `/path-pattern/*`
* `Delete:`   delete  `/path-pattern.*`
AND delete 
`/path-pattern/*`
* `Test:`   Restituisci &quot;ok&quot; ma non fai nulla

`CQ-Handle: <path-pattern>` - Percorso della risorsa contenuto da annullare. Nota: `<path-pattern>` in realtà è un &quot;percorso&quot; e non un &quot;pattern&quot;.

`CQ-Action-Scope: ResourceOnly` - Facoltativo: Se questa intestazione è impostata, il  `.stat` file non viene toccato.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Impostate queste intestazioni, se definite un elenco di URL di recupero automatico. `<bytes in request body>` è il numero di caratteri nel corpo HTTP

`<newline>` - Se si dispone di un corpo di richiesta, deve essere separato dall&#39;intestazione da una riga vuota.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Elenca gli URL che si desidera recuperare immediatamente dopo l’annullamento della validità.

## Risorse aggiuntive

Una buona panoramica e introduzione alla cache del dispatcher: [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html)

Ulteriori suggerimenti e trucchi di ottimizzazione: [https://helpx.adobe.com/experience-manager/kb/optimizing-the-dispatcher-cache.html#use-ttls](https://helpx.adobe.com/experience-manager/kb/optimizing-the-dispatcher-cache.html#use-ttls)

Documentazione del dispatcher con tutte le direttive spiegate: [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

Alcune domande frequenti: [https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html)

Registrazione di un webinar sull&#39;ottimizzazione del dispatcher - altamente consigliato: [https://my.adobeconnect.com/p7th2gf8k43?proto=true](https://my.adobeconnect.com/p7th2gf8k43?proto=true)

Presentazione &quot;La potenza sottovalutata dell&#39;annullamento della validità dei contenuti&quot;, conferenza &quot;adaptTo()&quot; a Potsdam 2018 [https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html)

Annullamento Della Convalida Delle Pagine Nella Cache Dalle AEM: [https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

## Passaggio successivo

* [2 - Schema dell&#39;infrastruttura](chapter-2.md)
