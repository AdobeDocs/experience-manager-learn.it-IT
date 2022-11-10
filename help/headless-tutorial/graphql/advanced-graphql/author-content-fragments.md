---
title: Frammenti di contenuto dell’autore - Concetti avanzati di AEM headless - GraphQL
description: In questo capitolo sui concetti avanzati di Adobe Experience Manager (AEM) Headless, scopri come utilizzare schede, data e ora, oggetti JSON e riferimenti ai frammenti nei frammenti di contenuto. Imposta i criteri delle cartelle per limitare l’inclusione di modelli di frammenti di contenuto .
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 998d3678-7aef-4872-bd62-0e6ea3ff7999
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '2911'
ht-degree: 1%

---

# Frammenti di contenuto dell’autore

In [capitolo precedente](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md), hai creato cinque modelli di frammenti di contenuto: Persona, team, posizione, indirizzo e informazioni di contatto. Questo capitolo illustra i passaggi necessari per creare frammenti di contenuto basati su tali modelli. Esplora inoltre come creare criteri per le cartelle per limitare i modelli di frammenti di contenuto che possono essere utilizzati nella cartella.

## Prerequisiti {#prerequisites}

Questo documento fa parte di un tutorial in più parti. Assicurati che [capitolo precedente](create-content-fragment-models.md) è stato completato prima di procedere con il presente capitolo.

## Obiettivi {#objectives}

In questo capitolo, scopri come:

* Creazione di cartelle e impostazione di limiti tramite i criteri delle cartelle
* Creare riferimenti a frammenti direttamente dall’editor Frammenti di contenuto
* Utilizzare i tipi di dati Tab, Date e Oggetto JSON
* Inserire contenuti e riferimenti a frammenti nell’editor di testo su più righe
* Aggiungere riferimenti a più frammenti
* Nidifica frammenti di contenuto

## Installare il contenuto di esempio {#sample-content}

Installa un pacchetto di AEM che contiene diverse cartelle e immagini di esempio utilizzate per accelerare l&#39;esercitazione.

1. Scarica [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)
1. In AEM, passa a **Strumenti** > **Distribuzione** > **Pacchetti** accesso **Gestione pacchetti**.
1. Carica e installa il pacchetto (file zip) scaricato nel passaggio precedente.

   ![Pacchetto caricato tramite gestione pacchetti](assets/author-content-fragments/install-starter-package.png)

## Creazione di cartelle e impostazione di limiti tramite i criteri delle cartelle

Nella home page di AEM, seleziona **Risorse** > **File** > **WKND condiviso** > **Inglese**. Qui puoi vedere le varie categorie di frammenti di contenuto, compresi Avventure e Collaboratori.

### Creare cartelle {#create-folders}

Passa a **Avventure** cartella. Puoi vedere che le cartelle per Team e Posizioni sono già state create per memorizzare i Frammenti di contenuto di Team e Posizioni.

Crea una cartella per i frammenti di contenuto di istruttori basati sul modello di frammento di contenuto personale.

1. Dalla pagina Avventure, seleziona **Crea** > **Cartella** nell&#39;angolo in alto a destra.

   ![Crea cartella](assets/author-content-fragments/create-folder.png)

1. Nella finestra modale Crea cartella visualizzata, immetti &quot;Istruttori&quot; nel **Titolo** campo . Osserva i &#39;s&#39; alla fine. I titoli delle cartelle contenenti molti frammenti devono essere plurali. Seleziona **Crea**.

   ![Finestra modale per la creazione di cartelle](assets/author-content-fragments/create-folder-modal.png)

   Ora hai creato una cartella per memorizzare gli istruttori di Avventura.

### Impostare i limiti utilizzando i criteri delle cartelle

AEM consente di definire autorizzazioni e criteri per le cartelle Frammento di contenuto. Utilizzando le autorizzazioni, è possibile concedere l’accesso a determinate cartelle solo a determinati utenti (autori) o gruppi di autori. Utilizzando i criteri delle cartelle, puoi limitare ciò che gli autori di modelli di frammento di contenuto possono utilizzare in tali cartelle. In questo esempio, limitiamo una cartella ai modelli Persona e Informazioni di contatto. Per configurare un criterio per le cartelle:

1. Seleziona la **Istruttori** cartella creata, quindi selezionare **Proprietà** dalla barra di navigazione superiore.

   ![Proprietà](assets/author-content-fragments/properties.png)

1. Seleziona la **Criteri** , quindi deseleziona **Ereditato da /content/dam/wknd-shared**. In **Modelli di frammenti di contenuto consentiti per percorso** selezionare l&#39;icona della cartella.

   ![Icona della cartella](assets/author-content-fragments/folder-icon.png)

1. Nella finestra di dialogo Seleziona percorso visualizzata, segui il percorso **conf** > **WKND condiviso**. Il modello per frammento di contenuto personale, creato nel capitolo precedente, contiene un riferimento al modello per frammento di contenuto di informazioni di contatto. Per creare un frammento di contenuto istruttore, è necessario consentire i modelli Persona e Informazioni contatto nella cartella Istruttori. Seleziona **Persona** e **Informazioni contatto**, quindi premi **Seleziona** per chiudere la finestra di dialogo.

   ![Seleziona percorso](assets/author-content-fragments/select-path.png)

1. Seleziona **Salva e chiudi** e seleziona **OK** nella finestra di dialogo di successo visualizzata.

1. È stato ora configurato un criterio per la cartella Istruttori. Passa a **Istruttori** e seleziona **Crea** > **Frammento di contenuto**. Gli unici modelli che è ora possibile selezionare sono **Persona** e **Informazioni contatto**.

   ![Criteri per cartelle](assets/author-content-fragments/folder-policies.png)

## Frammenti di contenuto per istruttori

Passa a **Istruttori** cartella. Da qui, creiamo una cartella nidificata per memorizzare le informazioni di contatto degli Istruttori.

