---
title: Definizione della struttura delle cartelle e della convenzione di denominazione dei file
description: La denominazione dei file è forse la decisione più importante da adottare per l’implementazione di Dynamic Media Classic. Anche la struttura delle cartelle è importante. Scopri perché è così importante e possibile adottare approcci per la struttura delle cartelle e i nomi dei file.
sub-product: dynamic-media
feature: null
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1211'
ht-degree: 0%

---


# Definizione della struttura delle cartelle e della convenzione di denominazione dei file {#folder-structure-filenaming}

Prima di iniziare a caricare tutti i contenuti, è opportuno considerare la struttura di cartelle da utilizzare e in particolare la convenzione di denominazione dei file. In un secondo momento, sarà probabilmente possibile risparmiare tempo e ripristinare le attività. È meglio coordinare queste decisioni in tutti i gruppi.

## Gerarchia delle cartelle e convenzione di denominazione dei file

La denominazione dei file è in genere la decisione più importante da prendere in merito all’implementazione di Dynamic Media Classic. Tuttavia, per capire perché è importante, parliamo innanzitutto della struttura delle cartelle.

### Gerarchia cartelle

La gerarchia delle cartelle è importante per voi e per la vostra azienda solo a scopo organizzativo — gli URL Dynamic Media Classic fanno riferimento solo al nome della risorsa, non alla cartella o al percorso. Indipendentemente da dove carichi un file, l’URL sarà lo stesso. Questo è molto diverso da come la maggior parte delle persone organizza immagini e contenuti per il Web, ma con Dynamic Media Classic non fa differenza.

Un’altra considerazione importante riguarda il numero di risorse o cartelle da memorizzare in ciascuna cartella. Se molte risorse sono memorizzate in una cartella, le prestazioni peggioreranno quando si visualizzano le risorse in Dynamic Media Classic. Non archiviate migliaia di risorse in una cartella. Al contrario, sviluppate una gerarchia organizzativa con meno di 500 risorse o cartelle all’interno di un determinato ramo della gerarchia. Questo non è un requisito rigoroso, ma contribuirà a mantenere tempi di risposta accettabili durante la visualizzazione o la ricerca delle risorse. In effetti, la raccomandazione consiste nel creare gerarchie ampie e superficiali, anziché strette e profonde.

Il modo più semplice per creare le cartelle è caricare l’intera struttura di cartelle mediante FTP e attivare l’opzione **Includi sottocartelle**. Questa opzione fa sì che Dynamic Media Classic ricrei la struttura delle cartelle sul sito FTP in Dynamic Media Classic.

Prima di caricare tutti i file, è consigliabile tenere in considerazione la struttura delle cartelle, in quanto l’organizzazione e la gestione locale dei file e delle cartelle sul computer risulta molto più semplice rispetto all’interno di Dynamic Media Classic. Ad esempio, all’interno di Dynamic Media Classic è possibile trascinare e rilasciare solo i file, ma non intere cartelle.

### Strategie delle cartelle

Per la strategia delle cartelle, considera gli elementi rilevanti per l’organizzazione. Di seguito sono riportati alcuni scenari di denominazione delle cartelle comuni:

- Mirror sito Web o suddivisione del prodotto. Ad esempio, se vendete vestiti, potreste avere cartelle per Uomini, Donne, Accessori e sottocartelle per Camicie e Scarpe.
- Strategia basata su SKU o ID prodotto. Ad esempio, con i rivenditori che hanno migliaia di articoli, potrebbe essere utile utilizzare numeri SKU o ID prodotto come nomi di cartella.
- Strategia del marchio. Ad esempio, i produttori che dispongono di più marchi possono scegliere il proprio marchio come cartelle di livello principale.

## Convenzione di denominazione dei file

La scelta di assegnare un nome ai file è forse la decisione iniziale più importante per quanto riguarda Dynamic Media Classic. Questo perché tutte le risorse in Dynamic Media Classic devono avere nomi univoci, indipendentemente dalla posizione in cui sono memorizzate nell’account.

Tutti gli URL e le transazioni in Dynamic Media Classic sono guidati da un ID risorsa, che è l’identificatore univoco di una risorsa nel database. Quando caricate un file, l’ID risorsa viene creato prendendo il nome del file e rimuovendo l’estensione. Ad esempio, _896649.jpg_ ottiene l’ _ID risorsa 896649_.

