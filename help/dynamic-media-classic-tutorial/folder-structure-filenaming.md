---
title: Determinare la struttura delle cartelle e la convenzione di denominazione dei file
description: La denominazione dei file è forse la decisione più importante che prenderai durante l’implementazione di Dynamic Media Classic. Anche la struttura delle cartelle è importante. Scopri perché è così importante e possibile adottare approcci per la struttura delle cartelle e i nomi dei file.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
duration: 275
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 0%

---

# Determinare la struttura delle cartelle e la convenzione di denominazione dei file {#folder-structure-filenaming}

Prima di iniziare a caricare tutto il contenuto, è consigliabile considerare la struttura di cartelle da utilizzare, in particolare la convenzione di denominazione dei file. In questo modo sarà possibile risparmiare tempo e ripristinare le attività in un secondo momento. È meglio coordinare queste decisioni tra tutti i gruppi.

## Gerarchia cartelle e convenzione per la denominazione dei file

La denominazione dei file è generalmente la decisione più importante che prendi per quanto riguarda l’implementazione di Dynamic Media Classic. Tuttavia, per capire perché è importante, parliamo prima della struttura delle cartelle.

### Gerarchia cartelle

La gerarchia delle cartelle è importante per te e per la tua azienda solo a scopo organizzativo: gli URL di Dynamic Media Classic fanno riferimento solo al nome della risorsa, non alla cartella o al percorso. Indipendentemente da dove carichi un file, l’URL è lo stesso. Questo è molto diverso dal modo in cui la maggior parte delle persone organizza le proprie immagini e i propri contenuti per il web, ma con Dynamic Media Classic non fa differenza.

Un’altra considerazione importante è il numero di risorse o cartelle da archiviare in ogni cartella. Se in una cartella sono memorizzate più risorse, le prestazioni diminuiscono quando si visualizzano le risorse in Dynamic Media Classic. Non memorizzare migliaia di risorse in una cartella. Piuttosto, sviluppa una gerarchia organizzativa con meno di 500 risorse o cartelle all’interno di un determinato ramo della gerarchia. Non si tratta di un requisito rigido, ma aiuta a mantenere tempi di risposta accettabili durante la visualizzazione o la ricerca delle risorse. In effetti, si consiglia di creare gerarchie ampie e superficiali, anziché strette e profonde.

Il modo più semplice per creare le cartelle consiste nel caricare l’intera struttura di cartelle tramite FTP e abilitare l’opzione **Includi sottocartelle**. Questa opzione consente a Dynamic Media Classic di ricreare la struttura di cartelle sul sito FTP in Dynamic Media Classic.

È consigliabile considerare la struttura delle cartelle prima di iniziare a caricare tutti i file, in quanto è molto più semplice organizzare e gestire i file e le cartelle localmente nel computer che in Dynamic Media Classic. Ad esempio, in Dynamic Media Classic è possibile trascinare solo file, ma non intere cartelle.

### Strategie cartella

Per la strategia delle cartelle, considera ciò che ha senso per la tua organizzazione. Di seguito sono riportati alcuni scenari comuni di denominazione delle cartelle:

- Sito web mirror o suddivisione del prodotto. Ad esempio, se hai venduto vestiti, potresti avere cartelle per Uomo, Donna e Accessori e sottocartelle per Camicie e Scarpe.
- Strategia basata su SKU o ID prodotto. Ad esempio, con i rivenditori che dispongono di migliaia di elementi, potrebbe essere utile utilizzare numeri SKU o ID prodotto come nomi delle cartelle.
- Strategia del marchio. Ad esempio, i produttori con più marchi possono scegliere i loro nomi come cartelle di livello principale.

## Convenzione di denominazione file

Come scegliere di denominare i file è forse la prima decisione più importante che si farà per quanto riguarda Dynamic Media Classic. Questo perché tutte le risorse in Dynamic Media Classic devono avere nomi univoci, indipendentemente da dove sono memorizzate nell’account.

Tutti gli URL e le transazioni in Dynamic Media Classic sono guidati da un ID risorsa, che è l’identificatore univoco di una risorsa nel database. Quando carichi un file, l’ID risorsa viene creato scegliendo il nome del file e rimuovendo l’estensione. Ad esempio: _896649.jpg_ ottiene risorsa _896649 ID_.

Regole relative agli ID risorsa:

