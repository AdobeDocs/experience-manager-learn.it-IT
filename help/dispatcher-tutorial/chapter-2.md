---
title: '"Capitolo 2 - Infrastruttura del Dispatcher"'
description: Comprendere la topologia di pubblicazione e dispatcher. Scopri le topologie e le impostazioni più comuni.
feature: Dispatcher
topic: Architettura
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1866'
ht-degree: 0%

---


# Capitolo 2 - Infrastruttura

## Impostazione di un&#39;infrastruttura di memorizzazione in cache

Abbiamo introdotto la topologia di base di un sistema Publish e un dispatcher nel Capitolo 1 di questa serie. Un set di server Publish e Dispatcher può essere configurato in molte varianti, a seconda del carico previsto, della topologia dei data center e delle proprietà di failover desiderate.

Descriveremo le topologie più comuni e descriveremo i vantaggi e il luogo in cui sono carenti. La lista - ovviamente - non può mai essere completa. L&#39;unico limite è la tua immaginazione.

### Configurazione &quot;Legacy&quot;

All&#39;inizio, il numero di potenziali visitatori era esiguo, l&#39;hardware era costoso e i server web non erano considerati critici come lo sono oggi. Una configurazione comune era quella di avere un Dispatcher che fungesse da load balancer e cache davanti a due o più sistemi Publish. Il server Apache al centro del Dispatcher era molto stabile e, nella maggior parte delle impostazioni, sufficientemente in grado di soddisfare una richiesta decente.

![Configurazione del Dispatcher &quot;Legacy&quot; - Non molto comune negli standard attuali](assets/chapter-2/legacy-dispatcher-setup.png)

*Configurazione del Dispatcher &quot;Legacy&quot; - Non molto comune negli standard attuali*

<br> 

Il dispatcher ha ricevuto il suo nome da: Sostanzialmente era inviare richieste. Questa configurazione non è più molto comune in quanto non è in grado di soddisfare i requisiti più elevati di prestazioni e stabilità richiesti oggi.

### Configurazione con più lame

Oggi una topologia leggermente diversa è più comune. Una topologia a più gambe avrebbe un Dispatcher per server di pubblicazione. Un load balancer dedicato (hardware) si trova di fronte all&#39;infrastruttura AEM che invia le richieste a queste due (o più) gambe:

