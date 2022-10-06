---
title: "Capitolo 1 - Concetti, schemi e modelli di Dispatcher"
description: Questo capitolo fornisce una breve introduzione alla cronologia e alla meccanica del Dispatcher e illustra come questo influenzi il modo in cui uno sviluppatore di AEM progetta i suoi componenti.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
exl-id: 3bdb6e36-4174-44b5-ba05-efbc870c3520
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '17460'
ht-degree: 0%

---

# Capitolo 1 - Concetti, schemi e modelli di Dispatcher

## Panoramica

Questo capitolo fornisce una breve introduzione alla cronologia e alla meccanica del Dispatcher e illustra come questo influenzi il modo in cui uno sviluppatore di AEM progetta i suoi componenti.

## Perché gli sviluppatori dovrebbero preoccuparsi dell&#39;infrastruttura

Dispatcher è una parte essenziale della maggior parte delle installazioni, se non di tutte AEM. Puoi trovare molti articoli online che illustrano come configurare il Dispatcher, nonché suggerimenti e trucchi.

Questi bit e pezzi di informazioni, tuttavia, iniziano sempre a livello molto tecnico - supponendo che tu sappia già cosa vuoi fare e quindi fornendo solo dettagli su come ottenere ciò che desideri. Non abbiamo mai trovato alcun documento concettuale che descriva _cosa e perché_ quando si tratta di ciò che puoi o non puoi fare con il dispatcher.

### Antipattern: Dispatcher come riflessione

Questa mancanza di informazioni di base porta a una serie di anti-modelli Abbiamo visto in una serie di progetti AEM:

1. Poiché Dispatcher è installato nel server Web Apache, è compito degli &quot;Dei Unix&quot; nel progetto configurarlo. Uno &quot;sviluppatore java mortale&quot; non deve preoccuparsi di se stessi con esso.

2. Lo sviluppatore Java deve assicurarsi che il suo codice funzioni... in seguito il dispatcher lo renderà magicamente veloce. Il dispatcher è sempre un retropensiero. Tuttavia, questo non funziona. Uno sviluppatore deve progettare il proprio codice tenendo presente il dispatcher. E deve conoscere i suoi concetti base per farlo.

### &quot;Prima far funzionare - poi renderlo veloce&quot; Non è sempre giusto

Forse avete sentito i consigli di programmazione _&quot;Prima fai funzionare - poi fallo in fretta.&quot;_. Non è del tutto sbagliato. Tuttavia, senza il contesto giusto, si tende a interpretarlo in modo errato e non viene applicato correttamente.

Il consiglio dovrebbe impedire allo sviluppatore di ottimizzare il codice in modo prematuro, che potrebbe non essere mai eseguito, o che venga eseguito così raramente, che un&#39;ottimizzazione non avrebbe un impatto sufficiente a giustificare lo sforzo da eseguire nell&#39;ottimizzazione. Inoltre, l&#39;ottimizzazione potrebbe portare a un codice più complesso e quindi all&#39;introduzione di bug. Quindi, se sei uno sviluppatore, non trascorri troppo tempo a micro-ottimizzare ogni singola riga di codice. Assicurati di scegliere le strutture di dati, gli algoritmi e le librerie giusti e di attendere l’analisi del punto attivo di un profiler per vedere dove un’ottimizzazione più accurata potrebbe aumentare le prestazioni complessive.

### Decisioni architettoniche e artefatti

Tuttavia, il consiglio &quot;Prima lo faccia funzionare - poi lo faccia in fretta&quot; è del tutto sbagliato quando si tratta di decisioni &quot;architettoniche&quot;. Cosa sono le decisioni architettoniche? In parole povere, sono le decisioni che sono costose, difficili e/o impossibili da cambiare successivamente. Tenete a mente che &quot;costoso&quot; a volte è lo stesso di &quot;impossibile&quot;.  Ad esempio, quando il progetto è a corto di budget, è impossibile implementare costose modifiche. I cambiamenti infrastrutturali a sono il primo tipo di cambiamenti in quella categoria che vengono alla mente della maggior parte delle persone. Ma c&#39;è anche un altro tipo di manufatti &quot;architettonici&quot; che possono diventare molto spiacevoli da cambiare:

1. Pezzi di codice nel &quot;centro&quot; di un&#39;applicazione, su cui si basano molti altri pezzi. Per modificarli, è necessario modificare e testare tutte le dipendenze contemporaneamente.

2. Gli artefatti, che sono coinvolti in alcuni scenari asincroni, dipendenti dal tempo in cui l&#39;input - e quindi il comportamento del sistema può variare molto casualmente. I cambiamenti possono avere effetti imprevedibili e possono essere difficili da testare.

3. Modelli software che vengono utilizzati e riutilizzati più e più volte, in tutti i pezzi e parti del sistema. Se il pattern software risulta non ottimale, tutti gli artefatti che utilizzano il pattern devono essere codificati nuovamente.

Ricorda? In cima a questa pagina abbiamo detto che Dispatcher è una parte essenziale di un&#39;applicazione AEM. L&#39;accesso a un&#39;applicazione web è molto casuale: gli utenti arrivano e vanno in tempi imprevedibili. Alla fine, tutti i contenuti saranno (o dovrebbero) memorizzati nella cache di Dispatcher. Quindi, se avete prestato molta attenzione, potreste aver capito che la memorizzazione in cache può essere vista come un manufatto &quot;architettonico&quot; e quindi deve essere compresa da tutti i membri del team, sviluppatori e amministratori.

Non stiamo dicendo che uno sviluppatore debba effettivamente configurare il Dispatcher. Devono conoscere i concetti, in particolare i limiti, per essere certi che il loro codice possa essere sfruttato anche dal Dispatcher.

Il Dispatcher non migliora magicamente la velocità del codice. Uno sviluppatore deve creare i propri componenti tenendo presente Dispatcher . Pertanto, deve sapere come funziona.

## Memorizzazione in cache del dispatcher - Principi di base

### Dispatcher come memorizzazione in cache Http - Load Balancer

Cos’è Dispatcher e perché si chiama innanzitutto &quot;Dispatcher&quot;?

Il Dispatcher è

* Prima di tutto una cache

* Un proxy inverso

* Un modulo per il server web Apache httpd, aggiungendo AEM funzioni correlate alla versatilità di Apache e lavorando senza problemi con tutti gli altri moduli Apache (come SSL o anche SSI include come vedremo più avanti)

Nei primi giorni del web, ci si aspetterebbe qualche centinaio di visitatori in un sito. Configurazione di un’istanza di Dispatcher, &quot;inviata&quot; o bilanciamento del carico di richieste a diversi server di pubblicazione AEM e che in genere era sufficiente, quindi il nome &quot;Dispatcher&quot;. Al giorno d&#39;oggi, tuttavia, questa configurazione non viene più utilizzata molto frequentemente.

In questo articolo troverai diversi modi per configurare Dispatcher e sistemi di pubblicazione. Cominciamo con alcune nozioni di base sulla memorizzazione in cache http.

![Funzionalità di base di una cache del Dispatcher](assets/chapter-1/basic-functionality-dispatcher.png)

*Funzionalità di base di una cache del Dispatcher*

<br> 

Le basi stesse del dispatcher sono spiegate qui. Il dispatcher è un semplice proxy di memorizzazione in cache inversa con la possibilità di ricevere e creare richieste HTTP. Un normale ciclo di richiesta/risposta fa così:

1. Un utente richiede una pagina
2. Il Dispatcher controlla se dispone già di una versione di cui è stato eseguito il rendering della pagina. Supponiamo che sia la prima richiesta per questa pagina e che Dispatcher non riesca a trovare una copia locale nella cache.
3. Il Dispatcher richiede la pagina dal sistema di pubblicazione
4. Nel sistema di pubblicazione, la pagina viene sottoposta a rendering da un modello JSP o HTL
5. La pagina viene restituita al Dispatcher
6. Il Dispatcher memorizza in cache la pagina
7. Il Dispatcher restituisce la pagina al browser
8. Se la stessa pagina viene richiesta una seconda volta, può essere servita direttamente dalla cache del Dispatcher senza la necessità di eseguirne nuovamente il rendering sull’istanza Publish. Questo consente di risparmiare tempo di attesa per i cicli utente e CPU sull’istanza Publish.

Stavamo parlando di &quot;pagine&quot; nell&#39;ultima sezione. Ma lo stesso schema si applica anche ad altre risorse come immagini, file CSS, download di PDF e così via.

#### Memorizzazione in cache dei dati

Il modulo Dispatcher sfrutta le funzionalità offerte dal server Apache in hosting. Risorse come pagine di HTML, download e immagini vengono memorizzate come file semplici nel file system Apache. È così semplice.

Il nome del file viene derivato dall’URL della risorsa richiesta. Se richiedi un file `/foo/bar.html` è memorizzato per esempio sotto /`var/cache/docroot/foo/bar.html`.

In linea di principio, se tutti i file sono memorizzati nella cache e quindi memorizzati statisticamente in Dispatcher, puoi estrarre il plug del sistema Publish e Dispatcher fungerebbe da semplice server web. Ma questo è solo per illustrare il principio. La vita reale è più complicata. Non è possibile memorizzare in cache tutto e la cache non è mai completamente &quot;piena&quot; in quanto il numero di risorse può essere infinito a causa della natura dinamica del processo di rendering. Il modello di un filesystem statico consente di generare un&#39;immagine approssimativa delle funzionalità del dispatcher. E aiuta a spiegare i limiti del dispatcher.

#### Struttura AEM URL e mappatura del file system

Per comprendere più dettagliatamente il Dispatcher, rivisitiamo la struttura di un semplice URL di esempio.  Diamo un&#39;occhiata all&#39;esempio seguente:

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&amp;otherparameter=value#fragment`

* `http` denota il protocollo

* `domain.com` è il nome di dominio

* `path/to/resource` è il percorso in cui la risorsa viene memorizzata in CRX e successivamente nel file system del server Apache

Da qui, le cose differiscono un po&#39; tra il filesystem AEM e il filesystem Apache.

In AEM,

* `pagename` è l’etichetta delle risorse

* `selectors` sta per una serie di selettori utilizzati in Sling per determinare come viene eseguito il rendering della risorsa. Un URL può avere un numero arbitrario di selettori. Sono separate da un punto. Una sezione di selettori potrebbe ad esempio essere qualcosa come &quot;french.mobile.cool&quot;. I selettori devono contenere solo lettere, cifre e trattini.

* `html` poiché è l’ultimo dei &quot;selettori&quot;, si chiama estensione . In AEM/Sling determina anche in parte lo script di rendering.

* `path/suffix.ext` è un’espressione simile a un percorso che può essere un suffisso per l’URL.  Può essere utilizzato negli script AEM per controllare ulteriormente la modalità di rendering di una risorsa. Avremo un&#39;intera sezione su questa parte più avanti. Per il momento, dovrebbe bastare sapere che può essere utilizzato come parametro aggiuntivo. I suffissi devono avere un&#39;estensione.

* `?parameter=value&otherparameter=value` è la sezione query dell’URL. Viene utilizzato per trasmettere parametri arbitrari a AEM. Gli URL con parametri non possono essere memorizzati nella cache e pertanto i parametri devono essere limitati ai casi in cui sono assolutamente necessari.

* `#fragment`, la parte del frammento di un URL non viene trasmessa a AEM viene utilizzata solo nel browser; nei framework JavaScript come &quot;parametri di indirizzamento&quot; o per passare a una determinata parte della pagina.

In Apache (*fai riferimento al diagramma seguente*),

* `pagename.selectors.html` viene utilizzato come nome del file nel file system della cache.

Se l’URL ha un suffisso `path/suffix.ext` quindi,

* `pagename.selectors.html` viene creato come cartella

* `path` una cartella `pagename.selectors.html` cartella

* `suffix.ext` è un file nel `path` cartella. Nota: Se il suffisso non ha un&#39;estensione, il file non viene memorizzato nella cache.

![Layout del file system dopo aver ricevuto gli URL dal Dispatcher](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png)

*Layout del file system dopo aver ricevuto gli URL dal Dispatcher*

<br> 

#### Limitazioni di base

La mappatura tra un URL, la risorsa e il nome del file è abbastanza semplice.

Potreste aver notato qualche trappola, tuttavia,

1. Gli URL possono diventare molto lunghi. Aggiunta della porzione &quot;percorso&quot; di un `/docroot` sul filesystem locale potrebbe facilmente superare i limiti di alcuni filesystem. L&#39;esecuzione del Dispatcher in NTFS su Windows può essere una sfida. Tuttavia sei sicuro con Linux.

2. Gli URL possono contenere caratteri speciali e umlaut. Questo di solito non è un problema per il dispatcher. Tieni presente tuttavia che l’URL viene interpretato in molte posizioni dell’applicazione. Più spesso, abbiamo visto strani comportamenti di un&#39;applicazione - solo per scoprire che un pezzo di codice raramente utilizzato (personalizzato) non è stato testato accuratamente per i caratteri speciali. Dovreste evitarle se potete. E se non potete, pianificate un test approfondito.

3. In CRX, le risorse hanno risorse secondarie. Ad esempio, una pagina avrà diverse sottopagine. Impossibile trovare una corrispondenza in un file system in quanto i file system dispongono di file o cartelle.

#### Gli URL senza estensione non vengono memorizzati nella cache

Gli URL devono sempre avere un&#39;estensione . Anche se puoi distribuire URL senza estensioni in AEM. Questi URL non verranno memorizzati nella cache in Dispatcher.

**Esempi**

`http://domain.com/home.html` è **memorizzabile in cache**

`http://domain.com/home` è **non memorizzabile in cache**

La stessa regola si applica quando l’URL contiene un suffisso. Il suffisso deve avere un&#39;estensione per essere memorizzabile nella cache.

**Esempi**

`http://domain.com/home.html/path/suffix.html` è **memorizzabile in cache**

`http://domain.com/home.html/path/suffix` è **non memorizzabile in cache**

Potreste chiedervi, cosa succede se la parte-risorsa non ha un&#39;estensione, ma il suffisso ne ha una? Beh, in questo caso l&#39;URL non ha alcun suffisso. Osserva l&#39;esempio seguente:

**Esempio**

`http://domain.com/home/path/suffix.ext`

La `/home/path/suffix` è il percorso della risorsa... quindi non esiste un suffisso nell’URL.

**Conclusione**

Aggiungi sempre estensioni sia al percorso che al suffisso. A volte, le persone consapevoli del SEO sostengono che questo ti sta classificando nei risultati delle ricerche. Ma una pagina non memorizzata nella cache sarebbe super lenta e si classificherebbe ancora più in basso.

#### URL di suffissi in conflitto

Considera di disporre di due URL validi

`http://domain.com/home.html`

e

`http://domain.com/home.html/suffix.html`

Sono assolutamente validi in AEM. Non vedrai alcun problema nel computer di sviluppo locale (senza Dispatcher). Molto probabilmente non si incontrerà alcun problema in UAT- o test di carico. Il problema che stiamo affrontando è così sottile che scivola attraverso la maggior parte dei test.  Ti colpirà duramente quando sei al momento del picco e sei limitato nel tempo per affrontarlo, probabilmente non hai accesso al server, né le risorse per correggerlo. Ci siamo stati...

Allora... qual è il problema?

`home.html` in un file system può essere un file o una cartella. Non entrambi nello stesso momento in AEM.

Se lo richiedi `home.html` in primo luogo, viene creato come file.

Richieste successive a `home.html/suffix.html` restituisce risultati validi, ma come file `home.html` &quot;blocca&quot; la posizione nel filesystem,  `home.html` non può essere creata una seconda volta come cartella e quindi `home.html/suffix.html` non è memorizzato nella cache.

![Posizione di blocco dei file nel file system che impedisce la memorizzazione in cache delle risorse secondarie](assets/chapter-1/file-blocking-position-in-filesystem.png)