- In Dynamic Media Classic non ci sono due risorse con lo stesso nome, indipendentemente dalla cartella in cui si trovano.
- Nei nomi viene fatta distinzione tra maiuscole e minuscole. Ad esempio, Chair.jpg, chair.jpg e CHAIR.jpg creerebbero tre ID di risorse diversi.
- Come best practice, gli ID risorsa non devono contenere spazi o simboli vuoti. L’utilizzo di spazi e simboli rende l’implementazione più difficile perché sarà necessario URL per codificare questi caratteri. Ad esempio, uno spazio &quot; &quot; diventa &quot;%20&quot;.

La convenzione di denominazione è essenzialmente il modo in cui si integra con Dynamic Media Classic. In genere, i sistemi di back-office non vengono integrati in Dynamic Media Classic perché si tratta di un sistema chiuso. È un partner passivo, in attesa di istruzioni sotto forma di URL.

La maggior parte degli utenti basa la convenzione di denominazione sul proprio SKU interno o ID prodotto, in modo che, quando viene richiamata una pagina web con informazioni su tale SKU, la pagina possa cercare automaticamente un’immagine con un nome simile. Se non è presente alcuna connessione tra il nome del file e lo SKU o l&#39;ID, il sistema di back office dovrà tenere traccia manualmente di ciascun nome di file e una persona dovrà mantenere tali associazioni, in breve, molto lavoro sia per il team IT che per il team dei contenuti.

### Strategie di denominazione dei file

La strategia di denominazione deve essere flessibile per espansioni future, in modo da evitare di dover rinominare dopo l’avvio. Di seguito sono riportate alcune strategie di denominazione tipiche:

**Nessuna immagine alternativa.** In questo scenario, disponi di una sola immagine per prodotto e non di visualizzazioni alternative o colorate. Assegna un nome a ogni immagine in base al suo SKU univoco o al numero ID prodotto. Quando la pagina viene caricata, il modello di pagina chiama l’ID risorsa con lo stesso numero SKU.

| SKU/PID | Nome file | ID risorsa |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Si tratta di un sistema molto semplice, utile se si hanno esigenze modeste. Tuttavia non è molto flessibile. Solo perché oggi non si dispone di immagini alternative non significa che non si avranno quelle immagini domani. Il prossimo scenario offre maggiore flessibilità.

**Utilizzando l’immagine, viste alternative, versioni colorate, campioni.** Questa strategia consente viste alternative e/o colorate, se disponibili. Invece di denominare l&#39;immagine solo dopo lo SKU, aggiungete un modificatore come &quot;_1&quot; e &quot;_2&quot; per le viste alternative e un codice colore di &quot;_RED&quot; o &quot;_BLU&quot; per le viste colorate. Se sono presenti sia immagini colorate che viste alternative per lo stesso prodotto, è possibile aggiungere &quot;_RED_1&quot; e &quot;_RED_2&quot; per la prima e la seconda vista di colore rosso. I campioni vengono denominati con lo SKU, il codice colore e l&#39;estensione &quot;_SW&quot;.

| SKU/PID | Categoria | Nome file | ID risorsa |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Visualizzazioni alternative | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|         | Visualizzazioni colorate | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_ROSSO AA123_MARRONE |
|         | Campioni | AA123_BLU_SW.tif | AA123_BLU_SW |
|         | Set immagini o set campioni |                                             | AA123 o AA123_SET | — |

Quando si tratta di insiemi di set di immagini, ad esempio Set di immagini e Set di campioni, anche il set deve avere un nome univoco. In questo caso, al set si può assegnare lo SKU di base come nome o lo SKU con un&#39;estensione &quot;_SET&quot;.

### Convenzione di denominazione e automazione

Un&#39;ultima parola sull&#39;importanza della convenzione di denominazione. Se desideri utilizzare dei set di immagini (ad esempio set di immagini o set di campioni), una convenzione di denominazione prevedibile ti consentirà di automatizzarne la creazione. Qualsiasi metodo basato su script, ad esempio un predefinito per set di batch, descritto in una sezione separata di questa esercitazione, può essere escluso da una convenzione di denominazione.

Il metodo alternativo consiste nel creare manualmente i set. Anche se la creazione manuale di set di immagini per 200 immagini potrebbe non risultare grandiosa, immaginate di avere più di 100.000 immagini. Ciò si verifica quando l’automazione della creazione del set diventa cruciale.
