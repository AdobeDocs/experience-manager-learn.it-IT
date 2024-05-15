---
title: Panoramica video
description: Dynamic Media Classic viene fornito con la conversione automatica del video al caricamento, lo streaming video su dispositivi desktop e mobili e set video adattivi ottimizzati per la riproduzione in base al dispositivo e alla larghezza di banda. Ulteriori informazioni sui video in Dynamic Media Classic e introduzione ai concetti video e alla terminologia. Quindi approfondisci come caricare e codificare i video, scegli predefiniti video per il caricamento, aggiungi o modifica un predefinito video, visualizza l’anteprima dei video in un visualizzatore video, distribuisci i video ai siti web e mobili, aggiungi didascalie e marcatori capitoli ai video e pubblica i visualizzatori video per gli utenti desktop e mobili.
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: dfbf316f-3922-4bc7-b3f3-2a5bbdeb7063
duration: 1293
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '6032'
ht-degree: 0%

---

# Panoramica video {#video-overview}

Dynamic Media Classic viene fornito con la conversione automatica del video al caricamento, lo streaming video su dispositivi desktop e mobili e set video adattivi ottimizzati per la riproduzione in base al dispositivo e alla larghezza di banda. Una delle cose più importanti riguardo ai video è che il flusso di lavoro è semplice: è progettato in modo che chiunque possa usarlo, anche se non ha familiarità con la tecnologia video.

Entro la fine di questa sezione dell’esercitazione saprai come:

- Caricare e codificare (transcodificare) i video in formati e dimensioni diversi
- Scegli tra i predefiniti video disponibili per il caricamento
- Aggiungere o modificare un predefinito di codifica video
- Anteprima di video in un visualizzatore video
- Distribuire video su siti Web e mobili
- Aggiungere didascalie e marcatori capitolo al video
- Personalizzare e pubblicare visualizzatori video per utenti desktop e mobili

>[!NOTE]
>
>Tutti gli URL in questo capitolo sono solo a scopo illustrativo; non sono collegamenti live.

## Panoramica del video su Dynamic Media Classic

Vediamo innanzitutto quali sono le possibilità offerte da Dynamic Media Classic.

### Funzionalità e funzionalità

La piattaforma video Dynamic Media Classic offre tutte le parti della soluzione video: caricamento, conversione e gestione di video; la possibilità di aggiungere didascalie e marcatori capitolo a un video; e la possibilità di utilizzare predefiniti per una riproduzione facile.

Semplifica la pubblicazione di video adattivi di alta qualità per lo streaming su più schermi, tra cui dispositivi mobili desktop, iOS, Android™, BlackBerry® e Windows. Un set video adattivo raggruppa le versioni dello stesso video che sono codificate in diversi formati e bit rate, ad esempio 400 kbps, 800 kbps e 1000 kbps. Il computer desktop o il dispositivo mobile rileva la larghezza di banda disponibile.

Inoltre, la qualità video viene commutata automaticamente se cambiano le condizioni di rete sul desktop o sul dispositivo mobile. Inoltre, se un cliente entra in modalità a tutto schermo su un desktop, il set video adattivo risponde utilizzando una risoluzione migliore, migliorando così l’esperienza di visualizzazione del cliente. L’utilizzo di set video adattivi offre la migliore riproduzione possibile per i clienti che riproducono video Dynamic Media Classic su più schermi e dispositivi.

### Gestione video

Lavorare con i video può essere più complesso che lavorare con immagini digitali fisse. Con il video, si affrontano numerosi formati e standard e l&#39;incertezza se il pubblico è in grado di riprodurre le clip. Dynamic Media Classic semplifica l&#39;utilizzo dei video, fornendo molti potenti strumenti &quot;sotto il cofano&quot;, ma rimuovendo la complessità del lavoro con essi.

Dynamic Media Classic riconosce e può lavorare con molti formati di origine diversi disponibili. Tuttavia, leggere il video è solo una parte del lavoro, è necessario anche convertire il video in un formato adatto al web. Dynamic Media Classic si occupa di questo aspetto permettendoti di convertire i video in video H.264.

La conversione del video può essere complicata utilizzando i numerosi strumenti professionali ed entusiasti disponibili. Dynamic Media Classic semplifica il lavoro offrendo semplici predefiniti ottimizzati per diverse impostazioni di qualità. Tuttavia, se desideri qualcosa di più personalizzato, puoi anche creare predefiniti personalizzati.

Se disponi di molti video, apprezzerai la capacità di gestire tutte le tue risorse insieme alle immagini e agli altri supporti in Dynamic Media Classic. Puoi organizzare, catalogare e cercare le risorse, comprese le risorse video, con il supporto affidabile dei metadati XMP.

### Riproduzione video

Analogamente al problema della conversione dei video per renderli facili da usare sul web e accessibili, è il problema dell’implementazione e della distribuzione dei video sul sito. Scegliere se acquistare un lettore o crearne uno proprio, rendendolo compatibile per vari dispositivi e schermi, e quindi mantenere i propri lettori può essere un&#39;occupazione a tempo pieno.

Anche in questo caso, l&#39;approccio di Dynamic Media Classic è quello di consentire di scegliere il predefinito e il visualizzatore che si adatta alle tue esigenze. Sono disponibili numerose opzioni di visualizzazione e una libreria di numerosi predefiniti.