*Posizione di blocco dei file nel file system che impedisce la memorizzazione in cache delle risorse secondarie*

<br> 

Se lo fai dall&#39;altra parte, prima richiedi `home.html/suffix.html` then `suffix.html` è memorizzato nella cache in una cartella `/home.html` all&#39;inizio. Tuttavia, questa cartella viene eliminata e sostituita da un file `home.html` quando successivamente richiedi `home.html` come risorsa.

![Eliminazione di una struttura del percorso quando un elemento padre viene recuperato come risorsa](assets/chapter-1/deleting-path-structure.png)

*Eliminazione di una struttura del percorso quando un elemento padre viene recuperato come risorsa*

<br> 

Pertanto, il risultato di ciò che è memorizzato nella cache è interamente casuale e a seconda dell’ordine delle richieste in arrivo. Ciò che rende le cose ancora più complicate, è il fatto che di solito hai più di un dispatcher. Inoltre, le prestazioni, il tasso di hit della cache e il comportamento possono variare in modo diverso da un Dispatcher all’altro. Se desideri scoprire perché il tuo sito web non risponde, devi essere sicuro di aver guardato il Dispatcher corretto con l’ordine di memorizzazione in cache sfortunato. Se stai guardando il Dispatcher che - per fortuna - ha un modello di richiesta più favorevole, ti perderai nel tentativo di trovare il problema.

#### Evitare URL in conflitto

È possibile evitare &quot;URL in conflitto&quot;, in cui un nome di cartella e un nome di file sono &quot;competitivi&quot; per lo stesso percorso nel filesystem, quando si utilizza un&#39;estensione diversa per la risorsa quando si dispone di un suffisso.

**Esempio**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Entrambi sono perfettamente memorizzabili nella cache,

![](assets/chapter-1/cacheable.png)

Scegliere un&#39;estensione dedicata &quot;dir&quot; per una risorsa quando si richiede un suffisso o si evita di utilizzare completamente il suffisso. Ci sono rari casi in cui sono utili. Ed è facile implementare correttamente questi casi.  Come vedremo nel prossimo capitolo quando parleremo di invalidazione e scaricamento della cache.

#### Richieste non memorizzabili nella cache

Rivediamo un rapido riassunto dell&#39;ultimo capitolo più alcune eccezioni. Dispatcher può memorizzare in cache un URL se è configurato come memorizzabile in cache e se si tratta di una richiesta GET. Non può essere memorizzato nella cache in una delle seguenti eccezioni.

**Richieste memorizzabili nella cache**

* La richiesta è configurata per essere memorizzabile nella cache nella configurazione di Dispatcher
* La richiesta è una richiesta di GET semplice

**Richieste o risposte non memorizzabili nella cache**

* Richiesta negata memorizzazione in cache dalla configurazione (percorso, pattern, tipo MIME)
* Risposte che restituiscono un &quot;Dispatcher: intestazione no-cache&quot;
* Risposta che restituisce un &quot;Cache-Control: intestazione no-cache|private&quot;
* Risposta che restituisce un &quot;Pragma: intestazione no-cache&quot;
* Richiesta con parametri di query
* URL senza estensione
* URL con un suffisso senza estensione
* Risposta che restituisce un codice di stato diverso da 200
* Richiesta POST

## Annullamento e scaricamento della cache

### Panoramica

L’ultimo capitolo elenca un gran numero di eccezioni, quando Dispatcher non è in grado di memorizzare in cache una richiesta. Ma ci sono altre cose da considerare: Solo perché Dispatcher _può_ memorizza in cache una richiesta, non significa necessariamente che _dovrebbe_.

Il punto è: Il caching di solito è facile. Dispatcher deve solo memorizzare il risultato di una risposta e restituirlo la prossima volta che la stessa richiesta viene ricevuta. Destra? Sbagliato!

La parte difficile è la _invalidazione_ o _scarico_ della cache. Dispatcher deve scoprirlo quando una risorsa è cambiata e deve essere nuovamente sottoposta a rendering.

Questo sembra essere un compito banale a prima vista.. ma non lo è. Ulteriori informazioni e informazioni sulle differenze tra risorse singole e semplici e pagine basate su una struttura a maglie elevate di più risorse.

### Risorse semplici e scaricamento

Abbiamo impostato il nostro sistema di AEM per creare dinamicamente una miniatura per ogni immagine quando richiesto con uno speciale selettore &quot;pollice&quot;:

`/content/dam/path/to/image.thumb.png`

E naturalmente forniamo un URL per distribuire l&#39;immagine originale con un URL senza selettore:

`/content/dam/path/to/image.png`

Scaricando entrambi, la miniatura e l&#39;immagine originale, finiremo con qualcosa come,

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

nel file system del nostro Dispatcher.

Ora, l’utente carica e attiva una nuova versione di quel file. Infine, una richiesta di annullamento della validità viene inviata da AEM al Dispatcher,

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

L&#39;annullamento della validità è così semplice: Una semplice richiesta GET a uno speciale URL &quot;/invalidate&quot; sul Dispatcher. Un corpo HTTP non è necessario, il &quot;payload&quot; è solo l’intestazione &quot;invalidate-path&quot;. Inoltre, il percorso invalidate nell’intestazione è la risorsa che AEM e non il file o i file memorizzati nella cache da Dispatcher. AEM solo delle risorse. Le estensioni, i selettori e i suffissi vengono utilizzati in fase di runtime quando viene richiesta una risorsa. AEM non esegue alcuna contabilità sui selettori utilizzati in una risorsa, pertanto il percorso della risorsa è tutto ciò che sa con certezza quando si attiva una risorsa.

Questo è sufficiente nel nostro caso. Se una risorsa è cambiata, possiamo tranquillamente presumere che siano cambiate anche tutte le rappresentazioni di tale risorsa. Nel nostro esempio, se l’immagine è cambiata, viene riprodotta anche una nuova miniatura.

Dispatcher può eliminare in modo sicuro la risorsa con tutti i rendering memorizzati nella cache. Farà qualcosa come,

`$ rm /content/dam/path/to/image.*`

rimozione `image.png` e `image.thumb.png` e tutte le altre rappresentazioni che corrispondono a quel pattern.

Molto semplice... purché si utilizzi una sola risorsa per rispondere a una richiesta.

### Riferimenti e contenuto incorporato

#### Problema relativo al contenuto scorrevole

A differenza delle immagini o di altri file binari caricati su AEM, le pagine HTML non sono animali solitari. Vivono in branchi e sono altamente interconnessi tra loro da collegamenti ipertestuali e riferimenti. Il semplice collegamento è innocuo, ma diventa difficile quando si parla di riferimenti a contenuti. Gli onnipresenti teaser o navigazione superiore sulle pagine sono riferimenti di contenuto.

#### Riferimenti ai contenuti e perché sono un problema

Diamo un esempio semplice. Un&#39;agenzia di viaggi ha una pagina web che promuove un viaggio in Canada. Questa promozione è disponibile nella sezione teaser in altre due pagine, nella pagina &quot;Home&quot; e in una pagina &quot;Programmi speciali invernali&quot;.

Poiché entrambe le pagine visualizzano lo stesso teaser, non sarebbe necessario chiedere all’autore di creare il teaser più volte per ciascuna pagina su cui deve essere visualizzato. Al contrario, la pagina di destinazione &quot;Canada&quot; riserva una sezione nelle proprietà della pagina per fornire le informazioni per il teaser, o meglio per fornire un URL che esegua il rendering del teaser nel suo complesso:

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

oppure

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

Solo su AEM funziona come charm, ma se utilizzi un’istanza di Dispatcher sull’istanza di pubblicazione accade qualcosa di strano.

Immaginate di aver pubblicato il vostro sito web. Il titolo sulla tua pagina Canada è &quot;Canada&quot;. Quando un visitatore richiede la pagina principale, che ha un riferimento teaser su tale pagina, il componente nella pagina &quot;Canada&quot; esegue il rendering di qualcosa di simile a

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

*in* la home page. La home page viene memorizzata da Dispatcher come file .html statico, incluso il teaser ed è in prima pagina nel file .

Ora l’addetto al marketing ha imparato che i titoli dei teaser devono essere utilizzabili. Quindi decide di cambiare il titolo da &quot;Canada&quot; a &quot;Visita Canada&quot;, e aggiorna anche l&#39;immagine.

Pubblica la pagina modificata &quot;Canada&quot; e rivede la home page pubblicata in precedenza per visualizzarne le modifiche. Ma qui non è cambiato niente. Mostra ancora il vecchio teaser. Lui controlla due volte lo &quot;Speciale Invernale&quot;. Questa pagina non è mai stata richiesta in precedenza e quindi non viene memorizzata nella cache statica del Dispatcher. Questa pagina è stata appena riprodotta da Pubblica e ora contiene il nuovo teaser &quot;Visita Canada&quot;.

![Dispatcher che memorizza il contenuto incluso non aggiornato nella home page](assets/chapter-1/dispatcher-storing-stale-content.png)

*Dispatcher che memorizza il contenuto incluso non aggiornato nella home page*

<br> 

Cos&#39;è successo? Dispatcher memorizza una versione statica di una pagina contenente tutto il contenuto e il markup disegnati da altre risorse durante il rendering.

Dispatcher, essendo un semplice server web basato su file system, è veloce ma anche relativamente semplice. Se una risorsa inclusa cambia, non se ne rende conto. Si aggrappa comunque al contenuto presente al momento del rendering della pagina che include .

La pagina &quot;Speciale invernale&quot; non è ancora stata sottoposta a rendering, quindi non esiste alcuna versione statica sul Dispatcher e viene quindi visualizzata con il nuovo teaser in quanto viene riprodotto di recente su richiesta.

Si potrebbe pensare che Dispatcher manterrà traccia di tutte le risorse che toccherà durante il rendering e lo scaricamento di tutte le pagine che hanno utilizzato questa risorsa, quando la risorsa cambia. Tuttavia, Dispatcher non esegue il rendering delle pagine. Il rendering viene eseguito dal sistema di pubblicazione. Dispatcher non sa quali risorse inserire in un file .html di cui è stato eseguito il rendering.

Non sei ancora convinto? Potreste pensare *&quot;deve esserci un modo per implementare un qualche tipo di monitoraggio delle dipendenze&quot;*. Beh, c&#39;è o c&#39;è più precisione *era*. Communiqué 3 il bisnonno di AEM aveva un tracker di dipendenza implementato nel _sessione_ utilizzato per il rendering di una pagina.

Durante una richiesta, ogni risorsa acquisita tramite questa sessione veniva tracciata come una dipendenza dell’URL di cui era in corso il rendering.

Ma si scoprì che tenere traccia delle dipendenze era molto costoso. Le persone hanno presto scoperto che il sito web è più veloce se hanno disattivato completamente la funzione di tracciamento delle dipendenze e si sono affidate al rendering di tutte le pagine html dopo la modifica di una pagina HTML. Inoltre, anche questo regime non era perfetto - ci sono stati una serie di insidie e eccezioni sulla strada. In alcuni casi, non stavi utilizzando la sessione predefinita delle richieste per ottenere una risorsa, ma una sessione di amministrazione per ottenere alcune risorse di supporto per eseguire il rendering di una richiesta. Queste dipendenze di solito non venivano tracciate e portavano a mal di testa e chiamate telefoniche al team di ops-team chiedendo di svuotare manualmente la cache. Siete stati fortunati se avevano una procedura standard per farlo. Ci sono stati altri gotchas lungo il percorso ma... smettiamola di ricordare. Questo porta indietro al 2005. Alla fine quella funzione è stata disattivata in Communiqué 4 per impostazione predefinita e non è tornata nel successore CQ5 che poi è diventato AEM.

### Annullamento automatico della validità

#### Quando Lo Scaricamento Completo È Più Economico Del Tracciamento Della Dipendenza

Poiché CQ5 ci affidiamo completamente all&#39;annullamento, più o meno, dell&#39;intero sito se cambia solo una delle pagine. Questa funzione è denominata &quot;Annullamento automatico della validità&quot;.

Ma ancora una volta - come può essere, che gettare via e ri-rendering centinaia di pagine è più economico che fare un corretto monitoraggio della dipendenza e un rendering parziale?

Ci sono due ragioni principali:

1. In un sito web medio, viene spesso richiesto solo un piccolo sottoinsieme di pagine. Quindi anche, se scarichi tutti i contenuti resi, solo qualche dozzina sarà effettivamente richiesto immediatamente dopo. Il rendering della coda lunga delle pagine può essere distribuito nel tempo, quando vengono effettivamente richieste. In realtà, il carico sulle pagine di rendering non è alto come ci si aspetterebbe. Naturalmente, ci sono sempre delle eccezioni... discuteremo alcuni trucchi su come gestire in modo equamente distribuito loady su siti web più grandi con cache di Dispatcher vuote, più tardi.

2. Tutte le pagine sono comunque collegate dalla navigazione principale. Quindi quasi tutte le pagine alla fine dipendono l&#39;una dall&#39;altra. Questo significa che anche il tracker della dipendenza più intelligente scoprirà ciò che già sappiamo: Se una delle pagine cambia, devi annullare la validità di tutte le altre.

Non ci credete? Illustriamo l&#39;ultimo punto.

Stiamo utilizzando lo stesso argomento dell’ultimo esempio con teaser che fanno riferimento al contenuto di una pagina remota. Solo ora stiamo usando un esempio più estremo: Una navigazione principale di cui è stato eseguito il rendering automatico. Come per il teaser, il titolo di navigazione viene tracciato dalla pagina collegata o &quot;remota&quot; come riferimento di contenuto. I titoli di navigazione remota non vengono memorizzati nella pagina attualmente sottoposta a rendering. È necessario ricordare che la navigazione viene riprodotta in ogni pagina del sito web. Quindi, il titolo di una pagina viene utilizzato più e più volte su tutte le pagine che hanno una navigazione principale. E se desideri modificare un titolo di navigazione, devi farlo una sola volta nella pagina remota, non in ogni pagina che fa riferimento alla pagina.

Nel nostro esempio, la navigazione mescola tutte le pagine utilizzando il &quot;NavTitle&quot; della pagina di destinazione per rappresentare un nome nella navigazione. Il titolo di navigazione per &quot;Islanda&quot; viene disegnato dalla pagina &quot;Islanda&quot; e riprodotto in ogni pagina che ha una navigazione principale.

![Navigazione principale inevitabilmente mescolando il contenuto di tutte le pagine con il loro &quot;NavTitles&quot;](assets/chapter-1/nav-titles.png)

*Navigazione principale inevitabilmente mescolando il contenuto di tutte le pagine con il loro &quot;NavTitles&quot;*

<br> 

Se si cambia il NavTitle sulla pagina Islanda da &quot;Islanda&quot; a &quot;Bellissima Islanda&quot; quel titolo cambia immediatamente su tutte le altre pagine menu principale. Pertanto, le pagine sottoposte a rendering e memorizzate nella cache prima di tale modifica diventano tutte obsolete e devono essere invalidate.

#### Implementazione dell’annullamento automatico della validità: Il file .stat

Ora, se avete un sito grande con migliaia di pagine, ci vorrebbe un bel po&#39; di tempo per scorrere tutte le pagine e cancellarle fisicamente. Durante questo periodo, Dispatcher poteva distribuire involontariamente contenuti non aggiornati. Ancora peggio, potrebbero verificarsi alcuni conflitti durante l’accesso ai file di cache, forse viene richiesta una pagina mentre viene semplicemente eliminata o una pagina viene nuovamente eliminata a causa di una seconda invalidazione che si verifica dopo un’attivazione successiva immediata. Considerate quale disordine sarebbe. Fortunatamente non è così. Dispatcher utilizza un trucco intelligente per evitare che: Invece di eliminare centinaia e migliaia di file, inserisce un file semplice e vuoto nella directory principale del file system quando un file viene pubblicato e quindi tutti i file dipendenti vengono considerati non validi. Questo file è denominato &quot;statfile&quot;. Lo statfile è un file vuoto. Ciò che conta dello statfile è solo la data di creazione.

