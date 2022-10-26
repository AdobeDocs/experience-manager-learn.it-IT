---
title: Introduzione ai modelli di base
description: Scopri i Modelli di base in Dynamic Media Classic, i modelli basati su immagini chiamati dal server di immagini e costituiti da immagini e testo di cui è stato eseguito il rendering. Un modello può essere modificato dinamicamente tramite l’URL dopo la pubblicazione. Scoprirai come caricare un Photoshop PSD in Dynamic Media Classic per utilizzarlo come base di un modello. Crea un semplice modello di base per merchandising composto da livelli immagine. Aggiungete i livelli di testo e rendeteli variabili mediante l’uso di parametri. Crea un URL modello e manipola l’immagine in modo dinamico tramite il browser web.
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: d4e16b45-0095-44b4-8c16-89adc15e0cf9
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '6260'
ht-degree: 0%

---

# Introduzione ai modelli di base {#basic-templates}

In termini Dynamic Media Classic, un modello è un documento che può essere modificato dinamicamente tramite l’URL dopo la pubblicazione del modello. Dynamic Media Classic offre modelli di base, modelli basati su immagini richiamati dal server di immagini e costituiti da immagini e testo di cui è stato eseguito il rendering.

Uno degli aspetti più potenti dei modelli è che dispongono di punti di integrazione diretti che consentono di collegarli al database. Non solo è possibile elaborare un&#39;immagine e ridimensionarla, ma è anche possibile eseguire una query sul database per trovare elementi nuovi o in vendita e visualizzarli come sovrapposizioni sull&#39;immagine. È possibile richiedere una descrizione dell&#39;elemento e fare in modo che venga visualizzato come etichetta in un font scelto e layout. Le possibilità sono illimitate.

I modelli di base possono essere implementati in diversi modi, da semplici a complessi. Esempio:

- merchandising di base. Utilizza etichette come &quot;spedizione gratuita&quot; se il prodotto ha la spedizione gratuita. Queste etichette vengono configurate dal team di merchandise in Photoshop e il web utilizza la logica per sapere quando applicarle all&#39;immagine.
- Merchandising avanzato. Ogni modello ha più variabili e può mostrare più di un’opzione allo stesso tempo. Utilizza un database, un inventario e regole aziendali per determinare quando mostrare un prodotto come &quot;Just In&quot;, &quot;Clearance&quot; o &quot;Sold Out&quot;. Può anche utilizzare la trasparenza dietro il prodotto per mostrarlo su diversi sfondi, come in stanze diverse. Gli stessi modelli e/o risorse possono essere riutilizzati nella pagina dei dettagli del prodotto per mostrare una versione più grande o zoomabile dello stesso prodotto su diversi sfondi.

È importante comprendere che Dynamic Media Classic fornisce solo la parte visiva di queste applicazioni basate su modelli. Le aziende Dynamic Media Classic o i loro partner di integrazione devono fornire le regole aziendali, il database e le competenze di sviluppo per creare le applicazioni. Non esiste un&#39;applicazione modello &quot;integrata&quot;; i designer configurano il modello in Dynamic Media Classic e gli sviluppatori utilizzano chiamate URL per modificare le variabili nel modello.

Alla fine di questa sezione dell’esercitazione saprai come:

- Carica un PSD Photoshop in Dynamic Media Classic per utilizzarlo come base di un modello.
- Crea un semplice modello di base per merchandising composto da livelli immagine.
- Aggiungete i livelli di testo e rendeteli variabili mediante l’uso di parametri.
- Crea un URL modello e manipola l’immagine in modo dinamico tramite il browser web.

>[!NOTE]
>
>Tutti gli URL di questo capitolo sono a solo scopo illustrativo; non sono collegamenti in diretta.

## Panoramica dei modelli di base

La definizione di un modello di base (o semplicemente &quot;modello&quot;) è un’immagine a livelli indirizzabile tramite URL. Il risultato finale è un&#39;immagine, ma può essere modificata dall&#39;URL. Può essere costituito da foto, testo o grafica, qualsiasi combinazione di risorse P-TIFF in Dynamic Media Classic.

I modelli sono più simili ai file di Photoshop PSD, in quanto hanno un flusso di lavoro simile e funzionalità simili.

- Entrambi sono costituiti da strati che sono come fogli di acetato impilato. È possibile comporre immagini parzialmente trasparenti e vedere attraverso le aree trasparenti di un livello i livelli sottostanti.
- I livelli possono essere spostati e ruotati per riposizionare il contenuto e le modalità di opacità e fusione possono essere modificate per rendere il contenuto parzialmente trasparente.
- È possibile creare livelli basati su testo. La qualità può essere molto elevata perché Image Server utilizza lo stesso motore di testo di Photoshop e Illustrator.
- Gli stili di livello semplici possono essere applicati a ogni livello per creare effetti speciali come ombre di rilascio o bagliori.

Tuttavia, a differenza dei PSD Photoshop, i livelli possono essere interamente dinamici e controllati tramite un URL sul server di immagini.

- È possibile aggiungere variabili a tutte le proprietà del modello, facilitandone la modifica immediata.
- Le variabili denominate parametri consentono di esporre solo la parte del modello che si desidera modificare.

È sufficiente aggiungere un segnaposto per ogni livello che varia invece di inserire tutti i livelli in un singolo file come si fa in Photoshop, e mostrarli e nasconderli (anche se è possibile farlo, se lo si preferisce).