Puoi distribuire facilmente i video al web e ai dispositivi mobili, poiché Dynamic Media Classic supporta i video HTML5, il che significa che puoi indirizzarli a utenti che eseguono vari browser, oltre che a utenti della piattaforma Android™ e iOS. Lo streaming video consente la riproduzione fluida di contenuti più lunghi o ad alta definizione, mentre il video progressive HTML5 dispone di predefiniti ottimizzati per il piccolo schermo.

I predefiniti visualizzatore per video sono parzialmente configurabili a seconda del tipo di visualizzatore.

Come tutti i visualizzatori, l’integrazione avviene tramite un singolo URL Dynamic Media Classic per visualizzatore o video.

>[!NOTE]
>
>Come best practice, utilizza i visualizzatori video di Dynamic Media Classic HTML5. I predefiniti utilizzati nei visualizzatori video di HTML5 sono lettori video affidabili. Combinando in un singolo lettore la possibilità di progettare i componenti di riproduzione utilizzando HTML5 e CSS, disporre di riproduzione incorporata e utilizzare lo streaming adattivo e progressivo a seconda delle funzionalità del browser, si estende la portata dei contenuti rich media agli utenti desktop, tablet e mobili e si garantisce un’esperienza video semplificata.

Un’ultima nota sui video Dynamic Media Classic che può essere applicata ad alcuni clienti: è possibile che non tutte le aziende abbiano la conversione automatica, lo streaming o i predefiniti video abilitati per il loro account. Se per qualche motivo non riesci ad accedere agli URL per lo streaming di video, questo potrebbe essere il motivo. Puoi caricare e pubblicare i video scaricati progressivamente e accedere a tutti i visualizzatori video. Tuttavia, per sfruttare tutte le funzionalità video di Dynamic Media Classic, contattate il vostro Account Manager o Sales Manager per attivarle.

Ulteriori informazioni su [Video in Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/quick-start-video.html).

## Video 101

### Concetti video e terminologia di base

Prima di iniziare, parliamo di alcuni termini che dovresti conoscere per utilizzare il video. Questi concetti non sono specifici per Dynamic Media Classic e, se stai per gestire video per un sito web professionale, faresti bene a ottenere qualche ulteriore formazione sull&#39;argomento. Ti consigliamo alcune risorse alla fine di questa sezione.

- **Codifica/transcodifica.** La codifica consiste nell’applicare la compressione video per convertire i dati video non compressi in un formato che ne semplifichi l’utilizzo. La trascodifica, sebbene simile, si riferisce alla conversione da un metodo di codifica a un altro.

   - I file video master creati con il software di editing video sono spesso troppo grandi e non sono nel formato corretto per la consegna a destinazioni online. Vengono generalmente codificati per la riproduzione rapida sul desktop e per l’editing, ma non per la distribuzione sul web.
   - Per convertire il video digitale nel formato e nelle specifiche corretti per la riproduzione su schermi diversi, i file video vengono transcodificati in un file di dimensioni ridotte ed efficienti, ottimali per la distribuzione sul web e sui dispositivi mobili.

- **Compressione video.** La riduzione della quantità di dati utilizzati per rappresentare immagini video digitali è una combinazione di compressione spaziale dell&#39;immagine e compensazione temporale del movimento.

   - La maggior parte delle tecniche di compressione sono lossy, il che significa che buttano fuori i dati per ottenere una dimensione più piccola.
   - Ad esempio, il video DV è compresso relativamente poco e consente di modificare facilmente il metraggio sorgente; tuttavia, è troppo grande per essere usato sul web o persino messo su un DVD.

- **Formati di file.** Il formato è un contenitore, simile a un file ZIP, che determina il modo in cui i file vengono organizzati nel file video, ma in genere non come vengono codificati.

   - I formati di file più comuni per il video sorgente includono, tra gli altri, Windows Media (WMV), QuickTime (MOV), Microsoft® AVI e MPEG. I formati pubblicati da Dynamic Media Classic sono MP4.
   - Un file video contiene in genere più tracce, ovvero una traccia video (senza audio) e una o più tracce audio (senza video), intercorrelate e sincronizzate.
   - Il formato del file video determina l&#39;organizzazione di queste diverse tracce di dati e metadati.

- **Codec** Un codec video descrive l’algoritmo con cui un video viene codificato utilizzando la compressione. L&#39;audio viene codificato anche attraverso un codec audio.

   - I codec riducono al minimo la quantità di informazioni necessarie per riprodurre il video. Anziché informazioni sui singoli frame, vengono memorizzate solo le informazioni sulle differenze tra un frame e quello successivo.
   - Poiché la maggior parte dei video cambia poco da un fotogramma all&#39;altro, i codec consentono tassi di compressione elevati, che si traducono in dimensioni di file più piccole.
   - Un lettore video decodifica il video in base al relativo codec e quindi visualizza una serie di immagini, o fotogrammi, sullo schermo.
   - I codec video più comuni includono H.264, On2 VP6 e H.263.

![immagine](assets/video-overview/bird-video.png)

- **Risoluzione.** Altezza e larghezza del video in pixel.

   - La dimensione del video sorgente viene determinata dalla fotocamera e dall&#39;output del software di editing. Una videocamera HD crea un video 1920 x 1080 ad alta risoluzione; tuttavia, per riprodurre correttamente sul web, è necessario effettuare il downsampling (ridimensionamento) a una risoluzione inferiore, ad esempio 1280 x 720, 640 x 480 o inferiore.
   - La risoluzione ha un impatto diretto sulle dimensioni del file e sulla larghezza di banda necessaria per riprodurre il video.

