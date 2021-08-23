---
title: Modifica del contenuto dell’autore e pubblicazione
seo-title: Guida introduttiva ad AEM Sites - Modifica del contenuto di authoring e pubblicazione
description: Utilizza l’editor pagina in Adobe Experience Manager, AEM, per aggiornare il contenuto del sito web. Scopri come i componenti vengono utilizzati per facilitare l’authoring. Scopri la differenza tra un ambiente di authoring e uno di pubblicazione AEM e come pubblicare le modifiche al sito live.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gestione dei contenuti
feature: Componenti core, Editor pagina
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 2%

---


# Modifica del contenuto dell’autore e pubblicazione {#author-content-publish}

>[!CAUTION]
>
> Le funzionalità di creazione rapida dei siti mostrate qui saranno rilasciate nella seconda metà del 2021. La relativa documentazione è disponibile a scopo di anteprima.

È importante comprendere in che modo un utente aggiorna il contenuto del sito web. In questo capitolo adotteremo la persona di un **Autore di contenuti** e apporteremo alcuni aggiornamenti editoriali al sito generato nel capitolo precedente. Alla fine del capitolo, pubblicheremo le modifiche per comprendere come viene aggiornato il sito live.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che i passaggi descritti nel capitolo [Crea un sito](./create-site.md) siano stati completati.

## Obiettivo {#objective}

1. Comprendi i concetti di **Pagine** e **Componenti** in AEM Sites.
1. Scopri come aggiornare il contenuto del sito web.
1. Scopri come pubblicare le modifiche al sito live.

## Creare una nuova pagina {#create-page}

In genere, un sito web viene suddiviso in pagine per creare un’esperienza multipagina. AEM struttura il contenuto allo stesso modo. Quindi, crea una nuova pagina per il sito.

1. Accedi al AEM **Author** Service utilizzato nel capitolo precedente.
1. Dalla schermata iniziale AEM fare clic su **Siti** > **Sito WKND** > **Inglese** > **Articolo**
1. Nell&#39;angolo in alto a destra fai clic su **Crea** > **Pagina**.

   ![Crea pagina](assets/author-content-publish/create-page-button.png)

   Verrà visualizzata la procedura guidata **Crea pagina** .

1. Scegli il modello **Pagina articolo** e fai clic su **Avanti**.

   Le pagine in AEM vengono create in base a un modello di pagina. I modelli di pagina saranno illustrati più dettagliatamente nel capitolo [Modelli di pagina](page-templates.md) .

1. Alla voce **Proprietà** immetti un **Titolo** di &quot;Ciao a tutti&quot;.
1. Imposta **Nome** su `hello-world` e fai clic su **Crea**.

   ![Proprietà pagina iniziale](assets/author-content-publish/initial-page-properties.png)

1. Nella finestra di dialogo a comparsa, fai clic su **Apri** per aprire la pagina appena creata.

## Creare un componente {#author-component}

I componenti AEM possono essere considerati come piccoli blocchi modulari di una pagina web. Suddividendo l’interfaccia utente in blocchi logici o componenti, diventa molto più semplice da gestire. Per riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita tramite la finestra di dialogo di authoring.

AEM fornisce un set di [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) che sono pronti per l&#39;uso per la produzione. I **Componenti core** variano da elementi di base come [Testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) e [Immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) a elementi di interfaccia utente più complessi come un [Carosello](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

Ora, creiamo alcuni componenti utilizzando AEM Editor pagina.

1. Passa alla pagina **Hello World** creata nell&#39;esercizio precedente.
1. Assicurati di essere in modalità **Modifica** e nella barra laterale sinistra fai clic sull&#39;icona **Componenti** .

   ![Barra laterale dell’editor pagina](assets/author-content-publish/page-editor-siderail.png)

   Viene aperta la libreria Componenti ed è riportato l’elenco dei Componenti disponibili utilizzabili nella pagina.

1. Scorri verso il basso e **Trascina+Rilascia** un componente **Testo (v2)** sull’area modificabile principale della pagina.

   ![Trascina e rilascia il componente testo](assets/author-content-publish/drag-drop-text-cmp.png)

1. Fai clic sul componente **Testo** per evidenziare, quindi fai clic sull&#39;icona **Chiave** ![Icona chiave](assets/author-content-publish/wrench-icon.png) per aprire la finestra di dialogo del componente. Inserisci del testo e salva le modifiche apportate alla finestra di dialogo.

   ![Componente Rich Text](assets/author-content-publish/rich-text-populated-component.png)

   Il componente **Testo** deve ora visualizzare il testo RTF nella pagina.

1. Ripeti i passaggi precedenti, eccetto trascina sulla pagina un’istanza del componente **Immagine(v2)** . Apri la finestra di dialogo del componente **Immagine** .

1. Nella barra a sinistra, passa a **Asset Finder** facendo clic sull&#39;icona **Risorse** ![icona risorsa](assets/author-content-publish/asset-icon.png).
1. **Trascina l’immagine** Dropan nella finestra di dialogo del componente e fai clic su  **** Non per salvare le modifiche.

   ![Aggiungi risorsa alla finestra di dialogo](assets/author-content-publish/add-asset-dialog.png)

1. Osserva che sono presenti componenti sulla pagina, come **Titolo**, **Navigazione**, **Ricerca** che sono fissi. Queste aree sono configurate come parte del Modello di pagina e non possono essere modificate su una singola pagina. Questo aspetto verrà approfondito nel prossimo capitolo.