Tutti i file nel dispatcher con una data di creazione più vecchia dello statfile sono stati sottoposti a rendering prima dell’ultima attivazione (e invalidazione) e vengono quindi considerati &quot;non validi&quot;. Sono ancora fisicamente presenti nel filesystem, ma il Dispatcher li ignora. Sono &quot;stantio&quot;. Ogni volta che viene effettuata una richiesta a una risorsa obsoleta, Dispatcher chiede al sistema AEM di eseguire nuovamente il rendering della pagina. La nuova pagina di cui è stato effettuato il rendering viene quindi memorizzata nel filesystem, ora con una nuova data di creazione ed è nuovamente aggiornata.

![La data di creazione del file .stat definisce quale contenuto è obsoleto e che è fresco](assets/chapter-1/creation-date.png)

*La data di creazione del file .stat definisce quale contenuto è obsoleto e che è fresco*

<br> 

Puoi chiederti perché si chiama &quot;.stat&quot;? E non forse &quot;.invalidate&quot;? Beh, potete immaginare, avere quel file nel vostro filesystem aiuta il Dispatcher a determinare quali risorse potrebbero essere *statisticamente* viene servito come da un server web statico. Non è più necessario eseguire il rendering dinamico di questi file.

La vera natura del nome, tuttavia, è meno metaforica. È derivato dalla chiamata di sistema Unix `stat()`, che restituisce il tempo di modifica di un file (tra le altre proprietà).

#### Mixaggio Convalida semplice e automatica

Ma aspetta... prima abbiamo detto che le singole risorse vengono fisicamente eliminate. Ora diciamo che uno stato più recente li renderebbe praticamente non validi agli occhi del Dispatcher. Perché poi la cancellazione fisica, prima?

La risposta è semplice. In genere si utilizzano entrambe le strategie in parallelo, ma per diversi tipi di risorse. Le risorse binarie, come le immagini, sono autonome. Non sono collegati ad altre risorse nel senso che hanno bisogno che le loro informazioni siano rese.

Le pagine HTML, invece, sono altamente interdipendenti. Quindi, si applicherebbe l&#39;annullamento automatico della validità su queste cose. Questa è l’impostazione predefinita in Dispatcher. Tutti i file appartenenti a una risorsa invalidata vengono eliminati fisicamente. Inoltre, i file che terminano con &quot;.html&quot; vengono invalidati automaticamente.

Dispatcher decide in merito all’estensione del file, se applicare o meno lo schema di annullamento automatico della validità.

Le fine del file per l’annullamento automatico della validità sono configurabili. In teoria puoi includere tutte le estensioni per l’annullamento automatico della validità. Ma tenete presente che questo ha un prezzo molto alto. Non vedrai le risorse non aggiornate consegnate in modo non intenzionale, ma le prestazioni di consegna si degradano notevolmente a causa dell’eccesso di invalidazione.

Immagina, ad esempio, di implementare uno schema in cui PNG e JPG vengono resi in modo dinamico e dipendono da altre risorse per farlo. Potrebbe essere utile ridimensionare le immagini ad alta risoluzione in una risoluzione più piccola compatibile con il web. Mentre ci si trova cambia anche il tasso di compressione. La risoluzione e il tasso di compressione in questo esempio non sono costanti fisse ma parametri configurabili nel componente che utilizza l&#39;immagine. Ora, se questo parametro viene modificato, è necessario annullare la validità delle immagini.

Nessun problema - abbiamo appena imparato che possiamo aggiungere immagini all&#39;annullamento automatico della validità e avere sempre immagini appena riprodotte ogni volta che cambia qualcosa.

#### Gettare fuori il bambino con l&#39;acqua di bagno

Esatto - ed è un problema enorme. Leggi di nuovo l&#39;ultimo paragrafo. &quot;...immagini appena riprodotte ogni volta che cambia qualcosa.&quot; Come sapete, un buon sito web viene cambiato costantemente; aggiungere nuovi contenuti, correggere un errore ortografico, modificare un teaser in un altro punto. Ciò significa che tutte le immagini vengono invalidate costantemente e devono essere riprodotte. Non sottovalutatelo. Il rendering e il trasferimento dinamico dei dati immagine funziona in millisecondi sul computer di sviluppo locale. Il vostro ambiente di produzione deve farlo un centinaio di volte più spesso - al secondo.

E siamo chiari: i vostri jpgs devono essere riprodotti, quando una pagina HTML cambia e viceversa. Esiste un solo &quot;bucket&quot; di file da invalidare automaticamente. Viene scaricato nel suo insieme. Senza alcun mezzo di scomposizione in ulteriori strutture dettagliate.

C&#39;è una buona ragione per cui l&#39;annullamento automatico della validità viene mantenuto su &quot;.html&quot; per impostazione predefinita. L&#39;obiettivo è mantenere quel secchio il più piccolo possibile. Non buttare via il bambino con l&#39;acqua da bagno semplicemente invalidando tutto - solo per essere sul lato sicuro.

Le risorse autonome devono essere servite sul percorso di tale risorsa. Questo aiuta molto l&#39;invalidazione. Semplificalo, non creare schemi di mappatura come &quot;risorsa /a/b/c&quot; è servito da &quot;/x/y/z&quot;. Assicurati che i componenti funzionino con le impostazioni di annullamento automatico della validità di Dispatcher predefinite. Non cercare di riparare un componente progettato in modo non corretto con l’annullamento della validità in eccesso in Dispatcher.

##### Eccezioni all’annullamento automatico della validità: Annullamento della validità di ResourceOnly

La richiesta di invalidazione per il Dispatcher di solito viene attivata dal sistema o dai sistemi di pubblicazione da un agente di replica.

Se ti senti molto sicuro delle tue dipendenze, puoi provare a creare un tuo agente di replica invalidante.

Sarebbe un po&#39; oltre questa guida andare nei dettagli, ma vogliamo darvi almeno qualche suggerimento.

1. So davvero cosa stai facendo. Ottenere il diritto di invalidazione è davvero difficile. Questo è uno dei motivi per cui l&#39;annullamento automatico della validità è così rigoroso; per evitare di consegnare contenuti obsoleti.