Utilizzando un segnaposto, puoi scambiare dinamicamente il contenuto di un livello con un’altra risorsa pubblicata e questo prenderà automaticamente le stesse proprietà (ad esempio, dimensioni e rotazione) del livello che ha sostituito.

Poiché i modelli di base sono generalmente progettati in Photoshop ma implementati tramite un URL, un progetto modello richiede una combinazione di competenze tecniche e di progettazione. In genere si presuppone che la persona che esegue il lavoro del modello creativo sia un designer di Photoshop e che la persona che implementa il modello sia uno sviluppatore web. I team creativi e di sviluppo devono collaborare strettamente affinché il modello abbia successo.

I progetti modello possono essere relativamente semplici o estremamente complessi a seconda delle regole di business e delle esigenze dell&#39;applicazione. I modelli di base vengono richiamati dal server di immagini, tuttavia, a causa della flessibilità dell’ambiente Dynamic Media Classic, è possibile nidificare i modelli all’interno di altri modelli, consentendo di creare immagini abbastanza complesse che possono essere collegate da variabili comunemente denominate.

- Ulteriori informazioni [Nozioni di base sui modelli](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/quick-start-template-basics.html).
- Scopri come creare un [Modello di base](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html#creating_a_template).

## Creazione di un modello di base

Quando lavori con un modello di base, in genere segui i passaggi del flusso di lavoro nel diagramma seguente. I passaggi contrassegnati con linee tratteggiate sono facoltativi se si utilizzano livelli di testo dinamici e sono indicati nelle istruzioni seguenti come &quot;Flusso di lavoro del testo&quot;. Se non si utilizza il testo, seguire solo il percorso principale.

![immagine](assets/basic-templates/basic-templates-1.png)

_Flusso di lavoro Modello di base ._

1. Progetta e crea le tue risorse. La maggior parte degli utenti esegue questa operazione in Adobe Photoshop. Progetta le risorse con le dimensioni desiderate: se si tratta di un&#39;immagine da 200 pixel per una pagina con miniature, progettala a 200 pixel. Se devi ingrandirlo, progettalo ad una dimensione di circa 2000 pixel. Utilizza Photoshop (e/o Illustrator salvato come bitmap) per creare le risorse e utilizza Dynamic Media Classic per assemblare le parti, gestire i livelli e aggiungere variabili.
2. Dopo aver progettato le risorse grafiche, caricale in Dynamic Media Classic. Anziché caricare singole risorse da PSD, consigliamo di caricare l’intero file PSD a livelli e di creare un file per livello da Dynamic Media Classic, utilizzando il comando **Gestisci livelli** al momento del caricamento (vedi di seguito per ulteriori dettagli). _Flusso di lavoro del testo: Se crei testo dinamico, carica anche i font. Il testo dinamico è variabile e controllato tramite l’URL. Se il testo è statico o contiene solo poche frasi brevi che non cambiano, ad esempio tag che dicono &quot;Nuovo&quot; o &quot;Vendita&quot;, invece di &quot;X% Off&quot;, con la X come numero variabile, consigliamo di pre-eseguire il rendering del testo in Photoshop e di caricarlo come livelli rasterizzati come immagini. È più semplice e puoi assegnare al testo lo stile desiderato._
3. Crea il modello in Dynamic Media Classic utilizzando l’editor Template Basics del menu Build e aggiungi livelli immagine. Flusso di lavoro del testo: Crea livelli di testo nello stesso editor. Questo passaggio è necessario quando si crea manualmente un modello in Dynamic Media Classic. Scegliete una dimensione dell’area di lavoro che corrisponda alla progettazione, trascinate e rilasciate le immagini nell’area di lavoro e impostate le proprietà del livello (dimensione, rotazione, opacità, ecc.). Non si inserisce ogni livello possibile sul modello, un solo segnaposto per livello immagine. _Flusso di lavoro del testo: Con lo strumento Testo potete creare livelli di testo, in modo analogo a come create i livelli di testo in Photoshop. È possibile scegliere un font e formattarlo utilizzando le stesse opzioni disponibili con lo strumento Photoshop Type._ Un altro flusso di lavoro consiste nel caricare un PSD e far sì che Dynamic Media Classic generi un modello &quot;gratuito&quot;, e può anche ricreare livelli di testo. Questo viene discusso più in dettaglio più avanti.
4. Una volta creati i livelli, aggiungi parametri (variabili) a qualsiasi proprietà di qualsiasi livello che desideri controllare tramite l’URL, inclusa la sorgente del livello (l’immagine stessa ). _Flusso di lavoro del testo: È inoltre possibile aggiungere parametri ai livelli di testo, sia per controllare il contenuto del testo e le dimensioni e la posizione del livello stesso, sia per tutte le opzioni di formattazione come il colore del font, la dimensione del font, il tracciamento orizzontale, ecc._
5. Crea un predefinito per immagini che corrisponda alle dimensioni del modello. A questo scopo, consigliamo di chiamare sempre il modello con dimensioni 1:1 e di aggiungere nitidezza a tutti i livelli immagine di grandi dimensioni che vengono ridimensionati per adattarlo al modello. Se crei un modello su cui ingrandire, questo passaggio non è necessario.
6. Pubblica, copia l’URL dall’anteprima di Dynamic Media Classic e testalo in un browser.

## Preparazione e caricamento delle risorse dei modelli in Dynamic Media Classic

Prima di caricare le risorse dei modelli in Dynamic Media Classic, dovrai completare alcuni passaggi preparatori.

### Preparazione di PSD per il caricamento

Prima di caricare il file Photoshop su Dynamic Media Classic, semplifica i livelli in Photoshop per facilitare il lavoro con e garantire la massima compatibilità con Image Server. Il tuo file PSD sarà spesso composto da molti elementi che Dynamic Media Classic non riconosce e potresti anche finire con molti piccoli pezzi che sono difficili da gestire. Assicurarsi di salvare un backup del PSD principale nel caso sia necessario modificare in seguito l&#39;originale. La copia semplificata verrà caricata e non il master.

![immagine](assets/basic-templates/basic-templates-2.jpg)

1. Semplifica la struttura del livello unendo/appiattendo i livelli correlati che devono essere attivati/disattivati insieme in un unico livello. Ad esempio, l’etichetta &quot;NEW&quot; e il banner blu vengono uniti in un singolo livello in modo da poterli mostrare o nascondere con un solo clic.
   ![immagine](assets/basic-templates/basic-templates-3.jpg)
2. Alcuni tipi di livelli ed effetti di livello non sono supportati da Dynamic Media Classic o dal server di immagini e devono essere rasterizzati prima del caricamento. In caso contrario, gli effetti potrebbero essere ignorati o i livelli scartati. Rasterizzare un livello significa convertirlo se non è modificabile. Per rasterizzare gli effetti di livello o i livelli di testo, create un livello vuoto, selezionatene e unitelo utilizzando **Livelli > Unisci livelli** o CTRL + E/CMD + E.

   - Dynamic Media Classic non può raggruppare o collegare livelli. Tutti i livelli di un gruppo o di un set collegato vengono convertiti in livelli separati che non sono più raggruppati/collegati.
   - Le maschere di livello vengono convertite in trasparenza al momento del caricamento.
   - I livelli di regolazione non sono supportati e vengono scartati.
   - I livelli di riempimento, come i livelli di colore solido, vengono rasterizzati.
   - I livelli di oggetti avanzati e i livelli vettoriali vengono rasterizzati in immagini normali al momento del caricamento e i filtri avanzati vengono applicati e rasterizzati.
   - Anche i livelli di testo verranno rasterizzati a meno che non si utilizzi l’opzione Estrai testo (vedi di seguito) per ulteriori informazioni.
   - La maggior parte degli effetti di livello viene ignorata e sono supportati solo alcuni metodi di fusione. In caso di dubbi, aggiungi effetti semplici in Dynamic Media Classic (ad esempio ombre interne o di rilascio, bagliori interni o esterni) oppure utilizza un livello vuoto per unire e rasterizzare l’effetto in Photoshop.

### Utilizzo dei font

Puoi anche caricare e pubblicare i font se hai bisogno di generare testo dinamico. L&#39;unico font incluso con Dynamic Media Classic è Arial.

È responsabilità di ogni azienda ottenere una licenza per l&#39;utilizzo di un font sul web — semplicemente avere un font installato sul tuo computer non ti dà il diritto di utilizzarlo commercialmente sul web, e la tua azienda potrebbe affrontare azioni legali da parte dell&#39;editore del font se utilizzato senza autorizzazione. Inoltre, i termini di licenza variano. Potrebbe essere necessario disporre di licenze separate per la stampa rispetto alla visualizzazione dello schermo, ad esempio.

Dynamic Media Classic supporta i font standard OpenType (OTF), TrueType (TTF) e Type 1 Postscript. Mac- non sono supportati solo i font della valigia, i file di raccolta dei tipi, i font di sistema di Windows e i font della macchina proprietari (come i font utilizzati da incisioni o ricami); sarà necessario convertirli in uno dei formati di font standard o sostituirli con un font simile da utilizzare in Dynamic Media Classic e sul server di immagini.

Dopo aver caricato i font in Dynamic Media Classic, come qualsiasi altra risorsa, devono anche essere pubblicati sul server di immagini. Un errore di modello molto comune è quello di dimenticare di pubblicare i font, che si tradurrà in un errore di immagine — il server di immagini non sostituirà un altro font al suo posto. Inoltre, se desideri utilizzare il **Estrai testo** al momento del caricamento, devi caricare i file dei font prima di caricare il PSD che utilizza tali font. La **Estrai testo** tenterà di ricreare il testo come livello di testo modificabile e inserirlo all’interno di un modello Dynamic Media Classic. Questo argomento è trattato nell’argomento successivo, Opzioni di PSD.

Tieni presente che i font hanno più nomi interni che spesso sono diversi dal nome del file esterno. Puoi vedere tutti i loro nomi diversi nella pagina Dettagli della risorsa in Dynamic Media Classic. Di seguito sono riportati i nomi del font Adobe Caslon Pro Semibold, elencato nella scheda Metadati in Dynamic Media Classic:

![immagine](assets/basic-templates/basic-templates-4.jpg)

_Scheda Metadati nella pagina Dettagli per un font in Dynamic Media Classic._

Dynamic Media Classic utilizza il nome file di questo font (ACaslonPro-Semibold) come ID risorsa, tuttavia questo non è il nome utilizzato dal modello. Il modello utilizza il nome RTF (Rich Text Format), elencato in basso. RTF è la &quot;lingua&quot; nativa del motore di testo di Image Server.

Se devi modificare i font tramite l’URL, devi chiamare il nome RTF del font (non l’ID risorsa), altrimenti riceverai un errore. In questo caso, il nome corretto per questo font sarebbe &quot;Adobe Caslon Pro&quot;. Discuteremo di più sui font e RTF nell&#39;argomento RTF e parametri di testo, qui sotto.

I formati di file di font più comuni presenti nei sistemi Windows e Mac sono OpenType e TrueType. L&#39;OpenType ha un&#39;estensione .OTF, mentre TrueType è .TTF. Entrambi i formati funzionano allo stesso modo in Dynamic Media Classic.

### Selezione Delle Opzioni Per Il Caricamento Del PSD

Non è necessario caricare un file Photoshop (PSD) per creare un modello; un modello può essere costruito da qualsiasi risorsa immagine in Dynamic Media Classic. Tuttavia, il caricamento di un PSD può semplificare l’authoring, in quanto in genere queste risorse sono già presenti in un PSD a livelli. Inoltre, Dynamic Media Classic genera automaticamente un modello quando carichi un PSD a livelli.

- **Mantenere I Livelli.** Questa è l&#39;opzione più importante. Questo comunica a Dynamic Media Classic di creare una risorsa immagine per livello Photoshop. Se questa opzione è deselezionata, tutte le altre opzioni sono disabilitate e PSD viene appiattito in un’unica immagine.
- **Crea** **Modello.** Questa opzione prende i vari livelli generati e crea automaticamente un modello combinandoli nuovamente insieme. L’utilizzo del modello generato automaticamente comporta il posizionamento di tutti i livelli in un unico file, mentre è necessario un solo segnaposto per livello. È abbastanza facile eliminare i livelli extra, ma se hai molti livelli, è più veloce ricrearli. Assicurati di rinominare il nuovo modello; in caso contrario, viene sovrascritto la prossima volta che ricarichi lo stesso PSD.
- **Estrai testo.** In questo modo i livelli di testo in PSD vengono ricreati come livelli di testo nel modello utilizzando il font caricato. Questo passaggio è necessario se il testo si trova su un percorso in Photoshop e desideri mantenere tale percorso nel modello. Questa funzione richiede l’utilizzo del **Crea modello** , poiché il testo estratto può essere creato solo da un modello generato al momento del caricamento.
- **Estende i livelli alle dimensioni dello sfondo.** Questa impostazione rende ogni livello della stessa dimensione dell’area di lavoro di PSD complessiva. Questo è molto utile per i livelli che rimarranno sempre fissati in posizione: altrimenti, quando si scambiano le immagini nello stesso livello, potrebbe essere necessario riposizionarle.
- **Denominazione dei livelli** Questo indica a Dynamic Media Classic come denominare ogni risorsa generata per livello. Consigliamo entrambi **Photoshop** **e livello** **Nome** o Photoshop e **Livello** **Numero**. Entrambe le opzioni utilizzano il nome di PSD come prima parte del nome e aggiungono il nome o il numero del livello alla fine. Ad esempio, se hai un PSD denominato &quot;shirt.psd&quot; e presenta livelli denominati &quot;front&quot;, &quot;maniche&quot; e &quot;collar&quot;, se lo carichi utilizzando **Photoshop e** Livello **Nome** In Dynamic Media Classic vengono generati gli ID risorsa &quot;shirt_front&quot;, &quot;shirt_maneves&quot; e &quot;shirt_collar&quot;. L’utilizzo di una di queste opzioni consente di garantire che il nome sia univoco in Dynamic Media Classic.

## Creazione di un modello con livelli immagine

Anche se Dynamic Media Classic può creare automaticamente un modello da un PSD a livelli, è necessario sapere come creare manualmente il modello. Come spiegato in precedenza, in alcuni casi non si desidera utilizzare il modello creato da Dynamic Media Classic.

### Interfaccia utente di base dei modelli

Per prima cosa, acquisiamo familiarità con l&#39;interfaccia di modifica.

Nel centro a sinistra si trova l’area di lavoro che mostra un’anteprima del modello finale. Sul lato destro sono presenti i pannelli Livelli e Proprietà livello . Queste aree sono dove lavori di più.

![immagine](assets/basic-templates/basic-templates-5.jpg)

_Pagina Genera nozioni di base sui modelli ._

- **Area di lavoro/anteprima.** Questa è la finestra principale. Qui puoi spostare, ridimensionare e ruotare i livelli con il mouse. I contorni dei livelli vengono visualizzati come linee tratteggiate.
- **Strati.** È simile al pannello Livelli di Photoshop. I livelli aggiunti al modello verranno visualizzati qui. I livelli sono sovrapposti dall’alto verso il basso — il livello superiore nel pannello Livelli è visualizzato sopra gli altri al di sotto nell’elenco.
- **Proprietà livello.** Qui è possibile regolare tutte le proprietà di un livello utilizzando controlli numerici. Selezionare prima un livello e quindi regolarne le proprietà.
- **Composito** **URL.** Nella parte inferiore dell’interfaccia utente è presente l’area Composite URL. Questo non verrà discusso in questa sezione dell&#39;esercitazione, tuttavia qui vedrai il tuo modello decostruito come una serie di modificatori URL di Image Serving. Questa area è modificabile: se hai familiarità con i comandi Image Server puoi modificare manualmente il modello qui. Tuttavia puoi anche romperlo. Come Photoshop, la numerazione dei livelli inizia da 0. L&#39;area di lavoro è il livello 0 e il primo livello che aggiungi è il livello 1. Le modalità di fusione determinano il modo in cui i pixel di un livello si fondono con i pixel al di sotto di esso. Potete creare una varietà di effetti speciali utilizzando le modalità di fusione.

#### Utilizzo dell’Editor di base dei modelli

Di seguito sono riportati i passaggi del flusso di lavoro per avviare il modello di base:

1. In Dynamic Media Classic, vai a **Genera > Nozioni di base sui modelli**. Non è possibile selezionare nulla oppure iniziare selezionando un&#39;immagine che diventa il primo livello del modello.
2. Scegliere una dimensione e premere **OK**. Questa dimensione deve corrispondere alla dimensione progettata in Photoshop. Verrà caricato l’editor modelli.
3. Se non hai selezionato un’immagine al passaggio 1, cerca o sfoglia un’immagine nel pannello delle risorse a sinistra e trascinala sull’area di lavoro.

   - L&#39;immagine viene ridimensionata automaticamente per adattarsi alle dimensioni dell&#39;area di lavoro. Se prevedi di sostituire le immagini ad alta risoluzione, tipicamente inserisci una delle tue grandi immagini P-TIFF (2000 px) e utilizzala come segnaposto.
   - Questo dovrebbe essere il livello più in basso del modello, tuttavia è possibile riordinare i livelli in un secondo momento.

4. Ridimensionare o riposizionare il livello direttamente nell’area di lavoro o regolando le impostazioni nel pannello Proprietà livello.
5. Trascina altri livelli immagine come necessario. Se lo desideri, aggiungi anche gli effetti dei livelli. Vedi l&#39;argomento _Aggiunta di effetti livello_ qui sotto.
6. Fai clic su **Salva**, scegli una posizione e assegna un nome al modello. È possibile visualizzare in anteprima, tuttavia a questo punto il modello sarà esattamente come un&#39;immagine Photoshop appiattita — non è ancora modificabile.

### Aggiunta di effetti livello

Image Server supporta alcuni effetti programmatici di livello, effetti speciali che modificano l’aspetto del contenuto di un livello. Funzionano in modo simile agli effetti di livello in Photoshop. Essi sono collegati a un livello ma controllati indipendentemente dal livello. È possibile regolarli o rimuoverli senza apportare una modifica permanente al livello stesso.

- **Ombra esterna**. Applica un&#39;ombreggiatura esterna ai bordi del livello, posizionata da un offset pixel x e y.
- **Ombra interna**. Applica un&#39;ombreggiatura all&#39;interno dei bordi del livello, posizionata da un offset pixel x e y.
- **Bagliore esterno**. Applica un effetto di bagliore uniformemente intorno a tutti i bordi del livello.
- **Bagliore interno**. Applica un effetto di bagliore uniformemente all’interno di tutti i bordi del livello.

![immagine](assets/basic-templates/basic-templates-6.jpg)

_Un livello con e senza ombra esterna_

Per aggiungere un effetto, fai clic su **Aggiungi effetto**, quindi scegli un effetto dal menu . Come per i livelli normali, potete selezionare un effetto nel pannello Livelli e usare il pannello Proprietà livello per regolarne le impostazioni.

Gli effetti ombreggiatura vengono spostati orizzontalmente o verticalmente dal livello, mentre gli effetti Bagliore vengono applicati uniformemente in tutte le direzioni. Gli effetti interni agiscono sulle parti opache dello strato, mentre gli effetti esterni influiscono solo sulle aree trasparenti.

Ulteriori informazioni[Aggiunta di effetti livello](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html#using-shadow-and-glow-effects-on-layers).

### Aggiunta di parametri

Se tutto ciò che si fa è combinare i livelli e salvarli, il risultato netto non è diverso da un&#39;immagine Photoshop appiattita. Ciò che rende i modelli speciali è la capacità di aggiungere parametri alle proprietà di ciascun livello, in modo che possano essere modificati dinamicamente tramite l’URL.

In termini Dynamic Media Classic, un parametro è una variabile che può essere collegata a una proprietà del modello in modo che possa essere manipolata tramite un URL. Quando aggiungi un parametro a un livello, Dynamic Media Classic espone tale proprietà nell’URL prefissando il nome del parametro con un simbolo del dollaro ($). Ad esempio, se crei un parametro denominato &quot;size&quot; per modificare le dimensioni di un livello, Dynamic Media Classic rinomina il parametro $size.

Se non aggiungi un parametro per una proprietà, tale proprietà rimane nascosta nel database Dynamic Media Classic e non viene visualizzata nell&#39;URL.

![immagine](assets/basic-templates/parameters.png)

Senza parametri, gli URL in genere sarebbero molto più lunghi, soprattutto se si utilizza anche testo dinamico. Il testo aggiunge molte dozzine di caratteri in più a ciascun URL.

Infine, il set iniziale di parametri diventa il valore predefinito delle proprietà nel modello. Se si crea il modello, si aggiungono parametri e quindi si chiama l&#39;URL senza i relativi parametri, Image Server creerà l&#39;immagine con tutti i valori predefiniti salvati nel modello. I parametri sono necessari solo se desideri modificare una proprietà. Se non è necessario modificare una proprietà, non è necessario impostare un parametro.

#### Creazione di parametri

Questo è il flusso di lavoro per la creazione dei parametri:

1. Fai clic sul pulsante **Parametri** accanto al nome del livello per il quale si desidera creare i parametri. Viene visualizzata la schermata Parameters (Parametri). Elenca ogni proprietà sul livello e il relativo valore.
1. Seleziona la **On** accanto al nome di ogni proprietà che si desidera rendere in un parametro. Viene visualizzato un nome di parametro predefinito. È possibile aggiungere parametri solo alle proprietà che sono state modificate rispetto al loro stato predefinito.

   - Ad esempio, se aggiungi un livello e lo mantieni nella sua posizione xy predefinita di 0,0, Dynamic Media Classic non esporrà un **Posizione** proprietà. Per correggere, spostare il livello almeno un pixel. Ora Dynamic Media Classic esporrà **Posizione** come proprietà puoi parametrizzare.
   - Per aggiungere un parametro alla proprietà show/hide (che attiva e disattiva il livello), fai clic sul pulsante **Mostra** o **Nascondi livello** icona per disattivare il livello (puoi riattivarlo, se lo desideri). Dynamic Media Classic ora esporrà un **Nascondi** proprietà che può essere parametrizzata.

1. Rinomina i nomi dei parametri predefiniti in modo da facilitarne l’identificazione nell’URL. Ad esempio, se desideri aggiungere un parametro per modificare il livello del banner sopra un’immagine, modifica il nome predefinito di &quot;layer_2_src&quot; in &quot;banner&quot;.
1. Press **Chiudi** per uscire dalla schermata Parameters (Parametri).
1. Ripeti questo processo per altri livelli facendo clic sul pulsante **Parametri** e aggiunta e ridenominazione dei parametri.
1. Al termine, salva le modifiche.

>[!TIP]
>
>Rinomina i parametri in modo significativo e sviluppa una convenzione di denominazione per standardizzare tali nomi. Assicurati che la convenzione di denominazione sia concordata in anticipo dai team di progettazione e sviluppo.
>
>Impossibile aggiungere un parametro perché la proprietà non è visibile? È sufficiente modificare la proprietà del livello dal valore predefinito (spostandolo, ridimensionandolo, nascondendolo, ecc.). Ora dovresti vedere la proprietà esposta.

Ulteriori informazioni [Parametri del modello](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html).

## Creazione di un modello con livelli di testo

Ora verrà illustrato come creare un modello di base che includa i livelli di testo.

### Informazioni sul testo dinamico

Ora è possibile creare un modello di base utilizzando i livelli immagine. Per molte applicazioni questo è tutto ciò di cui hai bisogno. Come mostrato nell’esercizio precedente, i livelli con testo semplice (come &quot;Vendita&quot; e &quot;Nuovo&quot;) possono essere rasterizzati e trattati come immagini perché il loro testo non deve essere modificato.

Tuttavia, cosa succede se hai bisogno di:

- Aggiungi un’etichetta con il valore &quot;25% Off&quot; e il valore 25% è variabile
- Aggiungi un’etichetta di testo con il nome del prodotto sopra l’immagine
- Localizzare i livelli in lingue diverse a seconda del paese in cui viene visualizzato il modello

In tal caso, è necessario aggiungere alcuni livelli di testo dinamici con parametri per controllare il testo e/o la formattazione.

Per creare il testo, è necessario caricare alcuni font, altrimenti Dynamic Media Classic verrà impostato su Arial come impostazione predefinita. I font devono anche essere pubblicati sul server di immagini, altrimenti genererà un errore nel momento in cui tenta di eseguire il rendering di qualsiasi testo che utilizza quel font.

### Parametri RTF e testo

Per aggiungere variabili al testo utilizzando lo strumento Nozioni di base sui modelli, è necessario comprendere il rendering del testo. Image Server genera il testo utilizzando il motore di testo di Adobe, lo stesso motore utilizzato da Photoshop e Illustrator, e lo compone come livello nell’immagine finale. Per comunicare con il motore, Image Server utilizza il formato RTF o RTF.

RTF è una specifica di formato file sviluppata da Microsoft per specificare la formattazione dei documenti. Si tratta di un linguaggio di markup standard utilizzato dalla maggior parte del software di elaborazione testi e di posta elettronica. Se si scrive in un URL &amp;text=\b1 Hello, Image Server genera un&#39;immagine con la parola &quot;Hello&quot; in grassetto, perché \b1 è il comando RTF per rendere il testo in grassetto.

La buona notizia è che Dynamic Media Classic genera il RTF per voi. Ogni volta che digiti del testo in un modello e aggiungi la formattazione, Dynamic Media Classic scrive silenziosamente il codice RTF nel modello automaticamente. Il motivo per cui lo menzioniamo è perché si stanno aggiungendo parametri direttamente al RTF stesso, quindi è importante che tu sia un po &#39;familiare con esso.

#### Creazione di livelli di testo

È possibile creare livelli di testo in un modello in Dynamic Media Classic nei due modi seguenti:

1. Strumento Testo in Dynamic Media Classic. Discuteremo di questo metodo di seguito. L’editor Modelli di base dispone di uno strumento che consente di creare una casella di testo, immettere testo e formattare il testo. Dynamic Media Classic genera il RTF in base alle esigenze e lo inserisce in un livello separato.
2. Estrai testo (al momento del caricamento). L’altro metodo consiste nel creare il livello di testo in Photoshop e salvarlo in PSD come livello di testo normale (invece di rasterizzarlo come livello di immagine). Quindi carica il file in Dynamic Media Classic e utilizza il **Estrai testo** opzione . Dynamic Media Classic converte ogni livello di testo Photoshop in un livello di testo Image Serving utilizzando i comandi RTF. Se utilizzi questo metodo, assicurati di caricare prima i font in Dynamic Media Classic, altrimenti Dynamic Media Classic sostituirà un font predefinito al momento del caricamento e non esiste un modo semplice per sostituire nuovamente il font corretto.

### Editor di testo

Il testo viene immesso mediante l’Editor di testo. L’Editor di testo è un’interfaccia WYSIWYG che consente di immettere e formattare il testo utilizzando controlli di formattazione simili a quelli di Photoshop o Illustrator.

![immagine](assets/basic-templates/basic-templates-9.jpg)

_Editor di testo di base dei modelli_

Farai la maggior parte del tuo lavoro nel **Anteprima** , che consente di immettere il testo e visualizzarlo come apparirà nel modello. C&#39;è anche un **Origine** , che viene utilizzato per modificare manualmente il RTF, se necessario.

Il flusso di lavoro generale consiste nell’utilizzare il **Anteprima** per digitare del testo.

Quindi selezionare il testo e scegliere una formattazione come il colore del font, la dimensione del font o la giustificazione utilizzando i controlli nella parte superiore. Dopo aver formattato il testo nel modo desiderato, fare clic su **Applica** per visualizzare l&#39;aggiornamento nell&#39;anteprima dell&#39;area di lavoro. Quindi chiudi l’Editor di testo per tornare alla finestra principale Nozioni di base sui modelli.

#### Utilizzo dell’Editor di testo

Di seguito sono riportati i passaggi del flusso di lavoro per aggiungere testo nella pagina della build Template Basics :

1. Fai clic sul pulsante **Testo** pulsante dello strumento nella parte superiore della pagina di creazione.
2. Trascinare una casella di testo in cui si desidera visualizzare il testo. La finestra Editor di testo si apre in una finestra modale. In background, il modello verrà visualizzato, ma non sarà modificabile finché non avrai completato la modifica del testo.
3. Digitare il testo di esempio che si desidera visualizzare al primo caricamento del modello. Ad esempio, se crei una casella di testo per un&#39;immagine e-mail personalizzata, il testo potrebbe dire &quot;Ciao Name. Ora è il momento di risparmiare!&quot; Successivamente, aggiungi un parametro di testo per sostituire Nome con un valore inviato sull’URL. Il testo verrà visualizzato nel modello sotto la finestra solo dopo aver fatto clic su **Applica**.
4. Per formattare il testo, selezionalo trascinandolo con il mouse e scegli un controllo di formattazione nell’interfaccia utente.

   - Ci sono molte opzioni di formattazione. Alcuni dei più comuni sono il font (faccia), la dimensione del font e il colore del font, così come la giustificazione sinistra/centro/destra.
   - Non dimenticare di selezionare prima il testo. In caso contrario, non potrai applicare alcuna formattazione.
   - Per scegliere un font diverso, assicurarsi di selezionare il testo e aprire il menu Font. L&#39;editor mostrerà un elenco di tutti i font caricati su Dynamic Media Classic. Se sul computer è installato anche un font, questo verrà visualizzato in nero. Se non è installato sul computer, verrà visualizzato in rosso. Tuttavia, esegue ancora il rendering nella finestra di anteprima quando fai clic su **Applica**. Devi solo caricare i font in Dynamic Media Classic per renderli disponibili a chiunque utilizzi Dynamic Media Classic. Una volta pubblicata, il server di immagini utilizzerà questi font per generare il testo; gli utenti non dovranno installare alcun font per visualizzare il testo creato perché fa parte di un&#39;immagine.
   - A differenza di Photoshop e Illustrator, Image Server può allineare il testo verticalmente nella casella di testo. L’impostazione predefinita è l’allineamento superiore. Per modificare questa impostazione, seleziona il testo e scegli **Medio** o **In basso** dal **Allineamento verticale** menu.
   - Se si rende il testo troppo grande per la casella (o la casella di testo è troppo piccola), tutto o parte di essa verrà ritagliato e scompare. Ridurre le dimensioni del font o ingrandire la casella.

5. Fai clic su **Applica** per visualizzare le modifiche attive nella finestra dell&#39;area di lavoro. Devi fare clic su **Applica** altrimenti le modifiche andranno perse.
6. Al termine, fai clic su **Chiudi**. Per tornare alla modalità di modifica, fate doppio clic sul livello di testo per riaprire l’Editor di testo.

L&#39;editor di testo visualizza esattamente la dimensione del font se il font è installato localmente sul sistema.

### Informazioni sull’aggiunta di parametri ai livelli di testo

Seguiamo ora un processo simile per l’aggiunta di parametri di testo, come per i parametri di livello. I livelli di testo possono anche assumere parametri di livello per dimensioni, posizione e così via; tuttavia, possono prendere parametri aggiuntivi che ti permettono di controllare qualsiasi aspetto del RTF.

A differenza dei parametri di livello, selezionate solo il valore da modificare e aggiungete un parametro a tale valore, anziché aggiungere un parametro all’intera proprietà.

![immagine](assets/basic-templates/basic-templates-10.jpg)

Esempio RTF:

![immagine](assets/basic-templates/sample-rtf.png)

Quando si esamina il RTF, è necessario capire dove si desidera modificare ogni impostazione. Nel RTF qui sopra, alcuni di essi potrebbero avere un senso e si può vedere da dove proviene la formattazione.

Potete vedere la frase Sandalo della zecca di cioccolato — questo è il testo stesso.

- C&#39;è un riferimento al carattere Scarsa Richard — qui è dove vengono selezionati i font.
- È disponibile un valore RGB: \red56\green53\blue4 — questo è il colore del testo.
- Anche se la dimensione del font è 20, il numero 20 non viene visualizzato. Tuttavia, si vede un comando \fs40 — per qualche motivo strano, RTF misura i font come punti di mezzo. Quindi \fs40 è la dimensione del carattere!

Hai abbastanza informazioni per creare i tuoi parametri, tuttavia c&#39;è un riferimento completo di tutti i comandi RTF nella documentazione di Image Serving. Visita il [Documentazione su Image Server](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html#concept-0d3136db7f6f49668274541cd4b6364c).

#### Aggiunta di parametri ai livelli di testo

Di seguito sono riportati i passaggi per aggiungere parametri ai livelli di testo.

1. Fai clic sul pulsante **Parametri** pulsante (a &quot;P&quot;) accanto al nome del livello di testo per il quale desideri creare parametri. Viene visualizzata la schermata Parameters (Parametri). La **Comune** elenca ciascuna proprietà del livello e il relativo valore. Qui puoi aggiungere parametri di livello normali.
1. Fai clic sul pulsante **Testo** scheda . Qui puoi vedere il RTF in alto; i parametri aggiunti si trovano sotto di essi.
1. Per aggiungere un parametro, evidenzia prima il valore da modificare e fai clic sul pulsante **Aggiungi parametro** pulsante . Assicurarsi di selezionare solo i valori per i comandi e non l&#39;intero comando stesso. Ad esempio, per impostare un parametro per il nome del font nel file RTF di esempio sopra, vorrei solo evidenziare &quot;Scarso Richard&quot; e aggiungere un parametro a esso, ma non anche il &quot;\f0&quot;. Quando fai clic su **Aggiungi parametro** , appare nell&#39;elenco seguente e il valore del parametro appare in rosso nel RTF mentre è ancora selezionato. Per rimuovere un parametro, fai clic sulla casella di controllo accanto a **On** per disattivare il parametro, scompare.
1. Fai clic su per rinominare il parametro in un nome più significativo.
1. Al termine, il tuo RTF viene evidenziato in verde dove esistono parametri e i tuoi nomi e valori dei parametri sono elencati di seguito.
1. Fai clic su **Chiudi** per uscire dalla schermata Parameters (Parametri). Quindi premere **Salva** , per salvare il modello. Se hai finito di modificare, premi **Chiudi** per uscire dalla pagina Template Basics (Nozioni di base sui modelli).
1. Fai clic su **Anteprima** per testare il modello in Dynamic Media Classic. Per verificare i parametri di testo, digitare nuovo testo o nuovi valori nella finestra di anteprima. Per modificare il font, è necessario digitare il nome RTF esatto del font.

>[!TIP]
>
>Per aggiungere parametri al colore del testo, aggiungi separatamente i parametri rosso, verde e blu. Ad esempio, se RTF è `\red56\green53\blue46`, aggiungi parametri separati di colore rosso, verde e blu per i valori 56, 53 e 46. Nell’URL, cambieresti il colore chiamando tutti e tre: `&$red=56&$green=53&$blue=46`.

Scopri come [Creare parametri di testo dinamici](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html#creating-dynamic-text-parameters).

## Pubblicazione e creazione degli URL dei modelli

### Creare un predefinito per immagini

La creazione di un predefinito per il modello non è un passaggio obbligatorio. Si consiglia di utilizzarlo come best practice, in modo che il modello venga sempre chiamato a una dimensione 1:1 e che venga aggiunta la nitidezza a tutti i livelli immagine di grandi dimensioni che vengono ridimensionati per adattarli al modello. Se si chiama un&#39;immagine senza un predefinito, Image Server potrebbe ridimensionare arbitrariamente l&#39;immagine alle dimensioni predefinite (circa 400 pixel) e non applicherà la nitidezza predefinita.

Non c&#39;è nulla di speciale in un predefinito immagine per un modello. Se disponi già di un predefinito per un’immagine statica della stessa dimensione, puoi utilizzarlo al suo posto.

### Pubblicazione

Sarà necessario eseguire una pubblicazione per visualizzare le modifiche inviate in diretta al server di immagini. Tieni presente cosa deve essere pubblicato: i vari livelli di risorsa immagine, i font per il testo dinamico e il modello stesso. Simile ad altre risorse rich media di Dynamic Media Classic come Set di immagini e Set 360 gradi, un Modello di base è una costruzione artificiale, è un elemento di riga nel database che fa riferimento alle immagini e ai font utilizzando una serie di comandi Image Serving. Quindi, quando pubblichi il modello, tutto quello che stai facendo è aggiornare i dati sul server di immagini.

Ulteriori informazioni [Pubblicazione del modello](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/publishing-templates.html).

### Costruzione URL modello

Un modello di base ha la stessa sintassi URL essenziale di una normale chiamata immagine come spiegato in precedenza. In genere, un modello contiene più modificatori, ovvero comandi separati da una e commerciale (&amp;), ad esempio parametri con valori. Tuttavia, la differenza principale consiste nel chiamare il modello come immagine principale, invece di richiamare un&#39;immagine statica.

![immagine](assets/basic-templates/basic-templates-11.jpg)

A differenza di Image Preset, che hanno un simbolo del dollaro ($) su ciascun lato del nome predefinito, i parametri hanno un simbolo del dollaro all&#39;inizio. Il posizionamento di quei simboli del dollaro è importante.

**Corretto:**

`$text=46-inch LCD HDTV`

**Scorretto:**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Come indicato in precedenza, i parametri vengono utilizzati per modificare il modello. Se chiami il modello senza parametri, tornerà alle impostazioni predefinite come progettato nello strumento di creazione dei modelli di base. Se non è necessario modificare una proprietà, non è necessario impostare un parametro.

![immagine](assets/basic-templates/sandals-without-with-parameters.png)
_Esempi di un modello senza parametri di impostazione (sopra) e con parametri (di seguito)._
