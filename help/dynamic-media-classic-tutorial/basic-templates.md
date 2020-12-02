---
title: Introduzione ai modelli di base
description: Scoprite i modelli di base in Dynamic Media Classic, modelli basati su immagini richiamati dal server immagini e costituiti da immagini e testo di rendering. Un modello può essere modificato in modo dinamico tramite l’URL dopo che è stato pubblicato. Scoprirete come caricare un file PSD Photoshop in Dynamic Media Classic per utilizzarlo come base di un modello. Create un semplice modello di base per merchandising composto da livelli di immagine. Aggiungete livelli di testo e rendeteli variabili mediante l’uso di parametri. Create un URL modello e manipolate l’immagine in modo dinamico tramite il browser Web.
sub-product: dynamic-media
feature: templates
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '6301'
ht-degree: 0%

---


# Introduzione ai modelli di base {#basic-templates}

In termini Dynamic Media Classic, un modello è un documento che può essere modificato dinamicamente tramite l’URL dopo che il modello è stato pubblicato. Dynamic Media Classic offre modelli di base, modelli basati sulle immagini richiamati dal server immagini e costituiti da immagini e testo di rendering.

Uno degli aspetti più potenti dei modelli è che dispongono di punti di integrazione diretti che consentono di collegarli al database. Pertanto, non solo potete fornire un&#39;immagine e ridimensionarla, ma potete anche eseguire una query sul database per trovare elementi nuovi o di vendita e fare in modo che vengano visualizzati come sovrapposizione sull&#39;immagine. Potete richiedere una descrizione dell&#39;elemento e visualizzarlo come etichetta in un font scelto e nel layout. Le possibilità sono illimitate.

I modelli di base possono essere implementati in diversi modi, da semplici a complessi. Esempio:

- merchandising di base. Utilizza etichette come &quot;spedizione gratuita&quot; se il prodotto dispone di spedizione gratuita. Queste etichette vengono configurate dal team merchandise in Photoshop e il Web utilizza la logica per sapere quando applicarle all&#39;immagine.
- Merchandising avanzato. Ogni modello ha più variabili e può mostrare più di un&#39;opzione allo stesso tempo. Utilizza un database, un inventario e regole aziendali per determinare quando mostrare un prodotto come &quot;Just In&quot;, &quot;Clearance&quot; o &quot;Sold Out&quot;. Può anche utilizzare la trasparenza dietro il prodotto per mostrarlo su diversi sfondi, ad esempio in stanze diverse. Gli stessi modelli e/o risorse possono essere riutilizzati nella pagina dei dettagli del prodotto per mostrare una versione più grande o ingrandibile dello stesso prodotto su diversi sfondi.

È importante comprendere che Dynamic Media Classic fornisce solo la parte visiva di queste applicazioni basate su modelli. Le aziende Dynamic Media Classic o i loro partner di integrazione devono fornire le regole aziendali, il database e le competenze di sviluppo per creare le applicazioni. Non esiste un&#39;applicazione modello &quot;integrata&quot;; i progettisti configurano il modello in Dynamic Media Classic e gli sviluppatori utilizzano chiamate URL per modificare le variabili nel modello.

Alla fine di questa sezione dell&#39;esercitazione potrai imparare a:

- Caricate un file PSD Photoshop in Dynamic Media Classic per usarlo come base di un modello.
- Create un semplice modello di base per merchandising composto da livelli di immagine.
- Aggiungete livelli di testo e rendeteli variabili mediante l’uso di parametri.
- Create un URL modello e manipolate l’immagine in modo dinamico tramite il browser Web.

>[!NOTE]
>
>Tutti gli URL di questo capitolo sono solo a scopo illustrativo; non sono collegamenti dinamici.

## Panoramica dei modelli di base

La definizione di modello di base (o semplicemente &quot;modello&quot;, per farla breve) è un’immagine a livelli indirizzabile agli URL. Il risultato finale è un’immagine, ma può essere modificata dall’URL. Può essere costituito da foto, testo o elementi grafici — qualsiasi combinazione di risorse P-TIFF in Dynamic Media Classic.

I modelli sono simili ai file PSD di Photoshop, in quanto hanno un flusso di lavoro simile e funzionalità simili.

- Entrambi sono composti di strati che sono come fogli di acetato impilato. Potete comporre le immagini parzialmente trasparenti e visualizzare attraverso le aree trasparenti di un livello i livelli sottostanti.
- I livelli possono essere spostati e ruotati per riposizionare il contenuto; i metodi di opacità e fusione possono essere modificati per rendere il contenuto parzialmente trasparente.
- Potete creare livelli basati su testo. La qualità può essere molto elevata perché Image Server usa lo stesso motore di testo di Photoshop e  Illustrator.
- Potete applicare stili di livello semplici a ciascun livello per creare effetti speciali quali ombre esterne o bagliori.

Tuttavia, a differenza dei file PSD Photoshop, i livelli possono essere interamente dinamici e controllati tramite un URL sul server immagini.

- Potete aggiungere variabili a tutte le proprietà del modello, semplificando al volo la modifica della composizione.
- Le variabili denominate parametri consentono di esporre solo la parte del modello da modificare.

È sufficiente aggiungere un segnaposto per ciascun livello che varia invece di inserire tutti i livelli in un singolo file, come si fa in Photoshop, e mostrarli e nasconderli (anche se potete farlo, se preferite).

Utilizzando un segnaposto, potete sostituire in modo dinamico il contenuto di un livello con un’altra risorsa pubblicata e avrà automaticamente le stesse proprietà (come dimensione e rotazione) del livello che ha sostituito.