- **Proporzioni di visualizzazione.** Rapporto tra la larghezza di un video e l’altezza di un video. Se le proporzioni del video non corrispondono a quelle del lettore, è possibile che vengano visualizzate delle &quot;barre nere&quot; o dello spazio vuoto. Due proporzioni comuni per la visualizzazione dei video:

   - 4:3 (1,33:1). Utilizzato per quasi tutti i contenuti televisivi a definizione standard.
   - 16:9 (1,78:1). Utilizzato per quasi tutti i contenuti TV widescreen, ad alta definizione (HDTV) e film.

- **Velocità in bit/velocità dati.** Quantità di dati codificata per costituire un singolo secondo di riproduzione video (in kilobit al secondo).

   - Generalmente, più basso è il bitrate, più è desiderabile per il web perché può essere scaricato più rapidamente. Tuttavia, può anche significare che la qualità è bassa a causa della perdita di compressione.
   - Un buon codec può bilanciare un bit rate basso con una buona qualità.

- **Frequenza fotogrammi (fotogrammi al secondo, o FPS).** Il numero di fotogrammi, o immagini fisse, per ogni secondo del video. In genere, la TV nordamericana (NTSC) viene trasmessa a 29,97 FPS; la TV europea e asiatica (PAL) viene trasmessa a 25 FPS; e il film (analogico e digitale) sono tipicamente a 24 (23,976) FPS.

   - Per rendere le cose più confuse, ci sono anche frame progressivi e interlacciati. Ogni fotogramma progressivo contiene un intero fotogramma dell&#39;immagine, mentre i fotogrammi interlacciati contengono ogni altra riga di pixel in un fotogramma. I fotogrammi vengono quindi riprodotti rapidamente e sembrano fondersi insieme. La pellicola utilizza un metodo di scansione progressiva, mentre il video digitale è tipicamente interlacciato.
   - In generale, non importa se il metraggio sorgente è interlacciato o meno: Dynamic Media Classic manterrà il metodo di scansione nel video convertito.
   - Consegna in streaming/progressiva. Lo streaming video è l’invio di contenuti multimediali in streaming continuo che possono essere riprodotti all’arrivo, mentre il video scaricato progressivamente viene scaricato come qualsiasi altro file da un server e memorizzato nella cache locale nel browser.

Speriamo che questo primer ti aiuti a comprendere le varie opzioni coinvolte nell&#39;utilizzo dei video Dynamic Media Classic.

## Flusso di lavoro video

Quando si lavora con i video in Dynamic Media Classic, si segue un flusso di lavoro di base simile a quello con le immagini.

![immagine](assets/video-overview/video-overview-2.png)

