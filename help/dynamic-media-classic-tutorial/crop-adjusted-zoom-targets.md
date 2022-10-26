---
title: Ritaglio, immagini regolate e destinazioni di zoom
description: L’immagine principale di Dynamic Media Classic supporta la creazione di versioni ritagliate separate di ogni immagine per mostrare i dettagli o per i campioni senza dover creare versioni ritagliate separate di ogni immagine. Scopri come ritagliare le immagini in Dynamic Media Classic e salvarle come nuovo file master o come immagine virtuale, salvare le immagini modificate virtuali e utilizzarle al posto delle risorse master e creare destinazioni zoom sulle immagini per mostrare i dettagli evidenziati.
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: a1d83c77-a9e4-4ed1-9b00-65fb002164c0
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2653'
ht-degree: 0%

---

# Ritaglio, immagini regolate e destinazioni di zoom {#crop-adjusted-zoom-targets}

Uno dei principali punti di forza del concetto di immagine principale di Dynamic Media Classic è che è possibile riadattare la risorsa immagine per molti utilizzi. In genere, è necessario creare versioni separate e ritagliate di ogni immagine per mostrare i dettagli o i campioni. Quando si utilizza Dynamic Media Classic, è possibile eseguire le stesse attività sul singolo master e salvare le versioni ritagliate come nuovi file fisici o come derivati virtuali che non occupano spazio di archiviazione.

Alla fine di questa sezione dell’esercitazione saprai come:

- Ritagliare le immagini in Dynamic Media Classic e salvarle come nuovi file master o come immagini virtuali. [Per saperne di più](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html).
- Salvare le immagini modificate virtuali e utilizzarle al posto delle risorse principali. [Per saperne di più](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/adjusting-image.html).
- Crea destinazioni di zoom sulle immagini per mostrarne le caratteristiche di rilievo. [Per saperne di più](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html).

## Ritaglio

Dynamic Media Classic dispone di alcuni strumenti di modifica delle immagini disponibili nell’interfaccia utente, tra cui lo strumento Ritaglio . Potrebbe essere utile ritagliare l’immagine principale in Dynamic Media Classic per una serie di motivi. Esempio:

- Non hai accesso al file originale. Si desidera visualizzare l&#39;immagine con un ritaglio o proporzioni diversi, ma non si dispone del file originale sul computer o si sta lavorando da casa. In questo caso è possibile accedere a Dynamic Media Classic, trovare l’immagine, ritagliarla e salvarla o salvarla come nuova versione.
- Per rimuovere lo spazio bianco in eccesso. L&#39;immagine è stata fotografata con troppo spazio bianco, il che rende il prodotto piccolo. Desideri che le immagini in miniatura riempiscano il quadro il più possibile.
- Per creare immagini regolate, copie virtuali di immagini che non occupano spazio su disco. Alcune aziende hanno regole di business che impongono loro di conservare copie separate della stessa immagine, ma con un nome diverso. O magari volete una versione ritagliata e non ritagliata della stessa immagine.
- Per creare nuove immagini da un&#39;immagine sorgente Ad esempio, potete creare campioni di colore o un dettaglio dell’immagine principale. Puoi farlo in Adobe Photoshop e caricarlo separatamente o usare lo strumento Ritaglio in Dynamic Media Classic.

>[!NOTE]
>
>Tutti gli URL nelle seguenti discussioni sul ritaglio sono a scopo puramente illustrativo; non sono collegamenti in diretta.

### Utilizzo dello strumento Ritaglio

Per accedere allo strumento Ritaglio in Dynamic Media Classic dalla pagina Dettagli di una risorsa o facendo clic sul pulsante **Modifica** pulsante . Potete usare lo strumento per ritagliare in due modi:

- Modalità di ritaglio predefinita in cui trascinare le maniglie della finestra di ritaglio o digitare i valori nella casella Dimensioni. Scopri come [Ritaglio manuale](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Rifila. Utilizza questo per rimuovere spazi bianchi aggiuntivi intorno all&#39;immagine calcolando il numero di pixel che non corrispondono all&#39;immagine. Scopri come [Ritaglio per taglio](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image).

### _Ritaglio manuale_

Quando salvi una versione ritagliata manualmente, l&#39;immagine viene ritagliata in modo permanente; Dynamic Media Classic sta effettivamente nascondendo i pixel aggiungendo un modificatore URL interno per ritagliare l’immagine. Quando pubblichi, sembrerà a tutti che l&#39;immagine sia ritagliata, tuttavia puoi tornare all&#39;Editor ritaglio e rimuovere il ritaglio in un momento successivo.

È quindi possibile scegliere se salvare come nuova immagine principale o come visualizzazione aggiuntiva del master. Un nuovo master è un nuovo file fisico (come TIFF o JPEG) che occupa spazio di archiviazione. Un&#39;altra visualizzazione è un&#39;immagine virtuale che non occupa spazio sul server. Non è consigliabile scegliere Sostituisci originale, in quanto questo sovrascrive il master e rende il ritaglio permanente. Se salvi come nuovo principale o come visualizzazione aggiuntiva, devi scegliere un nuovo ID risorsa. Come altri ID risorsa, deve essere un nome univoco in Dynamic Media Classic.

### _Ritaglio_

Se carichi un’immagine con troppi spazi bianchi (area di lavoro extra) intorno al soggetto principale dell’immagine, questa risulterà molto più piccola sul web quando viene ridimensionata. Questo è particolarmente vero per le immagini in miniatura che sono 150 pixel o più piccoli — il soggetto della foto può andare perso in tutto lo spazio extra intorno ad esso.

Confronta queste due versioni della stessa immagine.

![immagine](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

L&#39;immagine a destra viene resa molto più evidente rimuovendo lo spazio extra intorno al prodotto. Il taglio può essere eseguito un&#39;immagine alla volta, utilizzando lo strumento Ritaglio o come processo batch durante il caricamento. Si consiglia di eseguire come processo batch se si desidera che tutte le immagini vengano ritagliate in modo coerente ai limiti dell&#39;oggetto principale. Ritaglia le colture fino al rettangolo che circonda l&#39;immagine.

>[!NOTE]
>
>L&#39;opzione Trim non crea trasparenza intorno all&#39;immagine. A questo scopo, è necessario incorporare un tracciato di ritaglio nell’immagine e utilizzare il **Crea maschera dal percorso della clip** opzione di caricamento.
>
>Inoltre, per ripristinare lo stato originale di un&#39;immagine dopo averlo ritagliato quando hai utilizzato il **Salva** visualizzare l’immagine nella schermata Editor ritaglio e selezionare l’opzione **Reimposta** pulsante .

### _Ritaglio durante il caricamento_

Come accennato in precedenza, puoi anche scegliere di ritagliare le immagini durante il caricamento. Per utilizzare il ritaglio al momento del caricamento, fai clic sul pulsante **Opzioni processo** e in Opzioni ritaglio scegliere **Rifila**.

Dynamic Media Classic ricorderà questa opzione per il prossimo caricamento. Mentre potreste voler ritagliare le immagini per questo caricamento, potreste non volere ritagliarle per ogni caricamento. Un&#39;altra opzione consiste nell&#39;impostare un lavoro speciale pianificato di caricamento FTP e inserire le opzioni di ritaglio lì. In questo modo, esegui il lavoro solo quando era necessario ritagliare le immagini.

>[!IMPORTANT]
>
>Se imposti un ritaglio per il caricamento, Dynamic Media Classic inserirà un cookie per ricordare tale impostazione per la prossima volta. Come best practice, fai clic sul pulsante **Ripristina impostazioni predefinite società** prima del caricamento successivo, per cancellare tutte le opzioni di ritaglio rimaste dall’ultimo caricamento; in caso contrario, è possibile ritagliare accidentalmente il successivo batch di immagini.

### Ritaglio per URL

Sebbene non sia ovvio in Dynamic Media Classic, è anche possibile ritagliare tramite l’URL (o persino aggiungere ritaglio a un predefinito per immagini).

Ogni volta che utilizzi lo strumento Ritaglio, vedrai i valori URL nel campo in basso. Puoi prendere questi valori e applicarli direttamente a un&#39;immagine come modificatori URL.

![immagine](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_Modificatori del comando Ritaglia nella parte inferiore dell’Editor ritaglio_

![immagine](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Poiché la dimensione deve essere calcolata in base alle immagini quando si utilizza il ritaglio mediante il ritaglio, non può essere automatizzata tramite l’URL. Il ritaglio può essere eseguito solo al momento del caricamento o applicando un&#39;immagine alla volta.

### _Ritaglio nel predefinito per immagini_

I predefiniti per immagini dispongono di un campo in cui è possibile aggiungere ulteriori comandi Image Serving. Per aggiungere lo stesso ritaglio di cui sopra al predefinito immagine, modifica il predefinito e incolla o digita i valori nel campo Modificatori URL , quindi salva e pubblica.

![immagine](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_Aggiungi i comandi di ritaglio (o qualsiasi comando) ai modificatori URL del predefinito immagine._

Il ritaglio fa ora parte di quel predefinito per immagini e viene applicato automaticamente ogni volta che viene utilizzato. Naturalmente, questo metodo dipende da tutte le immagini che necessitano della stessa quantità di ritaglio. Se le tue immagini non vengono tutte riprese nello stesso modo, questo metodo non funzionerebbe per te.

## Immagini regolate

Quando si utilizza lo strumento Ritaglio, è possibile **Salva come visualizzazione aggiuntiva del master**. Una volta salvato, viene creato un nuovo tipo di risorsa Dynamic Media Classic, un’immagine regolata. Un&#39;immagine regolata, detta anche derivata, è un&#39;immagine virtuale. Non è affatto un&#39;immagine; è un riferimento al database (come un alias o un collegamento) all&#39;immagine master fisica.

### L&#39;immagine vera si può alzare?`?`

Si può sapere qual è il master e quale è l&#39;immagine regolata?

![immagine](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

Non dovresti essere in grado di dirlo senza guardare in Dynamic Media Classic e vedere il tipo di risorsa &quot;Immagine regolata&quot; per SBR_MAIN2.

Un&#39;immagine regolata non utilizza spazio su disco, in quanto esiste solo come elemento di riga nel database. È inoltre legato in modo permanente all’attività originaria; se l&#39;originale viene eliminato, anche l&#39;immagine regolata viene eliminata. Può essere costituita da un&#39;intera immagine non ritagliata o da una sola parte di un&#39;immagine (un ritaglio).

![immagine](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Generalmente si creano immagini regolate con lo strumento Ritaglio; tuttavia possono essere create anche con altri editor di immagini — gli strumenti Regola e Nitidezza .

Le immagini regolate richiedono un ID risorsa univoco. Quando vengono pubblicati (devi pubblicarli come qualsiasi altra risorsa), questi fungono da qualsiasi altra immagine e sono chiamati su un URL in base al loro ID risorsa. Nella pagina Dettaglio è possibile visualizzare le immagini regolate associate a un’immagine master sotto la sezione **Costruiti e derivati** scheda .

![immagine](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_Visualizzazioni regolate per l&#39;immagine principale ASIAN_BR_MAIN_

## Destinazioni di zoom

Le destinazioni di zoom si trovano anche nella **Modifica** menu e **Dettagli** pagina di un’immagine. Consentono di impostare &quot;punti attivi&quot; per evidenziare specifiche caratteristiche di merchandising di un&#39;immagine zoom. Invece di creare immagini separate ritagliando un master di grandi dimensioni, il visualizzatore zoom può servire i dettagli sopra l’immagine, insieme a una breve etichetta creata.

![immagine](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Poiché le destinazioni di zoom sono essenzialmente una funzione di merchandising e richiedono la conoscenza dei punti di vendita di un prodotto, in genere vengono create da una persona del team Merchandising o di prodotto di un&#39;azienda.

Il processo è molto semplice: fai clic sulla feature, assegnagli un nome descrittivo e salva. I target possono essere copiati da un&#39;immagine all&#39;altra se sono simili, tuttavia il processo è manuale. In Dynamic Media Classic non è possibile automatizzare la creazione di destinazioni di zoom, in quanto ogni immagine è diversa e presenta funzioni diverse.

Un altro fattore per decidere se utilizzare le destinazioni di zoom è la scelta del visualizzatore. Non tutti i tipi di visualizzatore possono visualizzare le destinazioni di zoom (ad esempio, il visualizzatore a comparsa non le supporta).

Scopri come [Creare destinazioni di zoom](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![immagine](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Utilizzo dello strumento Zoom Target

Ecco il flusso di lavoro per la creazione di destinazioni in Dynamic Media Classic.

1. Individua l&#39;immagine, fai clic sul pulsante **Modifica** e scegli **Destinazioni di zoom**.
2. Verrà caricato l’Editor di destinazione di zoom. Vedrete l&#39;immagine al centro, alcuni pulsanti nella parte superiore e un pannello di destinazione vuoto sulla destra. In basso a sinistra è selezionato un predefinito per visualizzatori. Il valore predefinito è &quot;Zoom1-Guided&quot;.
3. Sposta la casella rossa con il mouse e fai clic su per creare una nuova destinazione.

   - La casella rossa è l&#39;area di destinazione. Quando un utente fa clic su tale destinazione, esegue lo zoom nell&#39;area all&#39;interno della casella.
   - La dimensione della destinazione è determinata dalle dimensioni della visualizzazione all’interno del predefinito visualizzatore. Questo determina le dimensioni dell&#39;immagine di zoom principale. Vedi _Impostazione della dimensione della visualizzazione_ qui sotto.

4. Il target appena creato diventa blu e a destra viene visualizzata una versione in miniatura del target e il nome predefinito &quot;target-0&quot;.
5. Per rinominare la destinazione, fai clic sulla relativa miniatura, digita una nuova **Nome** e fai clic su **Invio** o **Scheda** — se cliccate via, il vostro nome non verrà salvato.
6. Quando la destinazione è selezionata, intorno alla casella sono presenti linee tratteggiate verdi che consentono di ridimensionarla e spostarla. Trascinare gli angoli per ridimensionare o trascinare la casella di destinazione per spostarla.

   - L’immagine verrà caricata nel visualizzatore di zoom personalizzato predefinito. Assicurati che il predefinito per visualizzatori supporti le destinazioni di zoom; in generale, tutti i predefiniti standard con la parola &quot;-Guided&quot; sono stati progettati per l’utilizzo con le destinazioni di zoom. Per utilizzare le destinazioni, passa il puntatore del mouse sulla miniatura di destinazione (o sull’icona del punto attivo) per visualizzare l’etichetta e fai clic su di essa per visualizzare lo zoom del visualizzatore in quella funzione.
   - Come per tutte le altre attività eseguite in Dynamic Media Classic, è necessario pubblicare le destinazioni di zoom per essere live sul web. Se utilizzi già un visualizzatore che supporta i target, questi verranno visualizzati immediatamente (dopo che la cache è stata cancellata). Tuttavia, se non utilizzi un visualizzatore abilitato per lo zoom, questi rimarrà nascosto.

      ![immagine](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Inoltre, se devi rimuovere una destinazione, selezionala facendo clic sulla relativa miniatura e premendo il pulsante **Elimina Target** o premere il tasto DELETE sulla tastiera.
8. Continua a fare clic per aggiungere nuovi target, rinominarli e/o ridimensionarli dopo l’aggiunta.
9. Al termine, fai clic sul pulsante **Salva** e quindi **Anteprima**.

### Impostazione delle dimensioni di visualizzazione nel predefinito per visualizzatori zoom

Parliamo un attimo di da dove provengono le dimensioni delle destinazioni di zoom. All’interno del predefinito per visualizzatori zoom è presente un’impostazione denominata dimensione visualizzazione. Le dimensioni della visualizzazione corrispondono alle dimensioni dell&#39;immagine dello zoom all&#39;interno del visualizzatore. È diverso dalle dimensioni dell’area di visualizzazione, che rappresentano le dimensioni totali del visualizzatore, inclusi i componenti e le immagini dell’interfaccia utente.

Quando crei una nuova destinazione, ne deriva le dimensioni e le proporzioni dalle dimensioni della visualizzazione. Ad esempio, se le dimensioni della visualizzazione sono 200 x 200, sarà possibile effettuare solo destinazioni quadrate, con un&#39;area di zoom massima di 200 pixel. Le destinazioni possono essere più grandi di 200 pixel, ma sempre quadrate. Ma questo significa anche che l&#39;immagine all&#39;interno del visualizzatore zoom è di soli 200 pixel — la dimensione della destinazione dello zoom ha una relazione diretta con la dimensione del visualizzatore. Prima di impostare i target, decideresti il tuo design del visualizzatore.

Tuttavia, per impostazione predefinita la dimensione della visualizzazione è vuota (impostata su 0 x 0), perché la dimensione dell’immagine della vista principale è dinamica e viene derivata automaticamente in base alle dimensioni dell’area di visualizzazione. Il problema è che se non si imposta esplicitamente una dimensione di visualizzazione nel predefinito, lo strumento Zoom Target non sa che dimensione assegnare alle destinazioni.

Quando carichi lo strumento Zoom Target, accanto al nome del predefinito viene visualizzata la dimensione della visualizzazione. Confronta le dimensioni della visualizzazione tra il predefinito Zoom1-Guided integrato e il predefinito ZT_AUTHORING personalizzato.

![immagine](assets/crop-adjusted-zoom-targets/view-size.jpg)

Potete vedere che il predefinito integrato ha una dimensione di 900 x 550, il che significa che il target non può mai diventare più piccolo di quella dimensione piuttosto grande. Probabilmente è troppo grande — se avete un&#39;immagine di 2000 pixel, potete chiamare fuori solo una funzione che è un minimo di 900 pixel attraverso. L’utente può effettuare lo zoom manualmente, ma non è possibile avvicinarlo di più. Impostando una dimensione di visualizzazione a 350 x 350, le destinazioni possono ingrandirsi o ingrandirsi. Ma se desideri un&#39;immagine zoom più grande nel visualizzatore, devi creare un nuovo predefinito perché il tuo è bloccato a 350 pixel.

### Creazione o modifica di un predefinito per visualizzatori che supporta le destinazioni di zoom

Per impostare le dimensioni della visualizzazione, crea o modifica un predefinito per visualizzatori che supporta le destinazioni di zoom.

1. Nel predefinito per visualizzatori, passa alla pagina **Impostazioni zoom** opzione .
2. Impostare Larghezza e Altezza.
3. Salva il predefinito e chiudi. Se desideri utilizzare questo predefinito sul tuo sito live, in seguito dovrai anche pubblicare.
4. Passa allo strumento Zoom Target e scegli il predefinito modificato in basso a sinistra. La nuova dimensione della visualizzazione verrà immediatamente visualizzata nelle destinazioni.