2. Se l’agente invia un’intestazione HTTP `CQ-Action-Scope: ResourceOnly`Ciò significa che questa singola richiesta di invalidazione non attiva un’annullamento automatico della validità. Questo ( [https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) un pezzo di codice potrebbe essere un buon punto di partenza per il tuo agente di replica.

3. `ResourceOnly`, impedisce solo l’annullamento automatico della validità. Per eseguire effettivamente la risoluzione e gli invalidamenti della dipendenza necessari, è necessario attivare autonomamente le richieste di invalidazione. Controlla le regole di svuotamento del dispatcher del pacchetto ([https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)) per ispirarsi a come potrebbe effettivamente accadere.

Non è consigliabile creare uno schema di risoluzione delle dipendenze. Ci sono troppi sforzi e pochi guadagni - e come detto prima, c&#39;è troppo che vi sbagliate.

Invece, devi scoprire quali risorse non hanno dipendenze da altre risorse e possono essere invalidate senza annullamento automatico della validità. Tuttavia, non è necessario utilizzare un agente di replica personalizzato per tale questione. Crea una regola personalizzata nella configurazione di Dispatcher che esclude queste risorse dall’annullamento automatico della validità.

Abbiamo detto che la navigazione principale o i teaser sono una fonte di dipendenze. Beh - se carichi la navigazione e i teaser in modo asincrono o li includi con uno script SSI in Apache, non avrai quella dipendenza da tracciare. Approfondiremo il caricamento asincrono dei componenti più avanti in questo documento quando parliamo di &quot;Sling Dynamic Includes&quot;.

Lo stesso vale per le finestre pop-up o i contenuti caricati in una Lightbox. Questi pezzi hanno anche raramente navigazioni (ovvero, &quot;dipendenze&quot;) e possono essere invalidati come una singola risorsa.

## Creazione di componenti con Dispatcher in Mind

### Applicazione della meccanica del Dispatcher in un esempio reale

Nell&#39;ultimo capitolo abbiamo spiegato come funziona la meccanica di base del Dispatcher, come funziona in generale e quali sono i limiti.

Ora vogliamo applicare queste tecniche a un tipo di componenti che molto probabilmente troverete nei requisiti del vostro progetto. Scegliamo il componente deliberatamente per dimostrare i problemi che dovrai affrontare prima o poi. Non temete - non tutte le componenti hanno bisogno di quella quantità di considerazione che presenteremo. Ma se vedete la necessità di costruire un tale componente, siete ben consapevoli delle conseguenze e sapete come gestirle.

### Pattern del componente di spooling (anti)

#### Componente immagine reattiva

Illustriamo un pattern comune (o antipattern) di un componente con binari interconnessi. Creeremo un componente &quot;respi&quot; - per &quot;responsive-image&quot;. Questo componente deve essere in grado di adattare l&#39;immagine visualizzata al dispositivo su cui viene visualizzata. Su desktop e tablet mostra la piena risoluzione dell&#39;immagine, su telefoni una versione più piccola con un ritaglio stretto - o forse anche un motivo completamente diverso (questa è chiamata &quot;direzione dell&#39;arte&quot; nel mondo dinamico).

Le risorse vengono caricate nell’area DAM di AEM e solo _referenza_ nel componente immagine dinamica.

Il componente respi si occupa sia del rendering del markup che della distribuzione dei dati immagine binari.

Il modo in cui lo implementiamo qui è un modello comune che abbiamo visto in molti progetti e anche uno dei componenti di base AEM si basa su questo modello. Pertanto, è molto probabile che uno sviluppatore possa adattare quel modello. Ha le sue macchie dolci in termini di incapsulamento, ma richiede molto sforzo per farlo Dispatcher-ready. Discuteremo diverse opzioni su come attenuare il problema in un secondo momento.

Chiamiamo il modello utilizzato qui il &quot;Pattern di Spooler&quot;, perché il problema risale ai primi giorni del Communiqué 3 dove c&#39;era un metodo &quot;spool&quot; che poteva essere chiamato su una risorsa per lo streaming dei suoi dati grezzi binari nella risposta.

Il termine originale &quot;spooling&quot; in realtà si riferisce a periferiche offline condivise lente, come le stampanti, quindi non viene applicato correttamente qui. Ma ci piace comunque il termine perché raramente è così distinguibile nel mondo online. E ogni modello dovrebbe comunque avere un nome distinguibile, giusto? Sta a voi decidere se si tratta di un modello o di un anti-pattern.

#### Implementazione

Di seguito è illustrata l’implementazione del componente per immagini reattive:

Il componente ha due parti; la prima parte esegue il rendering del markup HTML dell&#39;immagine, la seconda parte &quot;spool&quot; i dati binari dell&#39;immagine di riferimento. Poiché si tratta di un sito web moderno con un design reattivo, non stiamo eseguendo il rendering di un semplice sito web `<img src"…">` , ma un set di immagini in `<picture/>` tag . Per ogni dispositivo, carichiamo due immagini diverse nel DAM e le facciamo riferimento dal nostro componente immagine.

Il componente dispone di tre script di rendering (implementati in JSP, HTL o come servlet) ciascuno con un selettore dedicato:

1. `/respi.jsp` - senza selettore per eseguire il rendering del markup HTML
2. `/respi.img.java` per eseguire il rendering della versione desktop
3. `/respi.img.mobile.java` per eseguire il rendering della versione mobile.


Il componente viene posizionato nel parsys della home page. La struttura risultante nel CRX è illustrata di seguito.

![Struttura delle risorse dell&#39;immagine reattiva nel CRX](assets/chapter-1/responsive-image-crx.png)

*Struttura delle risorse dell&#39;immagine reattiva nel CRX*

<br> 

Viene eseguito il rendering del markup dei componenti in questo modo,

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

e... abbiamo finito con il nostro componente ben incapsulato.

#### Componente immagine reattiva in azione

Ora un utente richiede la pagina e le risorse tramite Dispatcher. Questo si traduce in file nel file system del Dispatcher come illustrato di seguito,

![Struttura in cache del componente immagine reattiva incapsulata](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Struttura in cache del componente immagine reattiva incapsulata*

<br> 

Considera un utente che carica e attiva una nuova versione delle due immagini floreali sul DAM. AEM invierà in base a una richiesta di invalidazione per

`/content/dam/flower.jpg`

e

`/content/dam/flower-mobile.jpg`

al Dispatcher. Tuttavia, queste richieste sono vane. I contenuti sono stati memorizzati nella cache come file sotto la sottostruttura del componente. Questi file sono ora obsoleti ma vengono comunque serviti in caso di richieste.

![Disallineamento della struttura che porta a contenuti obsoleti](assets/chapter-1/structure-mismatch.png)

*Disallineamento della struttura che porta a contenuti obsoleti*

<br> 

C&#39;è un&#39;altra avvertenza a questo approccio. Considera di utilizzare lo stesso fiore.jpg su più pagine. Quindi la stessa risorsa sarà memorizzata nella cache in più URL o file,

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Ogni volta che viene richiesta una pagina nuova e non memorizzata nella cache, le risorse vengono recuperate da AEM in URL diversi. Nessuna memorizzazione in cache del Dispatcher e nessuna memorizzazione in cache del browser possono velocizzare la consegna.

#### Dove risplende il motivo dello spooler

C&#39;è un&#39;eccezione naturale, dove questo modello anche nella sua forma semplice è utile: Se il binario è memorizzato nel componente stesso e non nel DAM. Questo tuttavia è utile solo per le immagini utilizzate una volta sul sito web e non archiviare le risorse nel DAM significa che si ha difficoltà a gestire le risorse. Immagina che la tua licenza di utilizzo per una particolare risorsa sia scaduta. Come puoi scoprire quali componenti hai utilizzato la risorsa?

Vedete? La &quot;M&quot; in DAM sta per &quot;Management&quot;, come in Digital Asset Management. Non volete rinunciare a quella funzione.

#### Conclusione

Dal punto di vista di uno sviluppatore AEM il modello sembrava super elegante. Ma con il Dispatcher inserito nell&#39;equazione, potreste essere d&#39;accordo, che l&#39;approccio ingenuo potrebbe non essere sufficiente.

Lasciamo a voi decidere se per il momento si tratta di un modello o di un anti-modello. E forse avete già delle buone idee in mente come mitigare i problemi descritti sopra? Bene. Allora dovreste essere ansiosi di vedere come altri progetti hanno risolto questi problemi.

### Risoluzione dei problemi comuni di Dispatcher

#### Panoramica

Parliamo di come potrebbe essere stato implementato un po&#39; più a portata di cache. Ci sono diverse opzioni. A volte non è possibile scegliere la soluzione migliore. Forse sei in un progetto già in esecuzione e hai un budget limitato per risolvere il &quot;problema della cache&quot; a portata di mano e non abbastanza per fare un refactoring completo. Oppure affronti un problema, che è più complesso del componente immagine di esempio.

Descriveremo i principi e le avvertenze nelle sezioni seguenti.

Ancora una volta, questo si basa sull&#39;esperienza reale. Abbiamo già visto tutti questi schemi allo stato selvatico, quindi non è un esercizio accademico. Questo è il motivo per cui vi stiamo mostrando alcuni anti-modelli, così avete la possibilità di imparare dagli errori che altri hanno già fatto.

#### Cache killer

>[!WARNING]
>
>Questo è un anti-pattern. Non usarlo. Mai.

Hai mai visto parametri di query come `?ck=398547283745`? Sono chiamati cache-killer (&quot;ck&quot;). L’idea è che se aggiungi un parametro di query, la risorsa non verrà memorizzata nella cache. Inoltre, se aggiungi un numero casuale come valore del parametro (come &quot;398547283745&quot;), l’URL diventa univoco e ti assicuri che nessun’altra cache tra il sistema di AEM e lo schermo sia in grado di memorizzare nella cache. Solitamente i sospetti intermedi sarebbero una cache &quot;Varnish&quot; di fronte al Dispatcher, un CDN o anche la cache del browser. Ancora: Non farlo. Desideri che le risorse siano memorizzate nella cache il più a lungo possibile. La cache è vostra amica. Non uccidere gli amici.

#### Annullamento automatico della validità

>[!WARNING]
>
>Questo è un anti-pattern. Evita di utilizzarlo per le risorse digitali. Prova a mantenere la configurazione predefinita del Dispatcher, che > è l’annullamento automatico della validità per i file &quot;.html&quot;, solo

A breve termine, puoi aggiungere &quot;.jpg&quot; e &quot;.png&quot; alla configurazione di annullamento automatico della validità in Dispatcher. Ciò significa che, ogni volta che si verifica un’invalidazione, è necessario eseguire nuovamente il rendering di tutti i formati &quot;.jpg&quot;, &quot;.png&quot; e &quot;.html&quot;.

Questo modello è super facile implementato se i proprietari aziendali lamentano di non vedere i loro cambiamenti si materializzano sul sito live abbastanza velocemente. Ma questo può offrirvi solo un po&#39; di tempo per trovare una soluzione più sofisticata.

Assicurati di comprendere gli ampi impatti delle prestazioni. Questo rallenterà notevolmente il tuo sito web e potrebbe anche influire sulla stabilità - se il tuo sito è un sito web ad alto carico con frequenti modifiche - come un portale di notizie.

#### impronta digitale URL

L&#39;impronta digitale di un URL sembra un&#39;unità cache-killer. Ma non lo è. Non è un numero casuale, ma un valore che caratterizza il contenuto della risorsa. Può trattarsi di un hash del contenuto della risorsa o, ancora più semplice, di una marca temporale al momento del caricamento, della modifica o dell’aggiornamento della risorsa.

Una marca temporale Unix è sufficientemente valida per un’implementazione nel mondo reale. Per una migliore leggibilità, in questa esercitazione viene utilizzato un formato più leggibile: `2018 31.12 23:59 or fp-2018-31-12-23-59`.

L’impronta digitale non deve essere utilizzata come parametro di query, in quanto gli URL con parametri di query non possono essere memorizzati nella cache. È possibile utilizzare un selettore o il suffisso per l&#39;impronta digitale.

Supponiamo, il file `/content/dam/flower.jpg` ha `jcr:lastModified` data del 31 dicembre 2018, 23:59. L’URL con l’impronta digitale è `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg`.

Questo URL rimane stabile, purché la risorsa a cui si fa riferimento (`flower.jpg`) non viene modificato. Può quindi essere memorizzato nella cache per un periodo di tempo indefinito e non è un killer di cache.

Nota: questo URL deve essere creato e servito dal componente immagine reattiva. Non è una funzionalità preconfigurata AEM.

Questo è il concetto di base. Ci sono però alcuni dettagli che potrebbero essere facilmente trascurati.

Nel nostro esempio, il componente è stato sottoposto a rendering e memorizzato nella cache a 23:59. Ora l&#39;immagine è stata modificata, diciamo alle 00:00.  Il componente _avrebbe_ genera un nuovo URL con impronta digitale nel relativo markup.

Potreste pensarci _dovrebbe_... ma non lo è. Poiché è stato modificato solo il binario dell’immagine e la pagina che include non è stata toccata, non è necessario eseguire nuovamente il rendering del markup HTML. Dispatcher serve quindi la pagina con la vecchia impronta digitale e quindi la vecchia versione dell’immagine.

![Componente immagine più recente dell’immagine di riferimento, senza rendering di nuova impronta digitale.](assets/chapter-1/recent-image-component.png)

*Componente immagine più recente dell’immagine di riferimento, senza rendering di nuova impronta digitale.*

<br> 

Ora, se riattivi la home page (o qualsiasi altra pagina del sito) lo statfile verrebbe aggiornato, Dispatcher considererà il file home.html obsoleto e lo rieseguirà con una nuova impronta digitale nel componente immagine.

Ma non abbiamo attivato la home page, giusto? E perché dovremmo attivare una pagina senza toccarla comunque? Inoltre, forse non disponiamo dei diritti sufficienti per attivare le pagine o il flusso di lavoro di approvazione richiede molto tempo e molto tempo, e non possiamo farlo con un preavviso limitato. Allora - cosa fare?

#### Lo strumento dell&#39;amministratore pigro - Diminuzione dei livelli di statfile

>[!WARNING]
>
>Questo è un anti-pattern. Utilizzalo solo a breve termine per acquistare un po&#39; di tempo e trovare una soluzione più sofisticata.

L&#39;amministratore pigro di solito &quot;_imposta l’annullamento automatico della validità su jpgs e lo statfile-level su zero, per risolvere sempre problemi di caching di tutti i tipi_.&quot; Troverai questo consiglio nei forum tecnologici e ti aiuta a risolvere il tuo problema di invalidazione.

Finora non abbiamo discusso a livello di statfile. In pratica, l’annullamento automatico della validità funziona solo per i file della stessa struttura secondaria. Tuttavia, il problema è che le pagine e le risorse di solito non vivono nella stessa struttura secondaria. Le pagine si trovano sotto `/content/mysite` considerando che gli attivi vivono sotto `/content/dam`.

Il &quot;livello statfile&quot; definisce dove a quale profondità sono i nodi principali dei sottoalberi. Nell&#39;esempio sopra il livello sarebbe &quot;2&quot; (1=/content, 2=/mysite,dam)

L’idea di &quot;diminuire&quot; il livello dello statfile a 0 consiste fondamentalmente nel definire l’intera struttura /content come una e unica sottostruttura per rendere le pagine e le risorse live nello stesso dominio di annullamento automatico della validità. Quindi avremmo solo su grande albero a livello (al docroot &quot;/&quot;). Ma in questo modo tutti i siti sul server vengono automaticamente invalidati ogni volta che viene pubblicato qualcosa, anche su siti completamente indipendenti. Fidati di noi: Questa è una cattiva idea a lungo termine, perché degraderai gravemente il tasso di hit della cache complessiva. Tutto ciò che è possibile fare è sperare che i server AEM abbiano abbastanza potenza di fuoco per funzionare senza cache.

In seguito potrai comprendere tutti i vantaggi di livelli di stato più profondi.

#### Implementazione di un agente di invalidazione personalizzato

In ogni caso, dobbiamo informare il Dispatcher in qualche modo, per annullare la validità delle HTML-Pages se un &quot;.jpg&quot; o &quot;.png&quot; è stato modificato per consentire il rendering con un nuovo URL.

Ciò che abbiamo visto nei progetti è, per esempio, agenti di replica speciali sul sistema di pubblicazione che inviano richieste di invalidazione per un sito ogni volta che un&#39;immagine di quel sito viene pubblicata.

In questo caso è molto utile se puoi ricavare il percorso del sito dal percorso della risorsa tramite convenzione di denominazione.

In generale, è consigliabile associare i siti e i percorsi delle risorse in questo modo:

**Esempio**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

In questo modo il tuo agente di svuotamento del Dispatcher personalizzato potrebbe facilmente inviare e invalidare la richiesta a /content/site-a quando incontra una modifica su `/content/dam/site-a`.

In realtà, non importa quale percorso dici a Dispatcher di annullare la validità - purché si trovi nello stesso sito, nello stesso &quot;sottoalbero&quot;. Non è nemmeno necessario utilizzare un vero percorso di risorse. Può essere anche &quot;virtuale&quot;:

```
GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy
```

![](assets/chapter-1/resource-path.png)

1. Un listener sul sistema di pubblicazione viene attivato quando un file nel DAM cambia

2. Il listener invia una richiesta di invalidazione al Dispatcher. A causa dell’annullamento automatico della validità, non è importante quale percorso inviamo nell’annullamento automatico della validità, a meno che non si trovi nella home page del sito - o sia più preciso a livello di stato del sito.

3. Lo statfile viene aggiornato.

4. La volta successiva, la pagina principale viene richiesta e viene riprodotta. La nuova impronta digitale/data viene ricavata dalla proprietà lastModified dell&#39;immagine come selettore aggiuntivo

5. Questo crea implicitamente un riferimento a una nuova immagine

6. Se l’immagine è effettivamente richiesta, viene creato e memorizzato un nuovo rendering in Dispatcher


#### Necessità di pulizia

Few. Completato. Urrà!

Beh... non ancora del tutto.

Il percorso,

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

non si riferisce ad alcuna delle risorse invalidate. Ricorda? Abbiamo solo invalidato una risorsa &quot;fittizia&quot; e ci siamo affidati all’annullamento automatico della validità per considerare &quot;home&quot; non valido. L&#39;immagine stessa potrebbe non essere mai _fisicamente_ soppresso. Quindi, la cache crescerà, crescerà e crescerà. Quando le immagini vengono modificate e attivate, ottengono nuovi nomi di file nel file system del Dispatcher.

Ci sono tre problemi con non eliminare fisicamente i file memorizzati nella cache e mantenerli indefinitamente:

1. Si sta sprecando la capacità di stoccaggio - ovviamente. Concesso - lo stoccaggio è diventato più economico e meno costoso negli ultimi anni. Negli ultimi anni, però, sono cresciute anche le risoluzioni e le dimensioni dei file, con l&#39;avvento di display simili alla retina, che hanno fame di immagini nitide.

2. Anche se i dischi rigidi sono diventati più economici, lo &quot;storage&quot; potrebbe non essere diventato più economico. Abbiamo visto una tendenza a non avere (a buon mercato) lo spazio di archiviazione a nudo dell&#39;hard disk di metallo ma noleggiare lo storage virtuale su un NAS da parte del fornitore del centro dati. Questo tipo di storage è un po&#39; più affidabile e scalabile ma anche un po&#39; più costoso. Potrebbe non voler sprecarlo immagazzinando rifiuti obsoleti. Questo non riguarda solo lo storage principale, ma anche i backup. Se disponi di una soluzione di backup preconfigurata, potresti non essere in grado di escludere le directory della cache. Alla fine stai anche eseguendo il backup dei dati dei rifiuti.

3. Ancora peggio: Avreste potuto acquistare licenze di utilizzo per determinate immagini solo per un periodo di tempo limitato, purché necessarie. Ora, se si archivia ancora l&#39;immagine dopo la scadenza di una licenza, ciò potrebbe essere visto come una violazione del copyright. Potresti non utilizzare più l’immagine nelle tue pagine web, ma Google le troverà comunque.

Quindi, finalmente, ti verrà fuori un po&#39; di lavoro domestico per pulire tutti i file più vecchi di... diciamo una settimana per tenere questo tipo di pulizia sotto controllo.

#### Abuso delle impronte digitali URL per attacchi di tipo Denial of Service

Ma aspetta, c&#39;è un altro difetto in questa soluzione:

Stiamo abusando di un selettore come parametro: fp-2018-31-12-23-59 viene generato dinamicamente come una sorta di &quot;cache-killer&quot;. Ma forse un bambino annoiato (o un crawler del motore di ricerca che è diventato selvaggio) inizia a richiedere le pagine:

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Ogni richiesta bypassa il Dispatcher, causando il caricamento su un&#39;istanza Publish. E, peggio ancora, crea un file corrispondente sul Dispatcher.

Quindi... invece di usare l&#39;impronta digitale come un semplice cache-killer, dovresti controllare la data jcr:lastModified dell&#39;immagine e restituire un 404 se non è la data prevista. Ci vuole un po&#39; di tempo e cicli della CPU sul sistema di pubblicazione... che è quello che si voleva evitare in primo luogo.

#### Avvertenze sulle impronte digitali URL nelle versioni ad alta frequenza

Puoi utilizzare lo schema di impronte digitali non solo per le risorse provenienti da DAM, ma anche per i file JS e CSS e le risorse correlate.

[Clientlibs con versione](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) è un modulo che utilizza questo approccio.

Ma qui si può affrontare un altro avvertimento con le impronte digitali URL: Collega l’URL al contenuto. Non puoi modificare il contenuto senza modificare anche l’URL (ovvero, aggiornare la data di modifica). In primo luogo, questo è l&#39;obiettivo per cui le impronte digitali sono progettate. Ma prendi in considerazione una nuova versione, con nuovi file CSS e JS e quindi nuovi URL con nuove impronte digitali. Tutte le pagine HTML contengono ancora riferimenti ai vecchi URL stampati. Quindi, per far funzionare in modo coerente la nuova versione, è necessario annullare la validità di tutte le pagine HTML contemporaneamente per forzare un nuovo rendering con riferimenti ai file appena impronte digitali. Se hai più siti che fanno affidamento sulle stesse librerie, questo può essere un notevole quantitativo di rendering - e qui non puoi sfruttare il `statfiles`. Preparati quindi a visualizzare i picchi di carico sui sistemi di pubblicazione dopo un rollout. Potresti considerare una distribuzione blu-verde con riscaldamento della cache o forse una cache basata su TTL davanti al tuo Dispatcher ... le possibilità sono infinite.

#### Una breve pausa

Wow - Ci sono molti dettagli da considerare, giusto? E rifiuta di essere compreso, testato e sottoposto a debug facilmente. E tutto per una soluzione apparentemente elegante. Certo, è elegante, ma solo da una prospettiva AEM. Insieme al Dispatcher diventa brutto.

Tuttavia, non risolve un problema di base, se un&#39;immagine viene utilizzata più volte su pagine diverse, viene memorizzata nella cache sotto tali pagine. Non c&#39;è molta sinergia nella memorizzazione in cache.

In generale, l&#39;impronta digitale degli URL è un buon strumento per avere nel tuo toolkit, ma è necessario applicarlo con attenzione, perché può causare nuovi problemi mentre risolve solo alcuni esistenti.

Quindi... era un capitolo lungo. Ma abbiamo visto questo modello così spesso, che ci è sembrato necessario darvi un quadro completo con tutti i pro e i contro. Le impronte digitali URL risolvono alcuni dei problemi inerenti al modello Spooler, ma lo sforzo di implementazione è abbastanza alto e devi considerare anche altre soluzioni più facili. Consigliamo di verificare sempre se è possibile basare gli URL sui percorsi delle risorse servite e non avere un componente intermedio. Lo affronteremo nel prossimo capitolo.

##### Risoluzione dipendenza runtime

Risoluzione della dipendenza runtime è un concetto che abbiamo considerato in un unico progetto. Ma pensarla così è diventata abbastanza complessa e abbiamo deciso di non implementarla.

Ecco l&#39;idea di base:

Dispatcher non è a conoscenza delle dipendenze delle risorse. Sono solo un mucchio di file singoli con una piccola semantica.

AEM anche poche informazioni sulle dipendenze. Manca di una semantica adeguata o di un &quot;rilevatore di dipendenza&quot;.

AEM alcuni riferimenti. Utilizza questa conoscenza per avvisarti quando tenti di eliminare o spostare una pagina o una risorsa di riferimento. A tale scopo, esegue una query sulla ricerca interna durante l’eliminazione di una risorsa. I riferimenti al contenuto hanno una forma molto particolare. Sono espressioni del percorso che iniziano con &quot;/content&quot;. Quindi, possono facilmente essere indicizzati full-text - e interrogati per quando necessario.

Nel nostro caso, avremmo bisogno di un agente di replica personalizzato sul sistema Publish, che attivi una ricerca per un percorso specifico quando quel percorso è cambiato.

Diciamo

`/content/dam/flower.jpg`

È stato modificato in Pubblica. L&#39;agente attiverebbe una ricerca per &quot;/content/dam/flower.jpg&quot; e troverebbe tutte le pagine che fanno riferimento a quelle immagini.

Potrebbe quindi inviare al Dispatcher una serie di richieste di invalidazione. Una per ogni pagina contenente la risorsa.

In teoria dovrebbe funzionare. Ma solo per dipendenze di primo livello. Non applicare lo schema per le dipendenze a più livelli, ad esempio quando utilizzi l’immagine su un frammento di esperienza utilizzato in una pagina. A dire il vero, riteniamo che l&#39;approccio sia troppo complesso - e potrebbero esserci problemi di runtime. E generalmente il miglior consiglio è di non fare computer costosi in gestori di eventi. E specialmente la ricerca può diventare molto costosa.

##### Conclusione

Speriamo di aver discusso a fondo il Pattern spooler per aiutarti a decidere quando usarlo e non utilizzarlo nella tua implementazione.

## Evitare i problemi di Dispatcher

### URL basati su risorse

Un modo molto più elegante per risolvere il problema della dipendenza è quello di non avere dipendenze affatto. Evita le dipendenze artificiali che si verificano quando si utilizza una risorsa per semplicemente proxy un&#39;altra, come abbiamo fatto nell&#39;ultimo esempio. Cercate di vedere le risorse come entità &quot;solitarie&quot; il più spesso possibile.

Il nostro esempio è facilmente risolto:

![Spooling dell’immagine con un servlet associato all’immagine, non al componente.](assets/chapter-1/spooling-image.png)

*Spooling dell’immagine con un servlet associato all’immagine, non al componente.*

<br> 

Utilizziamo i percorsi delle risorse originali per eseguire il rendering dei dati. Se è necessario eseguire il rendering dell’immagine originale così com’è, è sufficiente utilizzare AEM modulo di rendering predefinito per le risorse.

Se è necessario eseguire un’elaborazione speciale per un componente specifico, registreremo un servlet dedicato su quel percorso e un selettore per eseguire la trasformazione per conto del componente. Lo abbiamo fatto in questo caso esemplare con il &quot;.respi&quot;. selettore. È opportuno tenere traccia dei nomi dei selettori utilizzati nello spazio URL globale (ad esempio `/content/dam`) e hanno una buona convenzione per i nomi per evitare conflitti di denominazione.

A proposito, non vediamo alcun problema con la coerenza del codice. Il servlet può essere definito nello stesso pacchetto Java del modello sling dei componenti.

Possiamo anche utilizzare selettori aggiuntivi nello spazio globale, come

`/content/dam/flower.respi.thumbnail.jpg`

Facile, vero? Allora perché la gente inventa schemi complicati come Spooler?

Beh, potremmo risolvere il problema evitando il riferimento al contenuto interno perché il componente esterno ha aggiunto poco valore o informazioni al rendering della risorsa interna, che potrebbe facilmente essere codificato in set di selettori statici che controllano la rappresentazione di una risorsa solitaria.

Tuttavia, esiste una classe di casi che non è possibile risolvere facilmente con un URL basato su risorse. Chiamiamo questo tipo di caso, &quot;Componenti di inserimento dei parametri&quot;, e li discutiamo nel prossimo capitolo.

### Componenti di inserimento parametri

#### Panoramica

Lo spooler nell&#39;ultimo capitolo era solo un sottile involucro intorno a una risorsa. Ha causato più problemi che aiuto per risolvere il problema.

Potremmo facilmente sostituire il wrapping utilizzando un semplice selettore e aggiungere un servlet in base per soddisfare tali richieste.

Ma se il componente &quot;respi&quot; fosse più di un semplice proxy. Cosa succede se il componente contribuisce effettivamente al rendering del componente?

Introduciamo una piccola estensione del nostro componente &quot;respi&quot;, che è un po &#39;un cambio di gioco. Di nuovo, introdurremo prima alcune soluzioni ingenue per affrontare le nuove sfide e mostrare dove sono carenti.

#### Componente Respi2

Il componente respi2 è un componente che visualizza un’immagine reattiva, come il componente respi. Ma ha un leggero add-on,

![Struttura CRX: componente respi2 che aggiunge una proprietà Quality alla consegna](assets/chapter-1/respi2.png)

*Struttura CRX: componente respi2 che aggiunge una proprietà Quality alla consegna*

<br> 

Le immagini sono jpeg e jpegs può essere compresso. Quando comprimi un&#39;immagine jpeg, scegli la qualità per le dimensioni del file. La compressione è definita come parametro numerico di &quot;qualità&quot; che va da &quot;1&quot; a &quot;100&quot;. &quot;1&quot; significa &quot;piccola ma di scarsa qualità&quot;, &quot;100&quot; significa &quot;di ottima qualità ma file di grandi dimensioni&quot;. Quindi qual è il valore perfetto allora?

Come in tutti gli aspetti IT, la risposta è: &quot;Dipende.&quot;

Qui dipende dal motivo. I motivi con bordi ad alto contrasto come i motivi, tra cui testo scritto, foto di edifici, illustrazioni, schizzi o foto di scatole di prodotti (con contorni taglienti e testo scritto su di esso) di solito rientrano in quella categoria. I motivi con transizioni di colore e contrasto più morbide, come paesaggi o ritratti, possono essere compressi un po&#39; di più senza perdita di qualità visibile. Le fotografie della natura di solito rientrano in quella categoria.

Inoltre, a seconda di dove viene utilizzata l&#39;immagine, potrebbe essere utile utilizzare un parametro diverso. Una piccola miniatura in un teaser potrebbe subire una compressione migliore della stessa immagine utilizzata in un banner eroe a schermo intero. Ciò significa che il parametro di qualità non è innato all&#39;immagine, ma all&#39;immagine e al contesto. E al gusto dell&#39;autore.

In breve: Non esiste un&#39;impostazione perfetta per tutte le immagini. Non c&#39;è un unico modello per tutti. È meglio che decida l&#39;autore. Modificherà il parametro &quot;quality&quot; come proprietà nel componente fino a quando non sarà soddisfatto della qualità e non andrà oltre per non sacrificare la larghezza di banda.

Ora abbiamo un file binario nel DAM e un componente, che fornisce una proprietà di qualità. Come dovrebbe essere l’URL? Quale componente è responsabile dello spooling?

#### Approccio ingenuo 1: Trasmetti proprietà come parametri di query

>[!WARNING]
>
>Questo è un anti-pattern. Non usarlo.

Nell’ultimo capitolo, l’URL dell’immagine rappresentato dal componente era simile al seguente:

`/content/dam/flower.respi.jpg`

Tutto ciò che manca è il valore della qualità. Il componente sa quale proprietà viene immessa dall’autore.. Potrebbe facilmente essere trasmessa al servlet di rendering delle immagini come parametro di query quando viene eseguito il rendering del markup, come `flower.respi2.jpg?quality=60`:

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
>Questo potrebbe diventare un anti-pattern. La usi con attenzione.

![Trasferimento delle proprietà dei componenti come selettori](assets/chapter-1/passing-component-properties.png)

*Trasferimento delle proprietà dei componenti come selettori*

<br> 

Si tratta di una leggera variazione dell’ultimo URL. Solo questa volta utilizziamo un selettore per passare la proprietà al servlet in modo che il risultato sia memorizzabile nella cache:

`/content/dam/flower.respi.q-60.jpg`

Questo è molto meglio, ma ricordate quel brutto ragazzo di script dell&#39;ultimo capitolo che cerca questi modelli? Vedrà quanto lontano riesce a raggiungere con il ciclo dei valori:

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

Anche questo bypassa la cache e crea il carico sul sistema di pubblicazione. Potrebbe essere una cattiva idea. Puoi attenuare questo problema filtrando solo un piccolo sottoinsieme di parametri. Vuoi consentire solo `q-20, q-40, q-60, q-80, q-100`.

#### Filtro delle richieste non valide quando si utilizzano i selettori

Ridurre il numero di selettori è stato un buon inizio. Di regola, devi sempre limitare il numero di parametri validi a un minimo assoluto. Se lo si fa in modo intelligente, è anche possibile sfruttare un firewall per applicazioni Web all&#39;esterno di AEM utilizzando un set statico di filtri senza una conoscenza approfondita del sistema di AEM sottostante per proteggere i sistemi:

```
Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
       \.respi\.q-(20|40|60|80|100)\.jpg
```

Se non si dispone di un firewall per applicazioni web, è necessario filtrare in Dispatcher o in AEM. Se lo fai in AEM, assicurati che

1. Il filtro è implementato in modo super efficiente, senza accedere al CRX troppo e sprecare memoria e tempo.

2. Il filtro risponde a un messaggio di errore &quot;404 - Non trovato&quot;

Sottolineiamo ancora l&#39;ultimo punto. La conversazione HTTP si presenta così:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

Abbiamo visto anche implementazioni che filtrano parametri non validi ma restituiscono un rendering di fallback valido quando viene utilizzato un parametro non valido. Supponiamo di consentire solo parametri da 20 a 100. I valori compresi tra sono mappati su quelli validi. Quindi,

`q-41, q-42, q-43, …`

risponderebbe sempre alla stessa immagine di q-40:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Questo approccio non aiuta affatto. Queste richieste sono in realtà richieste valide.  Utilizzano la potenza di elaborazione e occupano spazio nella directory cache del Dispatcher.

Meglio è restituire un `301 – Moved permanently`:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

Qui AEM il browser. &quot;Non ho `q-41`. Ma ehi, puoi chiedermi `q-40` &quot;.

Questo aggiunge un ulteriore ciclo di richiesta-risposta alla conversazione, che è un po&#39; sovraccarico, ma è più economico che fare l&#39;elaborazione completa su `q-41`. E puoi sfruttare il file già memorizzato nella cache in `q-40`. Tuttavia, devi capire che 302 risposte non sono memorizzate nella cache di Dispatcher, stiamo parlando della logica che viene eseguita nel AEM. Ancora e ancora. Quindi è meglio renderlo sottile e veloce.

Personalmente ci piace il 404 rispondere di più. Lo rende incredibilmente ovvio quello che sta succedendo. Consente inoltre di rilevare gli errori sul sito web durante l’analisi dei file di registro. 301 s può essere destinato, dove 404 deve sempre essere analizzato ed eliminato.

## Sicurezza - Escursione

### Richieste di filtro

#### Dove filtrare meglio

Alla fine dell&#39;ultimo capitolo abbiamo sottolineato la necessità di filtrare il traffico in entrata per i selettori noti. Questo lascia il problema: Dove devo filtrare le richieste?

Beh - dipende. Prima è meglio è.

#### firewall per applicazioni web

Se si dispone di un dispositivo firewall per applicazioni Web o di un &quot;WAF&quot; progettato per la sicurezza Web, è assolutamente necessario utilizzare queste funzionalità. Tuttavia, potresti scoprire che il WAF è gestito da persone che hanno solo una conoscenza limitata della tua applicazione di contenuto e che filtrano le richieste valide o lasciano passare troppe richieste dannose. Forse scoprirete che le persone che gestiscono il WAF sono assegnate a un reparto diverso con diversi turni e orari di rilascio, la comunicazione potrebbe non essere così stretta come con i vostri compagni di squadra diretti e non si ottengono sempre i cambiamenti nel tempo, il che significa che alla fine il vostro sviluppo e la velocità dei contenuti soffrono.

Si potrebbe finire con alcune regole generali o anche un inserii nell&#39;elenco Bloccati, che la vostra sensazione istintiva dice, potrebbe essere inasprito.

#### Dispatcher e filtri di pubblicazione

Il passaggio successivo consiste nell’aggiungere regole di filtro URL nel core Apache e/o nel dispatcher.

Qui puoi accedere solo agli URL. I filtri basati su pattern sono limitati. Se hai bisogno di impostare un filtro più basato sui contenuti (come consentire file solo con un timestamp corretto) o desideri che alcuni dei filtri controllati sul tuo autore - finirai per scrivere qualcosa come un filtro del servlet personalizzato.

#### Monitoraggio e debug

In pratica avrete una certa sicurezza su ogni livello. Ma assicurati di disporre di mezzi per scoprire a quale livello una richiesta viene filtrata. Assicurati di avere accesso diretto al sistema Publish, al Dispatcher e ai file di log sul WAF per scoprire quale filtro nella catena blocca le richieste.

### Selettori e proliferazione dei selettori

L’approccio che utilizza i &quot;parametri-selettori&quot; nell’ultimo capitolo è rapido e semplice e può accelerare il tempo di sviluppo dei nuovi componenti, ma ha dei limiti.

L&#39;impostazione di una proprietà &quot;quality&quot; è solo un esempio semplice. Ma diciamo che il servlet si aspetta anche che i parametri per la &quot;larghezza&quot; siano più versatili.

È possibile ridurre il numero di URL validi riducendo il numero di possibili valori di selettore. Puoi eseguire le stesse operazioni anche con la larghezza:

qualità = q-20, q-40, q-60, q-80, q-100

larghezza = w-100, w-200, w-400, w-800, w-1000, w-1200

Ma tutte le combinazioni sono ora URL validi:

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Ora disponiamo già di 5x6=30 URL validi per una risorsa. Ogni proprietà aggiuntiva viene aggiunta alla complessità. E ci potrebbero essere proprietà che non possono essere ridotte a un ragionevole ammontare di valori.

Anche questo approccio ha dei limiti.

#### Esposizione involontaria di un’API

Cosa sta succedendo qui? Se guardiamo con attenzione, vediamo che gradualmente ci stiamo spostando da un sito web statisticamente reso a un sito web altamente dinamico. E inavvertitamente stiamo presentando un’API di rendering delle immagini al browser del cliente che in realtà era destinata solo agli autori.

L’autore modifica la pagina e imposta la qualità e le dimensioni di un’immagine. Avere le stesse funzionalità esposte da un servlet potrebbe essere visto come una funzione o come vettore per un attacco Denial of Service. Quello che è in realtà dipende dal contesto. Quanto è importante il business del sito web? Quanto carico è sui server? Quanto spazio libero rimane? Quanto budget avete per l&#39;implementazione? Dovete bilanciare questi fattori. Devi essere consapevole dei pro e dei contro.

## Il modello spooler - Rivisitato e riabilitato

### Come lo spooler evita l’esposizione dell’API

Abbiamo screditato il modello Spooler nell&#39;ultimo capitolo. È ora di riabilitarlo.

![](assets/chapter-1/spooler-pattern.png)

Il Pattern di spooler impedisce che il problema relativo all’esposizione di un’API di cui abbiamo discusso nell’ultimo capitolo. Le proprietà vengono memorizzate e incapsulate nel componente. Tutto ciò che dobbiamo accedere a queste proprietà è il percorso del componente. Non è necessario utilizzare l’URL come veicolo per trasmettere i parametri tra markup e rendering binario:

1. Il client esegue il rendering del markup HTML quando il componente viene richiesto all’interno del ciclo di richiesta principale

2. Il percorso del componente funge da riferimento di sfondo dal markup al componente

3. Il browser utilizza questo backreference per richiedere il binario

4. Quando la richiesta colpisce il componente, abbiamo tutte le proprietà in mano per ridimensionare, comprimere e spool i dati binari

5. L’immagine viene trasmessa tramite il componente al browser client

Dopo tutto, lo Spooler Pattern non è poi così male, ecco perché è così popolare. Se solo quando si tratta di invalidazione della cache non è così complicato..

### Il Spooler Invertito - Il meglio di entrambi i mondi?

Questo ci porta alla domanda. Perché non possiamo ottenere il meglio da entrambi i mondi? La buona incapsulazione del pattern spooler e le proprietà di memorizzazione in cache di un URL basato su risorse?

Dobbiamo ammettere che non lo abbiamo visto in un vero progetto live. Ma osiamo un piccolo esperimento mentale qui comunque - come punto di partenza per la vostra soluzione.

Chiameremo questo modello _Spooler invertito_. Lo spooler invertito deve essere basato sulla risorsa immagini, per avere tutte le proprietà di annullamento validità cache belle.

Ma non deve esporre alcun parametro. Tutte le proprietà devono essere incapsulate nel componente. Ma possiamo esporre il percorso dei componenti - come riferimento opaco alle proprietà.

Questo porta a un URL nel modulo:

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` è il percorso della risorsa dell’immagine

`.respi3` è un selettore per selezionare il servlet corretto per consegnare l’immagine

`.content-mysite-home-jcrcontent-par-respi` è un selettore aggiuntivo. Codifica il percorso del componente che memorizza la proprietà necessaria per la trasformazione dell’immagine. I selettori sono limitati a un intervallo di caratteri più piccolo rispetto ai percorsi. Lo schema di codifica qui è esemplare. Sostituisce &quot;/&quot; con &quot;-&quot;. Non si tiene conto del fatto che il percorso stesso può contenere anche &quot;-&quot;. Uno schema di codifica più sofisticato sarebbe consigliato in un esempio reale. Base64 dovrebbe essere ok. Ma rende il debug un po&#39; più difficile.

`.jpg` è il suffisso di file

### Conclusione

Wow.. la discussione dello spooler si è fatta sempre più complicata del previsto. Ti dobbiamo una scusa. Ma abbiamo ritenuto necessario presentarvi una moltitudine di aspetti - buoni e cattivi - in modo da poter sviluppare qualche intuizione su cosa funziona bene in Dispatcher-land e cosa no.

## A livello di file di stato e di file di stato

### Nozioni di base

#### Introduzione

Abbiamo già accennato brevemente al _statfile_ prima. È correlato all’annullamento automatico della validità:

Tutti i file di cache nel file system del Dispatcher configurati per l’annullamento automatico della validità sono considerati non validi se la data dell’ultima modifica è precedente alla data di annullamento della validità `statfile's` data dell’ultima modifica.

>[!NOTE]
>
>La data dell&#39;ultima modifica di cui stiamo parlando è la data in cui il file è stato richiesto dal browser del cliente e in ultima analisi creato nel file system. Non è il `jcr:lastModified` data della risorsa.

Data dell&#39;ultima modifica dello statfile (`.stat`) è la data in cui è stata ricevuta la richiesta di invalidazione da AEM sul Dispatcher.

Se disponi di più di un’istanza di Dispatcher, questo può causare effetti strani. Il browser può disporre di una versione più recente come Dispatcher (se si dispone di più di un Dispatcher). In alternativa, un’istanza di Dispatcher potrebbe ritenere obsoleta la versione del browser rilasciata dall’altro Dispatcher e inviare inutilmente una nuova copia. Questi effetti non hanno un impatto significativo sulle prestazioni o sui requisiti funzionali. E si livelleranno nel tempo, quando il browser ha la versione più recente. Tuttavia, può essere un po&#39; confuso quando ottimizzi e esegui il debug del comportamento di memorizzazione in cache del browser. Quindi sia avvertito.

#### Impostazione dei domini di annullamento della validità con /statfileslevel

Quando abbiamo introdotto l&#39;annullamento automatico della validità e lo stato del file, abbiamo detto che *tutto* i file vengono considerati non validi quando si verificano modifiche e tutti i file sono comunque interdipendenti.

Non è abbastanza preciso. Di solito, tutti i file che condividono una radice di navigazione principale comune sono interdipendenti. Ma un&#39;istanza AEM può ospitare un certo numero di siti web - *indipendente* siti web. Non condividere una navigazione comune - in realtà, non condividere nulla.

Non sarebbe uno spreco annullare la validità del sito B perché si verifica una modifica nel sito A? Sì. E non deve essere così.

Dispatcher fornisce un mezzo semplice per separare i siti: La `statfiles-level`.

È un numero che definisce da quale livello nel filesystem su, due sottoalberi sono considerati &quot;indipendenti&quot;.

Diamo un’occhiata al case predefinito in cui statfileslevel è 0.

![/statfileslevel &quot;0&quot;: The_ _.stat_ _viene creato nel docroot. Il dominio di invalidazione si estende su tutta l’installazione, compresi tutti i siti](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":` La `.stat` viene creato nel docroot. Il dominio di invalidazione si estende per l’intera installazione, compresi tutti i siti.

A prescindere dal file invalidato, il `.stat` file nella parte superiore del docroot dei dispatcher viene sempre aggiornato. Quindi, quando invalidiamo `/content/site-b/home`, anche tutti i file in `/content/site-a` vengono anche invalidati, in quanto sono più vecchi del `.stat` nel docroot. Chiaramente non quello di cui hai bisogno, quando invalidi `site-b`.

In questo esempio, è preferibile impostare la variabile `statfileslevel` a `1`.

Ora se pubblichi - e quindi invalida `/content/site-b/home` o qualsiasi altra risorsa sottostante `/content/site-b`, `.stat` viene creato in `/content/site-b/`.

Contenuto sottostante `/content/site-a/` non è interessato. Questo contenuto viene confrontato con un `.stat` file a `/content/site-a/`. Abbiamo creato due domini di invalidazione separati.

![Un livello statfileslevel &quot;1&quot; crea diversi domini di invalidazione](assets/chapter-1/statfiles-level-1.png)

*Un livello statfileslevel &quot;1&quot; crea diversi domini di invalidazione*

<br> 

Le installazioni di grandi dimensioni sono di solito strutturate un po&#39; più complesse e più profonde. Uno schema comune è quello di strutturare i siti per marchio, paese e lingua. In tal caso, puoi impostare statfileslevel anche più in alto. _1_ crea domini di invalidazione per marchio, _2_ per paese e _3_ per lingua.

### Necessità di una struttura del sito omogenea

Lo statfileslevel viene applicato in modo uniforme a tutti i siti della configurazione. Pertanto, è necessario che tutti i siti seguano la stessa struttura e inizino allo stesso livello.

Considera di avere alcuni marchi nel tuo portafoglio che sono venduti solo su pochi piccoli mercati mentre altri sono venduti in tutto il mondo. I piccoli mercati hanno una sola lingua locale mentre nel mercato globale ci sono paesi in cui si parla più di una lingua:

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

Il primo richiederebbe un `statfileslevel` di _2_, mentre quest&#39;ultimo richiede _3_.

Non è una situazione ideale. Se lo imposti su _3_, l’annullamento automatico della validità non funzionerebbe all’interno dei siti più piccoli tra i rami secondari `/home`, `/products` e `/about`.

Impostazione su _2_ significa che nei siti più grandi si dichiara `/canada/en` e `/canada/fr` dipendenti, che potrebbero non essere. Così ogni invalidazione in `/en` invalidare `/fr`. Questo porterà a un tasso di hit della cache leggermente ridotto, ma è comunque migliore rispetto alla distribuzione di contenuto obsoleto nella cache.

La soluzione migliore è ovviamente quella di rendere ugualmente profonde le radici di tutti i siti:

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

Qual è il livello giusto? Dipende dal numero di dipendenze tra i siti. Le inclusioni risolte per il rendering di una pagina sono considerate &quot;dipendenze complesse&quot;. Abbiamo dimostrato un _inclusione_ quando abbiamo introdotto _Teaser_ componente all&#39;inizio di questa guida.

_Collegamenti ipertestuali_ sono una forma più morbida di dipendenze. È molto probabile, che si farà collegamenti ipertestuali all&#39;interno di un sito web.. e non improbabile che si dispone di collegamenti tra i vostri siti web. I collegamenti ipertestuali semplici di solito non creano dipendenze tra siti web. Basta pensare a un link esterno impostato dal sito a facebook... Non dovresti eseguire il rendering della pagina se qualcosa cambia in facebook e viceversa, giusto?

Una dipendenza si verifica quando si legge il contenuto dalla risorsa collegata (ad esempio, il titolo di navigazione). Tali dipendenze possono essere evitate se vi limitate ai titoli di navigazione inseriti localmente e non li si disegna dalla pagina di destinazione (come si farebbe con i collegamenti esterni).

#### Dipendenza Imprevista

Tuttavia, potrebbe esserci una parte della tua configurazione, in cui i siti - presumibilmente indipendenti - si riuniscono. Diamo un&#39;occhiata a uno scenario reale che abbiamo trovato in uno dei nostri progetti.

Il cliente aveva una struttura del sito come quella tracciata nell&#39;ultimo capitolo:

```
/content/brand/country/language
```

Ad esempio:

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

Non c&#39;erano collegamenti navigabili tra i siti della lingua e non apparenti inclusioni, quindi abbiamo impostato lo statfileslevel su 3.

Tutti i siti servivano essenzialmente lo stesso contenuto. L&#39;unica differenza importante era la lingua.

I motori di ricerca come Google considerano ingannevole avere lo stesso contenuto su URL diversi. Un utente potrebbe voler provare a classificarsi più in alto o elencare più spesso creando farm che servono contenuti identici. I motori di ricerca riconoscono questi tentativi e classificano le pagine più basse che semplicemente riciclano il contenuto.

È possibile evitare di essere classificati in base al fatto di rendere trasparente, che in realtà si dispone di più pagine con lo stesso contenuto e che non si sta tentando di &quot;giocare&quot; il sistema (vedi [&quot;Comunica a Google le versioni localizzate della tua pagina&quot;](https://support.google.com/webmasters/answer/189077?hl=en)) impostando `<link rel="alternate">` tag per ogni pagina correlata nella sezione di intestazione di ogni pagina:

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

![Collega tutto](assets/chapter-1/inter-linking-all.png)

*Collega tutto*

<br> 

Alcuni esperti SEO sostengono addirittura che questo potrebbe trasferire la reputazione o il &quot;link-succo&quot; da un sito web di alto rango in una lingua allo stesso sito web in una lingua diversa.

Questo schema ha creato non solo una serie di collegamenti, ma anche alcuni problemi. Il numero di collegamenti necessari per _p_ in _n_ le lingue sono _p x (n<sup>2</sup>-n)_: Ogni pagina si collega a un’altra pagina (_n x n_) ad eccezione di se stesso (_-n_). Questo schema viene applicato a ogni pagina. Se disponiamo di un sito piccolo in 4 lingue con 20 pagine, ciascuno equivale a _240_ link.

In primo luogo, non è necessario che un editor mantenga manualmente questi collegamenti, che devono essere generati automaticamente dal sistema.

In secondo luogo, dovrebbero essere accurati. Ogni volta che il sistema rileva un nuovo &quot;relativo&quot;, vuoi collegarlo da tutte le altre pagine con lo stesso contenuto (ma in una lingua diversa).

Nel nostro progetto, vengono spesso visualizzate nuove pagine relative. Ma non si sono materializzati come collegamenti &quot;alternativi&quot;. Ad esempio, quando `de-de/produkte` La pagina è stata pubblicata sul sito web tedesco, non era immediatamente visibile sugli altri siti.

Il motivo era che nella nostra configurazione i siti dovevano essere indipendenti. Una modifica apportata al sito web tedesco non ha quindi provocato un&#39;invalidazione del sito web francese.

Sai già una soluzione per risolvere quel problema. È sufficiente ridurre lo statfileslevel a 2 per ampliare il dominio di invalidazione. Naturalmente, questo diminuisce anche il rapporto di hit della cache, soprattutto quando le pubblicazioni, e quindi gli invalidamenti si verificano più frequentemente.

Nel nostro caso era ancora più complicato:

Anche se avevamo lo stesso contenuto, i nomi dei marchi non erano diversi in ogni paese.

`shiny-brand` è stato chiamato `marque-brillant` in Francia e `blitzmarke` in Germania:

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Avrebbe dovuto impostare il `statfiles` livello a 1 : si sarebbe verificato un dominio di invalidazione troppo grande.

La ristrutturazione del sito avrebbe risolto questo problema. Unione di tutti i marchi in un’unica radice comune. Ma allora non avevamo la capacità, e questo ci avrebbe dato solo un livello 2.

Decidemmo di attenersi al livello 3 e pagammo il prezzo di non avere sempre aggiornato i link &quot;alternativi&quot;. Per attenuare il problema, sul Dispatcher era in esecuzione un cron-job &quot;leggibile&quot; che avrebbe comunque ripulito i file più vecchi di 1 settimana. Così alla fine tutte le pagine sono state riprodotte comunque ad un certo punto del tempo. Ma questo è un compromesso che deve essere deciso individualmente in ogni progetto.

## Conclusione

Abbiamo esaminato alcuni principi di base sul funzionamento di Dispatcher in generale e vi abbiamo fornito alcuni esempi in cui potrebbe essere necessario compiere un po&#39; più di sforzo di implementazione per farlo bene e dove si potrebbe voler fare compromessi.

Non sono stati forniti dettagli sulla configurazione in Dispatcher. Volevamo che tu capissi prima i concetti e i problemi di base, senza perderti troppo presto nella console. E - il lavoro di configurazione effettivo è ben documentato - se si comprendono i concetti di base, è necessario sapere a cosa servono i vari switch.

## Suggerimenti e trucchi per Dispatcher

Concluderemo la prima parte di questo libro con una raccolta casuale di suggerimenti e trucchi che potrebbero essere utili in una situazione o nell&#39;altra. Come abbiamo fatto prima, non presentiamo la soluzione, ma l&#39;idea in modo da poter comprendere l&#39;idea e il concetto e il collegamento ad articoli che descrivono la configurazione effettiva in modo più dettagliato.

### Correzione del tempo di annullamento della validità

Se installi un Author e un Publish di AEM, la topologia è un po&#39; strana. Contemporaneamente, l’autore invia i contenuti ai sistemi di pubblicazione e la richiesta di annullamento della validità ai Dispatcher. Poiché sia i sistemi di pubblicazione che il Dispatcher vengono scollegati dall’autore dalle code, i tempi possono essere un po’ sfortunati. Il Dispatcher può ricevere la richiesta di invalidazione dall’autore prima che il contenuto venga aggiornato sul sistema di pubblicazione.

Se nel frattempo un client richiede quel contenuto, Dispatcher richiede e archivia il contenuto non aggiornato.

Una configurazione più affidabile invia la richiesta di annullamento della validità dai sistemi di pubblicazione _dopo_ hanno ricevuto il contenuto. L&#39;articolo &quot;[Annullamento della validità della cache del Dispatcher da un’istanza di pubblicazione](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)&quot; descrive i dettagli.

**Riferimenti**

[helpx.adobe.com - Annullamento della validità della cache del Dispatcher da un&#39;istanza di pubblicazione](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)

### Memorizzazione in cache delle intestazioni HTTP

Ai vecchi tempi, Dispatcher stava semplicemente memorizzando file semplici nel file system. Se hai bisogno che le intestazioni HTTP vengano consegnate al cliente, lo hai fatto configurando Apache in base alle poche informazioni ricevute dal file o dalla posizione. Ciò era particolarmente fastidioso quando si implementava un&#39;applicazione web in AEM che si basava fortemente sulle intestazioni HTTP. Tutto funzionava correttamente nell’istanza di solo AEM, ma non quando si utilizzava un’istanza di Dispatcher.

Di solito hai iniziato a riapplicare le intestazioni mancanti alle risorse nel server Apache con `mod_headers` utilizzando le informazioni è possibile derivare dal percorso e dal suffisso delle risorse. Ma non è sempre stato sufficiente.

Particolarmente fastidioso era, anche con Dispatcher il primo _non memorizzato in cache_ la risposta al browser proveniva dal sistema Publish con un intervallo completo di intestazioni, mentre le risposte successive venivano generate da Dispatcher con un set limitato di intestazioni.

A partire da Dispatcher 4.1.11, Dispatcher può memorizzare le intestazioni generate dai sistemi Publish.

Questo ti consente di evitare la duplicazione della logica di intestazione nel Dispatcher e di liberare la piena potenza espressiva di HTTP e AEM.

**Riferimenti**

* [helpx.adobe.com - Memorizzazione in cache delle intestazioni di risposta](https://helpx.adobe.com/experience-manager/kb/dispatcher-cache-response-headers.html)

### Eccezioni di memorizzazione in cache individuali

Potrebbe essere utile memorizzare nella cache tutte le pagine e le immagini in generale, ma in alcune circostanze fare un&#39;eccezione. Ad esempio, si desidera memorizzare nella cache le immagini PNG, ma non le immagini PNG che mostrano un captcha (che si suppone venga modificato su ogni richiesta). Il Dispatcher potrebbe non riconoscere un captcha come un captcha.. ma AEM certamente. Può chiedere al Dispatcher di non memorizzare in cache quella richiesta inviando un’intestazione corrispondente con la risposta:

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Control e Pragma sono intestazioni ufficiali HTTP, che vengono propagate e interpretate da livelli superiori di memorizzazione in cache, come una CDN. La `Dispatcher` l’intestazione è solo un suggerimento per il Dispatcher di non memorizzare in cache. Può essere utilizzato per dire al Dispatcher di non memorizzare in cache, pur consentendo ai livelli superiori di memorizzazione in cache di farlo. In realtà, è difficile trovare un caso in cui potrebbe essere utile. Ma siamo sicuri che ce ne siano alcuni, da qualche parte.

**Riferimenti**

* [Dispatcher - Nessuna cache](https://helpx.adobe.com/experience-manager/kb/DispatcherNoCache.html)

### Memorizzazione in cache del browser

La risposta http più veloce è la risposta data dal browser stesso. Dove la richiesta e la risposta non devono viaggiare su una rete a un server web sotto carico elevato.

Puoi aiutare il browser a decidere quando chiedere al server una nuova versione del file impostando una data di scadenza per una risorsa.

Di solito, lo fai statisticamente utilizzando Apache&#39;s `mod_expires` oppure memorizzando la Cache-Control e l&#39;Expires Header proveniente da AEM se hai bisogno di un controllo più individuale.

Un documento memorizzato nella cache del browser può avere tre livelli di aggiornamento.

1. _fresco garantito_ - Il browser può utilizzare il documento memorizzato nella cache.

2. _Potenzialmente stantio_ - Il browser deve chiedere al server se il documento memorizzato nella cache è ancora aggiornato,

3. _Stabile_ - Il browser deve richiedere al server una nuova versione.

Il primo è garantito dalla data di scadenza impostata dal server. Se una risorsa non è scaduta, non è necessario richiederla nuovamente al server.

Se il documento ha raggiunto la sua data di scadenza, può comunque essere fresco. La data di scadenza viene impostata quando il documento viene consegnato. Ma spesso non si sa in anticipo quando sono disponibili nuovi contenuti - quindi questa è solo una stima conservativa.

Per determinare se il documento nella cache del browser è ancora lo stesso che sarebbe stato consegnato su una nuova richiesta, il browser può utilizzare il `Last-Modified` data del documento. Il browser chiede al server:

&quot;_Ho una versione del 10 giugno... ho bisogno di un aggiornamento?_&quot; E il server potrebbe rispondere con

&quot;_304 - La tua versione è ancora aggiornata_&quot; senza ritrasmettere la risorsa, o il server potrebbe rispondere con

&quot;_200 - ecco una versione più recente_&quot; nell’intestazione HTTP e nel contenuto più recente nel corpo HTTP.

Per far funzionare questa seconda parte, assicurati di trasmettere `Last-Modified` data al browser in modo che abbia un punto di riferimento per chiedere aggiornamenti.

Abbiamo spiegato prima che quando `Last-Modified` La data viene generata dal Dispatcher, può variare tra richieste diverse perché il file memorizzato nella cache, e la relativa data, viene generato quando il file viene richiesto dal browser. Un’alternativa sarebbe quella di utilizzare gli &quot;e-tag&quot;, ovvero numeri che identificano il contenuto effettivo (ad esempio generando un codice hash) invece di una data.

&quot;[Supporto Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)&quot; dal _Pacchetto ACS Commons_ utilizza questo approccio. Questo però ha un prezzo: Poiché l’E-Tag deve essere inviato come intestazione, ma il calcolo del codice hash richiede la lettura completa della risposta, la risposta deve essere completamente bufferizzata nella memoria principale prima che possa essere distribuita. Questo può avere un impatto negativo sulla latenza quando è più probabile che il sito web abbia risorse non memorizzate nella cache e, ovviamente, è necessario tenere d&#39;occhio la memoria utilizzata dal sistema AEM.

Se utilizzi le impronte digitali URL, puoi impostare date di scadenza molto lunghe. È possibile memorizzare in cache le risorse impronte digitali per sempre nel browser. Una nuova versione viene contrassegnata con un nuovo URL e le versioni precedenti non devono mai essere aggiornate.

Abbiamo usato le impronte digitali degli URL quando abbiamo introdotto il pattern di spooler. File statici provenienti da `/etc/design` (CSS, JS) raramente vengono modificati, rendendo loro buoni candidati da utilizzare come impronte digitali.

Per i file regolari di solito impostiamo uno schema fisso, come ricontrollare HTML ogni 30 minuti, le immagini ogni 4 ore e così via.

La memorizzazione in cache del browser è estremamente utile nel sistema di authoring. Desideri memorizzare nella cache il più possibile nel browser per migliorare l&#39;esperienza di modifica. Sfortunatamente, le risorse più costose, le pagine html non possono essere memorizzate nella cache... dovrebbero cambiare frequentemente sull&#39;autore.

Le librerie granite, che compongono AEM’interfaccia utente, possono essere memorizzate nella cache per un bel po’ di tempo. Puoi anche memorizzare nella cache i file statici dei siti (font, CSS e JavaScript) nel browser. Immagini uguali in `/content/dam` in genere possono essere memorizzati nella cache per circa 15 minuti in quanto non vengono modificati con la stessa frequenza di copia del testo sulle pagine. Le immagini non vengono modificate in modo interattivo in AEM. Vengono prima modificati e approvati, prima di essere caricati in AEM. Pertanto, è possibile supporre che non vengano modificate con la stessa frequenza del testo.

Memorizzazione in cache dei file dell&#39;interfaccia utente, i file della libreria dei siti e le immagini possono velocizzare notevolmente il ricaricamento delle pagine in modalità di modifica.



**Riferimenti**

*[developer.mozilla.org - Memorizzazione in cache](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

* [apache.org - Mod Scade](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [ACS Commons - Supporto Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### Troncamento degli URL

Le risorse memorizzate in

`/content/brand/country/language/…`

Ma naturalmente, questo non è l&#39;URL che desideri presentare al cliente. Per motivi di estetica, leggibilità e SEO, è possibile troncare la parte già rappresentata nel nome di dominio.

Se disponi di un dominio

`www.shiny-brand.fi`

di solito non c&#39;è bisogno di mettere il marchio e il paese nel percorso. Invece di

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

vorreste avere,

`www.shiny-brand.fi/home.html`

Devi implementare la mappatura su AEM, perché AEM deve sapere come eseguire il rendering dei collegamenti in base a quel formato troncato.

Ma non fare affidamento solo su AEM. Se lo fai, avrai percorsi come `/home.html` nella directory principale della cache. Ora, è quella la &quot;casa&quot; per il sito web finlandese o tedesco o canadese? E se c&#39;è un file `/home.html` in Dispatcher, come il Dispatcher sa che deve essere invalidato quando viene richiesta un’invalidazione per `/content/brand/fi/fi/home` entra.

Abbiamo visto un progetto con docroot separati per ogni dominio. È stato un incubo eseguire il debug e mantenere - e in realtà non l&#39;abbiamo mai visto funzionare senza problemi.

Potremmo risolvere i problemi ristrutturando la cache. Avevamo un singolo docroot per tutti i domini e le richieste di annullamento della validità potevano essere gestite 1:1 in quanto tutti i file sul server iniziavano con `/content`.

Anche la parte troncante era molto facile.  AEM generato collegamenti troncati a causa di una configurazione in `/etc/map`.

Ora quando una richiesta `/home.html` sta colpendo Dispatcher, la prima cosa che succede è applicare una regola di riscrittura che espande internamente il percorso.

Questa regola è stata impostata statisticamente in ogni configurazione vhost. In parole povere, le regole erano così,

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

Nel filesystem ora abbiamo semplice `/content`Percorsi basati su , che si trovavano anche su Author e Publish , utili per eseguire il debug. Per non parlare della corretta invalidazione - non era più un problema.

Nota: l’abbiamo fatto solo per gli URL &quot;visibili&quot;, URL visualizzati nello slot URL del browser. Gli URL per le immagini, ad esempio, erano ancora URL &quot;/content&quot; puri. Riteniamo che abbellire l’URL &quot;principale&quot; sia sufficiente in termini di ottimizzazione per i motori di ricerca.

Avere un docroot comune ha anche avuto un&#39;altra caratteristica bella. Quando qualcosa è andato storto in Dispatcher, potremmo ripulire l&#39;intera cache eseguendo,

`rm -rf /cache/dispatcher/*`

(qualcosa che potrebbe non essere necessario fare a picchi di carico elevati).

**Riferimenti**

* [apache.org - Riscrittura Mod](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [helpx.adobe.com - Mappatura delle risorse](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/resource-mapping.html)

### Gestione degli errori

Nelle classi AEM impara a programmare un gestore di errori in Sling. Questo non è così diverso dalla scrittura di un modello consueto. Basta scrivere un modello in JSP o HTL, giusto?

Sì, ma questa è solo la parte AEM. Ricorda : il Dispatcher non memorizza in cache `404 – not found` o `500 – internal server error` risposte.

Se esegui il rendering dinamico di queste pagine su ciascuna richiesta (non riuscita), disporrai di un carico elevato non necessario sui sistemi di pubblicazione.

Ciò che abbiamo trovato utile è non eseguire il rendering della pagina di errore completa quando si verifica un errore, ma solo di una versione semplificata e piccola - anche statica della pagina, senza ornamenti o logica.

Questo ovviamente non è quello che il cliente ha visto. Nel Dispatcher, ci siamo registrati `ErrorDocuments` così:

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Ora il sistema AEM poteva semplicemente avvisare il Dispatcher che qualcosa non andava e il Dispatcher poteva fornire una versione brillante e bella del documento di errore.

Due sono le cose da notare qui.

Primo, la `error-404.html` è sempre la stessa pagina. Quindi non c&#39;è un messaggio individuale come &quot;La tua ricerca &quot;_produkten_&quot; non ha dato un risultato&quot;. Potremmo conviverci facilmente con quello.

Secondo... beh, se vediamo un errore interno del server - o peggio ancora se incontriamo un&#39;interruzione del sistema di AEM, non c&#39;è modo di chiedere AEM di rendere una pagina di errore, giusto? La richiesta successiva necessaria, come definita nella `ErrorDocument` Anche la direttiva fallirebbe. Abbiamo risolto questo problema eseguendo un cron-job che periodicamente estrae le pagine di errore dalle rispettive posizioni definite tramite `wget` e memorizzarle in posizioni di file statiche definite nella `ErrorDocuments` direttiva.

**Riferimenti**

* [apache.org - Documenti di errore personalizzati](https://httpd.apache.org/docs/2.4/custom-error.html)

### Memorizzazione in cache di contenuti protetti

Dispatcher non controlla le autorizzazioni quando distribuisce una risorsa per impostazione predefinita. È implementato in questo modo di proposito - per velocizzare il tuo sito web pubblico. Se desideri proteggere alcune risorse tramite un accesso, sostanzialmente hai tre opzioni,

1. Protect la risorsa prima che la richiesta raggiunga la cache, ad esempio da un gateway SSO (Single Sign On) davanti al Dispatcher o come modulo nel server Apache

2. Escludere le risorse sensibili dalla cache e quindi distribuirle sempre live dal sistema Publish.

3. Utilizzare la memorizzazione in cache sensibile alle autorizzazioni in Dispatcher

E naturalmente, potete applicare la vostra combinazione di tutti e tre gli approcci.

**Opzione 1**. Un gateway &quot;SSO&quot; potrebbe comunque essere applicato dalla tua organizzazione. Se lo schema di accesso è molto grezzo, potrebbe non essere necessario ricevere informazioni da AEM per decidere se concedere o negare l&#39;accesso a una risorsa.

>[!NOTE]
>
>Questo pattern richiede un _Gateway_ che _intercettazioni_ ogni richiesta ed esegue _autorizzazione_ - concessione o negazione di richieste al Dispatcher. Se il tuo sistema SSO è un _autenticatore_, che stabilisce solo l&#39;identità di un utente che deve implementare l&#39;opzione 3. Se si leggono termini come &quot;SAML&quot; o &quot;OAauth&quot; nel manuale del sistema SSO, questo è un forte indicatore che è necessario implementare l&#39;Opzione 3.


**Opzione 2**. &quot;Non memorizzazione in cache&quot; generalmente è una cattiva idea. In questo caso, assicurati che la quantità di traffico e il numero di risorse sensibili escluse siano ridotti. Oppure assicurati che nel sistema Publish sia installata una cache in memoria, che i sistemi Publish possano gestire il carico risultante - più su quello della parte III di questa serie.

**Opzione 3**. Il &quot;caching sensibile alle autorizzazioni&quot; è un approccio interessante. Dispatcher memorizza in cache una risorsa, ma prima di distribuirla, chiede al sistema AEM se può farlo. In questo modo viene creata una richiesta aggiuntiva da Dispatcher alla pubblicazione, ma in genere il sistema di pubblicazione non esegue più il rendering di una pagina se è già memorizzata nella cache. Tuttavia, questo approccio richiede un’implementazione personalizzata. Trova i dettagli qui nell&#39;articolo [Memorizzazione in cache sensibile alle autorizzazioni](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html).

**Riferimenti**

* [helpx.adobe.com - Memorizzazione in cache sensibile alle autorizzazioni](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html)

### Impostazione del periodo di tolleranza

Se si esegue spesso l’invalidazione in breve tempo, ad esempio per l’attivazione di un albero o semplicemente per necessità di mantenere il contenuto aggiornato, può accadere che si stia costantemente scaricando la cache e che i visitatori stiano quasi sempre premendo una cache vuota.

Il diagramma seguente illustra una possibile tempistica per l’accesso a una singola pagina.  Il problema ovviamente peggiora quando il numero di pagine diverse richieste aumenta.

![Attivazioni frequenti che portano a una cache non valida per la maggior parte del tempo](assets/chapter-1/frequent-activations.png)

*Attivazioni frequenti che portano a una cache non valida per la maggior parte del tempo*

<br> 

Per attenuare il problema di questo &quot;temporale di invalidazione della cache&quot; come viene chiamato a volte, puoi essere meno rigoroso circa `statfile` interpretazione.

Puoi impostare Dispatcher in modo che utilizzi un `grace period` per annullamento automatico della validità. In questo modo si aggiungerebbe del tempo in più al `statfiles` data di modifica.

Diciamo, il tuo `statfile` ha un tempo di modifica di oggi 12:00 e `gracePeriod` è impostato su 2 minuti. Quindi tutti i file con annullamento automatico della validità sono considerati validi alle 12:01 e alle 12:02. Vengono riprodotti dopo le 12:02.

La configurazione di riferimento propone un `gracePeriod` di due minuti per una buona ragione. Potreste pensare &quot;Due minuti? Non è quasi niente. Posso facilmente aspettare 10 minuti perché il contenuto venga visualizzato...&quot;.  Potresti essere tentato di impostare un periodo più lungo, ad esempio 10 minuti, partendo dal presupposto che il contenuto venga visualizzato almeno dopo questi 10 minuti.

>[!WARNING]
>
>Non è così `gracePeriod` Sta lavorando. Il periodo di tolleranza è _not_ il tempo dopo il quale un documento è garantito l&#39;annullamento della validità, ma non si verifica alcuna invalidazione. Ogni invalidazione successiva che rientra in questo frame _prolunghe_ l&#39;intervallo di tempo - può essere indefinitamente lungo.

Illustra come `gracePeriod` in realtà sta lavorando con un esempio:

Supponiamo che tu stia gestendo un sito multimediale e che il personale addetto all&#39;editing fornisca regolarmente aggiornamenti dei contenuti ogni 5 minuti. Considera di impostare gracePeriod su 5 minuti.

Inizieremo con un rapido esempio alle 12:00.

12:00 - Lo statfile è impostato su 12:00. Tutti i file memorizzati nella cache sono considerati validi fino alle 12:05.

12:01 - Si verifica un’invalidazione. Questo prolunga il tempo di grata alle 12:06

12:05 - Un altro redattore pubblica il suo articolo - prolungando il tempo di grazia di un altro periodo di grazia a 12:10.

E così via.. il contenuto non viene mai invalidato. Ogni invalidazione *entro* il periodo di tolleranza prolunga effettivamente il tempo di tolleranza. La `gracePeriod` è progettato per resistere alla tempesta di invalidazione... ma alla fine dovete andare fuori nella pioggia... quindi, tenere il `gracePeriod` molto breve per evitare di nascondersi nel rifugio per sempre.

#### Un periodo di tolleranza deterministico

Vorremmo introdurre un&#39;altra idea su come superare una tempesta di invalidazione. È solo un&#39;idea. Non l&#39;abbiamo provato in produzione, ma abbiamo trovato il concetto abbastanza interessante da condividere l&#39;idea con voi.

La `gracePeriod` può diventare imprevedibile se l&#39;intervallo di replica regolare è inferiore al `gracePeriod`.

L&#39;idea alternativa è la seguente: Annulla la validità solo a intervalli di tempo fissi. Il tempo nel mezzo significa sempre servire contenuti datati. L’invalidazione avverrà alla fine, ma una serie di invalidazioni vengono raccolte per un’invalidazione &quot;collettiva&quot;, in modo che Dispatcher possa nel frattempo servire alcuni contenuti memorizzati nella cache e dare al sistema di pubblicazione un po’ di aria da respirare.

L’implementazione sarà simile alla seguente:

È possibile utilizzare uno &quot;Script di annullamento della validità personalizzato&quot; (vedere riferimento), eseguito dopo l’annullamento della validità. Questo script leggerà il `statfile's` ultima data di modifica e arrotondala all&#39;arresto successivo dell&#39;intervallo. Comando della shell Unix `touch --time`Specifica un’ora.

Ad esempio, se imposti il periodo di tolleranza su 30 secondi, Dispatcher arrotonda la data dell’ultima modifica dello statfile ai 30 secondi successivi. Richieste di annullamento della validità in un intervallo impostato per 30 secondi successivi.

![Se si posticipa l’annullamento della validità a 30 secondi, la frequenza degli hit viene aumentata.](assets/chapter-1/postponing-the-invalidation.png)

*Se si posticipa l’annullamento della validità a 30 secondi, la frequenza degli hit viene aumentata.*

<br> 

Gli hit della cache che si verificano tra la richiesta di invalidazione e lo slot successivo di 30 secondi round sono quindi considerati obsoleti; È stato eseguito un aggiornamento su Pubblica , ma Dispatcher continua a distribuire contenuti obsoleti.

Questo approccio potrebbe aiutare a definire periodi di tolleranza più lunghi senza dover temere che le richieste successive prolungino il periodo in modo indeterministico. Anche se come abbiamo detto prima - è solo un&#39;idea e non abbiamo avuto la possibilità di testarla.

**Riferimenti**

[helpx.adobe.com - Configurazione del Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Recupero automatico

Il tuo sito ha un modello di accesso molto particolare. Hai un elevato carico di traffico in entrata e la maggior parte del traffico è concentrata su una piccola frazione delle tue pagine. La pagina principale, le pagine di destinazione della campagna e le pagine di dettaglio dei prodotti più avanzate ricevono il 90% del traffico. Oppure, se si gestisce un nuovo sito, gli articoli più recenti hanno un numero di traffico più elevato rispetto a quelli più vecchi.

Ora, è molto probabile che queste pagine siano memorizzate nella cache di Dispatcher in quanto vengono richieste con tale frequenza.

Viene inviata al Dispatcher una richiesta di annullamento arbitrario della validità, che determina l’annullamento della validità di tutte le pagine, inclusa la pagina più comune.

Successivamente, dato che queste pagine sono così popolari, ci sono nuove richieste in entrata da diversi browser. Prendiamo ad esempio la home page.

Poiché la cache non è valida, tutte le richieste alla home page che si trovano contemporaneamente vengono inoltrate al sistema di pubblicazione generando un carico elevato.

![Richieste parallele alla stessa risorsa su cache vuota: Le richieste vengono inoltrate a Publish](assets/chapter-1/parallel-requests.png)

*Richieste parallele alla stessa risorsa su cache vuota: Le richieste vengono inoltrate a Publish*

Con il recupero automatico è possibile mitigare questo in una certa misura. La maggior parte delle pagine non convalidate viene ancora archiviata fisicamente in Dispatcher dopo l’annullamento automatico della validità. Sono solo _considerato_ vecchio. _Recupero automatico_ significa che si servono ancora queste pagine non aggiornate per alcuni secondi durante l&#39;avvio _un singolo_ richiedi al sistema di pubblicazione di recuperare nuovamente il contenuto non aggiornato:

![Distribuzione di contenuti obsoleti durante il recupero in background](assets/chapter-1/fetching-background.png)

*Distribuzione di contenuti obsoleti durante il recupero in background*

<br> 

Per abilitare il recupero, devi indicare al Dispatcher quali risorse recuperare dopo un’annullamento automatico della validità. Ricorda che qualsiasi pagina da attivare invalida automaticamente anche tutte le altre pagine, incluse quelle più popolari.

Re-fetching in realtà significa informare il Dispatcher in ogni (!) richiesta di annullamento della validità che desideri recuperare quelli più popolari e quelli più popolari.

A tal fine, inserisci un elenco di URL di risorse (URL effettivi, non solo percorsi) nel corpo delle richieste di invalidazione:

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

Quando Dispatcher vede una richiesta di questo tipo, attiva l’annullamento automatico della validità come di consueto e mette immediatamente in coda le richieste per recuperare nuovamente il contenuto fresco dal sistema di pubblicazione.

Poiché ora utilizziamo un corpo della richiesta, è necessario impostare anche il tipo di contenuto e la lunghezza del contenuto in base allo standard HTTP.

Dispatcher inoltre contrassegna internamente gli URL in base ai quali è in grado di distribuire direttamente queste risorse, anche se sono considerati non validi dall’annullamento automatico della validità.

Tutti gli URL elencati sono richiesti uno per uno. Quindi, non devi preoccuparti di creare un carico troppo alto sui sistemi di pubblicazione. Ma non vorreste neanche inserire troppi URL in quell&#39;elenco. Alla fine, la coda deve essere elaborata in un momento limitato per non distribuire contenuti non aggiornati per troppo tempo. È sufficiente includere le 10 pagine più frequentemente visitate.

Se analizzi la directory della cache del Dispatcher, vedrai i file temporanei contrassegnati con marche temporali. Si tratta dei file attualmente in fase di caricamento in background.

**Riferimenti**

[helpx.adobe.com - Annullamento della validità delle pagine in cache da AEM](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

### Protezione del sistema di pubblicazione

Dispatcher offre una maggiore sicurezza proteggendo il sistema di pubblicazione dalle richieste destinate solo a scopi di manutenzione. Ad esempio, non desideri esporti `/crx/de` o `/system/console` URL al pubblico.

Non danneggia l&#39;installazione di un firewall per applicazioni web (WAF) nel sistema. Ma questo aggiunge un numero significativo al vostro bilancio e non tutti i progetti sono in una situazione in cui possono permettersi e - non dimenticate - operare e mantenere un WAF.

Quello che vediamo spesso è un set di regole di riscrittura Apache nella configurazione di Dispatcher che impediscono l’accesso alle risorse più vulnerabili.

Ma si potrebbe anche considerare un approccio diverso:

In base alla configurazione di Dispatcher, il modulo Dispatcher è associato a una determinata directory:

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Ma perché legare l&#39;handler all&#39;intero docroot, quando è necessario filtrare dopo?

È innanzitutto possibile limitare il binding del gestore. `SetHandler` semplicemente associa un gestore a una directory, è possibile eseguire il binding del gestore a un URL o a un pattern URL:

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

In questo caso, non dimenticare di associare sempre il gestore del dispatcher all’URL di invalidazione del Dispatcher, altrimenti non sarai in grado di inviare richieste di invalidazione da AEM al Dispatcher.

Un’altra alternativa per utilizzare Dispatcher come filtro è quella di impostare le direttive di filtro nel `dispatcher.any`

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

Non imponiamo l&#39;uso di una direttiva rispetto all&#39;altra, ma raccomandiamo un&#39;adeguata combinazione di tutte le direttive.

Ma proponiamo che tu consideri la riduzione dello spazio URL il prima possibile nella catena, per quanto necessario, e lo faccia nel modo più semplice possibile. Tenete comunque presente che queste tecniche non sostituiscono un WAF su siti web altamente sensibili. Alcune persone chiamano queste tecniche &quot;Firewall dell&#39;uomo povero&quot; - per un motivo.

**Riferimenti**

[direttiva apache.org-sethandler](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[helpx.adobe.com - Configurazione dell’accesso al filtro dei contenuti](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#ConfiguringAccesstoContentfilter)

### Filtrare utilizzando espressioni regolari e globi

Nei primi giorni era possibile utilizzare solo &quot;globs&quot; - segnaposti semplici per definire i filtri nella configurazione di Dispatcher.

Fortunatamente è stato modificato nelle versioni successive di Dispatcher. Ora è possibile utilizzare anche espressioni regolari POSIX e accedere a varie parti di una richiesta per definire un filtro. Per qualcuno che ha appena iniziato con il Dispatcher che potrebbe essere dato per scontato. Ma se siete abituati ad avere solo i globi, in un certo senso è una sorpresa e facilmente può essere trascurato. Oltre alla sintassi di globs e regexes è semplicemente troppo simile. Confrontiamo due versioni che fanno lo stesso:

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

La versione B utilizza virgolette singole `'` per contrassegnare un _modello a espressione regolare_. &quot;Qualsiasi carattere&quot; è espresso utilizzando `.*`.

_Modelli di globulazione_, invece utilizza virgolette doppie `"` e puoi utilizzare solo segnaposti semplici come `*`.

Se conosci questa differenza, è banale - ma se non lo fai, puoi facilmente combinare le citazioni e passare un pomeriggio soleggiato a debugging della tua configurazione. Adesso siete avvisati.

&quot;Riconosco `'/url'` nella configurazione ... Ma cos&#39;è? `'/glob'` nel filtro si può chiedere?

Tale direttiva rappresenta l&#39;intera stringa di richiesta, incluso il metodo e il percorso. Potrebbe essere

`"GET /content/foo/bar.html HTTP/1.1"`

questa è la stringa rispetto alla quale verrebbe confrontato il pattern. I principianti tendono a dimenticare la prima parte, il `method` (GET, POST, ...). Quindi, uno schema

`/0002  { /glob "/content/\*" /type "allow" }`

Avrà sempre esito negativo perché &quot;/content&quot; non corrisponde a &quot;GET ..&quot; della richiesta.

Quindi quando si desidera utilizzare i Globi,

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

Così:

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Tieni presente che puoi combinare espressioni regex e glob in su regola.

Un&#39;ultima parola sui &quot;numeri di riga&quot; come `/005` davanti a ogni definizione,

Non hanno alcun significato! È possibile scegliere denominatori arbitrari per le regole. L&#39;uso dei numeri non richiede molto sforzo per pensare a uno schema, ma ricorda che l&#39;ordine conta.

Se hai centinaia di regole come questa:

```
/001
/002
/003
…
/100
…
```

e volete inserirne uno tra /001 e /002 cosa succede con i numeri successivi? State aumentando il loro numero? Inserisci numeri intermedi?

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

La numerazione, pur sembrando una scelta semplice in primo luogo, raggiunge i limiti a lungo termine. Ad essere onesti, scegliere i numeri come identificatori è comunque un cattivo stile di programmazione.

Vorremmo proporre un approccio diverso: Molto probabilmente non verranno forniti identificatori significativi per ogni singola regola di filtro. Ma probabilmente servono a uno scopo più grande, in modo che possano essere raggruppati in qualche modo secondo quello scopo. Ad esempio, &quot;configurazione di base&quot;, &quot;eccezioni specifiche dell&#39;applicazione&quot;, &quot;eccezioni globali&quot; e &quot;sicurezza&quot;.

Puoi quindi assegnare un nome e raggruppare le regole di conseguenza e fornire al lettore della configurazione (il tuo caro collega) un certo orientamento nel file:

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


Molto probabilmente aggiungerai una nuova regola a uno dei gruppi - o forse anche creerai un nuovo gruppo. In tal caso, il numero di elementi da rinominare/rinumerare è limitato a tale gruppo.

>[!WARNING]
>
>Le impostazioni più sofisticate suddividono le regole di filtro in un certo numero di file, inclusi i principali `dispatcher.any` file di configurazione. Un nuovo file non introduce tuttavia un nuovo namespace. Quindi se hai una regola &quot;001&quot; in un file e &quot;001&quot; in un altro otterrai un errore. Ancora più ragione per arrivare con nomi semanticamente forti.

**Riferimenti**

[helpx.adobe.com - Progettazione di modelli per le proprietà glob](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#DesigningPatternsforglobProperties)

### Specifiche del protocollo

L&#39;ultimo suggerimento non è un vero suggerimento, ma abbiamo sentito che valeva comunque la pena condividerlo con voi.

Nella maggior parte dei casi, AEM e Dispatcher funzionano come preconfigurato. Pertanto, non troverai una specifica completa del protocollo Dispatcher sul protocollo di invalidazione per creare la tua applicazione in cima. Le informazioni sono pubbliche, ma un po&#39; sparse su una serie di risorse.

Cerchiamo di colmare il divario in qualche misura qui. Esempio di una richiesta di annullamento della validità:

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

`POST /dispatcher/invalidate.cache HTTP/1.1` - La prima riga è l’URL dell’endpoint di controllo di Dispatcher e probabilmente non lo modificherai.

`CQ-Action: <action>` - Cosa dovrebbe accadere. `<action>` è:

* `Activate:` cancella `/path-pattern.*`
* `Deactive:` delete `/path-pattern.*`
E cancella `/path-pattern/*`
* `Delete:`   delete `/path-pattern.*`
E cancella 
`/path-pattern/*`
* `Test:`   Restituisci &quot;ok&quot; ma non fare nulla

`CQ-Handle: <path-pattern>` - Percorso della risorsa contenuto da invalidare. Nota: `<path-pattern>` è in realtà un &quot;percorso&quot; e non un &quot;modello&quot;.

`CQ-Action-Scope: ResourceOnly` - Facoltativo: Se questa intestazione è impostata, la `.stat` file non toccato.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Imposta queste intestazioni, se definisci un elenco di URL di recupero automatico. `<bytes in request body>` è il numero di caratteri nel corpo HTTP

`<newline>` - Se disponi di un corpo della richiesta, deve essere separato dall’intestazione da una riga vuota.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Elenca gli URL che desideri recuperare immediatamente dopo l’annullamento della validità.

## Risorse aggiuntive

Una buona panoramica e introduzione alla memorizzazione in cache di Dispatcher: [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html)

Documentazione di Dispatcher con tutte le direttive spiegate: [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

Alcune domande frequenti: [https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/it/experience-manager/using/dispatcher-faq.html)

Registrazione di un webinar sull’ottimizzazione di Dispatcher - altamente consigliato: [https://my.adobeconnect.com/p7th2gf8k43?proto=true](https://my.adobeconnect.com/p7th2gf8k43?proto=true)

Presentazione &quot;Il potere sottovalutato dell&#39;invalidazione dei contenuti&quot;, conferenza &quot;adaptTo()&quot; a Potsdam 2018 [https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html)

Annullamento della validità delle pagine in cache da AEM: [https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

## Passaggio successivo

* [2 - Modello dell&#39;infrastruttura](chapter-2.md)