1. Per iniziare, carica i file video su Dynamic Media Classic. Per eseguire questa operazione, apri la **Menu Strumenti** nella parte inferiore del pannello dell’estensione di Dynamic Media Classic, quindi scegli **Carica in Dynamic Media Classic > File nel nome della cartella**, o **Carica in Dynamic Media Classic > Cartelle nel nome della cartella**. Per &quot;Nome cartella&quot; si intende qualsiasi cartella in cui si sta navigando con l’estensione. I file video possono essere di grandi dimensioni, pertanto si consiglia di utilizzare l’FTP per il caricamento di file di grandi dimensioni. Come parte del caricamento, scegli uno o più predefiniti video per la codifica. Il video può essere transcodificato in video MP4 al momento del caricamento. Per ulteriori informazioni sull’utilizzo e la creazione di predefiniti di codifica, consulta l’argomento Predefiniti video di seguito. Informazioni su [Caricamento e codifica di video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. Seleziona o seleziona e modifica un predefinito visualizzatore video e visualizza l’anteprima del video. Puoi scegliere un predefinito visualizzatore predefinito oppure personalizzarlo. Se esegui il targeting di utenti di dispositivi mobili, non è necessario eseguire alcuna operazione qui perché le piattaforme mobili non richiedono un visualizzatore o un predefinito. Ulteriori informazioni su [Anteprima di video in un visualizzatore video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) e [Aggiunta o modifica di un predefinito per visualizzatori video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. Esegui una pubblicazione video, ottieni l’URL e integra. La differenza principale tra questo passaggio per il flusso di lavoro per video e quello per immagini è che esegui una pubblicazione video speciale invece (o forse e) la pubblicazione standard di Image Server. L’integrazione del visualizzatore video sul desktop funziona esattamente come l’integrazione del visualizzatore di immagini, ma per i dispositivi mobili è ancora più semplice: tutto ciò che serve è l’URL del video stesso.

### Informazioni sulla trascodifica

La trascodifica è stata definita in precedenza come il processo di conversione da un metodo di codifica a un altro. Nel caso di Dynamic Media Classic, si tratta del processo di conversione del video sorgente dal formato corrente in MP4. Questa operazione è necessaria prima che il video venga visualizzato nel browser desktop o su un dispositivo mobile.

Dynamic Media Classic è in grado di gestire tutte le attività di transcodifica, un enorme vantaggio. Puoi trascodificare il video e caricare i file già convertiti in MP4, ma questo può essere un processo complesso che richiede un software sofisticato. A meno che tu non sappia cosa stai facendo, in genere non otterrai buoni risultati al primo tentativo.

Dynamic Media Classic non solo converte i file per voi, ma lo rende anche facile da usare preset. Non è necessario avere molte informazioni sugli aspetti tecnici di questo processo, tutto quello che si dovrebbe sapere sono le dimensioni finali che si desidera ottenere dal sistema e la percezione dell&#39;ampiezza di banda degli utenti finali.

Anche se i predefiniti sono utili e soddisfano la maggior parte delle esigenze, a volte puoi desiderare qualcosa di più personalizzato. In tal caso, puoi creare un predefinito di codifica personalizzato. In Dynamic Media Classic, un predefinito di codifica è denominato predefinito video. Questo viene spiegato più avanti in questo capitolo.

### Informazioni sullo streaming

Un’altra importante funzione degna di nota è lo streaming video, una funzione standard della piattaforma video Dynamic Media Classic. I contenuti multimediali in streaming vengono ricevuti e presentati costantemente all&#39;utente finale durante la distribuzione. Ciò è significativo e auspicabile per diversi motivi.

Lo streaming richiede in genere una larghezza di banda inferiore rispetto al download progressivo, perché viene distribuita solo la parte del video guardata. Il server di streaming video Dynamic Media Classic e i visualizzatori utilizzano il rilevamento automatico della larghezza di banda per fornire il miglior flusso possibile per la connessione Internet di un utente.

Con lo streaming, la riproduzione del video inizia prima rispetto ad altri metodi. Inoltre, ottimizza l&#39;utilizzo delle risorse di rete, in quanto solo le parti del video visualizzate vengono inviate al cliente.

L’altro metodo di consegna è il download progressivo. Rispetto allo streaming video, il download progressivo offre un solo vantaggio: non è necessario un server di streaming per distribuire il video. E qui entra in gioco Dynamic Media Classic — Dynamic Media Classic ha un server di streaming integrato nella piattaforma, quindi non è necessario il fastidio o i costi aggiuntivi per la manutenzione di questo componente hardware dedicato.

I video a download progressivo possono essere trasmessi da qualsiasi normale server web. Anche se questo può essere comodo e potenzialmente conveniente, tieni presente che i download progressivi hanno funzionalità di ricerca e navigazione limitate e che gli utenti possono accedere ai contenuti e riutilizzarli. In alcune situazioni, come la riproduzione dietro firewall di rete rigidi, la distribuzione in streaming può essere bloccata; in questi casi, può essere auspicabile il rollback alla distribuzione progressiva.

Il download progressivo è una buona scelta per gli hobbisti o i siti web che hanno requisiti di traffico ridotti; se non si preoccupano se il loro contenuto è memorizzato nella cache del computer di un utente; se hanno solo bisogno di distribuire video di lunghezza inferiore (sotto i 10 minuti); o se i loro visitatori non possono ricevere video in streaming per qualche motivo.

Se hai bisogno di funzioni avanzate per controllare la distribuzione dei video e/o se devi mostrare i video a un pubblico più ampio (ad esempio, più di 100 visualizzatori simultanei), tracciare e riportare l’utilizzo o visualizzare statistiche, oppure se desideri offrire ai tuoi visualizzatori la migliore esperienza di riproduzione interattiva, devi effettuare lo streaming dei video.

Infine, se si è preoccupati di proteggere i contenuti multimediali per problemi di proprietà intellettuale o di gestione dei diritti, lo streaming offre una distribuzione più sicura dei video, perché i contenuti multimediali non vengono salvati nella cache del cliente durante lo streaming.

## Predefiniti per video

Quando carichi il video, scegli uno o più predefiniti contenenti le impostazioni per la conversione del video principale in un formato adatto al web tramite la codifica. I predefiniti per video sono disponibili in due versioni: predefiniti per video adattivo e predefiniti per codifica singola.

Consulta [Predefiniti video disponibili](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

I predefiniti per video adattivo sono attivati per impostazione predefinita, ovvero sono disponibili per la codifica. Se si desidera utilizzare un predefinito di codifica singolo, l&#39;amministratore deve attivarlo affinché venga visualizzato nell&#39;elenco dei predefiniti video.

Scopri come [Attivare o disattivare i predefiniti video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

Puoi scegliere uno dei tanti predefiniti forniti con Dynamic Media Classic oppure crearne uno tuo; tuttavia, per impostazione predefinita non è selezionato alcun predefinito da caricare. In altre parole, **se non selezioni un predefinito per video al momento del caricamento, il video non verrà convertito e potrebbe non essere pubblicabile**. Tuttavia, puoi convertire il video in modalità offline e caricarlo e pubblicarlo correttamente. I predefiniti per video sono necessari solo se si desidera che Dynamic Media Classic esegua la conversione automaticamente.

Al momento del caricamento, seleziona un predefinito video scegliendo **Opzioni video** nel pannello Opzioni processo. È quindi possibile scegliere se eseguire la codifica per Computer, Dispositivi mobili o Tablet.

- Computer per uso desktop. In questo caso, generalmente si trovano predefiniti più grandi (come ad esempio HD) che occupano una maggiore larghezza di banda.
- Mobile e Tablet creano video MP4 per dispositivi come iPhone e smartphone Android™. L&#39;unica differenza tra Mobile e Tablet è che i predefiniti di Tablet PC hanno in genere una larghezza di banda più elevata, perché sono basati sull&#39;utilizzo di WiFi. I predefiniti per dispositivi mobili sono ottimizzati per un utilizzo più lento di 3G.

### Domande da porsi prima di scegliere un predefinito

Quando si sceglie un predefinito, è necessario conoscere il pubblico e il metraggio sorgente. Cosa sai del tuo cliente? Come stanno guardando il video — con un monitor o un dispositivo mobile?

Qual è la risoluzione del video? Se scegliete un predefinito più grande dell&#39;originale, potete ottenere un video sfocato/pixelato. È corretto se il video è più grande del predefinito, ma non scegliere un predefinito più grande del video sorgente.

Quali sono le proporzioni? Se attorno al video convertito sono visualizzate delle barre nere, le proporzioni sono sbagliate. Dynamic Media Classic non è in grado di rilevare automaticamente queste impostazioni perché prima di caricarle dovrebbe esaminare il file.

### Raggruppamento opzioni video

I predefiniti per video determinano il modo in cui il video viene codificato specificando queste impostazioni. Se non conosci questi termini, consulta l’argomento Concetti video di base e terminologia, sopra.

![immagine](assets/video-overview/video-overview-3.jpg)

- **Proporzioni.** Standard 4:3 o widescreen 16:9.
- **Dimensione.** È uguale alla risoluzione dello schermo ed è misurata in pixel. Questo è legato alle proporzioni. Con un rapporto di 16:9, un video è di 432 x 240 pixel, mentre con un rapporto di 4:3 è di 320 x 240 pixel.
- **FPS.** I frame rate standard sono 30 frame al secondo, 25 frame al secondo o 24 frame al secondo (fps), a seconda degli standard video NTSC, PAL o Film. Questa impostazione non ha importanza, perché Dynamic Media Classic utilizza sempre la stessa frequenza fotogrammi del video sorgente.
- **Formato.** Questo è MP4.
- **Larghezza di banda.** Questa è la velocità di connessione desiderata per l’utente di destinazione. Hanno una connessione Internet veloce o lenta? In genere utilizzano computer desktop o dispositivi mobili? Questo è anche legato alla risoluzione (dimensione), perché più grande è il video, maggiore è la larghezza di banda richiesta.

### Determinazione della velocità dati o &quot;velocità in bit&quot; per il video

Il calcolo del bitrate per il video è uno dei fattori meno conosciuti per trasmettere il video al web, ma potenzialmente il più importante perché influisce direttamente sull’esperienza utente. Se si imposta il bit rate su un valore troppo alto, si otterrà una qualità video elevata ma prestazioni insoddisfacenti. Gli utenti con connessioni Internet più lente sono costretti ad attendere mentre il video viene costantemente messo in pausa durante la riproduzione. Tuttavia, se lo imposti su un valore troppo basso, la qualità ne risente. All&#39;interno del predefinito per video, Dynamic Media Classic consiglia una serie di dati in base alla larghezza di banda di destinazione. È un buon punto di partenza.

Tuttavia, se vuoi scoprirlo da solo, ti servirà una calcolatrice della velocità di trasmissione. Si tratta di uno strumento comunemente utilizzato dai professionisti del video e dagli appassionati per stimare la quantità di dati da inserire in un determinato flusso o elemento multimediale (ad esempio un DVD).

## Creazione di un predefinito video personalizzato

Talvolta potresti aver bisogno di un predefinito per video speciale che non corrisponde alle impostazioni dei predefiniti per video di codifica incorporati. Ciò può accadere se disponi di video personalizzati di una dimensione specifica, ad esempio un video creato da un software di animazione 3D o uno che è stato ritagliato dalle dimensioni originali. È possibile provare diverse impostazioni della larghezza di banda per ottenere video di qualità superiore o inferiore. In ogni caso, crea un predefinito video a codifica singola personalizzato.

### Flusso di lavoro predefinito per video

1. I predefiniti per video si trovano in **Setup (Configurazione) > Application Setup (Impostazione applicazione) > Video Presets (Predefiniti video)**. Qui troverai un elenco di tutti i predefiniti di codifica disponibili per la tua azienda.

   - Ogni account video in streaming ha decine di predefiniti pronti all’uso e, se crei dei tuoi predefiniti personalizzati, li vedi anche qui.
   - Puoi filtrare per tipo utilizzando il menu a discesa. I predefiniti sono divisi in Computer, Mobile e Tablet.
     ![immagine](assets/video-overview/video-overview-4.jpg)

2. La colonna Attivo consente di scegliere se visualizzare tutti i predefiniti al momento del caricamento o solo quelli selezionati. Se siete negli Stati Uniti, potete deselezionare i predefiniti PAL europei e, se siete nel Regno Unito/area EMEA, deselezionare i predefiniti NTSC.
3. Fai clic su **Aggiungi** per creare un predefinito personalizzato. Viene aperto il pannello Aggiungi predefinito video. Il processo è simile alla creazione di un predefinito immagine.
4. Per prima cosa, dagli un **Nome del predefinito** nell&#39;elenco dei predefiniti. Nell’esempio precedente, questo predefinito è per i video tutorial di acquisizione schermata.
5. Il **Descrizione** è opzionale, ma fornisce agli utenti una descrizione che descrive lo scopo di questo predefinito.
6. Il **Suffisso file codifica** viene aggiunto alla fine del nome del video che stai creando qui. Tieni presente che avrai un video principale e questo video codificato, che è un derivato del master, e che nessuna due risorse in Dynamic Media Classic può avere lo stesso ID risorsa.
7. **Dispositivo di riproduzione** è il formato di file video desiderato (computer, dispositivo mobile o tablet). Ricorda che Mobile e Tablet producono lo stesso formato MP4. Dynamic Media Classic deve solo sapere in quale categoria inserire il predefinito; tuttavia, la differenza teorica è che i predefiniti Tablet PC sono in genere per una connessione Internet più veloce perché tutti supportano WiFi.
8. **Velocità dati di destinazione** è qualcosa che dovrai capire da solo, tuttavia puoi vedere un intervallo suggerito sull’immagine qui sotto. In alternativa, è possibile trascinare il dispositivo di scorrimento sulla larghezza di banda di destinazione approssimativa. Per una figura più precisa, utilizzare un calcolatore della velocità di trasmissione. C&#39;è un po&#39; di tentativi ed errori coinvolti.

   ![immagine](assets/video-overview/video-overview-5.jpg)

9. Imposta il file sorgente **Proporzioni**. Questa impostazione è direttamente legata alle dimensioni, di seguito. Se si sceglie _Personalizzato_, dovrai immettere manualmente sia la larghezza che l’altezza.
10. Se si sceglie una proporzione, impostare un valore per **Dimensione risoluzione** , e Dynamic Media Classic inserirà automaticamente l’altro valore. Tuttavia, per le proporzioni personalizzate, inserisci entrambi i valori. La dimensione deve essere in linea con la velocità dati. Se imposti una velocità dati bassa e una dimensione elevata, la qualità sarà scadente.
11. Clic **Salva** per salvare il predefinito. A differenza di ogni altro predefinito, a questo punto non è necessario pubblicare, perché i predefiniti sono solo per il caricamento di file. Successivamente, dovrai pubblicare i video codificati, ma i predefiniti sono solo per uso interno di Dynamic Media Classic.
12. Per verificare che il predefinito per video sia nell’elenco di caricamento, vai a **Carica**. Scegli **Opzioni processo** ed espandi **Opzioni video**. Il predefinito è elencato nella categoria del dispositivo di riproduzione scelto (Computer, Dispositivo mobile o Tablet).

Ulteriori informazioni su [Aggiunta o modifica di un predefinito per video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## Aggiungere didascalie al video

A volte può essere utile aggiungere didascalie al video, ad esempio quando devi fornire il video ai visualizzatori in più lingue, ma non desideri duplicare l’audio in un’altra lingua o registrare nuovamente il video in lingue diverse. Inoltre, l&#39;aggiunta di sottotitoli garantisce una maggiore accessibilità per gli ipoudenti e l&#39;utilizzo di sottotitoli. Dynamic Media Classic consente di aggiungere facilmente sottotitoli ai video.

Scopri come [Aggiungi didascalie al video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-captions-video.html).

## Aggiungere marcatori capitolo al video

Per i video con moduli lunghi, è probabile che i visualizzatori apprezzino la facilità e la praticità offerte dalla navigazione nel video con i contrassegni dei capitoli. Dynamic Media Classic consente di aggiungere facilmente marcatori capitolo al video.

Scopri come [Aggiungere marcatori capitolo al video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## Argomenti sull’implementazione video

### Pubblica e copia URL

L’ultimo passaggio nel flusso di lavoro di Dynamic Media Classic consiste nel pubblicare i contenuti video. Tuttavia, il video dispone di un proprio processo di pubblicazione, denominato Pubblicazione su server video, disponibile in Avanzate.

![immagine](assets/video-overview/video-overview-6.jpg)

Scopri come [Pubblicare il video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

Dopo aver eseguito la pubblicazione di un video, puoi ottenere un URL per accedere ai tuoi video e a eventuali predefiniti per visualizzatori Dynamic Media Classic in un browser web. Tuttavia, se personalizzi o crei un predefinito visualizzatore video personalizzato, devi eseguire una pubblicazione del server immagini separata.

- Scopri come [Collegare un URL a un sito mobile o a un sito web](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- Scopri come [Incorporare il Visualizzatore video in una pagina Web](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

Puoi anche distribuire il video utilizzando un lettore video di terze parti o personalizzato.

Scopri come [Distribuire video tramite un lettore video di terze parti](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

Inoltre, se si desidera utilizzare anche le miniature video, ovvero l&#39;immagine estratta dal video, è necessario eseguire una pubblicazione su Image Server. Questo perché l&#39;immagine di anteprima del video risiede sul server immagini, mentre il video stesso si trova sul server video. Le miniature dei video possono essere utilizzate nei risultati della ricerca video e nelle playlist video e possono essere usate come &quot;fotogramma poster&quot; iniziale che appare nel visualizzatore video prima che il video venga riprodotto.

Ulteriori informazioni su [Utilizzo delle miniature video](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### Selezione e personalizzazione di un predefinito visualizzatore

Il processo di selezione e personalizzazione di un predefinito visualizzatore è identico a quello delle immagini. È possibile creare un predefinito o modificarne uno esistente e salvarlo con un nuovo nome, apportare modifiche ed eseguire una pubblicazione di Image Server. Tutti i predefiniti visualizzatore vengono pubblicati nel server immagini, non solo i predefiniti per le immagini, pertanto è necessario eseguire una pubblicazione immagine per visualizzare i predefiniti nuovi o modificati.

>[!TIP]
>
>Esegui una pubblicazione Image Server dopo la pubblicazione sul server video per pubblicare le miniature associate ai video.

## Ottimizzazione motore di ricerca video

L’ottimizzazione SEO (Search Engine Optimization) è il processo volto a migliorare la visibilità di un sito web o di una pagina web nei motori di ricerca. Sebbene i motori di ricerca siano eccellenti nella raccolta di informazioni sui contenuti basati su testo, non possono acquisire in modo adeguato informazioni sui video, a meno che queste non vengano fornite loro. Utilizzando Dynamic Media Classic Video SEO, puoi utilizzare i metadati per fornire ai motori di ricerca le descrizioni dei tuoi video. La funzione Video SEO consente di creare Video Sitemap e feed Media RSS (mRSS).

- **Video Sitemap**. Informa Google esattamente dove e cosa è il contenuto video su un sito. Pertanto, i video sono completamente ricercabili su Google. Ad esempio, una Video Sitemap può specificare il tempo di esecuzione e le categorie di video.
- **feed mRSS**. Utilizzato dagli editori di contenuti per inviare file multimediali a Yahoo! Ricerca video. Google supporta sia Video Sitemap che il protocollo di feed Media RSS (mRSS) per l’invio di informazioni ai motori di ricerca.

Quando crei Video Sitemap e feed mRSS, decidi quali campi di metadati dai file video includere. In questo modo, descrivi i video ai motori di ricerca in modo che i motori di ricerca possano indirizzare più accuratamente il traffico ai video sul sito web.

Una volta creata la mappa del sito o il feed, Dynamic Media Classic può pubblicarlo automaticamente, pubblicarlo manualmente o semplicemente generare un file che puoi modificare in un secondo momento. Inoltre, Dynamic Media Classic può generare e pubblicare automaticamente questo file ogni giorno.

Al termine del processo, invii il file o l’URL al motore di ricerca. Questa operazione viene eseguita al di fuori di Dynamic Media Classic, ma viene brevemente descritta di seguito.

### Requisiti per i file Sitemap/mRSS

Affinché Google e gli altri motori di ricerca non rifiutino i file, devono essere nel formato corretto e includere determinate informazioni. Dynamic Media Classic genera un file formattato correttamente; tuttavia, se le informazioni non sono disponibili per alcuni video, non vengono inclusi nel file.

I campi obbligatori sono Pagina di destinazione (l’URL della pagina che sta servendo il video, non l’URL del video stesso), Titolo e Descrizione. Ciascun video deve avere una voce per questi elementi, altrimenti non verrà incluso nel file generato. I campi facoltativi sono Tag e Categoria.

Sono disponibili altri due campi obbligatori: URL contenuto, URL della risorsa video e Miniatura, URL di un’immagine di miniatura del video. Tuttavia, Dynamic Media Classic inserirà automaticamente questi valori.

Il flusso di lavoro consigliato consiste nell’incorporare questi dati nei video prima di caricarli utilizzando i metadati XMP, e Dynamic Media Classic li estrarrà al momento del caricamento. Puoi utilizzare un’applicazione come Adobe Bridge, inclusa in tutte le applicazioni Adobe Creative Cloud, per compilare i dati in campi di metadati standard.

Seguendo questo metodo, non sarà necessario immettere manualmente questi dati con Dynamic Media Classic. Tuttavia, puoi anche utilizzare i predefiniti di metadati in Dynamic Media Classic, per immettere rapidamente gli stessi dati ogni volta.

Per ulteriori informazioni su tale argomento, consulta [Visualizzazione, aggiunta ed esportazione di metadati](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![immagine](assets/video-overview/video-overview-7.jpg)

Una volta inseriti i metadati, potrai visualizzarli nella Vista dettagli della risorsa video. È possibile che siano presenti anche parole chiave, che tuttavia si trovano sotto la scheda Parole chiave.

- Ulteriori informazioni su [Aggiunta di parole chiave](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- Ulteriori informazioni su [Video SEO](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- Informazioni su [Impostazioni per Video SEO](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### Impostazione di Video SEO

L’impostazione Video SEO inizia scegliendo il tipo di formato desiderato, il metodo di generazione e i campi di metadati da inserire nel file.

1. Vai a **Configurazione > Impostazione applicazione > Video SEO (Search Engine Optimization) > Impostazioni**.
2. Il giorno **Modalità di generazione** scegliere un formato di file. Il valore predefinito è Off, quindi per abilitarlo, scegli Video Sitemap, mRSS o Both.
3. Scegli se generare automaticamente o manualmente. Per semplicità, si consiglia di impostarlo su **Modalità automatica**. Se si sceglie Automatico, impostare anche **Contrassegna per pubblicazione** oppure i file non verranno pubblicati. I file Sitemap e RSS sono tipi di documento XML e devono essere pubblicati come qualsiasi altra risorsa. Utilizzare una delle modalità manuali se non sono disponibili tutte le informazioni o se si desidera eseguire una sola generazione.
4. Popola i tag di metadati utilizzati nei file. Questo passaggio non è facoltativo. Devi includere almeno i tre campi contrassegnati da un asterisco (\*): **Pagina di destinazione** , **Titolo**, e **Descrizione**. Per utilizzare i metadati per queste attività, trascina i campi dal pannello Metadati a destra in un campo corrispondente del modulo. Dynamic Media Classic compilerà automaticamente il campo segnaposto con i dati effettivi di ciascun video. Non è necessario utilizzare i campi di metadati. In questo caso è possibile digitare del testo statico, che verrà tuttavia visualizzato per ogni video.
5. Dopo aver inserito le informazioni nei tre campi obbligatori, Dynamic Media Classic abiliterà **Salva** e **Salva e genera** pulsanti. Fai clic su uno per salvare le impostazioni. Utilizzare **Salva** se si è in modalità automatica e si desidera che Dynamic Media Classic generi i file in un secondo momento. Utilizzare **Salva e genera** per creare immediatamente il file.

### Verifica e pubblicazione della mappa del sito video, del feed mRSS o di entrambi i file

I file generati verranno visualizzati nella directory principale (base) del tuo account.

![immagine](assets/video-overview/video-overview-8.jpg)

Questi file devono essere pubblicati, in quanto lo strumento Video SEO non può eseguire una pubblicazione da solo. Se sono contrassegnati per la pubblicazione, vengono inviati ai server di pubblicazione alla successiva esecuzione di una pubblicazione.

Dopo la pubblicazione, i file sono disponibili in questo formato URL.

![immagine](assets/video-overview/video-url-format.png)

Esempio:

![immagine](assets/video-overview/video-format-example.png)

### Invio ai motori di ricerca

L’ultimo passaggio del processo consiste nell’inviare i file e/o gli URL ai motori di ricerca. Dynamic Media Classic non può eseguire questo passaggio; tuttavia, supponendo che tu invii l’URL e non il file XML stesso, il feed deve essere aggiornato la prossima volta che il file viene generato e si verifica una pubblicazione.

Il metodo di invio al motore di ricerca varia, tuttavia per Google si utilizzano gli strumenti Webmaster di Google. A questo punto, vai a **Configurazione sito > Sitemap** , quindi fare clic su **Inviare una mappa del sito** pulsante. Qui puoi inserire l’URL Dynamic Media Classic nei file SEO.

### Rapporto Video SEO

Dynamic Media Classic fornisce un rapporto che mostra quanti video sono stati inclusi correttamente nei file e, cosa più importante, quali non sono stati inclusi a causa di errori. Per accedere al rapporto, vai a **Configurazione > Impostazione applicazione > Video SEO (Search Engine Optimization) > Rapporto**.

![immagine](assets/video-overview/video-overview-9.jpg)

## Implementazione mobile per video MP4

In Dynamic Media Classic non sono inclusi i predefiniti visualizzatore per dispositivi mobili, in quanto non è necessario che i visualizzatori riproducano il video su dispositivi mobili supportati. Se codificate nel formato MP4 H.264, convertendo al momento del caricamento o precodificando sul desktop, i tablet e gli smartphone supportati sono in grado di riprodurre i video senza bisogno di un visualizzatore. È supportato sui dispositivi Android e iOS (iPhone e iPad).

Il motivo per cui non è richiesto alcun visualizzatore è che entrambe le piattaforme dispongono del supporto nativo H.264. È possibile incorporare il video in una pagina Web HTML5 oppure incorporarlo nell&#39;applicazione stessa. I sistemi operativi Android e iOS forniranno un controller per riprodurre il video.

Per questo motivo, Dynamic Media Classic non fornisce l’URL a un visualizzatore per dispositivi mobili, ma fornisce direttamente un URL per il video. Nella finestra Anteprima di un video MP4 sono presenti collegamenti per desktop e dispositivi mobili. L’URL del dispositivo mobile punta al video pubblicato.

Una cosa importante da notare sui video pubblicati è che l’URL elenca il percorso completo del video, non solo l’ID risorsa. Quando si lavora con le immagini, le si chiama in base al relativo ID risorsa, indipendentemente dalla struttura delle cartelle. Tuttavia, per i video, devi specificare anche la struttura delle cartelle. Negli URL qui sopra, il video viene memorizzato nel percorso:

![immagine](assets/video-overview/mobile-implement-1.png)

Può anche essere espresso come nome società/percorso cartella/nome video.

### Metodo #1: Riproduzione browser — Codice HTML5

Per incorporare il video MP4 in una pagina web, utilizza il tag video HTML5.

![immagine](assets/video-overview/browser-playback.png)

Questo metodo funziona anche per il web desktop, tuttavia si possono incontrare problemi con il supporto del browser — non tutti i browser web desktop supportano nativamente il video H.264, incluso Firefox.

### Metodo #2: Riproduzione di app su iOS — Framework del lettore multimediale

In alternativa, puoi incorporare il video Dynamic Media Classic MP4 nel codice dell’app mobile. Di seguito è riportato un esempio generico per iOS che utilizza il framework Media Player fornito a solo scopo illustrativo:

![immagine](assets/video-overview/app-playback.png)

## Risorse aggiuntive

Osserva [Dynamic Medie Skill Builder: video in Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) webinar on-demand per scoprire come utilizzare le funzioni video in Dynamic Media Classic.