Sentitevi liberi di sperimentare con alcuni degli altri componenti. La documentazione relativa a ciascun [componente di base è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Una serie video dettagliata sull’ [authoring delle pagine è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Aggiornamenti alla pubblicazione {#publish-updates}

Gli ambienti AEM sono suddivisi tra **Author Service** e **Publish Service**. In questo capitolo sono state apportate diverse modifiche al sito sul **Servizio di authoring**. Per consentire ai visitatori del sito di visualizzare le modifiche, è necessario pubblicarle nel **Servizio di pubblicazione**.

![Diagramma di alto livello](assets/author-content-publish/author-publish-high-level-flow.png)

*Flusso di contenuti di alto livello da Author a Publish*

**1.** Gli autori dei contenuti apportano aggiornamenti al contenuto del sito. Gli aggiornamenti possono essere visualizzati in anteprima, rivisti e approvati per essere inviati in diretta.

**2.** Il contenuto è stato pubblicato. La pubblicazione può essere eseguita su richiesta o programmata per una data futura.

**3.** I visitatori del sito vedranno le modifiche che si rifletteranno sul servizio Publish.

### Pubblicare le modifiche

Quindi, pubblichiamo le modifiche.

1. Dalla schermata iniziale AEM passare a **Sites** e selezionare il **sito WKND**.
1. Fai clic su **Gestisci pubblicazione** nella barra dei menu.

   ![Gestisci pubblicazione](assets/author-content-publish/click-manage-publiciation.png)

   Poiché si tratta di un nuovo sito, vogliamo pubblicare tutte le pagine e possiamo utilizzare la procedura guidata Gestisci pubblicazione per definire esattamente cosa deve essere pubblicato.

1. In **Opzioni** lasciare le impostazioni predefinite su **Pubblica** e pianificarle per **Ora**. Fai clic su **Avanti**.
1. In **Ambito**, seleziona il **Sito WKND** e fai clic su **Includi elementi figlio**. Nella finestra di dialogo, deseleziona tutte le caselle. Vogliamo pubblicare il sito completo.

   ![Aggiorna ambito di pubblicazione](assets/author-content-publish/update-scope-publish.png)

1. Fai clic sul pulsante **Riferimenti pubblicati** . Nella finestra di dialogo, verifica che tutto sia controllato. Questo include il **Modello di sito di base AEM** e diverse configurazioni generate dal modello di sito. Fai clic su **Fine** per aggiornare.

   ![Pubblicare riferimenti](assets/author-content-publish/publish-references.png)

1. Infine, fai clic su **Pubblica** nell’angolo in alto a destra per pubblicare il contenuto.

## Visualizza contenuto pubblicato {#publish}

Quindi, accedi al servizio Publish per visualizzare le modifiche.

1. Un modo semplice per ottenere l’URL del servizio di pubblicazione è copiare l’URL dell’autore e sostituire la parola `author` con `publish`. Esempio:

   * **URL autore** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL di pubblicazione**  -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Aggiungi `/content/wknd.html` all’URL di pubblicazione in modo che l’URL finale sia simile a: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Se hai fornito un nome univoco durante la [creazione del sito](create-site.md), modifica `wknd.html` in modo che corrisponda al nome del sito.

1. Passando all’URL di pubblicazione, il sito dovrebbe essere visualizzato senza alcuna funzionalità di authoring AEM.

   ![Sito pubblicato](assets/author-content-publish/publish-url-update.png)

1. Utilizzando il menu **Navigazione** fai clic su **Articolo** > **Ciao a tutti** per passare alla pagina Ciao a tutti creata in precedenza.
1. Torna a **AEM Author Service** e apporta alcune modifiche aggiuntive al contenuto nell’Editor pagina.
1. Pubblica queste modifiche direttamente dall’interno dell’editor pagina facendo clic sull’icona **Proprietà pagina** > **Pubblica pagina**

   ![pubblicare direttamente](assets/author-content-publish/page-editor-publish.png)

1. Torna a **AEM Publish Service** per visualizzare le modifiche. Probabilmente gli aggiornamenti verranno visualizzati immediatamente da **not** . Questo perché il **servizio di pubblicazione AEM** include la memorizzazione in cache tramite un server web Apache e CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). [ Per impostazione predefinita, il contenuto HTML viene memorizzato nella cache per circa 5 minuti.

1. Per ignorare la cache a scopo di test/debug, aggiungi semplicemente un parametro di query come `?nocache=true`. L’URL sarà simile a `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Ulteriori dettagli sulla strategia di caching e sulle configurazioni disponibili [sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Puoi anche trovare l’URL del servizio Publish in Cloud Manager. Vai al **Programma Cloud Manager** > **Ambienti** > **Ambiente**.

   ![Visualizza servizio di pubblicazione](assets/author-content-publish/view-environment-segments.png)

   In **Segmenti di ambiente** puoi trovare i collegamenti ai servizi **Autore** e **Pubblica** .

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato e pubblicato le modifiche al tuo sito AEM!

### Passaggi successivi {#next-steps}

Scopri come creare e modificare [Modelli di pagina](./page-templates.md). Comprendere la relazione tra un modello di pagina e una pagina. Scopri come configurare i criteri di un modello di pagina per fornire governance granulare e coerenza del marchio per i contenuti.  Verrà creato un modello di articolo per la rivista ben strutturato basato su un modello di Adobe XD.