Poiché i modelli di base sono generalmente progettati in Photoshop ma distribuiti tramite un URL, un progetto modello richiede una combinazione di competenze tecniche e di progettazione. In genere, supponiamo che la persona che esegue il lavoro del modello creativo sia un designer Photoshop e che la persona che implementa il modello sia uno sviluppatore Web. I team creativi e di sviluppo devono collaborare strettamente affinché il modello abbia successo.

I progetti modello possono essere relativamente semplici o estremamente complessi a seconda delle regole aziendali e delle esigenze dell&#39;applicazione. I modelli di base vengono richiamati dal server immagini; tuttavia, grazie alla flessibilità dell’ambiente Dynamic Media Classic, è possibile nidificare i modelli anche all’interno di altri modelli, per creare immagini piuttosto complesse che possono essere collegate da variabili comunemente denominate.

- Ulteriori informazioni su [Funzioni di base dei modelli](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/quick-start-template-basics.html).
- Scoprite come creare un [modello di base](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#creating_a_template).

## Creazione di un modello di base

Quando si utilizza un modello di base, in genere si seguono i passaggi del flusso di lavoro nel diagramma seguente. I passaggi contrassegnati con linee punteggiate sono facoltativi se si utilizzano livelli di testo dinamici e sono indicati nelle istruzioni riportate di seguito come &quot;Flusso di lavoro del testo&quot;. Se non si utilizza il testo, seguire solo il percorso principale.

![immagine](assets/basic-templates/basic-templates-1.png)

_Flusso di lavoro Modello di base._

1. Progettate e create le risorse. La maggior parte degli utenti esegue questa operazione in  Adobe Photoshop. Progettare le risorse con le dimensioni esatte necessarie — se si tratta di un’immagine da 200 pixel per una pagina in miniatura, progettate l’immagine a 200 pixel. Per ingrandire la visualizzazione, progettate le immagini con una dimensione di circa 2000 pixel. Utilizzate Photoshop (e/o  Illustrator salvato come bitmap) per creare le risorse, quindi utilizzate Dynamic Media Classic per assemblare le parti, gestire i livelli e aggiungere variabili.
2. Dopo aver progettato le risorse grafiche, caricatele in Dynamic Media Classic. Invece di caricare singole risorse dal file PSD, consigliamo di caricare l’intero file PSD a livelli e di creare un file per livello mediante l’opzione **Mantieni livelli** al momento del caricamento (consultate di seguito per ulteriori dettagli). _Flusso di lavoro testo: Se create testo dinamico, caricate anche i font. Il testo dinamico è variabile e controllato tramite l’URL. Se il testo è statico o contiene solo poche frasi brevi che non vengono modificate — ad esempio, i tag che dicono &quot;Nuovo&quot; o &quot;Vendita&quot; anziché &quot;X% Disattivato&quot;, con la X come numero variabile — è consigliabile eseguire il pre-rendering del testo in Photoshop e caricare come livelli rasterizzati le immagini. Sarà più facile e si può formattare il testo esattamente come si desidera._
3. Create il modello in Dynamic Media Classic utilizzando l&#39;editor Template Basics del menu Genera e aggiungete i livelli immagine. Flusso di lavoro testo: Create dei livelli di testo nello stesso editor. Questo passaggio è richiesto quando si crea manualmente un modello in Dynamic Media Classic. Scegliete una dimensione di quadro che corrisponda alla progettazione, trascinate e rilasciate le immagini sul quadro e impostate le proprietà del livello (dimensione, rotazione, opacità, ecc.). Non inserite tutti i livelli possibili nel modello, ma solo un segnaposto per livello immagine. _Flusso di lavoro testo: Per creare dei livelli di testo, usate lo strumento testo, simile a quelli creati in Photoshop. È possibile scegliere un font e formattarlo utilizzando le stesse opzioni disponibili con lo strumento Photoshop Type._ Un altro flusso di lavoro consiste nel caricare un file PSD e nel fare in modo che Dynamic Media Classic generi un modello &quot;gratuito&quot;, e possa anche ricreare i livelli di testo. Questo verrà discusso più dettagliatamente in seguito.
4. Una volta creati i livelli, aggiungete parametri (variabili) a qualsiasi proprietà di qualsiasi livello che desiderate controllare tramite l’URL, inclusa la sorgente del livello (l’immagine stessa). _Flusso di lavoro testo: Potete anche aggiungere parametri ai livelli di testo, sia per controllare il contenuto del testo e le dimensioni e la posizione del livello stesso, sia per tutte le opzioni di formattazione come il colore del font, la dimensione del font, il tracciamento orizzontale, ecc._
5. Create un predefinito per immagini con le stesse dimensioni del modello. A questo scopo, il modello viene sempre chiamato con una dimensione 1:1 e anche per aggiungere la nitidezza a tutti i livelli di immagine grandi che vengono ridimensionati in modo da adattarlo al modello. Se state creando un modello su cui applicare lo zoom, questo passaggio non è necessario.
6. Pubblicate, copiate l’URL dall’anteprima di Dynamic Media Classic e verificatene il funzionamento in un browser.

## Preparazione e caricamento delle risorse dei modelli in Dynamic Media Classic

Prima di caricare le risorse modello in Dynamic Media Classic, dovrete completare alcuni passaggi preparatori.

### Preparazione del file PSD per il caricamento

Prima di caricare il file Photoshop in Dynamic Media Classic, semplificate i livelli in Photoshop per facilitare l’utilizzo e ottenere la massima compatibilità con il server immagini. Il file PSD è spesso costituito da molti elementi che non vengono riconosciuti da Dynamic Media Classic e che possono essere utilizzati anche per la gestione di piccoli elementi. Assicurarsi di salvare un backup del PSD principale nel caso sia necessario modificare in seguito l&#39;originale. La copia semplificata verrà caricata e non la copia principale.

![immagine](assets/basic-templates/basic-templates-2.jpg)

1. Semplificate la struttura del livello unendo/appiattendo i livelli correlati che devono essere attivati/disattivati insieme in un unico livello. Ad esempio, l’etichetta &quot;NEW&quot; e il banner blu vengono uniti in un singolo livello in modo da poterli mostrare o nascondere con un solo clic.
   ![immagine](assets/basic-templates/basic-templates-3.jpg)
2. Alcuni tipi di livelli ed effetti di livello non sono supportati da Dynamic Media Classic o dal server immagini e devono essere rasterizzati prima del caricamento. In caso contrario, gli effetti potrebbero essere ignorati o i livelli eliminati. Per rasterizzare un livello si intende la conversione se non è modificabile. Per rasterizzare gli effetti di livello o i livelli di testo, create un livello vuoto, selezionate entrambi i livelli e unitateli utilizzando **Livelli > Unisci livelli** o CTRL + E/CMD + E.

   - Dynamic Media Classic non può raggruppare o collegare i livelli. Tutti i livelli di un gruppo o di un set collegato verranno convertiti in livelli separati che non sono più raggruppati o collegati.
   - Le maschere di livello verranno convertite in trasparenza al momento del caricamento.
   - I livelli di regolazione non sono supportati e verranno scartati.
   - I livelli di riempimento, ad esempio i livelli Colore pieno, vengono rasterizzati.
   - I livelli Smart Object e i livelli vettoriali verranno rasterizzati in immagini normali al momento del caricamento, mentre i filtri avanzati verranno applicati e rasterizzati.
   - Anche i livelli di testo verranno rasterizzati a meno che non si utilizzi l’opzione Estrai testo (per ulteriori informazioni, vedi sotto).
   - La maggior parte degli effetti livello verranno ignorati e solo alcuni metodi di fusione sono supportati. In caso di dubbi, aggiungete effetti semplici in Dynamic Media Classic (ad esempio ombre interne o esterne, bagliori interni o esterni) oppure usate un livello vuoto per unire e rasterizzare l’effetto in Photoshop.

### Utilizzo dei font

Se è necessario generare testo dinamico, potete anche caricare e pubblicare i font. L’unico font incluso con Dynamic Media Classic è Arial.

È responsabilità di ogni azienda ottenere una licenza per l&#39;utilizzo di un font sul web — se si dispone semplicemente di un font installato sul computer, non si ha il diritto di utilizzarlo commercialmente sul Web, e la società potrebbe essere sottoposta a azioni legali da parte dell&#39;autore del font, se utilizzato senza autorizzazione. Inoltre, i termini di licenza variano — potrebbe essere necessario disporre di licenze separate per la stampa e la visualizzazione dello schermo, ad esempio.

Dynamic Media Classic supporta i font standard OpenType (OTF), TrueType (TTF) e Type 1 Postscript. I font di valigia, i file di raccolta di tipi, i font di sistema di Windows e i font di macchina proprietari (come i font utilizzati da incisioni o macchine per ricamo) non sono supportati. sarà necessario convertirli in uno dei formati di font standard o sostituire un font simile da utilizzare in Dynamic Media Classic e sul server immagini.

Dopo aver caricato i font su Dynamic Media Classic, come qualsiasi altra risorsa, questi devono essere pubblicati anche sul server immagini. Un errore di modello molto comune è quello di dimenticare di pubblicare i font, il che si tradurrà in un errore di immagine — il server immagini non sostituisce un altro font. Inoltre, se desiderate utilizzare l&#39;opzione **Estrai testo** durante il caricamento, dovete caricare i file di font prima di caricare il file PSD che utilizza tali font. La funzione **Estrai testo** tenterà di ricreare il testo come livello di testo modificabile e di inserirlo all&#39;interno di un modello Dynamic Media Classic. Questo argomento viene discusso nell&#39;argomento successivo, Opzioni PSD.

I font hanno più nomi interni che spesso differiscono dal nome del file esterno. Potete visualizzare tutti i loro nomi diversi nella pagina Dettagli per la risorsa in Dynamic Media Classic. Di seguito sono riportati i nomi dei font  Adobe Caslon Pro Semibold, elencati nella scheda Metadati di Dynamic Media Classic:

![immagine](assets/basic-templates/basic-templates-4.jpg)

_Scheda Metadati nella pagina Dettagli per un font in Dynamic Media Classic._

Dynamic Media Classic utilizza come ID risorsa il nome file di questo font (ACaslonPro-Semibold), ma questo non è il nome utilizzato dal modello. Il modello utilizza il nome RTF (Rich Text Format), elencato in basso. RTF è la lingua nativa del motore di testo di Image Server.

Per modificare i font tramite l’URL, è necessario chiamare il nome RTF del font (non l’ID risorsa) oppure si verifica un errore. In questo caso, il nome corretto per questo font sarà &quot; Adobe Caslon Pro&quot;. Per ulteriori informazioni sui font e sul RTF, consulta l’argomento RTF e parametri di testo, di seguito.

I formati di file di font più comuni trovati nei sistemi Windows e Mac sono OpenType e TrueType. OpenType ha un&#39;estensione .OTF, mentre TrueType è .TTF. Entrambi i formati funzionano allo stesso modo in Dynamic Media Classic.

### Selezione delle opzioni per il caricamento del file PSD

non è necessario caricare un file Photoshop (PSD) per creare un modello; in Dynamic Media Classic è possibile creare un modello basato su qualsiasi risorsa immagine. Tuttavia, il caricamento di un file PSD può facilitare l’authoring, poiché in genere queste risorse sono già presenti in un file PSD con più livelli. Inoltre, Dynamic Media Classic genererà automaticamente un modello quando caricate un file PSD con più livelli.

- **Mantieni i livelli.** Questa è l&#39;opzione più importante. Questo indica a Dynamic Media Classic di creare una risorsa immagine per livello Photoshop. Se questa opzione è deselezionata, tutte le altre opzioni vengono disattivate e il file PSD viene appiattito in una singola immagine.
- **** **CreateTemplate.** Questa opzione prende in considerazione i vari livelli generati e crea automaticamente un modello combinandoli di nuovo. L’utilizzo del modello generato automaticamente comporta il posizionamento di tutti i livelli in un unico file da parte di Dynamic Media Classic, mentre è necessario un solo segnaposto per livello. È abbastanza facile eliminare i livelli aggiuntivi, ma se avete molti livelli, è più veloce ricrearli. Assicuratevi di rinominare il nuovo modello; in caso contrario, verrà sovrascritto al successivo caricamento dello stesso file PSD.
- **Estrai testo.** Questo ricrea i livelli di testo nel file PSD come livelli di testo nel modello utilizzando il font caricato. Questo passaggio è richiesto se il testo si trova su un percorso in Photoshop e desiderate mantenere tale percorso nel modello. Questa funzione richiede l&#39;utilizzo dell&#39;opzione **Crea modello**, in quanto il testo estratto può essere creato solo da un modello generato al momento del caricamento.
- **Estende i livelli alle dimensioni dello sfondo.** Con questa impostazione ogni livello ha le stesse dimensioni del quadro PSD complessivo. Questo è molto utile per i livelli che rimarranno sempre fissi in posizione: in caso contrario, quando si scambiano le immagini nello stesso livello, potrebbe essere necessario riposizionarle.
- **Denominazione dei livelli.** Questo indica a Dynamic Media Classic come assegnare un nome a ciascuna risorsa generata per livello. È consigliabile **Photoshop** **e Layer** **Name** oppure Photoshop e **Layer** **Number**. Entrambe le opzioni utilizzano il nome PSD come prima parte del nome e aggiungono il nome o il numero del livello alla fine. Ad esempio, se disponete di un file PSD denominato &quot;shirt.psd&quot; e di livelli denominati &quot;front&quot;, &quot;sleeves&quot; e &quot;collar&quot;, se caricate utilizzando l&#39;opzione **Photoshop e** Layer **Name**, Dynamic Media Classic genererà gli ID delle risorse &quot;shirt_front&quot;,&quot;shirt_maniche&quot; e &quot;collar_shirt&quot;. L’utilizzo di una di queste opzioni garantisce che il nome sia univoco in Dynamic Media Classic.

## Creazione di un modello con livelli immagine

Anche se Dynamic Media Classic è in grado di creare automaticamente un modello da un file PSD a livelli, è necessario essere in grado di creare manualmente il modello. Come spiegato in precedenza, in alcuni casi non si desidera utilizzare il modello creato da Dynamic Media Classic.

### Interfaccia utente di base dei modelli

Per prima cosa, è necessario conoscere l&#39;interfaccia di modifica.

Nel centro a sinistra è collocata l’area di lavoro con un’anteprima del modello finale. Sul lato destro sono presenti i pannelli Livelli e Proprietà livello. Queste sono le aree in cui lavorerete di più.

![immagine](assets/basic-templates/basic-templates-5.jpg)

_Pagina Genera modelli di base._

- **Anteprima/Area di lavoro.** Questa è la finestra principale. Qui è possibile spostare, ridimensionare e ruotare i livelli con il mouse. I contorni dei livelli sono visualizzati come linee tratteggiate.
- **Livelli.** È simile al pannello dei livelli Photoshop. Quando aggiungete dei livelli al modello, questi verranno visualizzati qui. I livelli sono sovrapposti dall’alto verso il basso — il livello superiore nel pannello Livelli viene visualizzato sopra gli altri sotto di esso nell’elenco.
- **Proprietà livello.** Qui è possibile regolare tutte le proprietà di un livello utilizzando controlli numerici. Selezionate prima un livello, quindi regolatene le proprietà.
- **** **CompositeURL.** Nella parte inferiore dell’interfaccia è presente l’area dell’URL composito. Questo non verrà discusso in questa sezione dell’esercitazione, ma qui il modello verrà decostruito come una serie di modificatori URL per il server immagini. Questa area è modificabile — se avete familiarità con i comandi Image Server, potete modificare manualmente il modello qui. Tuttavia è anche possibile romperlo. Come per Photoshop, la numerazione dei livelli inizia da 0. Il Canvas è un livello 0 e il primo livello aggiunto è il livello 1. I metodi di fusione determinano il modo in cui i pixel di un livello si fondono con i pixel sottostanti. Potete creare una serie di effetti speciali utilizzando i metodi di fusione.

#### Utilizzo dell&#39;Editor di base dei modelli

Di seguito sono riportati i passaggi del flusso di lavoro per avviare il modello di base:

1. In Dynamic Media Classic passate a **Genera > Funzioni di base dei modelli**. Potete scegliere se non selezionare nulla oppure iniziare selezionando un’immagine che diventerà il primo livello del modello.
2. Scegliete una dimensione e premete **OK**. Questa dimensione deve corrispondere alla dimensione progettata in Photoshop. Verrà caricato l’editor modelli.
3. Se al passaggio 1 non è stata selezionata un’immagine, cercate o individuate un’immagine nel pannello delle risorse a sinistra e trascinatela nell’area di lavoro.

   - L&#39;immagine viene ridimensionata automaticamente per adattarsi alle dimensioni del quadro. Se intendete sostituire le immagini ad alta risoluzione, in genere inserite una delle immagini P-TIFF grandi (2000 px) e utilizzatela come segnaposto.
   - Questo dovrebbe essere il livello più in basso del modello, ma potete riordinare i livelli in un secondo momento.

4. Ridimensionate o riposizionate il livello direttamente nell’area di lavoro oppure regolando le impostazioni nel pannello Proprietà livello.
5. Trascinate i livelli di immagine desiderati. Se lo desiderate, aggiungete anche gli effetti dei livelli. Vedere l&#39;argomento _Aggiunta di effetti livello_, sotto.
6. Fare clic su **Salva**, scegliere una posizione e assegnare al modello un nome. A questo punto potete visualizzare l’anteprima, ma il modello sarà esattamente come un’immagine Photoshop appiattita — non è ancora modificabile.

### Aggiunta di effetti livello

Image Server supporta alcuni effetti programmatici di livello: effetti speciali che modificano l’aspetto dei contenuti di un livello. Funzionano in modo simile agli effetti di livello in Photoshop. Essi sono collegati a un livello ma controllati indipendentemente da esso. Potete regolarli o rimuoverli senza apportare modifiche permanenti al livello stesso.

- **Ombra** esterna. Applica un’ombra all’esterno dei bordi del livello, posizionata da un offset pixel x e y.
- **Ombra** Interna. Applica un’ombra all’interno dei bordi del livello, posizionata da un offset pixel x e y.
- **Bagliore** esterno. Applica un effetto bagliore uniformemente intorno a tutti i bordi del livello.
- **Bagliore** interno. Applica un effetto bagliore uniformemente all’interno di tutti i bordi del livello.

![immagine](assets/basic-templates/basic-templates-6.jpg)

_Un livello con e senza ombra esterna_

Per aggiungere un effetto, fate clic su **Aggiungi effetto** e scegliete un effetto dal menu. Come per i livelli normali, potete selezionare un effetto nel pannello Livelli e usare il pannello Proprietà livello per regolarne le impostazioni.

Gli effetti ombra vengono scostati orizzontalmente o verticalmente dal livello, mentre gli effetti Bagliore vengono applicati uniformemente in tutte le direzioni. Gli effetti interni agiscono sulla parte opaca del livello, mentre gli effetti esterni agiscono solo sulle aree trasparenti.

Per saperne di più sull&#39;aggiunta di effetti di livello[.](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#using-shadow-and-glow-effects-on-layers)

### Aggiunta di parametri

Se si combinano i livelli e li si salva, il risultato netto non è diverso da un’immagine Photoshop appiattita. Ciò che rende i modelli speciali è la possibilità di aggiungere parametri alle proprietà di ciascun livello, in modo che possano essere modificati in modo dinamico tramite l’URL.

In termini Dynamic Media Classic, un parametro è una variabile che può essere collegata a una proprietà del modello e può quindi essere manipolata tramite un URL. Quando aggiungete un parametro a un livello, Dynamic Media Classic espone tale proprietà nell’URL anteponendo il nome del parametro al simbolo del dollaro ($) — ad esempio, se create un parametro denominato &quot;size&quot; per modificare le dimensioni di un livello, Dynamic Media Classic rinominerà il parametro $size.

Se non aggiungete un parametro per una proprietà, tale proprietà rimane nascosta nel database Dynamic Media Classic e non viene visualizzata nell’URL.

![immagine](assets/basic-templates/parameters.png)

Senza parametri, gli URL sarebbero generalmente molto più lunghi, soprattutto se si utilizza anche testo dinamico. Il testo aggiunge molte dozzine di caratteri aggiuntivi a ciascun URL.

Infine, il set iniziale di parametri diventerà il valore predefinito delle proprietà nel modello. Se create il modello, aggiungete parametri e quindi richiamate l’URL senza i relativi parametri, Image Server creerà l’immagine con tutti i valori predefiniti salvati nel modello. I parametri sono necessari solo se si desidera modificare una proprietà. Se non è necessario modificare una proprietà, non è necessario impostare un parametro.

#### Creazione di parametri

Flusso di lavoro per la creazione di parametri:

1. Fate clic sul pulsante **Parametri** accanto al nome del livello per il quale desiderate creare dei parametri. Viene visualizzata la schermata Parametri. Elenca ciascuna proprietà del livello e il relativo valore.
1. Selezionate l&#39;opzione **On** accanto al nome di ciascuna proprietà da convertire in parametro. Viene visualizzato un nome di parametro predefinito. È possibile aggiungere parametri solo alle proprietà che sono state modificate dallo stato predefinito.

   - Ad esempio, se aggiungete un livello e lo mantenete nella posizione xy predefinita pari a 0,0, Dynamic Media Classic non espone una proprietà **Position**. Per correggere il problema, spostate il livello di almeno un pixel. Ora Dynamic Media Classic espone **Position** come proprietà parametrizzabile.
   - Per aggiungere un parametro alla proprietà show/hide (che attiva e disattiva il livello), fate clic sull&#39;icona **Show** o **Hide Layer** per disattivare il livello (potete riattivarlo se lo desiderate). Dynamic Media Classic ora espone una proprietà **Nascondi** che può essere parametrizzata.

1. Rinominate i nomi dei parametri predefiniti in modo da facilitarne l’identificazione nell’URL. Ad esempio, se desiderate aggiungere un parametro per modificare il livello del banner sopra un’immagine, modificate il nome predefinito &quot;layer_2_src&quot; in &quot;banner&quot;.
1. Premere **Close** per uscire dalla schermata Parametri.
1. Ripetete questa procedura per altri livelli facendo clic sul pulsante **Parametri** e aggiungendo e rinominando i parametri.
1. Al termine, salvate le modifiche.

>[!TIP]
>
>Rinominare i parametri in modo significativo e sviluppare una convenzione di denominazione per standardizzarli. Assicuratevi che la convenzione di denominazione sia approvata in anticipo dai team di progettazione e sviluppo.
>
>Impossibile aggiungere un parametro perché la proprietà non è visibile? Modificate semplicemente la proprietà del livello dal valore predefinito (spostando, ridimensionando, nascondendo, ecc.). È ora necessario visualizzare la proprietà esposta.

Ulteriori informazioni su [Template Parameters](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html).

## Creazione di un modello con livelli di testo

Scoprite come creare un modello di base che includa i livelli di testo.

### Informazioni sul testo dinamico

Ora è possibile creare un modello di base utilizzando i livelli delle immagini. Per molte applicazioni questo è tutto ciò che serve. Come mostrato nell’esercizio precedente, i livelli con testo semplice (come &quot;Vendita&quot; e &quot;Nuovo&quot;) possono essere rasterizzati e trattati come immagini perché il testo non deve essere modificato.

Tuttavia, cosa succede se è necessario:

- Aggiungete un&#39;etichetta con la dicitura &quot;25% disattivato&quot;, con un valore pari al 25% variabile
- Aggiungere un&#39;etichetta di testo con il nome del prodotto sopra l&#39;immagine
- Localizzate i livelli in lingue diverse a seconda del paese in cui è visualizzato il modello

In tal caso, è necessario aggiungere alcuni livelli di testo dinamici con parametri per controllare il testo e/o la formattazione.

Per creare del testo, dovete caricare alcuni font — in caso contrario, l’impostazione predefinita di Dynamic Media Classic è Arial. I font devono essere pubblicati anche sul server immagini, altrimenti si verificherà un errore nel momento in cui tenterà di eseguire il rendering del testo che utilizza tale font.

### Parametri RTF e testo

Per aggiungere variabili al testo con lo strumento Funzioni di base dei modelli, è necessario comprendere il rendering del testo. Image Server genera il testo con il motore di testo  Adobe, lo stesso motore utilizzato da Photoshop e  Illustrator, e lo compone come livello nell’immagine finale. Per comunicare con il motore, Image Server utilizza il formato RTF (Rich Text Format).

RTF è una specifica di formato file sviluppata da Microsoft per specificare la formattazione dei documenti. Si tratta di un linguaggio di markup standard utilizzato dalla maggior parte del software di elaborazione testi ed e-mail. Se si scrive in un URL &amp;text=\b1 Hello, Image Server genera un&#39;immagine con la parola &quot;Hello&quot; in grassetto, perché \b1 è il comando RTF per rendere il testo in grassetto.

La buona notizia è che Dynamic Media Classic genera il RTF per voi. Ogni volta che digitate del testo in un modello e aggiungete della formattazione, Dynamic Media Classic scrive automaticamente il codice RTF nel modello. Il motivo per cui ne parliamo è perché si aggiungeranno parametri direttamente al RTF stesso, quindi è importante che si è un po &#39;familiarità con esso.

#### Creazione di livelli di testo

Potete creare livelli di testo in un modello in Dynamic Media Classic nei due modi seguenti:

1. Strumento Testo in Dynamic Media Classic. Discuteremo di questo metodo di seguito. L’editor Funzioni di base dei modelli dispone di uno strumento che consente di creare una casella di testo, immettere testo e formattare il testo. Dynamic Media Classic genera il RTF come necessario e lo inserisce in un livello separato.
2. Estrai testo (al momento del caricamento). L’altro metodo consiste nel creare il livello di testo in Photoshop e salvarlo nel file PSD come livello di testo normale (invece di rasterizzarlo come livello di immagine). Quindi caricate il file in Dynamic Media Classic e utilizzate l&#39;opzione **Estrai testo**. Dynamic Media Classic converte ciascun livello di testo Photoshop in un livello di testo Image Server utilizzando i comandi RTF. Se utilizzate questo metodo, assicuratevi prima di caricare i font in Dynamic Media Classic. In caso contrario, Dynamic Media Classic sostituirà un font predefinito al momento del caricamento e non esiste un modo semplice per sostituire nuovamente il font corretto.

### Editor di testo

Per immettere il testo, usate l’Editor di testo. L’Editor di testo è un’interfaccia WYSIWYG che consente di immettere e formattare il testo utilizzando controlli di formattazione simili a quelli di Photoshop o  Illustrator.

![immagine](assets/basic-templates/basic-templates-9.jpg)

_Template Basics Text Editor._

La maggior parte del lavoro verrà eseguito nella scheda **Anteprima**, che consente di inserire del testo e visualizzarlo così come apparirà nel modello. È inoltre disponibile una scheda **Origine**, che viene utilizzata per modificare manualmente il RTF, se necessario.

Il flusso di lavoro generale consiste nell&#39;utilizzare la scheda **Anteprima** per digitare del testo.

Selezionate quindi il testo e scegliete una formattazione come il colore del font, la dimensione del font o la giustificazione utilizzando i controlli nella parte superiore. Dopo aver impostato lo stile desiderato, fare clic su **Applica** per visualizzare l&#39;aggiornamento nell&#39;anteprima dell&#39;area di lavoro. Quindi chiudete l’Editor di testo per tornare alla finestra principale Funzioni di base dei modelli.

#### Utilizzo dell’Editor di testo

Di seguito sono illustrati i passaggi del flusso di lavoro per l&#39;aggiunta di testo nella pagina della build Template Basics:

1. Fate clic sul pulsante dello strumento **Testo** nella parte superiore della pagina di generazione.
2. Trascinate fuori una casella di testo in cui desiderate visualizzare il testo. La finestra Editor di testo si apre in una finestra modale. In background, il modello verrà visualizzato, ma non potrà essere modificato finché non avrai completato la modifica del testo.
3. Digitate il testo di esempio da visualizzare al primo caricamento del modello. Ad esempio, se state creando una casella di testo per un&#39;immagine e-mail personalizzata, il testo potrebbe contenere la dicitura &quot;Hi Name&quot;. Ora è il momento di risparmiare!&quot; Successivamente si aggiungeva un parametro di testo per sostituire Nome con un valore inviato sull’URL. Il testo verrà visualizzato nel modello sotto la finestra solo dopo aver fatto clic su **Applica**.
4. Per formattare il testo, selezionatelo trascinandolo con il mouse e scegliete un controllo di formattazione nell’interfaccia utente.

   - Ci sono molte opzioni di formattazione. Tra i font più comuni (faccia), dimensione del font e colore del font, nonché giustificazione sinistra/centro/destra.
   - Non dimenticare di selezionare prima il testo. In caso contrario non sarà possibile applicare alcuna formattazione.
   - Per scegliere un font diverso, selezionate il testo e aprite il menu Font. Nell’editor verrà visualizzato un elenco di tutti i font caricati in Dynamic Media Classic. Se sul computer è installato anche un font, questo viene visualizzato in nero. Se non è installato sul computer, verrà visualizzato in rosso. Tuttavia, sarà comunque eseguito il rendering nella finestra di anteprima quando si fa clic su **Applica**. Per renderli disponibili a chiunque utilizzi Dynamic Media Classic è sufficiente caricare i font. Dopo la pubblicazione, il server immagini utilizzerà tali font per generare il testo — gli utenti non devono installare alcun font per visualizzare il testo creato, perché fa parte di un&#39;immagine.
   - A differenza di Photoshop e  Illustrator, Image Server può allineare il testo verticalmente nella casella di testo. L&#39;impostazione predefinita è l&#39;allineamento superiore. Per modificare questa impostazione, selezionate il testo e scegliete **Medio** o **Inferiore** dal menu **Allineamento verticale**.
   - Se rendete il testo troppo grande per la casella (o la casella di testo è troppo piccola), tutto o parte di esso verrà ritagliato e scompare. Ridurre la dimensione del font o ingrandire la casella.

5. Fare clic su **Applica** per vedere se le modifiche diventano effettive nella finestra dell&#39;area di lavoro. È necessario fare clic su **Applica**, altrimenti si perderanno le modifiche.
6. Al termine, fare clic su **Chiudi**. Se desiderate tornare alla modalità di modifica, fate doppio clic sul livello di testo per riaprire l’Editor di testo.

L&#39;editor di testo visualizza in anteprima esattamente la dimensione del font se il font è installato localmente sul sistema.

### Informazioni sull&#39;aggiunta di parametri ai livelli di testo

Seguiamo ora un processo simile per l’aggiunta di parametri di testo, come per i parametri di livello. I livelli di testo possono inoltre utilizzare parametri di livello per dimensioni, posizione e così via; tuttavia, possono utilizzare parametri aggiuntivi per controllare qualsiasi aspetto del RTF.

A differenza dei parametri dei livelli, potete solo selezionare il valore che desiderate modificare e aggiungere un parametro a tale valore, anziché aggiungere un parametro all’intera proprietà.

![immagine](assets/basic-templates/basic-templates-10.jpg)

Esempio RTF:

![immagine](assets/basic-templates/sample-rtf.png)

Quando si esamina il RTF, è necessario determinare in quale posizione si desidera modificare ciascuna impostazione. Nel RTF precedente, alcuni potrebbero avere senso e potete vedere da dove viene la formattazione.

Potete vedere la frase Sandalo della zecca di cioccolato — questo è il testo stesso.

- C&#39;è un riferimento al font Poor Richard — in questa area sono selezionati i font.
- Potete visualizzare un valore RGB: \red56\green53\blue4 — questo è il colore del testo.
- Anche se la dimensione del font è 20, non viene visualizzato il numero 20. Tuttavia, viene visualizzato un comando \fs40 — per qualche motivo strano, il formato RTF misura i font come mezzo punto. \fs40 è la dimensione del font!

Per creare i parametri sono disponibili informazioni sufficienti, ma nella documentazione Image Serving è disponibile un riferimento completo a tutti i comandi RTF. Visitate la [Documentazione su Image Server](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html#concept-0d3136db7f6f49668274541cd4b6364c).

#### Aggiunta di parametri ai livelli di testo

Seguono i passaggi per aggiungere parametri ai livelli di testo.

1. Fate clic sul pulsante **Parametri** (a &quot;P&quot;) accanto al nome del livello di testo per il quale desiderate creare dei parametri. Viene visualizzata la schermata Parametri. La scheda **Common** elenca ciascuna proprietà del livello e il relativo valore. Qui potete aggiungere parametri di livello normali.
1. Fare clic sulla scheda **Testo**. Qui potete vedere il RTF nella parte superiore; i parametri che aggiungete saranno al di sotto di essi.
1. Per aggiungere un parametro, evidenziate prima il valore da modificare e fate clic sul pulsante **Aggiungi parametro**. Assicurarsi di selezionare solo i valori per i comandi e non l&#39;intero comando stesso. Ad esempio, per impostare un parametro per il nome del font nell&#39;esempio RTF precedente, evidenzierei solo &quot;Poor Richard&quot; e aggiungerei un parametro, ma non il &quot;\f0&quot;. Quando si fa clic su **Aggiungi parametro**, questo viene visualizzato nell&#39;elenco seguente e il valore del parametro viene visualizzato in rosso nel RTF mentre è ancora selezionato. Se devi rimuovere un parametro, fai clic sulla casella di controllo accanto a **On** per disattivare tale parametro e questo scomparirà.
1. Fate clic per rinominare il parametro in un nome più significativo.
1. Al termine, il RTF verrà evidenziato in verde dove esistono parametri e i nomi e i valori dei parametri saranno elencati di seguito.
1. Fare clic su **Chiudi** per uscire dalla schermata Parametri. Premere quindi **Save** , per salvare il modello. Se avete terminato la modifica, premete **Chiudi** per uscire dalla pagina Funzioni di base dei modelli.
1. Fate clic su **Anteprima** per verificare il modello in Dynamic Media Classic. Per verificare i parametri di testo, digitate nuovo testo o nuovi valori nella finestra di anteprima. Per modificare il font, è necessario digitare il nome RTF esatto del font.

>[!TIP]
>
>Per aggiungere parametri al colore del testo, aggiungete separatamente i parametri per rosso, verde e blu. Ad esempio, se il RTF è `\red56\green53\blue46`, per i valori 56, 53 e 46 è possibile aggiungere parametri separati di colore rosso, verde e blu. Nell’URL, il colore viene modificato chiamando tutti e tre: `&$red=56&$green=53&$blue=46`.

Scoprite come [Creare parametri di testo dinamici](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html#creating-dynamic-text-parameters).

## Pubblicazione e creazione degli URL dei modelli

### Creare un predefinito per immagini

La creazione di un predefinito per il modello non è un passaggio obbligatorio. È consigliabile usarlo come procedura ottimale, in modo che il modello venga sempre chiamato con una dimensione 1:1 e che venga applicata la nitidezza a tutti i livelli di immagine grandi che vengono ridimensionati in modo da adattarlo al modello. Se richiamate un’immagine senza un predefinito, il server immagini potrebbe ridimensionare l’immagine in modo arbitrario alle dimensioni predefinite (circa 400 pixel) e non applicherà la nitidezza predefinita.

Un predefinito per immagini per un modello non ha nulla di speciale. Se disponete già di un predefinito per un’immagine statica delle stesse dimensioni, potete usarlo.

### Pubblicazione

Sarà necessario eseguire una pubblicazione per visualizzare le modifiche inviate in diretta sul server immagini. Tieni presente cosa deve essere pubblicato: i vari livelli delle risorse immagine, i font per il testo dinamico e il modello stesso. Come per altre risorse per contenuti multimediali dinamici classici come set di immagini e set 360 gradi, un modello di base è una costruzione artificiale — si tratta di un elemento di riga nel database che fa riferimento alle immagini e ai font utilizzando una serie di comandi Image Server. Quando pubblicate il modello, tutto ciò che state facendo è aggiornare i dati sul server immagini.

Ulteriori informazioni su [Pubblicazione del modello](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/publishing-templates.html).

### Costruzione URL modello

Un modello di base ha la stessa sintassi URL essenziale di una normale chiamata immagine, come spiegato in precedenza. Un modello avrà in genere più modificatori — comandi separati da una e commerciale (&amp;) — come parametri con valori. Tuttavia, la differenza principale sta nel fatto che il modello viene chiamato come immagine principale, invece di richiamare un’immagine statica.

![immagine](assets/basic-templates/basic-templates-11.jpg)

A differenza dei predefiniti per immagini, con un simbolo di dollaro ($) su ciascun lato del nome del predefinito, i parametri hanno un singolo simbolo di dollaro all’inizio. Il posizionamento di questi simboli di dollaro è importante.

**Corretto:**

`$text=46-inch LCD HDTV`

**Scorretto:**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Come indicato in precedenza, i parametri vengono utilizzati per modificare il modello. Se richiamate il modello senza parametri, tornerà alle impostazioni predefinite, come indicato nello strumento di authoring Funzioni di base dei modelli. Se non è necessario modificare una proprietà, non è necessario impostare un parametro.

![](assets/basic-templates/sandals-without-with-parameters.png)
_imageEsempi di un modello senza impostare i parametri (sopra) e con i parametri (sotto)._
