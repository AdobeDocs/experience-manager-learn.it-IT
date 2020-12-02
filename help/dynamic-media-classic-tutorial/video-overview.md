---
title: Panoramica video
description: Dynamic Media Classic viene fornito con la conversione automatica di video in fase di caricamento, streaming video su dispositivi desktop e mobili e set video adattivi ottimizzati per la riproduzione in base al dispositivo e alla larghezza di banda. Scopri di più su video in Dynamic Media Classic e scopri i concetti e la terminologia del video. Per informazioni dettagliate su come caricare e codificare i video, scegliete i predefiniti per video per caricare, aggiungere o modificare un predefinito per video, visualizzare in anteprima i video in un visualizzatore video, distribuire video su siti Web e mobili, aggiungere didascalie e marcatori capitolo ai video e pubblicare i visualizzatori video per utenti desktop e mobili.
sub-product: dynamic-media
feature: Viewer Presets
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '6222'
ht-degree: 0%

---


# Panoramica video {#video-overview}

Dynamic Media Classic viene fornito con la conversione automatica di video in fase di caricamento, streaming video su dispositivi desktop e mobili e set video adattivi ottimizzati per la riproduzione in base al dispositivo e alla larghezza di banda. Uno degli aspetti più importanti del video è che il flusso di lavoro è semplice — è progettato in modo che chiunque possa usarlo, anche se non ha molta familiarità con la tecnologia video.

Alla fine di questa sezione dell&#39;esercitazione potrai imparare a:

- Caricare e codificare video (transcodificare) in dimensioni e formati diversi
- Scegliete tra i predefiniti video disponibili per il caricamento
- Aggiunta o modifica di un predefinito di codifica video
- Anteprima dei video in un visualizzatore video
- Implementare i video in siti Web e mobili
- Aggiunta di didascalie e marcatori capitolo a video
- Personalizzare e pubblicare i visualizzatori video per utenti desktop e mobili

>[!NOTE]
>
>Tutti gli URL di questo capitolo sono solo a scopo illustrativo; non sono collegamenti dinamici.

## Panoramica di Dynamic Media Classic Video

Cominciamo innanzitutto a comprendere meglio le possibilità di utilizzo dei video in Dynamic Media Classic.

### Funzionalità

La piattaforma video Dynamic Media Classic offre tutte le parti della soluzione video — caricamento, conversione e gestione di video; la possibilità di aggiungere didascalie e marcatori di capitolo a un video; e la possibilità di utilizzare i predefiniti per facilitare la riproduzione.

Consente di pubblicare facilmente video adattivi di alta qualità per lo streaming su schermi diversi, inclusi i dispositivi desktop, iOS, Android, Blackberry e Windows Mobile. Un set video adattivo raggruppa versioni dello stesso video codificate con diversi bitrate e formati quali 400, 800 e 1000 kbps. Il computer desktop o il dispositivo mobile rileva la larghezza di banda disponibile.

Inoltre, la qualità video viene modificata automaticamente in modo dinamico se le condizioni della rete cambiano sul desktop o sul dispositivo mobile. Inoltre, se un cliente passa alla modalità a schermo intero su un computer desktop, il set video adattivo risponde utilizzando una risoluzione migliore, in modo da migliorare l’esperienza di visualizzazione del cliente. L’utilizzo di set video adattivi rappresenta la soluzione ottimale per i clienti che riproducono video Dynamic Media Classic su schermi e dispositivi diversi.

### Gestione video

L’utilizzo di video può essere più complesso rispetto all’utilizzo di immagini statiche digitali. I video consentono di gestire numerosi formati e standard e di stabilire se il pubblico sarà in grado di riprodurre le clip. Dynamic Media Classic consente di lavorare facilmente con i video, fornendo molti potenti strumenti &quot;sotto la cappa&quot;, ma rimuovendo la complessità di lavorare con loro.

Dynamic Media Classic riconosce e può essere utilizzato con molti formati sorgente diversi disponibili. Tuttavia, leggere il video è solo una parte dello sforzo — è inoltre necessario convertire il video in un formato adatto al Web. Dynamic Media Classic si occupa di questo problema consentendo di convertire i video in video H.264.

La conversione del video da soli può diventare molto complicato utilizzando i numerosi strumenti professionali ed entusiasti disponibili. Dynamic Media Classic semplifica la gestione grazie alla possibilità di disporre di semplici predefiniti ottimizzati per diverse impostazioni di qualità. Tuttavia, se desiderate un’impostazione più personalizzata, potete anche creare dei predefiniti personalizzati.

Se disponete di molti video, apprezzerete la possibilità di gestire tutte le risorse insieme alle immagini e agli altri contenuti multimediali in Dynamic Media Classic. Potete organizzare, catalogare ed effettuare ricerche nelle risorse, comprese le risorse video, con un solido supporto per metadati XMP.

### Riproduzione video

Simile al problema della conversione dei video per renderli accessibili e semplici da Web, è il problema di implementazione e distribuzione dei video nel sito. Scegliere se acquistare un lettore o costruirne uno proprio, rendendolo compatibile per vari dispositivi e schermi, e quindi mantenere i giocatori può essere un&#39;occupazione a tempo pieno.

Anche in questo caso, l’approccio di Dynamic Media Classic consiste nel consentire di scegliere il predefinito e il visualizzatore adatto alle proprie esigenze. Potete scegliere tra diverse opzioni di visualizzatore e disporre di una libreria di numerosi predefiniti.

