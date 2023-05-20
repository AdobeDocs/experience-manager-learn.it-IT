---
title: Flusso di lavoro principale di Dynamic Media Classic e anteprima delle risorse
description: 'Scopri il flusso di lavoro principale in Dynamic Media Classic, che include i tre passaggi seguenti: Creazione (e caricamento), Creazione (e pubblicazione) e Consegna. Poi scopri come visualizzare in anteprima le risorse in Dynamic Media Classic.'
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2703'
ht-degree: 1%

---

# Flusso di lavoro principale di Dynamic Media Classic e anteprima delle risorse {#main-workflow}

Dynamic Media supporta il processo di creazione (e caricamento), authoring (e pubblicazione) e distribuzione. Per iniziare, carica le risorse, quindi esegui un’operazione con tali risorse, ad esempio la creazione di un set di immagini e infine la pubblicazione per renderle live. Il passaggio Build è facoltativo per alcuni flussi di lavoro. Ad esempio, se l’obiettivo è solo quello di ridimensionare e ingrandire le immagini in modo dinamico o di convertire e pubblicare i video per lo streaming, non sono necessari passaggi di generazione.

![immagine](assets/main-workflow/create-author-deliver.jpg)

Il flusso di lavoro nelle soluzioni Dynamic Media Classic consiste in tre passaggi principali:

1. Creare (e caricare) SourceContent
2. Creare (e pubblicare) risorse
3. Consegna risorse

## Passaggio 1: creare (e caricare)

Inizio del flusso di lavoro. In questo passaggio, raccogli o crei il contenuto sorgente che si adatta al flusso di lavoro che stai utilizzando e lo carichi in Dynamic Media Classic. Il sistema supporta più tipi di file per immagini, video e font, ma anche per PDF, Adobe Illustrator e Adobe InDesign.

