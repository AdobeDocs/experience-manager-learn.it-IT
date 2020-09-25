---
title: Flusso di lavoro principale Dynamic Media Classic e anteprima delle risorse
description: 'Scopri il flusso di lavoro principale in Dynamic Media Classic, che include tre passaggi: Creazione (e caricamento), Autore (e pubblicazione) e Consegna. Quindi scoprite come visualizzare in anteprima le risorse in Dynamic Media Classic.'
sub-product: dynamic-media
feature: workflow
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '2729'
ht-degree: 1%

---


# Flusso di lavoro principale Dynamic Media Classic e anteprima delle risorse {#main-workflow}

Dynamic Media supporta un processo di creazione (e caricamento), creazione (e pubblicazione) e distribuzione di flussi di lavoro. Per iniziare, caricate le risorse e quindi fate qualcosa con quelle risorse come la creazione di un set di immagini, e infine pubblicatele per renderle attive. Il passaggio Genera è facoltativo per alcuni flussi di lavoro. Ad esempio, se l’obiettivo è solo quello di eseguire il ridimensionamento dinamico e lo zoom sulle immagini o convertire e pubblicare i video per lo streaming, non sono necessari passaggi di compilazione.

![immagine](assets/main-workflow/create-author-deliver.jpg)

Il flusso di lavoro nelle soluzioni Dynamic Media Classic consiste di tre passaggi principali:

1. Creare (e caricare) SourceContent
2. Creare (e pubblicare) risorse
3. Distribuisci risorse

## Passaggio 1: Crea (e carica)

Questo è l&#39;inizio del flusso di lavoro. In questo passaggio, potete raccogliere o creare il contenuto sorgente adatto al flusso di lavoro in uso e caricarlo in Dynamic Media Classic. Il sistema supporta più tipi di file per immagini, video e font, ma anche per PDF,  Adobe Illustrator e  Adobe InDesign.

Consultate l&#39;elenco completo dei tipi [di file](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats)supportati.

Potete caricare il contenuto sorgente in diversi modi:

- Direttamente dal desktop o dalla rete locale. [Scopri come](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- Da un server FTP Dynamic Media Classic. [Scopri come](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

La modalità predefinita è Da desktop, dove potete cercare i file nella rete locale e avviare il caricamento.

![immagine](assets/main-workflow/upload.jpg)

>[!TIP]
>
>Non aggiungere manualmente le cartelle. Eseguite invece un caricamento dall&#39;FTP e utilizzate l&#39;opzione **Includi sottocartelle** per ricreare la struttura delle cartelle all&#39;interno di Dynamic Media Classic.

Per impostazione predefinita, le due opzioni di caricamento più importanti sono attivate: **Contrassegna per pubblicazione**, di cui abbiamo già discusso in precedenza, e **Sovrascrivi**. Sovrascrivi indica che se il file caricato ha lo stesso nome di un file già presente nel sistema, il nuovo file sostituirà la versione esistente. Se deselezionate questa opzione, il file potrebbe non essere caricato.

### Opzioni Sovrascrivi per il caricamento delle immagini

Esistono quattro varianti dell’opzione Sovrascrivi immagine che possono essere impostate per l’intera società e che spesso non vengono comprese. In breve, potete impostare le regole in modo che le risorse con lo stesso nome vengano sovrascritte più frequentemente, oppure che le sovrascritture avvengano meno frequentemente (nel qual caso la nuova immagine verrà rinominata con l’estensione &quot;-1&quot; o &quot;-2&quot;).

- **Sovrascrivi in cartella corrente, nome/estensione**stessa immagine di base.
Questa opzione è la regola più restrittiva per la sostituzione. Richiede che l’immagine sostitutiva venga caricata nella stessa cartella dell’originale e che abbia la stessa estensione del nome file dell’originale. Se questi requisiti non sono soddisfatti, viene creato un duplicato.

- **Sovrascrivi nella cartella corrente, nome della stessa risorsa base, indipendentemente dall’estensione**.
Richiede che l’immagine sostitutiva venga caricata nella stessa cartella dell’originale, ma l’estensione del nome file può essere diversa dall’originale. Ad esempio, sedia.tif sostituisce sedia.jpg.

- **Sovrascrivi in qualsiasi cartella, nome/estensione**della stessa risorsa base.
Richiede che l’immagine sostitutiva abbia la stessa estensione dell’immagine originale (ad esempio, sedia.jpg deve sostituire sedia.jpg, non sedia.tif). Tuttavia, potete caricare l’immagine sostitutiva in una cartella diversa da quella dell’originale. L’immagine aggiornata si trova nella nuova cartella; il file non può più essere trovato nella posizione originale.

- **Sovrascrivi in qualsiasi cartella, nome della stessa risorsa base, indipendentemente dall’estensione**.
Questa opzione è la regola di sostituzione più inclusiva. Potete caricare un’immagine sostitutiva in una cartella diversa da quella dell’originale, caricare un file con un’estensione diversa e sostituire il file originale. Se il file originale si trova in un’altra cartella, l’immagine sostitutiva si trova nella nuova cartella in cui è stata caricata.

Ulteriori informazioni sull&#39;opzione [Sovrascrivi immagini](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Sebbene non sia necessario, durante il caricamento utilizzando uno dei due metodi indicati sopra, potete specificare le Opzioni processo per quel particolare caricamento — ad esempio, per pianificare un caricamento periodico, impostate le opzioni di ritaglio al momento del caricamento e molti altri ancora. Questi possono essere utili per alcuni flussi di lavoro, quindi vale la pena considerare se possono essere per i vostri.

Ulteriori informazioni sulle opzioni [](https://docs.adobe.com/content/help/it-IT/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options)processo.

Il caricamento è il primo passaggio necessario in qualsiasi flusso di lavoro perché Dynamic Media Classic non può funzionare con contenuti che non sono già presenti nel sistema. Dietro le quinte durante il caricamento, il sistema registra tutte le risorse caricate con il database centralizzato Dynamic Media Classic, assegna un ID e lo copia nell’archivio. Inoltre, il sistema converte i file immagine in un formato che consente il ridimensionamento dinamico e lo zoom e converte i file video in formato MP4 adatto al Web.

### Concetto: Ecco cosa accade alle immagini quando vengono caricate in Dynamic Media Classic

Quando caricate un’immagine di qualsiasi tipo in Dynamic Media Classic, questa viene convertita in un formato immagine principale denominato TIFF piramidale o P-TIFF. Un P-TIFF è simile al formato di un&#39;immagine bitmap TIFF con più livelli, con la differenza che invece di diversi livelli, il file contiene più dimensioni (risoluzioni) della stessa immagine.

![immagine](assets/main-workflow/pyramid-p-tiff.png)

Quando l’immagine viene convertita, Dynamic Media Classic acquisisce un’istantanea delle dimensioni intere dell’immagine, la ridimensiona per metà e la salva, la ridimensiona per metà e la salva di nuovo, e così via finché non viene riempita con anche multipli delle dimensioni originali. Ad esempio, un file P-TIFF da 2000 pixel avrà dimensioni (e dimensioni inferiori) di 1000, 500, 250 e 125 pixel nello stesso file. Il file P-TIFF è il formato di quella che in Dynamic Media Classic viene chiamata &quot;immagine principale&quot;.

Quando si richiede un&#39;immagine di determinate dimensioni, la creazione del P-TIFF consente al server immagini per Dynamic Media Classic di trovare rapidamente le dimensioni maggiori successive e di ridurle. Ad esempio, se caricate un’immagine da 2000 pixel e richiedete un’immagine da 100 pixel, Dynamic Media Classic troverà la versione da 125 pixel e la ridurrà a 100 pixel invece di ridimensionarla da 2000 a 100 pixel. Questo rende l&#39;operazione molto veloce. Inoltre, quando si esegue lo zoom su un’immagine, il visualizzatore zoom può solo richiedere una sezione dell’immagine con zoom, anziché l’intera immagine a risoluzione. Questo è il modo in cui il formato dell&#39;immagine principale, il file P-TIFF, supporta sia il ridimensionamento dinamico che lo zoom.

Allo stesso modo, potete caricare il video sorgente principale in Dynamic Media Classic; al momento del caricamento, Dynamic Media Classic può ridimensionarlo automaticamente e convertirlo in formato MP4 adatto al Web.

### Regole del Miniature per determinare le dimensioni ottimali per le immagini caricate

**Caricate le immagini delle dimensioni maggiori desiderate.**

- Per eseguire lo zoom, caricate un’immagine ad alta risoluzione di un intervallo di 1500-2500 pixel per il lato più lungo. Considerate quanti dettagli desiderate fornire, la qualità delle immagini sorgente e la dimensione del prodotto mostrato. Ad esempio, caricate un’immagine da 1000 pixel per un piccolo anello, ma un’immagine da 3000 pixel per un’intera scena delle stanze.
- Se non è necessario eseguire lo zoom, caricatelo con le stesse dimensioni che verranno visualizzate. Ad esempio, se disponete di logo o immagini iniziali/banner da inserire sulle pagine, caricateli esattamente con le loro dimensioni 1:1 e chiamateli esattamente con quelle stesse dimensioni.

**Non campionare mai o ingrandire le immagini prima di caricare in Dynamic Media Classic.** Ad esempio, non campionare un’immagine più piccola per ottenere un’immagine da 2000 pixel. Non avrà un buon aspetto. Rendi le tue immagini il più possibile perfette prima del caricamento.

**Lo zoom non ha dimensioni minime, ma per impostazione predefinita gli utenti non possono ingrandire oltre il 100%.** Se l’immagine è troppo piccola, non viene applicato alcun ingrandimento o viene applicato solo un piccolo zoom per evitare che venga visualizzata in modo negativo.

**Sebbene non vi siano requisiti minimi per le dimensioni delle immagini, non è consigliabile caricare immagini di grandi dimensioni.** Un’immagine gigante può essere considerata di oltre 4000 pixel. Il caricamento di immagini di queste dimensioni può mostrare potenziali difetti come granelli di polvere o peli nell’immagine. Tali immagini occuperanno anche più spazio sul server Dynamic Media Classic, il che può causare il superamento dei limiti di archiviazione contratti.

Ulteriori informazioni sul [caricamento dei file](https://docs.adobe.com/content/help/it-IT/dynamic-media-classic/using/upload-publish/uploading-files.html).

## Passaggio 2: Autore (e Pubblica)

Dopo aver creato e caricato i contenuti, potete creare nuove risorse per contenuti multimediali avanzati dalle risorse caricate eseguendo uno o più flussi di lavoro secondari. Questo include tutti i diversi tipi di raccolte di set — Set di immagini, campioni, set 360 gradi e file multimediali diversi, nonché modelli. Include anche video. Verranno forniti maggiori dettagli su ciascun tipo di set di raccolte di immagini e di contenuti multimediali in un secondo momento. Tuttavia, in quasi tutti i casi, potete iniziare selezionando una o più risorse (o non dispongono di risorse selezionate) e scegliendo il tipo di risorsa da creare. Ad esempio, potete selezionare un’immagine principale e alcune viste dell’immagine e scegliere di creare un set di immagini, una raccolta di viste alternative dello stesso prodotto.

>[!IMPORTANT]
>
>Accertatevi che tutte le risorse siano contrassegnate per la pubblicazione. Per impostazione predefinita, tutte le risorse vengono contrassegnate automaticamente per la pubblicazione al momento del caricamento. Tuttavia, tutte le risorse appena create dal contenuto caricato dovranno essere contrassegnate anche per la pubblicazione.

Dopo aver creato la nuova risorsa, eseguirete un processo di pubblicazione. Potete eseguire questa operazione manualmente o pianificare un processo di pubblicazione eseguito automaticamente. La pubblicazione consente di copiare tutto il contenuto dalla sfera privata di Dynamic Media Classic alla sfera pubblica del server di pubblicazione dell’equazione. Il prodotto di un processo di pubblicazione per file multimediali dinamici è un URL univoco per ciascuna risorsa pubblicata.

Il server su cui si pubblica dipende dal tipo di contenuto e dal flusso di lavoro. Ad esempio, tutte le immagini vanno al server immagini e i video in streaming al server FMS. Per comodità, parleremo di un evento &quot;publish&quot; come singolo evento per un singolo server.

La pubblicazione pubblica tutto il contenuto contrassegnato per la pubblicazione — non solo il contenuto. In genere, un singolo amministratore pubblica per conto di tutti gli utenti anziché di singoli utenti che eseguono una pubblicazione. L’amministratore può pubblicare come necessario oppure impostare un processo periodico giornaliero, settimanale o anche ogni 10 minuti che verrà pubblicato automaticamente. Pubblicate i contenuti in base a una pianificazione utile per la vostra attività.

>[!TIP]
>
>Automatizzate i processi di pubblicazione e pianificate una pubblicazione completa da eseguire ogni giorno alle 12:00 o in qualsiasi momento in ritardo alla sera.

### Concetto: Informazioni sull’URL di Dynamic Media Classic

Il prodotto finale di un flusso di lavoro Dynamic Media Classic è un URL che indirizza alla risorsa (set di immagini o set video adattivo). Questi URL sono molto prevedibili e seguono lo stesso pattern. Nel caso delle immagini, ogni immagine viene generata dall’immagine principale P-TIFF.

Di seguito è riportata la sintassi per l’URL di un’immagine con due esempi:

![immagine](assets/main-workflow/dmc-url.jpg)

Nell’URL, tutto ciò che si trova a sinistra del punto interrogativo è il percorso virtuale di un’immagine specifica. Tutto a destra del punto interrogativo è un modificatore Image Server, un’istruzione per l’elaborazione dell’immagine. Se sono presenti più modificatori, questi sono separati da e-mail.

Nel primo esempio, il percorso virtuale dell&#39;immagine &quot;Backpack_A&quot; è `http://sample.scene7.com/is/image/s7train/BackpackA`. I modificatori Image Server ridimensionano l’immagine a una larghezza di 250 pixel (da wid=250) e ricampionano l’immagine utilizzando l’algoritmo di interpolazione Lanczos, che viene reso più nitida quando viene ridimensionata (da resMode=sharp2).

Il secondo esempio applica alla stessa immagine Backpack_A quella nota come &quot;predefinito per immagini&quot;, come indicato da $!_template300$. I simboli $ su entrambi i lati dell’espressione indicano che all’immagine viene applicato un predefinito per immagini, un set di modificatori di immagini inclusi nel pacchetto.

Una volta capito come vengono combinati gli URL di Dynamic Media Classic, puoi capire come modificarli a livello di programmazione e come integrarli più in profondità nel sito e nei sistemi di back-end.

### Concetto: Informazioni sul ritardo nella memorizzazione nella cache

Le risorse appena caricate e pubblicate verranno visualizzate immediatamente, mentre le risorse aggiornate potrebbero subire un ritardo di 10 ore nella memorizzazione nella cache. Per impostazione predefinita, tutte le risorse pubblicate hanno almeno 10 ore di scadenza. Diciamo minimo, perché ogni volta che l&#39;immagine viene visualizzata, inizia un orologio che non scade finché non sono trascorse 10 ore in cui nessuno ha visualizzato l&#39;immagine. Questo periodo di 10 ore corrisponde al &quot;Tempo di vita&quot; per una risorsa. Una volta scaduta la cache per la risorsa, è possibile distribuire la versione aggiornata.

In genere si tratta di un problema solo se si è verificato un errore e l’immagine/risorsa ha lo stesso nome della versione pubblicata in precedenza, ma si è verificato un problema con l’immagine. Ad esempio, avete accidentalmente caricato una versione a bassa risoluzione o il direttore artistico non ha approvato l’immagine. In questo caso, richiamate l’immagine originale e sostituitela con una nuova versione con lo stesso ID risorsa.

Scoprite come cancellare [manualmente la cache per gli URL da aggiornare](https://docs.adobe.com/content/help/en/experience-manager-65/assets/dynamic/invalidate-cdn-cached-content.html).

>[!TIP]
>
>Per evitare problemi con il ritardo nella cache, lavorate sempre in anticipo — una sera, un giorno, due settimane, ecc. Crea in tempo per il QA/l&#39;accettazione da parte dei soggetti interni per provare il tuo lavoro prima di pubblicarlo. Anche lavorando una sera prima è possibile apportare modifiche e ripubblicare quella sera. Al mattino, sono trascorse 10 ore e la cache si aggiorna con l&#39;immagine corretta.

- Ulteriori informazioni sulla [creazione di un processo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job)di pubblicazione.
- Ulteriori informazioni sulla [pubblicazione](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Passaggio 3: Consegna

Il prodotto finale di un flusso di lavoro Dynamic Media Classic è un URL che fa riferimento alla risorsa. L’URL può puntare a una singola immagine, a un set di immagini, a un set 360 gradi o a un altro set di immagini o video. È necessario utilizzare tale URL e fare qualcosa con esso, ad esempio modificare il codice HTML in modo che i `<IMG>` tag puntino all&#39;immagine Dynamic Media Classic invece di puntare a un&#39;immagine proveniente dal sito corrente.

Nel passaggio Consegna, devi integrare tali URL nel tuo sito Web, nell’app mobile, nella campagna e-mail o in qualsiasi altro punto di contatto digitale in cui vuoi visualizzare la risorsa.

Esempio di integrazione dell’URL Dynamic Media Classic per un’immagine in un sito Web:

![immagine](assets/main-workflow/example-url-1.png)

L’URL in rosso è l’unico elemento specifico di Dynamic Media Classic.

Il team IT o il partner di integrazione possono essere leader nella scrittura e modifica del codice per integrare gli URL Dynamic Media Classic nel sito.  Adobe dispone di un team di consulenza in grado di fornire assistenza a tale scopo, sia attraverso la fornitura di assistenza tecnica, creativa o generale.

Per soluzioni più complesse come i visualizzatori zoom, o per visualizzatori che combinano lo zoom con viste alternative, in genere l’URL punta a un visualizzatore ospitato da Dynamic Media Classic, e anche all’interno di tale URL è un riferimento a un ID risorsa.

Esempio di collegamento (in rosso) che aprirà un set di immagini in un visualizzatore in una nuova finestra a comparsa:

![immagine](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Devi integrare gli URL Dynamic Media Classic nel tuo sito Web, nell’app mobile, nell’e-mail e in altri punti di contatto digitali. Dynamic Media Classic non può farlo!

## Anteprima delle risorse

Probabilmente desiderate visualizzare l’anteprima delle risorse caricate o che state creando o modificando per essere certi che vengano visualizzate come desiderate dai clienti. Per accedere alla finestra Anteprima, fate clic sul pulsante **Anteprima** , sulla miniatura della risorsa, nella parte superiore del pannello **** Sfoglia/Genera, oppure scegliete **File > Anteprima**. Nella finestra di un browser, viene visualizzata un’anteprima della risorsa attualmente presente nel pannello, che si tratti di un’immagine, di un video o di una risorsa creata come un set di immagini.

### Anteprima dimensione dinamica (predefiniti per immagini)

Potete visualizzare l’anteprima delle immagini in più dimensioni mediante l’anteprima **Dimensioni** . Viene caricato un elenco dei predefiniti per immagini disponibili. Discuteremo dei predefiniti per immagini più tardi, ma considereremo questi predefiniti come &quot;ricette&quot; per caricare l’immagine in una dimensione specificata con una quantità specifica di nitidezza e qualità dell’immagine.

### Zoom Preview

Potete anche utilizzare l’opzione **Zoom** per visualizzare l’anteprima dell’immagine in uno dei numerosi predefiniti di zoom preimpostati, basati su diversi visualizzatori zoom inclusi.

Ulteriori informazioni sull’ [anteprima delle risorse](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/previewing-asset.html).