Regole relative agli ID risorsa:

- All’interno di Dynamic Media Classic non è possibile assegnare lo stesso nome a due risorse, indipendentemente dalla cartella in cui si trovano.
- Per i nomi viene fatta distinzione tra maiuscole e minuscole. Ad esempio, sedia.jpg, sedia.jpg e CHAIR.jpg creerebbero tre ID risorsa diversi.
- Come procedura ottimale, gli ID risorsa non devono contenere spazi o simboli vuoti. L’uso di spazi e simboli rende più difficile l’implementazione perché sarà necessario codificare URL per questi caratteri. Ad esempio, uno spazio &quot; diventa &quot;%20&quot;.

La convenzione di denominazione è essenzialmente il modo in cui vi integrate con Dynamic Media Classic. In genere, i sistemi back-office non vengono integrati in Dynamic Media Classic perché si tratta di un sistema chiuso. È un partner passivo, in attesa di istruzioni sotto forma di URL.

La maggior parte degli utenti basa la propria convenzione di denominazione sugli SKU o gli ID prodotto interni, in modo che quando una pagina Web viene richiamata con informazioni su tale SKU, la pagina possa cercare automaticamente un’immagine con un nome simile. Se non esiste una connessione tra il nome del file e lo SKU o l&#39;ID, il sistema di back-office dovrà tenere traccia manualmente di ciascun nome file, e una persona dovrà mantenere tali associazioni — in breve, molto lavoro sia per l&#39;IT che per i team di contenuti.

### Strategie di denominazione dei file

La strategia di denominazione deve essere flessibile per l’espansione futura, in modo da evitare di dover rinominare una volta avviata la procedura. Di seguito sono riportate alcune tipiche strategie di denominazione:

**Nessuna immagine alternativa.** In questo scenario, disponete solo di un&#39;immagine per prodotto e non di viste alternative o colorate. Denominate in modo rigoroso ogni immagine in base al relativo numero SKU o ID prodotto univoco. Quando la pagina viene caricata, il modello di pagina richiama l’ID risorsa con lo stesso numero SKU.

| SKU/PID | Nome file | ID risorsa |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Questo è un sistema molto semplice, e buono se avete bisogni modesti. Tuttavia non è molto flessibile. Solo perché non hai immagini alternative oggi non significa che non avrai quelle immagini domani. Lo scenario successivo offre maggiore flessibilità.

**Usando l’immagine, viste alternative, versioni colorate, campioni.** Questa strategia consente visualizzazioni alternative e/o viste colorate, se disponibili. Invece di assegnare un nome all’immagine dopo solo lo SKU, potete aggiungere un modificatore come &quot;_1&quot; e &quot;_2&quot; per le viste alternative e un codice colore di &quot;_RED&quot; o &quot;_BLU&quot; per le viste a colori. Se disponete sia di immagini colorate che di viste alternative per lo stesso prodotto, potete aggiungere &quot;_RED_1&quot; e &quot;_RED_2&quot; per la prima e la seconda visualizzazione a colori rossi. I campioni vengono denominati con SKU, codice colore e un&#39;estensione &quot;_SW&quot;.

| SKU/PID | Categoria | Nome file | ID risorsa |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Visualizzazioni Alt | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Viste colorate | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Campioni | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Set di immagini o set di campioni |  | AA123 o AA123_SET | -- |

Quando si tratta di raccolte di set, ad esempio set di immagini e set di campioni, anche il set stesso deve avere un nome univoco. In questo caso, al set potrebbe essere assegnato lo SKU di base come nome, o lo SKU con estensione &quot;_SET&quot;.

### Convenzione di denominazione e automazione

Un&#39;ultima parola sull&#39;importanza di nominare la convenzione. Se desiderate usare dei set (ad esempio set di immagini o set di campioni), una convenzione di denominazione prevedibile vi consentirà di automatizzarne la creazione. Qualsiasi metodo con script, ad esempio un predefinito per set di batch, che verrà descritto in una sezione separata di questa esercitazione, può essere eliminato da una convenzione di denominazione.

Il metodo alternativo consiste nel creare manualmente i set. Mentre la creazione manuale di set di immagini per 200 immagini potrebbe non essere un grande lavoro, immaginate di disporre di più di 100.000 immagini. Questo è il momento in cui l&#39;automazione della creazione di set diventa cruciale.