Potete facilmente distribuire i video al Web e ai dispositivi mobili, poiché Dynamic Media Classic supporta i video HTML5, il che significa che potete indirizzare gli utenti che eseguono vari browser, nonché gli utenti delle piattaforme Android e iOS. Lo streaming video consente una riproduzione fluida di contenuti più lunghi o ad alta definizione, mentre i video HTML5 progressivi sono dotati di predefiniti ottimizzati per lo schermo di piccole dimensioni.

I predefiniti per visualizzatori per i video sono configurabili parzialmente a seconda del tipo di visualizzatore.

Come tutti i visualizzatori, l’integrazione avviene tramite un singolo URL Dynamic Media Classic per visualizzatore o video.

>[!NOTE]
>
>Come procedura ottimale, usate i visualizzatori per video HTML5 Dynamic Media Classic. I predefiniti utilizzati nei visualizzatori video HTML5 sono lettori video affidabili. Combinando in un singolo lettore la possibilità di progettare i componenti di riproduzione in HTML5 e CSS, di usufruire di riproduzione incorporata e di utilizzare lo streaming adattativo e progressivo a seconda delle capacità del browser, i contenuti multimediali potranno essere visti dagli utenti di computer desktop, tablet e dispositivi mobili e potranno così usufruire di un&#39;esperienza video ottimizzata.

Un&#39;ultima nota sul video Dynamic Media Classic che può essere applicata ad alcuni clienti: non tutte le società possono avere attivato per il proprio account la conversione automatica, lo streaming o i predefiniti per video. Se per qualche motivo non è possibile accedere agli URL per lo streaming video, questo potrebbe essere il motivo. Potrete comunque caricare e pubblicare video con download progressivo e accedere a tutti i visualizzatori video. Tuttavia, per sfruttare tutte le funzionalità video di Dynamic Media Classic, contatta il tuo Account Manager o il responsabile vendite per abilitare queste funzioni.

Ulteriori informazioni su [Video in Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/quick-start-video.html).

## Video 101

### Concetti video e terminologia di base

Prima di iniziare, discutiamo alcuni termini con i quali devi avere familiarità per lavorare con i video. Questi concetti non sono specifici di Dynamic Media Classic e se intendete gestire i video per un sito Web professionale, fareste bene a ricevere ulteriori informazioni sull&#39;argomento. Alla fine di questa sezione verranno consigliate alcune risorse.

- **Codifica/transcodifica.** La codifica consente di applicare la compressione video per convertire i dati video non compressi in un formato che facilita l’utilizzo. La transcodifica, pur essendo simile, si riferisce alla conversione da un metodo di codifica a un altro.

   - I file video principali creati con il software di editing video sono spesso troppo grandi e non nel formato corretto per la distribuzione a destinazioni online. Sono in genere codificati per la riproduzione rapida sul desktop e per la modifica, ma non per la distribuzione sul Web.
   - Per convertire i video digitali nel formato e nelle specifiche corretti per la riproduzione su schermi diversi, i file video vengono transcodificati in un formato file più piccolo ed efficiente, ideale per la visualizzazione sul Web e su dispositivi mobili.

- **Compressione video.** Ridurre la quantità di dati utilizzati per rappresentare le immagini video digitali ed è una combinazione di compressione spaziale dell&#39;immagine e compensazione del movimento temporale.

   - La maggior parte delle tecniche di compressione è senza perdita di dati, il che significa che eliminano i dati per ottenere dimensioni più ridotte.
   - Ad esempio, il video DV viene compresso relativamente poco e consente di modificare facilmente il materiale sorgente, ma è troppo grande per essere utilizzato sul Web o anche messo su un DVD.

- **Formati di file.** Il formato è un contenitore, simile a un file ZIP, che determina come i file vengono organizzati nel file video, ma in genere non come vengono codificati.

   - I formati di file comuni per i video sorgente includono Windows Media (WMV), QuickTime (MOV), Microsoft AVI e MPEG, tra gli altri. I formati pubblicati da Dynamic Media Classic sono MP4.
   - Un file video in genere contiene più tracce — una traccia video (senza audio) e una o più tracce audio (senza video) — correlati e sincronizzati.
   - Il formato del file video determina in che modo vengono organizzate le diverse tracce di dati e metadati.

- **Codec.** Un codec video descrive l’algoritmo in base al quale un video viene codificato mediante la compressione. L&#39;audio viene codificato anche tramite un codec audio.

   - I codec riducono la quantità di informazioni necessarie per riprodurre il video. Invece delle informazioni su ciascun fotogramma, vengono memorizzate solo le informazioni relative alle differenze tra un fotogramma e l’altro.
   - Poiché la maggior parte dei video cambia poco da un fotogramma all’altro, i codec consentono elevati tassi di compressione, il che riduce le dimensioni dei file.
   - Un lettore video decodifica il video in base al relativo codec e visualizza una serie di immagini o fotogrammi sullo schermo.
   - I codec video più comuni includono H.264, On2 VP6 e H.263.

![immagine](assets/video-overview/bird-video.png)

