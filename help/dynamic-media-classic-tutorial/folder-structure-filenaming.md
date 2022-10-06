---
title: Determinare la struttura delle cartelle e la convenzione per la denominazione dei file
description: La denominazione dei file è forse la decisione più importante da prendere durante l’implementazione di Dynamic Media Classic. Anche la struttura delle cartelle è importante. Scopri perché è così importante e possibile adottare approcci per la struttura delle cartelle e i nomi dei file.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 0%

---

# Determinare la struttura delle cartelle e la convenzione per la denominazione dei file {#folder-structure-filenaming}

Prima di iniziare a caricare tutti i contenuti, è opportuno considerare la struttura delle cartelle da utilizzare e in particolare la convenzione per la denominazione dei file. In seguito sarà probabilmente possibile risparmiare tempo e dover ripristinare le attività. È meglio coordinare queste decisioni tra tutti i gruppi.

## Convenzione sulla gerarchia delle cartelle e la denominazione dei file

La denominazione dei file è generalmente la decisione più importante che si prende in merito all&#39;implementazione di Dynamic Media Classic. Tuttavia, per capire perché è importante, parliamo prima della struttura delle cartelle.

### Gerarchia cartelle

La gerarchia delle cartelle è importante solo per te e per la tua azienda a scopo organizzativo: gli URL Dynamic Media Classic fanno riferimento solo al nome della risorsa, non alla cartella o al percorso. Indipendentemente da dove carichi un file, l’URL è lo stesso. Questo è molto diverso da come la maggior parte delle persone organizzano le proprie immagini e contenuti per il web, ma con Dynamic Media Classic non fa differenza.

Un’altra considerazione importante è il numero di risorse o cartelle da archiviare in ciascuna cartella. Se molte risorse sono memorizzate in una cartella, le prestazioni peggiorano quando si visualizzano le risorse in Dynamic Media Classic. Non archiviare migliaia di risorse in una cartella. Al contrario, sviluppa una gerarchia organizzativa con meno di 500 risorse o cartelle all’interno di un determinato ramo della gerarchia. Questo non è un requisito rigoroso, ma aiuta a mantenere tempi di risposta accettabili durante la visualizzazione o la ricerca delle risorse. In effetti, il consiglio è quello di creare gerarchie larghe e poco profonde piuttosto che strette e profonde.

Il modo più semplice per creare le cartelle è caricare l&#39;intera struttura delle cartelle tramite FTP e abilitare l&#39;opzione **Includi sottocartelle**. Questa opzione fa sì che Dynamic Media Classic ricrei la struttura delle cartelle sul sito FTP in Dynamic Media Classic.

Prima di iniziare a caricare tutti i file, è consigliabile tenere conto della struttura delle cartelle, in quanto è molto più semplice organizzare e gestire i file e le cartelle localmente sul computer rispetto a Dynamic Media Classic. Ad esempio, puoi solo trascinare e rilasciare file, ma non intere cartelle, all’interno di Dynamic Media Classic.

### Strategie cartelle

Per la strategia delle cartelle, considera ciò che ha senso per la tua organizzazione. Di seguito sono riportati alcuni scenari comuni per la denominazione delle cartelle:

- Eseguire il mirroring del sito Web o della suddivisione del prodotto. Ad esempio, se vendi abbigliamento, potresti avere cartelle per Uomini, Donne e Accessori e sottocartelle per Camicie e Scarpe.
- Strategia basata su SKU o ID prodotto. Ad esempio, con i rivenditori che hanno migliaia di elementi, potrebbe essere utile utilizzare numeri SKU o ID prodotto come nomi di cartelle.
- Strategia del marchio. Ad esempio, i produttori che hanno più marchi possono scegliere il proprio marchio come cartelle di livello superiore.

## Convenzione sulla denominazione dei file

La scelta di assegnare un nome ai file è forse la decisione più importante in anticipo che si prenderà per quanto riguarda Dynamic Media Classic. Questo perché tutte le risorse in Dynamic Media Classic devono avere nomi univoci, indipendentemente da dove sono memorizzate nell’account.

Tutti gli URL e le transazioni in Dynamic Media Classic sono determinati da un ID risorsa, che è l’identificatore univoco della risorsa nel database. Quando carichi un file, l’ID risorsa viene creato prendendo il nome del file e rimuovendo l’estensione . Ad esempio: _896649.jpg_ ottiene la risorsa _ID 896649_.

