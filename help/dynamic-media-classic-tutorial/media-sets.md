---
title: Set di file multimediali diversi per immagini, campioni, set 360 gradi
description: Una delle capacità più utili e potenti di Dynamic Media Classic è il supporto per la creazione di set di file multimediali avanzati come Immagine, Campioni, Set 360 gradi e Set di file multimediali diversi. Scopri cos’è ciascun set di file multimediali avanzati e come creare ciascun tipo in Dynamic Media Classic. Per ulteriori informazioni sui predefiniti per set di batch, che automatizzano il processo di creazione di set di file multimediali avanzati al momento del caricamento.
sub-product: dynamic-media
feature: sets
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1456'
ht-degree: 1%

---


# Set di file multimediali diversi per immagini, campioni, set 360 gradi {#media-sets}

Oltre alle singole immagini per il ridimensionamento dinamico e lo zoom, le raccolte di set Dynamic Media Classic offrono un&#39;esperienza online più completa. In questa sezione dell’esercitazione verrà illustrato come creare i seguenti set di file multimediali avanzati in Dynamic Media Classic:

- Set immagini
- Set di campioni
- Set 360 gradi
- Set di file multimediali diversi

Inoltre, verrà illustrato come utilizzare i predefiniti per set di batch per automatizzare la creazione di set tramite un caricamento.

## Tutto Ciò Che Desideri Sapere Sui Set

Accanto al ridimensionamento dinamico di base e allo zoom, i set sono probabilmente il sottoprodotto Dynamic Media Classic più utilizzato. I set sono essenzialmente risorse &quot;virtuali&quot; che non contengono immagini effettive, ma consistono in un insieme di relazioni con altre immagini e/o video. L&#39;attrattiva principale dei set è che sono mini-applicazioni pronte &quot;fuori dalla mensola&quot;. Con questo si intende che ogni visualizzatore set contiene una propria logica e un’interfaccia in modo che tutto ciò che dovete fare è chiamarli sul sito. Inoltre, richiedono solo di tenere traccia di un singolo ID risorsa per set, anziché dover gestire direttamente tutte le risorse e le relazioni dei membri.

Quando create un set, questo viene gestito come una risorsa separata che deve essere contrassegnata per la pubblicazione e pubblicata prima che possa essere distribuito da un URL. È necessario pubblicare anche tutte le risorse dei membri.

### Tipi di set

Scopri i quattro tipi di set che puoi creare in Dynamic Media Classic: Set di immagini, campioni, set 360 gradi e file multimediali diversi.

## Set immagini

Questo è il tipo di set più comune. In genere viene utilizzato per le viste alternative dello stesso elemento. È costituita da più immagini caricate nel visualizzatore facendo clic sulla miniatura associata dell’immagine.

![immagine](assets/media-sets/image-set-1.jpg)

_Esempio di un set di immagini_

L’URL del set di immagini riportato sopra potrebbe essere:

![immagine](assets/media-sets/image-set-url-1.png)

- Per ulteriori informazioni sui set di immagini, consultate Avvio [rapido ai set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html)di immagini.
- Scoprite come [creare un set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set)di immagini.

### Set di campioni

Questo tipo di set viene in genere utilizzato per visualizzare viste colorate dello stesso elemento. È costituito da coppie di immagini e campioni di colore.

La differenza principale tra un set di campioni e un set di immagini consiste nel fatto che i set di campioni utilizzano un’immagine diversa come campione selezionabile, mentre per i set di immagini viene utilizzata una versione miniatura dell’immagine originale con cui è possibile fare clic.

I set di campioni non colorano le immagini (un concetto comune). Le immagini vengono semplicemente sostituite, esattamente come in un set di immagini. Le mini immagini campione potrebbero essere state create con Photoshop, ogni colore potrebbe essere stato fotografato separatamente, oppure lo strumento di ritaglio in Dynamic Media Classic avrebbe potuto essere utilizzato per creare un campione da una delle immagini a colori.

![immagine](assets/media-sets/image-set-2.jpg)

_Esempio di un set di campioni_

L’URL del set di campioni riportato sopra potrebbe essere:

![immagine](assets/media-sets/image-set_url.png)

- Per ulteriori informazioni sui set di campioni, consultate Avvio [rapido ai set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html)di campioni.
- Scoprite come [creare un set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set)di campioni.

### Set 360 gradi

Questo set viene in genere utilizzato per visualizzare un elemento a 360 gradi. Come per i set di campioni, i set 360 gradi non utilizzano la magia 3D. il vero lavoro è quello di creare molte foto di un&#39;immagine da tutti i lati. Il visualizzatore consente semplicemente di passare da un’immagine all’altra come un’animazione stop-motion.