Segui i passaggi descritti nella sezione [creazione di cartelle](#create-folders) per creare una cartella denominata &quot;Informazioni di contatto&quot;. La cartella nidificata eredita i criteri della cartella principale. Puoi configurare criteri più specifici in modo che la nuova cartella creata consenta solo l’utilizzo del modello Informazioni contatto .

### Creare un frammento di contenuto per istruttori

Creiamo quattro persone che possono essere aggiunte a un team di istruttori di Avventura.

1. Dalla cartella Istruttori, crea un frammento di contenuto basato sul modello di frammento di contenuto personale e assegnagli il titolo &quot;Jacob Wester&quot;.

   Il frammento di contenuto appena creato si presenta come segue:

   ![Frammento di contenuto della persona](assets/author-content-fragments/person-content-fragment.png)

1. Inserisci il seguente contenuto nei campi:

   * **Nome completo**: Jacob Wester
   * **Biografia**: Jacob Wester è stato un istruttore di trekking per dieci anni e ha amato ogni minuto di esso! Jacob è un cercatore d&#39;avventura con un talento per arrampicarsi su roccia e fare il backpack. Jacob è il vincitore di gare di arrampicata, tra cui la battaglia del boulder della baia. Jacob attualmente vive in California.
   * **Livello esperienza istruttore**: Esperto
   * **Competenze**: Arrampicata, Surf, Backpackaging
   * **Dettagli amministratore**: Jacob Wester coordina da tre anni le avventure di backpackaging.

1. In **Immagine profilo** aggiungi un riferimento al contenuto a un&#39;immagine. Sfoglia per **WKND condiviso** > **Inglese** > **Collaboratori** > **jacob_wester.jpg** per creare un percorso per l&#39;immagine.

### Creare un riferimento a un frammento dall’editor Frammenti di contenuto {#fragment-reference-from-editor}

AEM consente di creare un riferimento a un frammento direttamente dall’editor Frammento di contenuto. Creiamo un riferimento alle informazioni di contatto di Jacob.

1. Seleziona **Nuovo frammento di contenuto** sotto **Informazioni contatto** campo .

   ![Nuovo frammento di contenuto](assets/author-content-fragments/new-content-fragment.png)

1. Viene visualizzata la finestra modale Nuovo frammento di contenuto . Sotto la scheda Seleziona destinazione , segui il percorso **Avventure** > **Istruttori** e seleziona la casella di controllo accanto alla **Informazioni contatto** cartella. Seleziona **Successivo** per passare alla scheda Proprietà .

   ![Finestra modale Nuovo frammento di contenuto](assets/author-content-fragments/new-content-fragment-modal.png)

1. Nella scheda Proprietà, immetti &quot;Jacob Wester Contact Info&quot; nel campo **Titolo** campo . Seleziona **Crea**, quindi premi **Apri** nella finestra di dialogo di successo visualizzata.

   ![Scheda Proprietà](assets/author-content-fragments/properties-tab.png)

   Vengono visualizzati nuovi campi che consentono di modificare il frammento di contenuto delle informazioni di contatto.

   ![Frammento di contenuto delle informazioni di contatto](assets/author-content-fragments/contact-info-content-fragment.png)

1. Inserisci il seguente contenuto nei campi:

   * **Telefono**: 2009-888-0000
   * **E-mail**: jwester@wknd.com

   Al termine, seleziona **Salva**. È stato creato un frammento di contenuto Informazioni contatto.

1. Per tornare al frammento di contenuto dell’istruttore, seleziona **Jacob Wester** nell’angolo in alto a sinistra dell’editor.

   ![Passa al frammento di contenuto originale](assets/author-content-fragments/back-to-jacob-wester.png)

   La **Informazioni contatto** Il campo contiene ora il percorso del frammento Contact Info a cui si fa riferimento. Riferimento a un frammento nidificato. Il frammento di contenuto dell’istruttore completato si presenta così:

   ![Frammento di contenuto Jacob Wester](assets/author-content-fragments/jacob-wester-content-fragment.png)

1. Seleziona **Salva e chiudi** per salvare il frammento di contenuto. È ora disponibile un nuovo frammento di contenuto per istruttore.

### Creazione di ulteriori frammenti

Segui lo stesso processo descritto nel [sezione precedente](#fragment-reference-from-editor) creare altri tre frammenti di contenuto per istruttori e tre frammenti di contenuto per questi istruttori. Aggiungi il seguente contenuto nei frammenti Istruttori:

**Stacey Roswells**

| Campi | Valori |
| --- | --- |
| Titolo frammento di contenuto | Stacey Roswells |
| Nome e cognome | Stacey Roswells |
| Informazioni di contatto | /content/dam/wknd-shared/en/adventures/istrtors/contact-info/stacey-roswells-contact-info |
| Immagine profilo | /content/dam/wknd-shared/en/contributors/stacey-roswells.jpg |
| Biografia | Stacey Roswells è un esperto arrampicatore e avventuriero alpino. Nato a Baltimora, nel Maryland, Stacey è il più giovane di sei bambini. Il padre di Stacey era un tenente colonnello della marina militare americana e la madre era un moderno istruttore di danza. La famiglia di Stacey si trasferì frequentemente con i compiti del padre, e scattò le prime foto quando il padre era in residenza in Thailandia. Ed è qui che Stacey ha imparato a arrampicarsi. |
| Livello esperienza istruttore | Avanzate  |
| Competenze | Arrampicata | Sci | Backpackaging |

**Kumar Selvaraj**

| Campi | Valori |
| --- | --- |
| Titolo frammento di contenuto | Kumar Selvaraj |
| Nome e cognome | Kumar Selvaraj |
| Informazioni di contatto | /content/dam/wknd-shared/en/adventures/istrtors/contact-info/kumar-selvaraj-contact-info |
| Immagine profilo | /content/dam/wknd-shared/en/contributors/kumar-selvaraj.jpg |
| Biografia | Kumar Selvaraj è un istruttore professionale certificato AMGA esperto il cui obiettivo principale è quello di aiutare gli studenti a migliorare le loro abilità di arrampicata e trekking. |
| Livello esperienza istruttore | Avanzate  |
| Competenze | Arrampicata | Backpackaging |

**Ayo Ogunseinde**

| Campi | Valori |
| --- | --- |
| Titolo frammento di contenuto | Ayo Ogunseinde |
| Nome e cognome | Ayo Ogunseinde |
| Informazioni di contatto | /content/dam/wknd-shared/en/adventures/istrtors/contact-info/ayo-ogunseinde-contact-info |
| Immagine profilo | /content/dam/wknd-shared/en/contributors/ayo-ogunseinde-237739.jpg |
| Biografia | Ayo Ogunseinde è un alpinista professionista e istruttore di zaino che vive a Fresno, California Centrale. L&#39;obiettivo di Ayo è guidare gli escursionisti nelle loro avventure più epiche del parco nazionale. |
| Livello esperienza istruttore | Avanzate  |
| Competenze | Arrampicata | Ciclismo | Backpackaging |

Lascia la **Informazioni aggiuntive** campo vuoto.

Aggiungere le seguenti informazioni nei frammenti Informazioni contatto:

| Titolo frammento di contenuto | Telefono | E-mail |
| ------- | -------- | -------- |
| Stacey Roswells Informazioni di contatto | 2009-888-0011 | sroswells@wknd.com |
| Kumar Selvaraj Informazioni di contatto | 2009-888-0002 | kselvaraj@wknd.com |
| Ayo Ogunseinde Contatti | 2009-888-0304 | aogunseinde@wknd.com |

Ora puoi creare un team!

## Frammenti di contenuto dell’autore per le posizioni

Passa a **Posizioni** cartella. Qui sono presenti due cartelle nidificate già create: Parco Nazionale dello Yosemite e Yosemite Valley Lodge.

![Cartella Posizioni](assets/author-content-fragments/locations-folder.png)

Ignora la cartella Yosemite Valley Lodge per il momento. Ritorniamo a questa sezione più avanti quando creiamo una posizione che funge da base per il nostro team di istruttori.

Passa a **Parco nazionale dello Yosemite** cartella. Attualmente, contiene solo una foto del Parco Nazionale dello Yosemite. Creiamo un frammento di contenuto utilizzando il modello di frammento di contenuto della posizione e titoliamolo &quot;Parco nazionale dello Yosemite&quot;.

### Segnaposto Tabulazione

AEM consente di utilizzare segnaposto per schede per raggruppare diversi tipi di contenuto e semplificare la lettura e la gestione dei frammenti di contenuto. Nel capitolo precedente, sono stati aggiunti segnaposto tabulazione al modello Posizione. Di conseguenza, il frammento di contenuto posizione dispone ora di due sezioni a schede: **Dettagli posizione** e **Indirizzo**.

![Segnaposto per tabulazione](assets/author-content-fragments/tabs.png)

La **Dettagli posizione** la scheda contiene **Nome**, **Descrizione**, **Informazioni contatto**, **Immagine posizione** e **Meteo per stagione** campi, mentre **Indirizzo** contiene un riferimento a un frammento di contenuto indirizzo. Le schede consentono di definire con chiarezza quali tipi di contenuto devono essere compilati, facilitando la gestione dell’authoring dei contenuti.

### Tipo di dati oggetto JSON

La **Meteo per stagione** è un tipo di dati oggetto JSON, il che significa che accetta dati in formato JSON. Questo tipo di dati è flessibile e può essere utilizzato per tutti i dati che desideri includere nel contenuto.

Puoi visualizzare la descrizione del campo creata nel capitolo precedente passando il cursore sull’icona delle informazioni a destra del campo.

![Icona di informazioni oggetto JSON](assets/author-content-fragments/json-object-info.png)

In questo caso, dobbiamo fornire il tempo medio per la posizione. Immetti i seguenti dati:

```json
{
    "summer": "81 / 89°F",
    "fall": "56 / 83°F",
    "winter": "46 / 51°F",
    "spring": "57 / 71°F"
}
```

La **Meteo per stagione** Il campo dovrebbe ora essere simile al seguente:

![Oggetto JSON](assets/author-content-fragments/json-object.png)

### Aggiungi contenuto

Aggiungiamo il resto del contenuto al frammento di contenuto Posizione per eseguire una query sulle informazioni con GraphQL nel capitolo successivo.

1. In **Dettagli posizione** immettere le seguenti informazioni nei campi:

   * **Nome**: Parco nazionale dello Yosemite
   * **Descrizione**: Il parco nazionale dello Yosemite si trova sulle montagne della Sierra Nevada della California. È famosa per le sue splendide cascate, gli alberi giganti di sequoia, e vista iconica delle scogliere El Capitan e Mezza Dome. Escursioni e campeggio sono i migliori modi per sperimentare Yosemite. Numerosi sentieri offrono infinite opportunità di avventura e esplorazione.

1. Da **Informazioni contatto** creare un frammento di contenuto basato sul modello Contact Info e denominarlo &quot;Yosemite National Park Contact Info&quot;. Segui lo stesso processo descritto nella sezione precedente su [creazione di un riferimento a un frammento dall’editor](#fragment-reference-from-editor) e inserire i seguenti dati nei campi:

   * **Telefono**: 2009-999-0000
   * **E-mail**: yosemite@wknd.com

1. Da **Immagine posizione** campo , cerca **Avventure** > **Posizioni** > **Parco nazionale dello Yosemite** > **yosemite-national-park.jpeg** per creare un percorso per l&#39;immagine.

   Nel capitolo precedente in cui è stata configurata la convalida dell&#39;immagine, le dimensioni dell&#39;immagine della posizione devono essere inferiori a 2560 x 1800 e le dimensioni del file devono essere inferiori a 3 MB.

1. Con tutte le informazioni aggiunte, il **Dettagli posizione** La scheda ora si presenta così:

   ![Scheda Dettagli posizione completata](assets/author-content-fragments/location-details-tab-completed.png)

1. Passa a **Indirizzo** scheda . Da **Indirizzo** creare un frammento di contenuto denominato &quot;Indirizzo del parco nazionale di Yosemite&quot; utilizzando il modello del frammento di contenuto di indirizzo creato nel capitolo precedente. Segui lo stesso processo descritto nella sezione su [creazione di un riferimento a un frammento dall’editor](#fragment-reference-from-editor) e inserire i seguenti dati nei campi:

   * **Indirizzo via**: 9010 Curry Village Drive
   * **Città**: Valle dello Yosemite
   * **Stato**: CA
   * **CAP**: 95389
   * **Paese**: Stati Uniti

1. Completato **Indirizzo** scheda del frammento del Parco Nazionale dello Yosemite si presenta così:

   ![Scheda Indirizzo posizione completata](assets/author-content-fragments/location-address-tab-completed.png)

1. Seleziona **Salva e chiudi**.

### Creare un altro frammento

1. Passa a **Yosemite Valley Lodge** cartella. Crea un frammento di contenuto utilizzando il modello di frammento di contenuto della posizione e assegnagli il titolo &quot;Yosemite Valley Lodge&quot;.

1. In **Dettagli posizione** immettere le seguenti informazioni nei campi:

   * **Nome**: Yosemite Valley Lodge
   * **Descrizione**: Yosemite Valley Lodge è un hub per riunioni di gruppo e ogni tipo di attività, come lo shopping, la ristorazione, la pesca, l&#39;escursionismo, e molti altri.

1. Da **Informazioni contatto** creare un frammento di contenuto basato sul modello Contact Info e denominarlo &quot;Yosemite Valley Lodge Contact Info&quot;. Segui lo stesso processo descritto nella sezione su [creazione di un riferimento a un frammento dall’editor](#fragment-reference-from-editor) e immetti i seguenti dati nei campi del nuovo frammento di contenuto:

   * **Telefono**: 2009-992-0000
   * **E-mail**: yosemitelodge@wknd.com

   Salva il frammento di contenuto appena creato.

1. Torna a **Yosemite Valley Lodge** e vai al **Indirizzo** scheda . Da **Indirizzo** creare un frammento di contenuto denominato &quot;Indirizzo del lodge della valle di Yosemite&quot; utilizzando il modello del frammento di contenuto dell&#39;indirizzo creato nel capitolo precedente. Segui lo stesso processo descritto nella sezione su [creazione di un riferimento a un frammento dall’editor](#fragment-reference-from-editor) e inserire i seguenti dati nei campi:

   * **Indirizzo via**: 9006 Yosemite Lodge Drive
   * **Città**: Parco nazionale dello Yosemite
   * **Stato**: CA
   * **CAP**: 95389
   * **Paese**: Stati Uniti

   Salva il frammento di contenuto appena creato.

1. Torna a **Yosemite Valley Lodge**, quindi seleziona **Salva e chiudi**. La **Yosemite Valley Lodge** La cartella contiene ora tre frammenti di contenuto: Yosemite Valley Lodge, Yosemite Valley Lodge Contatti Info e Yosemite Valley Lodge Indirizzo.

   ![Cartella Lodge Yosemite Valley](assets/author-content-fragments/yosemite-valley-lodge-folder.png)

## Creazione di un frammento di contenuto del team

Sfoglia cartelle in **Team** > **Team Yosemite**. La cartella Team Yosemite contiene attualmente solo il logo del team.

![Cartella Team Yosemite](assets/author-content-fragments/yosemite-team-folder.png)

Creiamo un frammento di contenuto utilizzando il modello di frammento di contenuto del team e assegnargli il titolo &quot;Team Yosemite&quot;.

### Riferimenti a contenuti e frammenti nell’editor di testo su più righe

AEM consente di aggiungere contenuti e riferimenti a frammenti direttamente nell’editor di testo su più righe e di recuperarli in un secondo momento utilizzando le query GraphQL. Aggiungiamo sia riferimenti al contenuto che a frammenti nel **Descrizione** campo .

1. Innanzitutto, aggiungi il seguente testo nel **Descrizione** campo: &quot;Il team di avventurieri professionisti e istruttori di trekking che lavorano nel parco nazionale Yosemite.&quot;

1. Per aggiungere un riferimento al contenuto, seleziona la **Inserisci risorsa** nella barra degli strumenti dell’editor di testo su più righe.

   ![Icona Inserisci risorsa](assets/author-content-fragments/insert-asset-icon.png)

1. Nel modale visualizzato, seleziona **team-yosemite-logo.png** e premere **Seleziona**.

   ![Seleziona immagine](assets/author-content-fragments/select-image.png)

   Il riferimento al contenuto viene ora aggiunto nel **Descrizione** campo .

Tenere presente che nel capitolo precedente è stato consentito l’aggiunta di riferimenti a frammenti al **Descrizione** campo . Aggiungiamo uno qui.

1. Seleziona la **Inserisci frammento di contenuto** nella barra degli strumenti dell’editor di testo su più righe.

   ![Icona Inserisci frammento di contenuto](assets/author-content-fragments/insert-content-fragment-icon.png)

1. Sfoglia per **WKND condiviso** > **Inglese** > **Avventure** > **Posizioni** > **Yosemite Valley Lodge** > **Yosemite Valley Lodge**. Press **Seleziona** per inserire il frammento di contenuto.

   ![Finestra modale Inserisci frammento di contenuto](assets/author-content-fragments/insert-content-fragment-modal.png)

   La **Descrizione** Il campo è ora simile al seguente:

   ![Campo descrizione](assets/author-content-fragments/description-field.png)

Ora sono stati aggiunti i riferimenti al contenuto e al frammento direttamente nell’editor di testo su più righe.

### Tipo di dati data e ora

Diamo un’occhiata al tipo di dati Data e ora. Seleziona la **Calendario** sul lato destro del **Data di fondazione del team** per aprire la visualizzazione calendario.

![Vista calendario data](assets/author-content-fragments/date-calendar-view.png)

Le date passate o future possono essere impostate utilizzando le frecce avanti e indietro su entrambi i lati del mese. Supponiamo che il team Yosemite sia stato fondato il 24 maggio 2016, quindi stabiliremo la data per allora.

### Aggiungere riferimenti a più frammenti

Aggiungiamo gli istruttori al riferimento al frammento Membri del team.

1. Seleziona **Aggiungi** in **Membri del team** campo .

   ![Pulsante Aggiungi](assets/author-content-fragments/add-button.png)

1. Nel nuovo campo visualizzato, seleziona l’icona della cartella per aprire il modale Seleziona percorso . Sfoglia le cartelle in **WKND condiviso** > **Inglese** > **Avventure** > **Istruttori**, quindi seleziona la casella di controllo accanto a **giacobo**. Press **Seleziona** per salvare il percorso.

   ![Percorso riferimento frammento](assets/author-content-fragments/fragment-reference-path.png)

1. Seleziona la **Aggiungi** pulsante altre tre volte. Utilizza i nuovi campi per aggiungere i tre istruttori rimanenti al team. La **Membri del team** il campo ora si presenta così:

   ![Campo membri del team](assets/author-content-fragments/team-members-field.png)

1. Seleziona **Salva e chiudi** per salvare il frammento di contenuto del team.

### Aggiungere riferimenti a un frammento di contenuto avventura

Infine, aggiungiamo i frammenti di contenuto appena creati a un’avventura.

1. Passa a **Avventure** > **Yosemite Backpack** e aprire il frammento di contenuto del backpack Yosemite. Nella parte inferiore del modulo sono visualizzati i tre campi creati nel capitolo precedente: **Posizione**, **Team istruttore** e **Amministratore**.

1. Aggiungi il riferimento al frammento nel **Posizione** campo . Il percorso della posizione deve fare riferimento al frammento di contenuto del parco nazionale di Yosemite creato: `/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park`.

1. Aggiungi il riferimento al frammento nel **Team istruttore** campo . Il percorso Team deve fare riferimento al frammento di contenuto del team Yosemite creato: `/content/dam/wknd-shared/en/adventures/teams/yosemite-team/yosemite-team`. Riferimento a un frammento nidificato. Il frammento di contenuto del team contiene un riferimento al modello Persona che fa riferimento ai modelli Informazioni di contatto e Indirizzo. Di conseguenza, i frammenti di contenuto nidificati sono a tre livelli in basso.

1. Aggiungi il riferimento al frammento nel **Amministratore** campo . Diciamo che Jacob Wester è un amministratore per la Yosemite Backpackaging Avventure. Il percorso deve portare al frammento di contenuto di Jacob Wester e apparire come segue: `/content/dam/wknd-shared/en/adventures/instructors/jacob-wester`.

1. Ora hai aggiunto tre riferimenti di frammento a un frammento di contenuto avventura. I campi si presentano così:

   ![Riferimenti al frammento di avventura](assets/author-content-fragments/adventure-fragment-references.png)

1. Seleziona **Salva e chiudi** per salvare il contenuto.

## Congratulazioni. 

Congratulazioni. Ora hai creato frammenti di contenuto basati sui modelli avanzati di frammenti di contenuto creati nel capitolo precedente. È stato inoltre creato un criterio per le cartelle per limitare la selezione di modelli di frammenti di contenuto all’interno di una cartella.

## Passaggi successivi

In [capitolo successivo](/help/headless-tutorial/graphql/advanced-graphql/explore-graphql-api.md), informazioni sull’invio di query GraphQL avanzate tramite l’ambiente di sviluppo integrato (IDE) GraphiQL. Queste query ci consentono di visualizzare i dati creati in questo capitolo e di aggiungere tali query all’app WKND.