Regole relative agli ID risorsa:

- In Dynamic Media Classic non è possibile assegnare lo stesso nome a due risorse, indipendentemente dalla cartella in cui si trovano.
- I nomi sono sensibili all’uso di maiuscole e minuscole. Ad esempio, Chair.jpg, chair.jpg e CHAIR.jpg creerebbero tre diversi ID risorsa.
- Come best practice, gli ID risorsa non devono contenere spazi o simboli vuoti. L’utilizzo di spazi e simboli rende l’implementazione più difficile perché dovrai codificare questi caratteri nell’URL. Ad esempio, uno spazio &quot; &quot; diventa &quot;%20&quot;.

La convenzione di denominazione è essenzialmente la modalità di integrazione con Dynamic Media Classic. In genere non si integrano i sistemi di back-office in Dynamic Media Classic perché si tratta di un sistema chiuso. È un partner passivo, in attesa di istruzioni sotto forma di URL.

La maggior parte degli utenti basa la propria convenzione di denominazione sul proprio SKU interno o ID di prodotto in modo che quando una pagina web viene richiamata con informazioni su tale SKU, la pagina possa cercare automaticamente un’immagine con un nome simile. Se non c&#39;è alcuna connessione tra il nome del file e lo SKU o l&#39;ID, allora il sistema di back-office dovrà tenere traccia manualmente di ciascun nome file, e una persona dovrà mantenere tali associazioni, in breve, un sacco di lavoro sia per il team IT che per il team di contenuti.

### Strategie di denominazione dei file

La strategia di denominazione deve essere flessibile per l’espansione futura, in modo da evitare di dover rinominare una volta avviato. Di seguito sono riportate alcune tipiche strategie di denominazione:

**Nessuna immagine alternativa.** In questo scenario, disponi solo di un’immagine per prodotto e non di viste alternative o colorate. Denomina rigorosamente ogni immagine in base al suo codice SKU o ID prodotto univoco. Quando la pagina viene caricata, il modello di pagina chiama l’ID risorsa con lo stesso numero SKU.

| SKU/PID | Nome file | ID risorsa |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Questo è un sistema molto semplice, e buono se hai bisogni modesti. Tuttavia non è molto flessibile. Solo perché non hai immagini alternative oggi non significa che non avrai quelle immagini domani. Lo scenario successivo offre maggiore flessibilità.

**Utilizzando l’immagine, le viste alternative, le versioni colorate, i campioni.** Questa strategia consente viste alternative e/o viste colorate, se disponibili. Invece di assegnare un nome all’immagine dopo solo lo SKU, aggiungi un modificatore come &quot;_1&quot; e &quot;_2&quot; per le viste alternative e un codice colore di &quot;_RED&quot; o &quot;_BLU&quot; per le viste colorate. Se per lo stesso prodotto sono disponibili immagini colorate e viste alternative, è possibile aggiungere &quot;_RED_1&quot; e &quot;_RED_2&quot; per la prima e la seconda vista a colori rossi. I campioni vengono denominati con SKU, codice colore e un&#39;estensione &quot;_SW&quot;.

| SKU/PID | Categoria | Nome file | ID risorsa |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Visualizzazioni Alt | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Viste colorate | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Campioni | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Set di immagini o set di campioni |  | AA123 o AA123_SET | — |

Quando si tratta di set di raccolte, ad esempio Set di immagini e Set di campioni, anche il set stesso deve avere un nome univoco. In questo caso, al set potrebbe essere assegnato il nome SKU di base, o la SKU con estensione &quot;_SET&quot;.

### Convenzione di denominazione e automazione

Un&#39;ultima parola sull&#39;importanza di nominare la convenzione. Se desideri utilizzare i set (ad esempio Set di immagini o Set di campioni), una convenzione di denominazione prevedibile consentirà di automatizzarne la creazione. Qualsiasi metodo con script, ad esempio un Batch Set Preset, di cui verrà illustrato l&#39;utilizzo in una sezione separata di questa esercitazione, può essere disattivato da una convenzione di denominazione.

Il metodo alternativo consiste nel creare manualmente i set. Mentre la creazione manuale di set di immagini per 200 immagini potrebbe non essere un grande lavoro, immaginate di avere più di 100.000 immagini. Questo è il momento in cui l&#39;automazione della creazione dei set diventa cruciale.
