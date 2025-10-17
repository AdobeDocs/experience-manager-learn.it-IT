---
title: Introduzione all’authoring e alla pubblicazione | Creazione rapida di siti AEM
description: Utilizza l’editor pagina in Adobe Experience Manager, AEM, per aggiornare il contenuto del sito web. Scopri come i Componenti vengono utilizzati per facilitare l'authoring. Scopri la differenza tra gli ambienti di authoring e pubblicazione di AEM e come pubblicare le modifiche al sito live.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1285'
ht-degree: 100%

---

# Introduzione all&#39;authoring e alla pubblicazione {#author-content-publish}

È importante comprendere in che modo un utente aggiornerà i contenuti del sito web. In questo capitolo adotteremo la persona di un **Autore di contenuti** e apporteremo alcuni aggiornamenti editoriali al sito generati nel capitolo precedente. Alla fine del capitolo, pubblicheremo le modifiche per comprendere come viene aggiornato il sito live.

## Prerequisiti {#prerequisites}

Questo è un tutorial in più parti e si presume che i passaggi descritti nel capitolo [Creare un sito](./create-site.md) siano stati completati.

## Obiettivo {#objective}

1. Informazioni sui concetti di **pagine** e **componenti** in AEM Sites.
1. Scopri come aggiornare il contenuto del sito web.
1. Scopri come pubblicare le modifiche al sito live.

## Creare una nuova pagina {#create-page}

Un sito web viene in genere suddiviso in pagine per formare un&#39;esperienza con più pagine. AEM struttura il contenuto nello stesso modo. Quindi, creare una nuova pagina per il sito.

1. Accedere al servizio **Authoring** AEM utilizzato nel capitolo precedente.
1. Dalla schermata iniziale di AEM, fare clic su **Sites** > **Sito WKND** > **Inglese** > **Articolo**
1. Nell&#39;angolo superiore destro fare clic su **Creare** > **Pagina**.

   ![Creare Pagina](assets/author-content-publish/create-page-button.png)

   Verrà visualizzata la procedura guidata **Creare pagina**.

1. Scegli il modello **Pagina articolo** e fai clic su **Avanti**.

   Le pagine in AEM vengono create in base a un Modello pagina. I modelli di pagina sono descritti in dettaglio nel capitolo [Modelli di pagina](page-templates.md).

1. In **Proprietà** inserire un **Titolo** di &quot;Hello World&quot;.
1. Imposta **Name** `hello-world` e fai clic su **Crea**.

   ![Proprietà pagina iniziale](assets/author-content-publish/initial-page-properties.png)

1. Nel pop-up della finestra di dialogo, fare clic su **Apri** per aprire la pagina appena creata.

## Authoring di un componente {#author-component}

I componenti AEM possono essere considerati come piccoli blocchi modulari di base di una pagina web. Dividendo l&#39;interfaccia utente in blocchi logici o componenti, diventa molto più semplice da gestire. Per poter riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita tramite la finestra di dialogo di authoring.

AEM fornisce un set di [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) pronti per l&#39;utilizzo in produzione. I **Componenti core** vanno da elementi di base come [Testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=it) e [Immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=it) a elementi dell&#39;interfaccia utente più complessi come un [Carosello](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=it).

Quindi, creare alcuni componenti utilizzando l&#39;Editor pagina di AEM.