I set 360 gradi possono essere ruotati in una direzione lungo un singolo asse oppure creati alternativamente come set 360 gradi 2D — girare su più assi. Ad esempio, un&#39;auto può essere ruotata mentre tutte le ruote sono a terra, e poi può essere &quot;capovolto&quot; e ruotato anche sulle sue ruote posteriori. Per un set 360 gradi 2D impostato correttamente, il numero di immagini per riga per ogni asse deve essere lo stesso. In altre parole, se state ruotando su due assi, è necessario il doppio delle immagini rispetto a un singolo angolo di rotazione.

![immagine](assets/media-sets/image-set-3.png)

_Esempio di un set 360 gradi_

L’URL del set 360 gradi riportato sopra potrebbe essere:

![immagine](assets/media-sets/spin-set.png)

- Per ulteriori informazioni sui set 360 gradi, consultate [Avvio rapido ai set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html)360 gradi.
- Scoprite come [creare un set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set)360 gradi.

## Set di file multimediali diversi

Questo è un insieme combinato. Consente di combinare i set precedenti e aggiungere video in un singolo visualizzatore. In questo flusso di lavoro, potete creare prima qualsiasi set di componenti e quindi assemblarli in un set di file multimediali diversi.

![immagine](assets/media-sets/image-set-4.png)

_Esempio di set di file multimediali diversi_

L’URL del set di file multimediali diversi riportato sopra potrebbe essere:

![immagine](assets/media-sets/image-set-url-1.png)

- Per ulteriori informazioni sui set di file multimediali diversi, consultate [Avvio rapido ai set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html)di file multimediali diversi.

- Scoprite come [creare un set](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)di file multimediali diversi.

Per visualizzare un’immagine per lo zoom, un set o un video sul sito Web, occorre chiamarla in un &quot;visualizzatore&quot; Dynamic Media Classic. Dynamic Media Classic include visualizzatori per risorse multimediali avanzate quali set di campioni, set 360 gradi, video e molti altri.

Ulteriori informazioni sui [visualizzatori per  AEM Assets e Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html).

## Predefiniti set di batch

Finora abbiamo discusso su come creare manualmente i set utilizzando la funzione Dynamic Media Classic Build. Tuttavia, è possibile automatizzare la creazione di set di immagini e di set 360 gradi utilizzando un predefinito per set di batch, purché sia presente una convenzione di denominazione standard.

Ogni predefinito ha un nome univoco e contiene un set autonomo di istruzioni che definisce come creare il set utilizzando immagini che corrispondono alle convenzioni di denominazione definite. Nel predefinito, definite innanzitutto le convenzioni di denominazione per le risorse da raggruppare in un set. Potete quindi creare un predefinito per set di batch che faccia riferimento a tali immagini.

Sebbene sia possibile creare il predefinito autonomamente (in **Configurazione > Impostazione applicazione > Predefiniti** set di batch), si consiglia di impostare il team di consulenza o il supporto tecnico. Ecco perché:

- I predefiniti per set di batch possono essere complessi per la configurazione — sono alimentati da espressioni regolari e, a meno che non si sia uno sviluppatore, questa sintassi potrebbe essere sconosciuta o confusa.
- Una volta creati, vengono attivati per impostazione predefinita. Nessuna funzione di annullamento. Se iniziate a caricare migliaia di immagini e il predefinito non è configurato correttamente, potete finire con centinaia o migliaia di set interrotti da trovare ed eliminare manualmente.

In precedenza era stata suggerita una semplice convenzione di denominazione che sarebbe stato molto semplice da incorporare in un predefinito per set di batch. Tuttavia, poiché i predefiniti sono molto flessibili, possono gestire strategie di denominazione complesse. In breve, le immagini che appartengono a un set devono essere legate insieme da un nome comune — spesso si tratta del numero SKU o dell&#39;ID prodotto. In Dynamic Media Classic potete assegnarvi una convenzione di denominazione predefinita per tutte le immagini da usare per un predefinito, oppure potete creare più predefiniti, ciascuno con regole di denominazione diverse.

I predefiniti per set di batch vengono applicati solo al momento del caricamento; non possono essere eseguite dopo il caricamento delle immagini. È quindi importante pianificare la convenzione di denominazione e creare un predefinito prima di iniziare a caricare tutte le immagini.

Una volta creati i predefiniti, l’amministratore società può scegliere se sono attivi o inattivi. Attivo significa che verranno visualizzati nella pagina di caricamento in Opzioni **** processo, mentre i predefiniti inattivi rimarranno nascosti.

Scoprite come [creare un predefinito](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)per set di batch.

### Utilizzo dei predefiniti per set di batch durante il caricamento

Di seguito è illustrato l’utilizzo dei predefiniti per set di batch durante il caricamento dopo la creazione:

1. Fate clic su **Carica** e scegliete **Da desktop** o **Mediante FTP**.
2. Fate clic su Opzioni **** processo.
3. Aprite l’opzione Predefiniti **per set di** batch e verificate o deselezionate il predefinito da usare con il caricamento.
4. Al termine del caricamento, cercate i set finiti nella cartella.

Ulteriori informazioni sui predefiniti per set di [batch](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