- **Risoluzione.** Altezza e larghezza del video, in pixel.

   - Le dimensioni del video sorgente sono determinate dalla fotocamera e dall&#39;uscita del software di editing. Una videocamera HD di solito crea video ad alta risoluzione 1920 x 1080, ma per una riproduzione fluida sul Web, è necessario eseguire il downsampling (ridimensionamento) a una risoluzione inferiore, ad esempio 1280 x 720, 640 x 480 o inferiore.
   - La risoluzione ha un impatto diretto sulla dimensione del file e sulla larghezza di banda necessaria per riprodurre il video.

- **Proporzioni di visualizzazione.** Proporzioni di larghezza di un video fino all’altezza di un video. Quando le proporzioni del video non corrispondono al rapporto del lettore, è possibile che vengano visualizzate delle &quot;barre nere&quot; o uno spazio vuoto. Due proporzioni comuni utilizzate per visualizzare i video sono:

   - 4:3 (1.33:1). Utilizzata per quasi tutti i contenuti di trasmissione TV a definizione standard.
   - 16:9 (1,78:1). Utilizzata per quasi tutti i contenuti e filmati TV ad alta definizione (HDTV) a schermo intero.

- **Bitrate/velocità dati.** La quantità di dati codificati per creare un secondo di riproduzione video (in kilobit al secondo).

   - Generalmente, più basso è il bit rate, più è desiderabile per il Web perché può essere scaricato più rapidamente. Tuttavia può anche significare che la qualità è bassa a causa della perdita di compressione.
   - Un buon codec dovrebbe bilanciare il bit rate basso con una buona qualità.

- **Frame rate (fotogrammi al secondo o FPS).** Il numero di fotogrammi o immagini fisse per ogni secondo di video. Generalmente, la TV nordamericana (NTSC) viene trasmessa in 29,97 fps; La TV europea e asiatica (PAL) viene trasmessa in 25 fps; e le pellicole (analogiche e digitali) sono generalmente in 24 (23.976) FPS.

   - Per rendere le cose più confuse, ci sono anche fotogrammi progressivi e interlacciati. Ogni fotogramma progressivo contiene un’intera cornice immagine, mentre i fotogrammi interlacciati contengono ogni altra riga di pixel in una cornice immagine. I fotogrammi vengono quindi riprodotti molto rapidamente e sembrano fondersi insieme. Il film utilizza un metodo di scansione progressiva, mentre il video digitale è generalmente interlacciato.
   - In generale, non importa se il filmato sorgente sia interlacciato o meno — Dynamic Media Classic mantiene il metodo di scansione nel video convertito.
   - Streaming/consegna progressiva. Lo streaming video è l’invio di contenuti multimediali in un flusso continuo che può essere riprodotto al momento dell’arrivo, mentre il video scaricato progressivamente viene scaricato come qualsiasi altro file da un server e memorizzato nella cache locale del browser.

Speriamo che questo primer vi aiuti a comprendere le varie opzioni coinvolte nell’utilizzo di video Dynamic Media Classic.

## Flusso di lavoro video

Quando lavorate con i video in Dynamic Media Classic, seguite un flusso di lavoro di base simile a quello delle immagini.

![immagine](assets/video-overview/video-overview-2.png)

1. Per iniziare, caricate i file video in Dynamic Media Classic. A questo scopo, aprite il **menu Strumenti** nella parte inferiore del pannello di estensione Dynamic Media Classic e scegliete **Carica in Dynamic Media Classic > File to folder name** oppure **Carica in Dynamic Media Classic > Folders to name** (Carica in file multimediali dinamici classici > Cartelle come nome cartella). &quot;Nome cartella&quot; è la cartella che si sta attualmente sfogliando con l’estensione. I file video possono essere di grandi dimensioni, pertanto consigliamo di utilizzare l&#39;FTP per caricare file di grandi dimensioni. Come parte del caricamento, scegliete uno o più predefiniti per video per la codifica dei video. Il video può essere transcodificato in video MP4 al momento del caricamento. Per ulteriori informazioni sull’uso e la creazione dei predefiniti di codifica, consultate l’argomento Predefiniti per video di seguito. Informazioni su [Caricamento e codifica di video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. Selezionate o selezionate e modificate un predefinito per visualizzatori video e visualizzate l’anteprima del video. Potete scegliere un predefinito per visualizzatori predefinito o personalizzarne uno personalizzato. Se utilizzate come destinazione gli utenti di dispositivi mobili, non è necessario eseguire alcuna operazione perché le piattaforme mobili non richiedono un visualizzatore o un predefinito. Per ulteriori informazioni su [Anteprima dei video in un visualizzatore video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) e [Aggiunta o modifica di un predefinito per visualizzatori video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. Eseguite una pubblicazione video, ottenete l’URL e integrate. La differenza principale tra questo passaggio per il flusso di lavoro del video e il flusso di lavoro dell’immagine sta nel fatto che verrà eseguita una pubblicazione video speciale invece (o forse anche) della pubblicazione standard di Image Server. L’integrazione del visualizzatore video sul desktop funziona esattamente come l’integrazione del visualizzatore di immagini, ma per i dispositivi mobili è ancora più semplice: tutto ciò di cui avete bisogno è l’URL del video stesso.

### Informazioni sulla transcodifica

La transcodifica è stata definita in precedenza come processo di conversione da un metodo di codifica a un altro. Nel caso di Dynamic Media Classic, è in corso la conversione del video sorgente dal formato corrente a MP4. Questo è necessario prima che il video venga visualizzato nel browser desktop o su un dispositivo mobile.