1. Passare alla pagina **Hello World** creata nell&#39;esercizio precedente.
1. Verificare di essere in modalità **Modifica** e nella barra laterale a sinistra fare clic sull&#39;icona **Componenti**.

   ![Barra laterale dell&#39;editor pagina](assets/author-content-publish/page-editor-siderail.png)

   Verrà aperta la libreria dei componenti con l&#39;elenco dei componenti disponibili che possono essere utilizzati nella pagina.

1. Scorri verso il basso e **Trascina** un componente **Testo (v2)** nell’area modificabile principale della pagina.

   ![Trascina il componente testo](assets/author-content-publish/drag-drop-text-cmp.png)

1. Fai clic sul componente **Testo** da evidenziare, quindi fai clic sull’icona a forma di **chiave inglese** ![icona chiave inglese](assets/author-content-publish/wrench-icon.png) per aprire la finestra di dialogo del componente. Inserisci del testo e salva le modifiche nella finestra di dialogo.

   ![Componente Rich Text](assets/author-content-publish/rich-text-populated-component.png)

   Il componente **Testo** ora dovrebbe mostrare sulla pagina il testo formattato.

1. Ripeti i passaggi precedenti, ad eccezione del trascinamento di un’istanza del componente **Image(v2)** sulla pagina. Apri la finestra di dialogo **Immagine** del componente.

1. Nella barra a sinistra, passa a **Ricerca risorse** facendo clic sull’icona **Risorse** ![icona risorse](assets/author-content-publish/asset-icon.png).
1. **Trascina** un’immagine nella finestra di dialogo del componente e fai clic su **Fine** per salvare le modifiche.

   ![Aggiungi una risorsa alla finestra di dialogo](assets/author-content-publish/add-asset-dialog.png)

1. Osserva che nella pagina sono presenti componenti fissi, come **Titolo**, **Navigazione**, **Ricerca**. Queste aree sono configurate come parte del Modello di pagina e non possono essere modificate in una singola pagina. Questo aspetto verrà approfondito nel prossimo capitolo.

Puoi provare alcuni degli altri componenti. La documentazione relativa a ciascun [componente core è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it). Una serie video dettagliata sull’[authoring delle pagine è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html?lang=it&=it).

## Pubblicare gli aggiornamenti {#publish-updates}

Gli ambienti AEM sono suddivisi tra un **Servizio di authoring** e un **Servizio di pubblicazione**. In questo capitolo sono state apportate diverse modifiche al sito nel **Servizio di authoring**. Per consentire ai visitatori del sito di visualizzare le modifiche, è necessario pubblicarle nel **Servizio di pubblicazione**.

![Diagramma di alto livello](assets/author-content-publish/author-publish-high-level-flow.png)

*Flusso di alto livello dei contenuti da authoring a pubblicazione*

**1.** Gli autori di contenuti apportano aggiornamenti al contenuto del sito. Gli aggiornamenti possono essere visualizzati in anteprima, rivisti e approvati per essere inviati in produzione.

**2.** Il contenuto viene pubblicato. La pubblicazione può essere eseguita su richiesta o pianificata per una data futura.

**3.** I visitatori del sito vedranno le modifiche riflesse sul Servizio di pubblicazione.

### Pubblicare le modifiche

Ora pubblichiamo le modifiche.

1. Dalla schermata iniziale di AEM, passa a **Sites** e seleziona il **sito WKND**.
1. Fai clic su **Gestisci pubblicazione** nella barra dei menu.

   ![Gestisci pubblicazione](assets/author-content-publish/click-manage-publiciation.png)

   Poiché si tratta di un sito nuovo di zecca, vogliamo pubblicare tutte le pagine e possiamo utilizzare la procedura guidata Gestisci pubblicazione per definire esattamente ciò che deve essere pubblicato.

1. In **Opzioni** lascia le impostazioni predefinite su **Pubblica** e pianifica per **Ora**. Fai clic su **Avanti**.
1. In **Ambito**, seleziona il **sito WKND** e fai clic su **Includi impostazioni secondarie**. Nella finestra di dialogo, seleziona **Includi elementi secondari**. Deseleziona le altre caselle per assicurarti che l’intero sito sia pubblicato.

   ![Aggiorna ambito di pubblicazione](assets/author-content-publish/update-scope-publish.png)

1. Fai clic sul pulsante **Riferimenti pubblicati**. Nella finestra di dialogo, verifica che tutto sia selezionato. Questo includerà il **modello di sito standard** e diverse configurazioni generate dal modello per siti. Fai clic su **Fine** per aggiornare.

   ![Pubblica riferimenti](assets/author-content-publish/publish-references.png)

1. Infine, seleziona la casella accanto a **Sito WKND** e fai clic su **Avanti** nell’angolo in alto a destra.
1. Nel passaggio **Flussi di lavoro**, immetti un **Titolo flusso di lavoro**. Può essere qualsiasi testo e può essere utile in seguito come parte di un audit trail. Inserisci &quot;Pubblicazione iniziale&quot; e fare clic su **Pubblica**.

![Pubblicazione iniziale passaggio flusso di lavoro](assets/author-content-publish/workflow-step-publish.png)

## Vista contenuto pubblicato {#publish}

Quindi, passa al servizio di pubblicazione per visualizzare le modifiche.

1. Un modo semplice per ottenere l&#39;URL del servizio Publish consiste nel copiare l&#39;URL di authoring e sostituire la parola `author` con `publish`. Ad esempio:

   * **URL di authoring** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL di Pubblicazione** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Aggiungi `/content/wknd.html` all&#39;URL di Pubblicazione in modo che l&#39;URL finale sia simile al seguente: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Modificare `wknd.html` in modo che corrisponda al nome del sito, se durante la [creazione del sito](create-site.md) è stato fornito un nome univoco.

1. Passando all&#39;URL di Pubblicazione, dovresti visualizzare il sito senza alcuna funzionalità di authoring di AEM.

   ![Sito pubblicato](assets/author-content-publish/publish-url-update.png)

1. Utilizzando il menu **Navigazione**, fare clic su **Articolo** > **Hello World** per passare alla pagina Hello World creata in precedenza.
1. Tornare al **Servizio di authoring di AEM** e apportare alcune modifiche aggiuntive al contenuto nell&#39;Editor pagina.
1. Pubblica queste modifiche direttamente dall&#39;editor pagina facendo clic sull&#39;icona **Proprietà pagina** > **Pubblica pagina**

   ![pubblicazione diretta](assets/author-content-publish/page-editor-publish.png)

1. Tornare al **Servizio di Pubblicazione di AEM** per visualizzare le modifiche. Probabilmente **non** sarà possibile visualizzare immediatamente gli aggiornamenti. Questo perché il **servizio di pubblicazione di AEM** include [memorizzazione in cache tramite un server web Apache e CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html?lang=it&=it). Per impostazione predefinita, il contenuto HTML viene memorizzato nella cache per circa 5 minuti.

1. Per ignorare la cache a scopo di test/debug, è sufficiente aggiungere un parametro di query come `?nocache=true`. L&#39;URL sarà simile a `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Ulteriori dettagli sulla strategia di memorizzazione in cache e sulle configurazioni disponibili [sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html?lang=it&=it).

1. È possibile anche trovare l&#39;URL del servizio di Pubblicazione in Cloud Manager. Passare al **Programma Cloud Manager** > **Ambienti** > **Ambiente**.

   ![Vista servizio di pubblicazione](assets/author-content-publish/view-environment-segments.png)

   In **Segmenti di ambiente** è possibile trovare collegamenti ai servizi di **Authoring** e **Pubblicazione**.

## Congratulazioni. {#congratulations}

Congratulazioni, sono state appena create e pubblicate le modifiche al sito AEM!

### Passaggi successivi {#next-steps}

In un&#39;implementazione reale, la pianificazione di un sito con modelli e progettazioni dell&#39;interfaccia utente in genere precede la creazione del sito. Scopri come utilizzare i kit di interfaccia utente di Adobe XD per progettare e accelerare l&#39;implementazione di Adobe Experience Manager Sites nella [pianificazione dell&#39;interfaccia utente con Adobe XD](./ui-planning-adobe-xd.md).

Continuare a esplorare le funzionalità di AEM Sites? È possibile passare direttamente al capitolo su [Modelli di pagina](./page-templates.md) per comprendere la relazione tra un modello di pagina e una pagina.