Consulta l’elenco completo di [Tipi di file supportati](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

Puoi caricare il contenuto sorgente in diversi modi:

- Direttamente dal desktop o dalla rete locale. [Scopri come](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- Da un server FTP di Dynamic Media Classic. [Scopri come](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

La modalità predefinita è Da desktop, in cui è possibile cercare i file nella rete locale e avviare il caricamento.

![immagine](assets/main-workflow/upload.jpg)

>[!TIP]
>
>Non aggiungere manualmente le cartelle. Esegui un caricamento da FTP e utilizza **Includi sottocartelle** per ricreare la struttura delle cartelle all’interno di Dynamic Media Classic.

Le due opzioni di caricamento più importanti sono attivate per impostazione predefinita: **Contrassegna per pubblicazione**, di cui abbiamo parlato prima, e **Sovrascrivere**. Sovrascrivi significa che se il file che viene caricato ha lo stesso nome di un file già presente nel sistema, il nuovo file sostituirà la versione esistente. Se deselezioni questa opzione, il file potrebbe non essere caricato.

### Opzioni Di Sovrascrittura Durante Il Caricamento Delle Immagini

L’opzione Sovrascrivi immagine può essere impostata per l’intera azienda in quattro varianti che spesso vengono fraintese. In breve, puoi impostare le regole in modo che le risorse con lo stesso nome vengano sovrascritte più frequentemente oppure desideri che le sovrascritture si verifichino meno frequentemente (in tal caso la nuova immagine verrà rinominata con un’estensione &quot;-1&quot; o &quot;-2&quot;).

- **Sovrascrivi in cartella corrente, nome/estensione immagine di base uguale**.
Questa opzione è la regola più rigorosa per la sostituzione. È necessario caricare l&#39;immagine sostitutiva nella stessa cartella dell&#39;originale e che l&#39;immagine sostitutiva abbia la stessa estensione del nome file dell&#39;originale. Se questi requisiti non vengono soddisfatti, viene creato un duplicato.

- **Sovrascrivi in cartella corrente, nome come risorsa base, ignora estensione**.
Richiede di caricare l&#39;immagine sostitutiva nella stessa cartella dell&#39;originale, tuttavia l&#39;estensione del nome file può essere diversa dall&#39;originale. Ad esempio, chair.tif sostituisce chair.jpg.

- **Sovrascrivi in qualsiasi cartella, nome/estensione come risorsa base**.
Richiede che l’immagine sostitutiva abbia la stessa estensione del nome file dell’immagine originale (ad esempio, chair.jpg deve sostituire chair.jpg, non chair.tif ). Tuttavia, è possibile caricare l&#39;immagine sostitutiva in una cartella diversa da quella originale. L&#39;immagine aggiornata si trova nella nuova cartella; il file non è più disponibile nella posizione originale.

- **Sovrascrivi in qualsiasi cartella, nome come risorsa base, ignora estensione**.
Questa opzione è la regola di sostituzione più inclusiva. È possibile caricare un&#39;immagine sostitutiva in una cartella diversa da quella originale, caricare un file con un&#39;estensione diversa e sostituire il file originale. Se il file originale si trova in una cartella diversa, l&#39;immagine sostitutiva risiede nella nuova cartella in cui è stata caricata.

Ulteriori informazioni su [Opzione Sovrascrivi immagini](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Sebbene non sia obbligatorio, durante il caricamento utilizzando uno dei due metodi descritti sopra, è possibile specificare le Opzioni processo per quel particolare caricamento, ad esempio per pianificare un caricamento ricorrente, impostare le opzioni di ritaglio al momento del caricamento e molte altre ancora. Questi possono essere utili per alcuni flussi di lavoro, quindi vale la pena considerare se possono esserlo per i tuoi.

Ulteriori informazioni su [Opzioni processo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

Il caricamento è il primo passaggio necessario in qualsiasi flusso di lavoro perché Dynamic Media Classic non può funzionare con contenuti non già presenti nel sistema. Dietro le quinte durante il caricamento, il sistema registra ogni risorsa caricata con il database Dynamic Media Classic centralizzato, assegna un ID e lo copia nell&#39;archiviazione. Inoltre, il sistema converte i file di immagine in un formato che consente il ridimensionamento e lo zoom dinamici e converte i file video nel formato MP4 compatibile con il Web.

### Concetto: cosa succede alle immagini caricate in Dynamic Media Classic

Quando carichi un’immagine di qualsiasi tipo in Dynamic Media Classic, questa viene convertita in un formato immagine principale denominato Pyramid TIFF o P-TIFF. Un TIFF P è simile al formato di un&#39;immagine bitmap di TIFF con livelli, con la differenza che il file contiene più dimensioni (risoluzioni) della stessa immagine anziché livelli diversi.

![immagine](assets/main-workflow/pyramid-p-tiff.png)

Quando l&#39;immagine viene convertita, Dynamic Media Classic crea un&#39;&quot;istantanea&quot; delle dimensioni intere dell&#39;immagine, la ridimensiona della metà e la salva, la ridimensiona di nuovo della metà e la salva e così via fino a riempirla di multipli delle dimensioni originali. Ad esempio, un TIFF-P da 2000 pixel ha dimensioni di 1000, 500, 250 e 125 pixel (e più piccole) nello stesso file. Il file P-TIFF è il formato di quella che in Dynamic Media Classic viene definita &quot;immagine principale&quot;.

Quando si richiede un&#39;immagine di determinate dimensioni, la creazione di P-TIFF consente al server immagini per Dynamic Media Classic di trovare rapidamente le dimensioni successive più grandi e di ridimensionarle. Ad esempio, se carichi un’immagine da 2000 pixel e richiedi un’immagine da 100 pixel, Dynamic Media Classic trova la versione da 125 pixel e la ridimensiona a 100 pixel anziché da 2000 a 100 pixel. In questo modo l&#39;operazione è molto veloce. Inoltre, quando si ingrandisce un’immagine, il visualizzatore zoom può richiedere solo una sezione dell’immagine da ingrandire, anziché l’intera immagine a risoluzione completa. In questo modo il formato dell&#39;immagine master, il file P-TIFF, supporta sia il dimensionamento dinamico che lo zoom.

Allo stesso modo, potete caricare il vostro video sorgente principale su Dynamic Media Classic, e al caricamento Dynamic Media Classic può automaticamente ridimensionarlo e convertirlo nel formato MP4 Web-friendly.

### Regole generali per determinare le dimensioni ottimali per le immagini caricate

**Carica le immagini con le dimensioni più grandi che desideri.**

- Per eseguire lo zoom, caricate un&#39;immagine ad alta risoluzione con una gamma di 1500-2500 pixel nelle dimensioni più lunghe. Considera la quantità di dettagli che desideri fornire, la qualità delle immagini sorgente e le dimensioni del prodotto mostrato. Ad esempio, caricate un&#39;immagine da 1000 pixel per un anello minuscolo, ma un&#39;immagine da 3000 pixel per un&#39;intera scena della stanza.
- Se non è necessario eseguire lo zoom, caricalo nelle dimensioni esatte visualizzate. Ad esempio, se desideri inserire nelle pagine loghi o immagini di banner, caricale esattamente alla dimensione 1:1 e chiamale esattamente a quella dimensione.

**Non sovrastimare o far esplodere le immagini prima di caricarle su Dynamic Media Classic.** Ad esempio, non eseguire l’upsampling di un’immagine più piccola per renderla un’immagine da 2000 pixel. Non avrà un buon aspetto. Rendi le tue immagini il più possibile perfette prima di caricarle.

**Non esiste una dimensione minima per lo zoom, ma per impostazione predefinita i visualizzatori non eseguiranno lo zoom oltre il 100%.** Se l&#39;immagine è troppo piccola, non verrà ingrandita affatto o verrà ingrandita solo di una piccola quantità per evitare che risulti danneggiata.

**Non esiste un valore minimo per la dimensione dell’immagine, ma si sconsiglia di caricare immagini giganti.** Un’immagine gigante può essere considerata di oltre 4000 pixel. Caricare immagini di queste dimensioni può mostrare potenziali difetti come granelli di polvere o peli nell&#39;immagine. Tali immagini occupano più spazio sul server Dynamic Media Classic, il che può comportare il superamento dei limiti di archiviazione previsti dal contratto.

Ulteriori informazioni su [Caricamento dei file](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Passaggio 2: Creazione (e pubblicazione)

Dopo aver creato e caricato i contenuti, creerai nuove risorse rich media dalle risorse caricate eseguendo uno o più flussi di lavoro secondari. Questo include tutti i diversi tipi di raccolte di set: Set di immagini, Campioni, Rotazioni e File multimediali diversi, nonché Modelli. Include anche un video. In seguito verranno forniti maggiori dettagli su ogni tipo di set di raccolta di immagini e sui rich media video. Tuttavia, nella maggior parte dei casi, per iniziare seleziona una o più risorse (o non hai selezionato alcuna risorsa) e scegli il tipo di risorsa da generare. Ad esempio, puoi selezionare un’immagine principale e alcune visualizzazioni di tale immagine e scegliere di creare un set di immagini, una raccolta di visualizzazioni alternative dello stesso prodotto.

>[!IMPORTANT]
>
>Assicurati che tutte le risorse siano contrassegnate per la pubblicazione. Anche se per impostazione predefinita tutte le risorse sono contrassegnate automaticamente per la pubblicazione al caricamento, tutte le risorse appena create dal contenuto caricato dovranno essere contrassegnate per la pubblicazione.

Dopo aver creato la nuova risorsa, eseguirai un processo di pubblicazione. Puoi eseguire questa operazione manualmente o pianificare un processo di pubblicazione che viene eseguito automaticamente. La pubblicazione copia tutti i contenuti dalla sfera privata di Dynamic Media Classic a quella pubblica e la sfera server di pubblicazione dell’equazione. Il prodotto di un processo di pubblicazione Dynamic Media è un URL univoco per ogni risorsa pubblicata.

Il server in cui esegui la pubblicazione dipende dal tipo di contenuto e di flusso di lavoro. Ad esempio, tutte le immagini vengono inviate al server immagini e trasmesse in streaming al server FMS. Per comodità si parla di &quot;pubblicazione&quot; come di un singolo evento su un singolo server.

La pubblicazione pubblica tutti i contenuti contrassegnati per la pubblicazione, non solo i contenuti. In genere, un singolo amministratore pubblica le per conto di tutti anziché di singoli utenti che eseguono una pubblicazione. L’amministratore può pubblicare in base alle esigenze o impostare un processo ricorrente giornaliero, settimanale o anche ogni 10 minuti che verrà pubblicato automaticamente. Pubblicare secondo un programma appropriato per la tua azienda.

>[!TIP]
>
>Automatizza i processi di pubblicazione e pianifica l’esecuzione di una pubblicazione completa ogni giorno alle 00:00 o in qualsiasi momento della sera.

### Concetto: Informazioni sull’URL di Dynamic Media Classic

Il prodotto finale di un flusso di lavoro Dynamic Media Classic è un URL che punta alla risorsa (set di immagini o set di video adattivi). Questi URL sono molto prevedibili e seguono lo stesso pattern. Nel caso delle immagini, ogni immagine viene generata dall’immagine master P-TIFF.

Di seguito è riportata la sintassi per l’URL di un’immagine, con un paio di esempi:

![immagine](assets/main-workflow/dmc-url.jpg)

Nell’URL, tutto ciò che si trova a sinistra del punto interrogativo è il percorso virtuale di un’immagine specifica. Tutto a destra del punto interrogativo è un modificatore di Image Server, un&#39;istruzione per elaborare l&#39;immagine. Quando si dispone di più modificatori, questi vengono separati da e commerciali.

Nel primo esempio, il percorso virtuale dell&#39;immagine &quot;Backpack_A&quot; è `http://sample.scene7.com/is/image/s7train/BackpackA`. I modificatori Image Server ridimensionano l&#39;immagine a una larghezza di 250 pixel (da wid=250) e la ricampionano utilizzando l&#39;algoritmo di interpolazione Lanczos, che aumenta la nitidezza durante il ridimensionamento (da resMode=sharp2).

Il secondo esempio applica il cosiddetto &quot;predefinito immagine&quot; alla stessa immagine Backpack_A, come indicato da $!_template300$. I simboli $ su entrambi i lati dell&#39;espressione indicano che all&#39;immagine viene applicato un predefinito immagine, un set di modificatori di immagini.

Una volta compreso come vengono assemblati gli URL di Dynamic Media Classic, puoi modificarli a livello di programmazione e integrarli più a fondo nel sito e nei sistemi back-end.

### Concetto: Informazioni sul ritardo nella memorizzazione in cache

Le nuove risorse caricate e pubblicate vengono visualizzate immediatamente, mentre le risorse aggiornate possono essere soggette al ritardo di 10 ore nella memorizzazione in cache. Per impostazione predefinita, tutte le risorse pubblicate sono disponibili almeno 10 ore prima della scadenza. Diciamo minimo, perché ogni volta che l&#39;immagine viene visualizzata, inizia un orologio che non scade prima di 10 ore in cui nessuno ha visualizzato quell&#39;immagine. Questo periodo di 10 ore è il &quot;Time to Live&quot; di una risorsa. Una volta scaduta la cache per quella risorsa, è possibile distribuire la versione aggiornata.

In genere si tratta di un problema solo se si è verificato un errore e l’immagine/risorsa ha lo stesso nome della versione pubblicata in precedenza, ma si è verificato un problema con l’immagine. Ad esempio, avete caricato accidentalmente una versione a bassa risoluzione o il direttore artistico non ha approvato l&#39;immagine. In questo caso, vuoi richiamare l’immagine originale e sostituirla con una nuova versione utilizzando lo stesso ID risorsa.

Scopri come [Cancella manualmente la cache per gli URL da aggiornare](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=it).

>[!TIP]
>
>Per evitare problemi di ritardo nella memorizzazione in cache, lavora sempre in anticipo: una sera, un giorno, due settimane, ecc. Costruisci in tempo per il QA/l&#39;accettazione da parte delle parti interne per comprovare il tuo lavoro prima di rilasciarlo al pubblico. Lavorare ancora una sera prima consente di apportare modifiche e ripubblicare quella sera. La mattina sono trascorse 10 ore e la cache viene aggiornata con l’immagine corretta.

- Ulteriori informazioni su [Creazione di un processo di pubblicazione](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- Ulteriori informazioni su [Pubblicazione](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Passaggio 3: consegnare

Ricorda che il prodotto finale di un flusso di lavoro Dynamic Media Classic è un URL che punta alla risorsa. L’URL può puntare a una singola immagine, a un set di immagini, a un set 360 gradi o a un altro insieme di immagini o video. Devi prendere quell’URL e fare qualcosa con esso, ad esempio modificare il HTML in modo che il `<IMG>` i tag puntano all’immagine Dynamic Media Classic anziché a un’immagine proveniente dal sito corrente.

Nel passaggio Consegna, devi integrare tali URL nel sito web, nell’app mobile, nella campagna e-mail o in qualsiasi altro punto di contatto digitale in cui desideri visualizzare la risorsa.

Esempio di integrazione dell’URL Dynamic Media Classic di un’immagine in un sito web:

![immagine](assets/main-workflow/example-url-1.png)

L’URL in rosso è l’unico elemento specifico di Dynamic Media Classic.

Il tuo team IT o il tuo partner di integrazione può prendere l’iniziativa di scrivere e modificare il codice per integrare gli URL di Dynamic Media Classic nel sito. Adobe dispone di un team di consulenza che può contribuire a questo sforzo, fornendo assistenza tecnica, creativa o generale.

Per soluzioni più complesse, come i visualizzatori di zoom o i visualizzatori che combinano lo zoom con viste alternative, in genere l’URL punta a un visualizzatore ospitato da Dynamic Media Classic e all’interno di tale URL rappresenta un riferimento a un ID risorsa.

Esempio di un collegamento (in rosso) che aprirà un set di immagini in un visualizzatore in una nuova finestra a comparsa:

![immagine](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Devi integrare gli URL di Dynamic Media Classic nel tuo sito web, nell’app mobile, nella posta elettronica e in altri punti di contatto digitali: Dynamic Media Classic non può farlo per te!

## Anteprima delle risorse

È probabile che tu voglia visualizzare in anteprima le risorse caricate, che stai creando o modificando, per essere certo che vengano visualizzate nel modo desiderato quando i clienti le visualizzano. È possibile accedere alla finestra Anteprima facendo clic su qualsiasi **Anteprima** , nella miniatura della risorsa, nella parte superiore della sezione **Sfoglia/Genera pannello**, o andando in **File > Anteprima**. In una finestra del browser, viene visualizzata un’anteprima di qualsiasi risorsa presente nel pannello, che si tratti di un’immagine, di un video o di una risorsa creata come un set di immagini.

### Anteprima dimensione dinamica (predefiniti immagine)

È possibile visualizzare in anteprima le immagini in più dimensioni utilizzando **Dimensioni** anteprima. Viene caricato un elenco dei predefiniti immagine disponibili. I predefiniti immagine verranno discussi in seguito, ma vengono considerati come &quot;ricette&quot; per il caricamento dell&#39;immagine in una dimensione specifica con quantità specifiche di nitidezza e qualità dell&#39;immagine.

### Anteprima zoom

È inoltre possibile utilizzare **Zoom** opzione per visualizzare l’anteprima dell’immagine in uno dei molti predefiniti di zoom predefiniti, basati su diversi visualizzatori di zoom inclusi.

Ulteriori informazioni su [Anteprima delle risorse](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html).