Dynamic Media Classic è in grado di gestire tutte le transcodifiche, un vantaggio enorme. Potete transcodificare il video voi stessi e caricare i file già convertiti in MP4, ma questo può essere un processo complesso che richiede un software sofisticato. A meno che non si sappia cosa si sta facendo, in genere non si ottengono buoni risultati al primo tentativo.

Dynamic Media Classic non solo converte i file per voi, ma li rende anche semplici grazie alla possibilità di utilizzare i predefiniti. Non c&#39;è bisogno di sapere molto sul lato tecnico di questo processo — tutto ciò che si dovrebbe sapere è approssimativamente la dimensione finale che si desidera ottenere dal sistema e un senso della larghezza di banda che gli utenti finali hanno.

I predefiniti pregenerati sono pratici e coprono la maggior parte delle esigenze, ma a volte possono essere personalizzati. In questo caso potete creare un predefinito di codifica personalizzato. In Dynamic Media Classic, un predefinito di codifica è denominato predefinito per video. Questo verrà spiegato più avanti in questo capitolo.

### Lo streaming

Un&#39;altra caratteristica importante da notare è lo streaming video, una funzione standard della piattaforma video Dynamic Media Classic. I supporti di streaming vengono ricevuti e presentati costantemente agli utenti finali durante la distribuzione. Ciò è significativo e auspicabile per una serie di ragioni.

In genere, lo streaming richiede una larghezza di banda inferiore rispetto al download progressivo, in quanto solo la parte del video visualizzata viene effettivamente distribuita. Il server di streaming video Dynamic Media Classic e i visualizzatori utilizzano il rilevamento automatico della larghezza di banda per fornire il miglior flusso possibile per la connessione Internet dell’utente.

Con lo streaming, la riproduzione del video inizia prima rispetto ad altri metodi. Consente inoltre di utilizzare in modo più efficiente le risorse di rete perché solo le parti del video visualizzate vengono inviate al client.

L&#39;altro metodo di consegna è il download progressivo. Rispetto allo streaming video, il download progressivo presenta un solo vantaggio: non è necessario un server di streaming per distribuire il video. Ed è qui che entra in gioco Dynamic Media Classic — Dynamic Media Classic dispone di un server di streaming integrato nella piattaforma, per cui non è necessario avere la seccatura o i costi aggiuntivi per mantenere questo componente hardware dedicato.

Il video per il download progressivo può essere distribuito da qualsiasi normale server Web. Anche se questo può essere conveniente e potenzialmente conveniente, tenete presente che i download progressivi hanno capacità di ricerca e navigazione limitate e che gli utenti possono accedere e riadattare i contenuti. In alcune situazioni, come la riproduzione dietro firewall di rete molto rigidi, la distribuzione in streaming può essere bloccata; in questi casi, può essere utile ripristinare la distribuzione progressiva.

Il download progressivo è una buona scelta per gli hobbisti o i siti Web che hanno requisiti di traffico ridotti; se non gli dispiace se il contenuto è memorizzato nella cache del computer di un utente; se devono distribuire solo video di lunghezza inferiore a 10 minuti; oppure se i visitatori non possono ricevere lo streaming video per qualche motivo.

Se è necessario disporre di funzioni avanzate e controllare la distribuzione dei video, e/o se è necessario visualizzare il video a un pubblico più ampio (ad esempio, diverse centinaia di visualizzatori simultanei), tenere traccia e segnalare l’utilizzo o le statistiche di visualizzazione, oppure se si desidera offrire agli utenti la migliore esperienza di riproduzione interattiva.

Infine, per proteggere i file multimediali da problemi di proprietà intellettuale o di gestione dei diritti, lo streaming fornisce una distribuzione più sicura dei video, perché il contenuto multimediale non viene salvato nella cache del cliente durante lo streaming.

## Predefiniti per video

Quando caricate il video, potete scegliere tra uno o più predefiniti che contengono le impostazioni per la conversione del video principale in un formato Web mediante codifica. I predefiniti per video sono disponibili in due versioni: Predefiniti video adattati e Predefiniti codifica singola.