![Configurazione del Dispatcher standard - Facile da gestire e mantenere](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configurazione del Dispatcher standard - Facile da gestire e mantenere*

<br> 

Ecco i motivi di questo tipo di configurazione,

1. I siti web in media servono molto più traffico di quello che hanno in passato. Pertanto, è necessario aumentare l&#39;&quot;infrastruttura Apache&quot;.

2. La configurazione &quot;Legacy&quot; non forniva ridondanza a livello di Dispatcher. Se Apache Server è stato disattivato, l&#39;intero sito web era irraggiungibile.

3. I server Apache sono economici. Sono basati su open source e, avendo un centro dati virtuale, possono essere forniti molto velocemente.

4. Questa configurazione fornisce un modo semplice per uno scenario di aggiornamento &quot;continuo&quot; o &quot;scaglionato&quot;. È sufficiente arrestare Dispatcher 1 durante l’installazione di un nuovo pacchetto software su Publish 1. Al termine dell’installazione e dopo aver testato in modo sufficiente la pubblicazione 1 dalla rete interna, pulisci la cache su Dispatcher 1 e riavviala durante il ritiro di Dispatcher 2 per la manutenzione di Publish 2.

5. L&#39;invalidazione della cache diventa molto semplice e deterministica in questa configurazione. Poiché un solo sistema di pubblicazione è connesso a un’unica istanza di Dispatcher, è disponibile un’unica istanza di Dispatcher da annullare la validità. L&#39;ordine e la tempistica dell&#39;invalidazione sono banali.

### Impostazione di &quot;Scala in uscita&quot;

I server Apache sono economici e facili da eseguire, perché non spingere un po&#39; di più il ridimensionamento di quel livello. Perché non hai due o più istanze di Dispatcher davanti a ogni server Publish?

![Configurazione di &quot;Scala fuori&quot; - Dispone di alcune aree applicative, ma anche di limitazioni e avvertenze](assets/chapter-2/scale-out-setup.png)

*Configurazione di &quot;Scala fuori&quot; - Dispone di alcune aree applicative, ma anche di limitazioni e avvertenze*

<br> 

Potete assolutamente farlo! E ci sono molti scenari di applicazione validi per quella configurazione. Ma ci sono anche alcune limitazioni e complessità che dovreste considerare.

#### Annullamento della validità

Ogni sistema di pubblicazione è connesso a una moltitudine di istanze di Dispatcher e deve essere invalidato quando il contenuto è stato modificato.

#### Manutenzione

Va da sé che la configurazione iniziale dei sistemi Dispatcher e Publish è un po&#39; più complessa. Ma ricorda anche che lo sforzo di una versione &quot;continua&quot; è un po&#39; più alto. I sistemi AEM possono e devono essere aggiornati durante l&#39;esecuzione. Ma è saggio non farlo mentre stanno attivamente servendo le richieste. Di solito si desidera aggiornare solo una parte dei sistemi di pubblicazione, mentre gli altri continuano a servire attivamente il traffico e poi, dopo il test, passano all&#39;altra parte. Se sei fortunato e puoi accedere al load-balancer nel processo di distribuzione, puoi disattivare il routing ai server in manutenzione qui. Se utilizzi un servizio di bilanciamento del carico condiviso senza accesso diretto, arresta i dispatcher della pubblicazione che desideri aggiornare. Più ci sono, più dovrete chiudere. Se c&#39;è un numero elevato e si sta pianificando aggiornamenti frequenti, è consigliabile un po&#39; di automazione. Se non avete strumenti di automazione, comunque, scalare è una cattiva idea.

In un progetto precedente abbiamo usato un trucco diverso per rimuovere un sistema di pubblicazione dal bilanciamento del carico senza avere accesso diretto al load-balancer stesso.

Il load-balancer di solito &quot;ping&quot;, una pagina particolare per vedere se il server è in esecuzione. Una scelta banale di solito è fare il ping della homepage. Ma se volete usare il ping per segnalare al load-balancer di non bilanciare il traffico scegliereste qualcos&#39;altro. Puoi creare un modello o un servlet dedicato che può essere configurato per rispondere con `"up"` o `"down"` (nel corpo o come codice di risposta http). La risposta di quella pagina ovviamente non deve essere memorizzata nella cache del dispatcher, quindi viene sempre recuperata di recente dal sistema Publish. Ora, se configuri il load balancer per controllare questo modello o servlet puoi facilmente lasciare che la pubblicazione &quot;far finta&quot; che sia giù. Non fa parte del bilanciamento del carico e può essere aggiornato.

#### Distribuzione mondiale

&quot;Distribuzione mondiale&quot; è una configurazione di scalabilità orizzontale in cui sono presenti più istanze di Dispatcher davanti a ciascun sistema di pubblicazione, ora distribuiti in tutto il mondo per essere più vicini al cliente e fornire prestazioni migliori. Naturalmente, in questo scenario non si dispone di un load balancer centrale ma di uno schema di bilanciamento del carico basato su DNS e geo-IP.

>[!NOTE]
>
>In realtà, stai creando una rete di distribuzione dei contenuti (CDN) con questo approccio - quindi dovresti considerare l&#39;acquisto di una soluzione CDN preconfigurata invece di costruirne una tu. La creazione e la manutenzione di una rete CDN personalizzata non è un compito banale.

#### Scala orizzontale

Anche in un datacenter locale una topologia &quot;Scala in uscita&quot; con più Dispatcher davanti a ogni sistema di pubblicazione presenta alcuni vantaggi. Se vedi i colli di bottiglia delle prestazioni sui server Apache a causa di un alto traffico (e di un buon tasso di hit della cache) e non puoi più scalare l&#39;hardware (aggiungendo CPU, RAM e dischi più veloci) puoi migliorare le prestazioni aggiungendo Dispatcher. Questo si chiama &quot;scala orizzontale&quot;. Tuttavia, questo ha dei limiti, specialmente quando si invalida spesso il traffico. Descriveremo l&#39;effetto nella sezione successiva.

#### Limiti della topologia di scalabilità orizzontale

L&#39;aggiunta di server proxy dovrebbe normalmente aumentare le prestazioni. Esistono tuttavia scenari in cui l’aggiunta di server può effettivamente ridurre le prestazioni. Come? Considera di avere un portale di notizie, dove si introducono nuovi articoli e pagine ogni minuto. Un’istanza di Dispatcher invalida per &quot;annullamento automatico della validità&quot;: Ogni volta che una pagina viene pubblicata, tutte le pagine nella cache dello stesso sito vengono invalidate. Questa è una funzionalità utile - l&#39;abbiamo coperta nel [Capitolo 1](chapter-1.md) di questa serie - ma significa anche che quando si hanno frequenti modifiche sul sito web si sta invalidando la cache molto spesso. Se disponi di un’unica istanza di Dispatcher per ogni istanza di pubblicazione, il primo visitatore che richiede una pagina, attiva un re-caching di tale pagina. Il secondo visitatore ottiene già la versione cache.

In presenza di due Dispatcher, il secondo visitatore ha il 50% di possibilità che la pagina non sia memorizzata nella cache e quindi una latenza maggiore si verifica quando la pagina viene riprodotta. Dispatcher ancora più numerosi per pubblicazione peggiorano ulteriormente le cose. Il server di pubblicazione riceve più carico perché deve eseguire nuovamente il rendering della pagina per ogni dispatcher separatamente.

![Prestazioni ridotte in uno scenario di scalabilità orizzontale con scaricamenti frequenti della cache.](assets/chapter-2/decreased-performance.png)

*Prestazioni ridotte in uno scenario di scalabilità orizzontale con scaricamenti frequenti della cache.*

<br> 

#### Riduzione Dei Problemi Di Scala Eccessiva

Per attenuare i problemi, è possibile utilizzare uno storage condiviso centrale per tutti i Dispatcher o sincronizzare i file system dei server Apache. Possiamo fornire solo un’esperienza di prima mano limitata, ma prepararci a che questo aggiunga alla complessità del sistema e possa introdurre una nuova classe di errori.

Abbiamo avuto alcuni esperimenti con NFS - ma NFS introduce enormi problemi di prestazioni a causa del blocco dei contenuti. Ciò ha in realtà ridotto le prestazioni complessive.

**Conclusione**  - La condivisione di un file system comune tra diversi dispatcher NON è un approccio consigliato.

Se si verificano problemi di prestazioni, ridimensiona ugualmente Pubblica e Dispatcher per evitare il caricamento di picco sulle istanze di Publisher. Non esiste una regola d&#39;oro sul rapporto Publish / Dispatcher - dipende fortemente dalla distribuzione delle richieste e dalla frequenza delle pubblicazioni e degli invalidamenti della cache.

Se sei anche preoccupato della latenza vissuta da un visitatore, considera l’utilizzo di una rete di distribuzione dei contenuti, il recupero della cache, il riscaldamento preventivo della cache, l’impostazione di un tempo di tolleranza come descritto in [Capitolo 1](chapter-1.md) di questa serie o fai riferimento ad alcune idee avanzate di [Parte 3](chapter-3.md).

### Impostazione &quot;Cross Connected&quot;

Un&#39;altra configurazione che abbiamo visto ogni tanto è la configurazione &quot;cross-connection&quot;: Le istanze di pubblicazione non dispongono di Dispatcher dedicati, ma tutti i Dispatcher sono connessi a tutti i sistemi di pubblicazione.

![Topologia interconnessa: Maggiore ridondanza e maggiore complessità](assets/chapter-2/cross-connected-setup.png)

<br> 

*Topologia interconnessa: Maggiore ridondanza e maggiore complessità.*

A prima vista, ciò offre una maggiore ridondanza per un budget relativamente ridotto. Quando uno dei server Apache è inattivo, puoi comunque disporre di due sistemi Publish che eseguono il rendering. Inoltre, se si verifica un arresto anomalo di uno dei sistemi di pubblicazione, il caricamento nella cache rimane gestito da due Dispatcher.

Questo però ha un prezzo.

Primo, togliere una gamba per la manutenzione è abbastanza ingombrante. In realtà, questo è lo scopo di questo progetto; essere più resiliente e rimanere in piedi e in funzione con tutti i mezzi possibili. Abbiamo visto piani di manutenzione complicati su come affrontarli. Riconfigura prima il Dispatcher 2, rimuovendo la connessione incrociata. Riavvio di Dispatcher 2. Arresto di Dispatcher 1, aggiornamento Pubblica 1, ... e così via. Si dovrebbe considerare attentamente se questo si sviluppa fino a più di due gambe. Arriverete alla conclusione che in realtà aumenta la complessità, i costi ed è una formidabile fonte di errore umano. Sarebbe meglio automatizzare questo processo. Quindi meglio controllare, se avete effettivamente le risorse umane per includere questo compito di automazione nel vostro programma di progetto. Anche se con questo si potrebbero risparmiare alcuni costi hardware, si potrebbe spendere il doppio per il personale IT.

In secondo luogo, è possibile che alcune applicazioni utente siano in esecuzione sul AEM che richiede un accesso. Utilizza le sessioni permanenti per garantire che un utente venga sempre servito dalla stessa istanza AEM in modo da poter mantenere lo stato di sessione in tale istanza. Avendo questa configurazione interconnessa, è necessario assicurarsi che le sessioni permanenti funzionino correttamente sul load balancer e sui Dispatcher. Non impossibile - ma è necessario essere consapevoli di questo e aggiungere alcune ore di configurazione e test aggiuntive, che - ancora una volta - potrebbero livellare i risparmi che avevate pianificato salvando l&#39;hardware.

### Conclusione

Non consigliamo di utilizzare questo schema di connessione incrociata come opzione predefinita. Tuttavia, se decidi di utilizzarlo, dovrai valutare attentamente i rischi e i costi nascosti e pianificare l’inclusione dell’automazione della configurazione come parte del progetto.

## Passaggio successivo

* [3 - Argomenti avanzati della memorizzazione in cache](chapter-3.md)