Consultate [Predefiniti video disponibili](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

I predefiniti per video adattivi sono attivati per impostazione predefinita, il che significa che sono disponibili per la codifica. Se desiderate usare un predefinito codifica singola, l’amministratore dovrà attivarlo per visualizzarlo nell’elenco dei predefiniti per video.

Scoprite come [Attivare o disattivare i predefiniti per video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

Potete scegliere uno dei numerosi predefiniti pregenerati forniti con Dynamic Media Classic oppure creare dei predefiniti personalizzati; tuttavia, per impostazione predefinita non è selezionato alcun predefinito per il caricamento. In altre parole, **se non selezionate un predefinito per video al momento del caricamento, il video non verrà convertito e potrebbe non essere pubblicato**. Tuttavia potete convertire il video offline e caricarlo e pubblicarlo correttamente. I predefiniti per video sono richiesti solo se desiderate che sia possibile eseguire la conversione da Dynamic Media Classic.

Al momento del caricamento, selezionate un predefinito per video scegliendo **Opzioni video** nel pannello Opzioni processo. Potete quindi scegliere se eseguire la codifica per Computer, Mobile o Tablet.

- Il computer è per l&#39;uso desktop. In questo esempio vengono generalmente individuati predefiniti più grandi (ad esempio HD) che richiedono maggiore larghezza di banda.
- Mobile e Tablet creano video MP4 per dispositivi quali iPhone e smartphone Android. L&#39;unica differenza tra Mobile e Tablet è che i predefiniti Tablet in genere dispongono di una larghezza di banda più elevata, perché sono basati sull&#39;utilizzo WiFi. I predefiniti per dispositivi mobili sono ottimizzati per un utilizzo 3G più lento.

### Domande da porre prima di scegliere un predefinito

Quando scegliete un predefinito, è necessario conoscere sia il pubblico che le riprese sorgente. Cosa sai del tuo cliente? Come stanno guardando il video — con un monitor o un dispositivo mobile?

Qual è la risoluzione del tuo video? Se scegliete un predefinito più grande dell’originale, potete ottenere un video sfocato o con pixel. È corretto se il video è più grande del predefinito, ma non scegliete un predefinito più grande del video sorgente.

Quali sono le sue proporzioni? Se visualizzate delle barre nere intorno al video convertito, scegliete le proporzioni sbagliate. Dynamic Media Classic non è in grado di rilevare automaticamente queste impostazioni perché prima di caricare il file deve essere esaminato.

### Suddivisione opzioni video

I predefiniti per video determinano in che modo il video verrà codificato specificando queste impostazioni. Se non hai familiarità con questi termini, consulta l’argomento Concetti video e Terminologia di base, qui sopra.

![immagine](assets/video-overview/video-overview-3.jpg)

- **Proporzioni.** Generalmente standard 4:3 o wide-screen 16:9.
- **Dimensione.** Si tratta della stessa risoluzione dello schermo, misurata in pixel. Questo è correlato alle proporzioni. Con un rapporto di 16:9, un video sarà di 432 x 240 pixel, mentre a 4:3 sarà di 320 x 240 pixel.
- **FPS.** I frame rate standard sono 30, 25 o 24 frame al secondo (fps), a seconda dello standard video — NTSC, PAL o Film. Questa impostazione non ha importanza, perché Dynamic Media Classic utilizza sempre lo stesso frame rate del video sorgente.
- **Formato.** Sarà MP4.
- **Larghezza di banda.** Questa è la velocità di connessione desiderata per l’utente di destinazione. Hanno una connessione internet veloce o lenta? Utilizzano generalmente computer desktop o dispositivi mobili? Ciò è anche correlato alla risoluzione (dimensione), poiché più grande è il video, più larghezza di banda è necessaria.

### Determinazione della velocità dati o del &quot;bitrate&quot; per il video

Il calcolo del bitrate per il video è uno dei fattori meno conosciuti per la trasmissione video al Web, ma potenzialmente il più importante, in quanto influisce direttamente sull’esperienza dell’utente. Se impostate il bitrate su un valore troppo elevato, otterrete una qualità video elevata ma prestazioni insufficienti. Gli utenti con connessioni Internet più lente saranno costretti ad aspettare mentre il video viene messo in pausa costantemente durante la riproduzione. Tuttavia, se lo impostate su un valore troppo basso, la qualità ne risentirà. All’interno del predefinito per video, Dynamic Media Classic suggerisce una serie di dati a seconda della larghezza di banda di destinazione. È un buon punto di partenza.

Tuttavia, se volete capirlo da soli, avrete bisogno di una calcolatrice del bit rate. Si tratta di uno strumento comunemente utilizzato da professionisti e appassionati di video per stimare la quantità di dati da inserire in un dato flusso o elemento multimediale (come un DVD).

## Creazione di un predefinito per video personalizzato

A volte può essere necessario un predefinito per video speciale che non corrisponda alle impostazioni dei predefiniti per video di codifica incorporati. Questo può accadere se avete un video personalizzato di una dimensione specifica, ad esempio un video creato da un software di animazione 3D o uno ritagliato dalla dimensione originale. È possibile che desideriate sperimentare con diverse impostazioni di larghezza di banda per trasmettere video di qualità superiore o inferiore. In ogni caso, dovrete creare un predefinito video per codifica singola personalizzato.

### Flusso di lavoro predefinito per video

1. I predefiniti per video si trovano in **Configurazione > Impostazione applicazione > Predefiniti per video**. Qui trovate un elenco di tutti i predefiniti di codifica disponibili per la vostra società.

   - Ogni account video in streaming dispone di decine di predefiniti e se create dei predefiniti personalizzati, potete visualizzarli anche qui.
   - Potete filtrare in base al tipo utilizzando il menu a discesa. I predefiniti sono suddivisi in Computer, Mobile e Tablet.
      ![immagine](assets/video-overview/video-overview-4.jpg)

2. La colonna Attivo consente di scegliere se visualizzare tutti i predefiniti al momento del caricamento o solo quelli scelti. Se siete negli Stati Uniti, potete deselezionare i predefiniti PAL europei e, se siete nella regione UK/EMEA, deselezionate i predefiniti NTSC.
3. Fate clic sul pulsante **Aggiungi** per creare un predefinito personalizzato. Viene aperto il pannello Aggiungi predefinito per video. La procedura in questo caso è simile alla creazione di un predefinito per immagini.
4. Innanzitutto, specificate un **nome predefinito** da visualizzare nell&#39;elenco dei predefiniti. Nell’esempio precedente, questo predefinito è per i video di esercitazione sull’acquisizione dello schermo.
5. La **Descrizione** è facoltativa, ma fornirà agli utenti una descrizione che descriverà lo scopo di questo predefinito.
6. Il **Suffisso file di codifica** verrà aggiunto alla fine del nome del video che si sta creando qui. Tenete presente che avrete anche un video principale e un video codificato, derivato dal master, e che nessuna due risorse in Dynamic Media Classic può avere lo stesso ID risorsa.
7. **Il** dispositivo di riproduzione consente di scegliere il formato di file video desiderato (Computer, Mobile o Tablet). Mobile e Tablet producono lo stesso formato MP4. Dynamic Media Classic deve sapere in quale categoria inserire il predefinito; tuttavia, la differenza teorica è che i predefiniti Tablet sono generalmente per una connessione Internet più veloce, perché tutti supportano il WiFi.
8. **Target Data** Rate è una cosa che dovrai trovare per te, ma puoi vedere un intervallo suggerito sull&#39;immagine sottostante. In alternativa, potete trascinare il cursore sulla larghezza di banda di destinazione approssimativa. Per una figura più precisa, utilizzate un calcolatore del bitrate. Ci sono un po&#39; di tentativi ed errori coinvolti.

   ![immagine](assets/video-overview/video-overview-5.jpg)

9. Impostare le **Proporzioni** del file di origine. Questa impostazione è direttamente legata alle dimensioni, sotto. Se scegliete _Personalizzato_, dovrete immettere manualmente sia la larghezza che l&#39;altezza.
10. Se scegliete una proporzione, impostate un valore per **Dimensione risoluzione** e Dynamic Media Classic inserirà automaticamente l’altro valore. Tuttavia, per le proporzioni personalizzate, inserite entrambi i valori. Le dimensioni devono essere in linea con la velocità dati. Se si imposta una velocità dati molto bassa e una dimensione elevata, la qualità risulterebbe scarsa.
11. Fate clic su **Salva** per salvare il predefinito. A differenza di ogni altro predefinito, a questo punto non è necessario pubblicare i predefiniti, perché sono solo per caricare i file. Successivamente, sarà necessario pubblicare i video codificati, ma i predefiniti sono solo per uso interno di Dynamic Media Classic.
12. Per verificare che il predefinito video sia nell&#39;elenco di caricamento, andate a **Carica**.Scegliete **Opzioni processo** ed espandete **Opzioni video**. Il predefinito verrà elencato nella categoria relativa al dispositivo di riproduzione selezionato (Computer, Mobile o Tablet).

Ulteriori informazioni sull&#39;[aggiunta o modifica di un predefinito per video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## Aggiungere sottotitoli al video

In alcuni casi può essere utile aggiungere sottotitoli al video — ad esempio, quando è necessario fornire il video agli utenti in più lingue, ma non si desidera duplicare l’audio in un’altra lingua o registrare nuovamente il video in lingue diverse. Inoltre, l&#39;aggiunta di didascalie offre una maggiore accessibilità per chi ha problemi di udito e utilizza sottotitoli codificati. Dynamic Media Classic semplifica l’aggiunta di didascalie ai video.

Scoprite come [aggiungere sottotitoli a video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-captions-video.html).

## Aggiungere marcatori capitolo al video

Per i video di formato esteso, è probabile che gli utenti apprezzino la capacità e la convenienza offerte dalla navigazione del video con marcatori capitolo. Dynamic Media Classic consente di aggiungere facilmente marcatori capitolo al video.

Scoprite come [aggiungere marcatori capitolo a video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## Argomenti di implementazione video

### Pubblica e copia URL

L’ultimo passaggio del flusso di lavoro per Dynamic Media Classic consiste nel pubblicare i contenuti video. Tuttavia, il video dispone di un proprio processo di pubblicazione, denominato Pubblicazione sul server video, disponibile in Avanzate.

![immagine](assets/video-overview/video-overview-6.jpg)

Scoprite come [Pubblicare il video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

Una volta eseguita la pubblicazione di un video, potrete ottenere un URL per accedere ai video e a eventuali predefiniti per visualizzatori Dynamic Media Classic preconfigurati in un browser Web. Tuttavia, se personalizzate o create un predefinito per visualizzatori video personalizzato, dovrete comunque eseguire una pubblicazione separata sul server immagini.

- Scoprite come [Collegare un URL a un sito mobile o a un sito Web](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- Scoprite come [incorporare il visualizzatore video in una pagina Web](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

Potete anche distribuire il video utilizzando un lettore video personalizzato o di terze parti.

Scoprite come [Implementare i video con un lettore video di terze parti](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

Inoltre, se desiderate utilizzare anche le miniature video — l&#39;immagine estratta dal video — sarà inoltre necessario eseguire una pubblicazione su Image Server. Questo perché l’immagine in miniatura del video si trova sul server immagini, mentre il video stesso si trova sul server video. Le miniature video possono essere usate nei risultati di ricerca video, nelle playlist video e come &quot;fotogramma poster&quot; iniziale che appare nel visualizzatore video prima della riproduzione del video.

Ulteriori informazioni sull&#39;utilizzo di [Miniature video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### Selezione e personalizzazione di un predefinito per visualizzatori

La procedura per selezionare e personalizzare un predefinito per visualizzatori è identica a quella per le immagini. Potete creare un nuovo predefinito o modificarne uno esistente e salvarlo con un nuovo nome, apportare modifiche ed eseguire una pubblicazione da Image Server. Tutti i predefiniti per visualizzatori vengono pubblicati sul server immagini, non solo sui predefiniti per immagini, pertanto per visualizzare i predefiniti nuovi o modificati dovete eseguire una pubblicazione di immagini.

>[!TIP]
>
>Dopo la pubblicazione sul server video, eseguite una pubblicazione su Image Server per pubblicare tutte le miniature associate ai video.

## Ottimizzazione motore di ricerca video

L’ottimizzazione SEO (Search Engine Optimization, ottimizzazione motore di ricerca) consente di migliorare la visibilità di un sito Web o di una pagina Web nei motori di ricerca. Mentre i motori di ricerca ottengono ottime informazioni sui contenuti basati su testo, non possono acquisire in modo adeguato le informazioni sui video a meno che tali informazioni non vengano fornite loro. Mediante Dynamic Media Classic Video SEO, potete usare i metadati per fornire ai motori di ricerca le descrizioni dei video. La funzione Video SEO consente di creare Video Sitemap e feed RSS multimediali (mRSS).

- **Video Sitemap**. Comunica a Google esattamente dove si trova il contenuto video in un sito e cosa lo contiene. Di conseguenza, i video sono completamente ricercabili su Google. Ad esempio, un Video Sitemap può specificare il tempo di esecuzione e le categorie di video.
- **feed** mRSS. Utilizzato dagli editori di contenuti per trasmettere file multimediali a Yahoo! Video Search. Google supporta sia Video Sitemap che il protocollo di feed mRSS per l&#39;invio di informazioni ai motori di ricerca.

Quando create Video Sitemap e feed mRSS, potete decidere quali campi di metadati includere nei file video. In questo modo potete descrivere i video ai motori di ricerca in modo che i motori di ricerca possano indirizzare più accuratamente il traffico verso i video presenti sul vostro sito Web.

Una volta creata la mappa del sito o il feed, potete fare in modo che Dynamic Media Classic lo pubblichi automaticamente, lo pubblichi manualmente o semplicemente generi un file che possa essere modificato in un secondo momento. Inoltre, Dynamic Media Classic può generare e pubblicare automaticamente questo file ogni giorno.

Al termine del processo, invierete il file o l&#39;URL al motore di ricerca. Questa operazione viene eseguita al di fuori di Dynamic Media Classic; tuttavia ne discuteremo brevemente qui di seguito.

### Requisiti per i file Sitemap/mRSS

Affinché Google e altri motori di ricerca non rifiutino i file, devono essere nel formato corretto e includere alcune informazioni. Dynamic Media Classic genererà un file formattato correttamente; tuttavia, se le informazioni non sono disponibili per alcuni dei video, non saranno incluse nel file.

I campi richiesti sono Pagina di destinazione (l’URL della pagina che trasmette il video, non l’URL del video stesso), Titolo e Descrizione. Ogni video deve avere una voce per questi elementi, altrimenti non verrà incluso nel file generato. I campi opzionali sono Tag e Categoria.

Esistono altri due campi obbligatori: URL contenuto, URL della risorsa video stessa e Miniatura, URL di una miniatura del video — tuttavia Dynamic Media Classic inserirà automaticamente tali valori.

Il flusso di lavoro consigliato consiste nell’incorporare questi dati nei video prima di caricarli con XMP metadati, e Dynamic Media Classic li estrae al momento del caricamento. Utilizzereste un&#39;applicazione come  Adobe Bridge — incluso in tutte le applicazioni Adobe Creative Cloud — per compilare i dati in campi di metadati standard.

Seguendo questo metodo, non dovrete immettere manualmente questi dati utilizzando Dynamic Media Classic. Tuttavia, potete anche usare i predefiniti per metadati in Dynamic Media Classic, per immettere rapidamente gli stessi dati ogni volta.

Per ulteriori informazioni sull&#39;argomento, consultate [Visualizzazione, aggiunta ed esportazione di metadati](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![immagine](assets/video-overview/video-overview-7.jpg)

Una volta compilati i metadati, potrete visualizzarli nella visualizzazione Dettagli della risorsa video. Anche le parole chiave possono essere presenti, ma si trovano nella scheda Parole chiave.

- Ulteriori informazioni sull&#39; [Aggiunta di parole chiave](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- Ulteriori informazioni su [Video SEO](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- Informazioni su [Impostazioni per Video SEO](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### Impostazione di Video SEO

L’impostazione di Video SEO inizia con la scelta del tipo di formato desiderato, del metodo di generazione e dei campi di metadati da inserire nel file.

1. Andate a **Configurazione > Impostazione applicazione > Video SEO > Impostazioni**.
2. Scegliere un formato di file dal menu **Modalità generazione**. Il valore predefinito è Disattivato, quindi per attivarlo scegliete Video Sitemap, mRSS o Entrambi.
3. Scegliete se generare automaticamente o manualmente. Per semplicità, si consiglia di impostarlo su **Modalità automatica**. Se scegliete Automatico, impostate anche l&#39;opzione **Contrassegna per pubblicazione**, altrimenti i file non andranno in diretta. I file Sitemap e RSS sono tipi di documento XML e devono essere pubblicati come qualsiasi altra risorsa. Utilizzate una delle modalità manuali se non avete tutte le informazioni pronte ora, o se desiderate eseguire una sola generazione.
4. Compilate i tag di metadati che verranno utilizzati nei file. Questo passaggio non è facoltativo. Come minimo, è necessario includere tre campi contrassegnati da un asterisco (\*): **Pagina di destinazione**, **Titolo** e **Descrizione**. Per utilizzare i metadati per queste attività, trascinare i campi dal pannello Metadati a destra in un campo corrispondente del modulo. Dynamic Media Classic comporrà automaticamente il campo segnaposto con i dati effettivi provenienti da ciascun video. Non è necessario utilizzare i campi di metadati. In questo caso potete digitare del testo statico, ma per ciascun video verrà visualizzato lo stesso testo.
5. Dopo aver inserito le informazioni nei tre campi richiesti, Dynamic Media Classic abilita i pulsanti **Salva** e **Salva e genera**. Fate clic su uno per salvare le impostazioni. Utilizzare **Save** se si è in modalità automatica e si desidera che i file vengano generati in un secondo momento da Dynamic Media Classic. Utilizzare **Save &amp; Generate** per creare il file immediatamente.

### Verifica e pubblicazione di Video Sitemap, feed mRSS o entrambi i file

I file generati verranno visualizzati nella directory principale (base) dell&#39;account.

![immagine](assets/video-overview/video-overview-8.jpg)

Questi file devono essere pubblicati, in quanto lo strumento Video SEO non può eseguire autonomamente una pubblicazione. Fintanto che sono contrassegnati per la pubblicazione, verranno inviati ai server di pubblicazione al successivo avvio della pubblicazione.

Dopo la pubblicazione, i file saranno disponibili in questo formato URL.

![immagine](assets/video-overview/video-url-format.png)

Esempio:

![immagine](assets/video-overview/video-format-example.png)

### Invio ai motori di ricerca

La fase finale del processo consiste nell’inviare i file e/o gli URL ai motori di ricerca. Dynamic Media Classic non può eseguire questo passaggio per voi; tuttavia, se inviate l’URL e non il file XML stesso, il feed dovrebbe essere aggiornato al successivo generazione del file e al verificarsi di una pubblicazione.

Il metodo di invio al motore di ricerca varia, tuttavia per Google si utilizza Google Webmaster Tools. Una volta raggiunta tale posizione, andate a **Configurazione sito > Sitemap** e fate clic sul pulsante **Invia un Sitemap**. Qui puoi inserire l’URL Dynamic Media Classic nei file SEO.

### Video SEO Report

Dynamic Media Classic fornisce un rapporto che mostra quanti video sono stati inclusi correttamente nei file e, cosa più importante, che non sono stati inclusi a causa di errori. Per accedere al rapporto, passate a **Configurazione > Impostazione applicazione > Video SEO > Report**.

![immagine](assets/video-overview/video-overview-9.jpg)

## Implementazione mobile per video MP4

Dynamic Media Classic non include i predefiniti per visualizzatori per dispositivi mobili perché i visualizzatori non sono necessari per riprodurre video sui dispositivi mobili supportati. Se effettuate la codifica in formato H.264 MP4 — mediante conversione al momento del caricamento o precodifica sul desktop, tablet e smartphone supportati saranno in grado di riprodurre i video senza bisogno di un visualizzatore. Questo è supportato sui dispositivi Android e iOS (iPhone e iPad).

Il motivo per cui non è richiesto alcun visualizzatore è che entrambe le piattaforme dispongono del supporto H.264 nativo. Potete incorporare il video in una pagina Web HTML5 oppure incorporarlo nell’applicazione stessa, e i sistemi operativi Android e iOS forniranno un controller per la riproduzione del video.

Di conseguenza, Dynamic Media Classic non fornisce un URL a un visualizzatore per dispositivi mobili, ma fornisce un URL diretto al video. Nella finestra Anteprima per un video MP4, saranno presenti collegamenti per Desktop e Mobile. L’URL mobile fa riferimento al video pubblicato.

Una cosa importante da notare sul video pubblicato è che l’URL elenca il percorso completo del video, non solo l’ID risorsa. Quando lavorate con le immagini, l’immagine viene chiamata in base al relativo ID risorsa, indipendentemente dalla struttura delle cartelle. Tuttavia, per i video, dovete specificare anche la struttura delle cartelle. Negli URL riportati sopra, il video viene memorizzato nel percorso:

![immagine](assets/video-overview/mobile-implement-1.png)

Questo può essere espresso anche come nome della società/percorso della cartella/nome del video.

### Metodo n. 1: Riproduzione browser — Codice HTML5

Per incorporare il video MP4 in una pagina Web, utilizzate il tag video HTML5 .

![immagine](assets/video-overview/browser-playback.png)

Questo metodo funzionerà anche per il Web desktop, ma potresti avere dei problemi con il supporto del browser — non tutti i browser Web per desktop supportano in modo nativo video H.264, incluso Firefox.

### Metodo n. 2: Riproduzione delle app su iOS — Framework Media Player

In alternativa, potete incorporare il video Dynamic Media Classic MP4 nel codice dell’applicazione mobile. Di seguito è riportato un esempio generico per iOS che utilizza il framework Media Player fornito solo a scopo illustrativo:

![immagine](assets/video-overview/app-playback.png)

## Risorse aggiuntive

Guardate il Generatore di competenze per contenuti multimediali dinamici: Video in Dynamic Media Classic[ webinar on-demand per scoprire come usare le funzioni video in Dynamic Media Classic.](https://seminars.adobeconnect.com/p2ueiaswkuze)
