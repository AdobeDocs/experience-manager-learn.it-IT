---
title: Introduzione all’authoring e alla pubblicazione | Creazione rapida di siti AEM
description: Utilizza l’editor pagina in Adobe Experience Manager, AEM, per aggiornare il contenuto del sito web. Scopri come i Componenti vengono utilizzati per facilitare l’authoring. Scopri la differenza tra gli ambienti di authoring e pubblicazione dell’AEM e come pubblicare le modifiche sul sito live.
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 314
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 1%

---

# Introduzione all’authoring e alla pubblicazione {#author-content-publish}

È importante comprendere in che modo un utente aggiornerà i contenuti del sito web. In questo capitolo adotteremo la persona di un **Autore del contenuto** e apporta alcune modifiche redazionali al sito generato nel capitolo precedente. Alla fine del capitolo, pubblicheremo le modifiche per comprendere come viene aggiornato il sito live.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti in cui si presume che i passaggi descritti in [Creare un sito](./create-site.md) capitolo sono stati completati.

## Obiettivo {#objective}

1. Comprendere i concetti di **Pagine** e **Componenti** in AEM Sites.
1. Scopri come aggiornare il contenuto del sito web.
1. Scopri come pubblicare le modifiche al sito live.

## Crea una nuova pagina {#create-page}

Un sito web viene in genere suddiviso in pagine per formare un’esperienza multipagina. L’AEM struttura il contenuto allo stesso modo. Quindi, crea una nuova pagina per il sito.

1. Accedere all’AEM **Autore** Servizio utilizzato nel capitolo precedente.
1. Dalla schermata iniziale dell’AEM, fai clic su **Sites** > **Sito WKND** > **Inglese** > **Articolo**
1. Nell’angolo in alto a destra fai clic su **Crea** > **Pagina**.

   ![Crea pagina](assets/author-content-publish/create-page-button.png)

   Verrà visualizzata la **Crea pagina** procedura guidata.

1. Scegli la **Pagina articolo** e fai clic su **Successivo**.

   Le pagine in AEM vengono create in base a un Modello pagina. I modelli di pagina vengono esaminati in dettaglio nella sezione [Modelli di pagina](page-templates.md) capitolo.

1. Sotto **Proprietà** immetti un **Titolo** di Hello World.
1. Imposta il **Nome** essere `hello-world` e fai clic su **Crea**.

   ![Proprietà pagina iniziale](assets/author-content-publish/initial-page-properties.png)

1. Nella finestra di dialogo a comparsa fai clic su **Apri** per aprire la pagina appena creata.

## Creare un componente {#author-component}

I Componenti AEM possono essere considerati come piccoli elementi costitutivi modulari di una pagina web. Dividendo l’interfaccia utente in blocchi logici o componenti, diventa molto più semplice da gestire. Per poter riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita tramite la finestra di dialogo di authoring.

L&#39;AEM fornisce una serie di [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) pronti per l’uso. Il **Componenti core** da elementi di base come [Testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) e [Immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=it) a elementi dell’interfaccia utente più complessi come [Carosello](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=it).

Quindi, crea alcuni componenti utilizzando l’Editor pagina AEM.

1. Accedi a **Hello World** pagina creata nell&#39;esercizio precedente.
1. Verifica di essere in **Modifica** e nella barra laterale a sinistra fai clic sul pulsante **Componenti** icona.

   ![Barra laterale dell’editor pagina](assets/author-content-publish/page-editor-siderail.png)

   Verrà aperta la libreria dei componenti con l’elenco dei componenti disponibili che possono essere utilizzati nella pagina.

1. Scorri verso il basso e **Trascina** a **Testo (v2)** nell’area modificabile principale della pagina.

   ![Trascina + rilascia il componente testo](assets/author-content-publish/drag-drop-text-cmp.png)

1. Fai clic su **Testo** per evidenziare e quindi fare clic sul pulsante **chiave inglese** icona ![Icona chiave inglese](assets/author-content-publish/wrench-icon.png) per aprire la finestra di dialogo del componente. Inserisci del testo e salva le modifiche nella finestra di dialogo.

   ![Componente Rich Text](assets/author-content-publish/rich-text-populated-component.png)

   Il **Testo** Il componente ora deve visualizzare il testo RTF sulla pagina.

1. Ripeti i passaggi precedenti, tranne trascinare un’istanza del **Immagine (v2)** sulla pagina. Apri **Immagine** finestra di dialogo del componente.

1. Nella barra a sinistra, passa a **Asset Finder** facendo clic su **Risorse** icona ![icona risorsa](assets/author-content-publish/asset-icon.png).
1. **Trascina** un’immagine nella finestra di dialogo del componente e fai clic su **Fine** per salvare le modifiche.

   ![Aggiungi risorsa alla finestra di dialogo](assets/author-content-publish/add-asset-dialog.png)

1. Osserva che nella pagina sono presenti componenti come **Titolo**, **Navigazione**, **Ricerca** che sono corretti. Queste aree sono configurate come parte del Modello pagina e non possono essere modificate in una singola pagina. Questo aspetto verrà approfondito nel prossimo capitolo.

Puoi provare alcuni degli altri componenti. Documentazione su ciascuno [I componenti core sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it). Serie video dettagliata su [L’authoring delle pagine si trova qui](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Aggiornamenti di pubblicazione {#publish-updates}

Gli ambienti AEM sono suddivisi tra **Servizio Author** e un **Servizio di pubblicazione**. In questo capitolo abbiamo apportato diverse modifiche al sito sulla **Servizio Author**. Affinché i visitatori del sito possano visualizzare le modifiche, è necessario pubblicarle in **Servizio di pubblicazione**.

![Diagramma ad alto livello](assets/author-content-publish/author-publish-high-level-flow.png)

*Flusso di alto livello dei contenuti dall’ambiente di authoring a quello di pubblicazione*

**1.** Gli autori dei contenuti apportano aggiornamenti al contenuto del sito. Gli aggiornamenti possono essere visualizzati in anteprima, rivisti e approvati per essere inviati in diretta.

**2.** Il contenuto viene pubblicato. La pubblicazione può essere eseguita su richiesta o pianificata per una data futura.

**3.** I visitatori del sito vedranno le modifiche che si riflettono sul servizio di pubblicazione.

### Pubblicare le modifiche

Ora pubblichiamo le modifiche.

1. Dalla schermata iniziale dell’AEM, vai a **Sites** e seleziona la **Sito WKND**.
1. Fai clic su **Gestisci pubblicazione** nella barra dei menu.

   ![Gestisci pubblicazione](assets/author-content-publish/click-manage-publiciation.png)

   Poiché si tratta di un sito nuovo di zecca, vogliamo pubblicare tutte le pagine e possiamo utilizzare la procedura guidata Gestisci pubblicazione per definire esattamente ciò che deve essere pubblicato.

1. Sotto **Opzioni** lascia le impostazioni predefinite su **Pubblica** e pianificalo per **Ora**. Fai clic su **Avanti**.
1. Sotto **Ambito**, seleziona la **Sito WKND** e fai clic su **Includi impostazioni figlio**. Nella finestra di dialogo, seleziona **Includi elementi figlio**. Deseleziona le altre caselle per assicurarti che l’intero sito sia pubblicato.

   ![Aggiorna ambito di pubblicazione](assets/author-content-publish/update-scope-publish.png)

1. Fai clic su **Riferimenti pubblicati** pulsante. Nella finestra di dialogo, verifica che tutto sia selezionato. Ciò includerà **Modello di sito standard** e diverse configurazioni generate dal modello di sito. Clic **Fine** da aggiornare.

   ![Pubblica riferimenti](assets/author-content-publish/publish-references.png)

1. Infine, seleziona la casella accanto a **Sito WKND** e fai clic su **Successivo** in alto a destra.
1. In **Flussi di lavoro** passaggio, immetti un **Titolo flusso di lavoro**. Può essere qualsiasi testo e può essere utile in seguito come parte di un audit trail. Inserisci &quot;Initial publish&quot; e fai clic su **Pubblica**.

![Pubblicazione iniziale del passaggio del flusso di lavoro](assets/author-content-publish/workflow-step-publish.png)

## Visualizza contenuto pubblicato {#publish}

Quindi, passa al servizio Publish per visualizzare le modifiche.

1. Un modo semplice per ottenere l’URL del servizio di pubblicazione consiste nel copiare l’URL dell’autore e sostituire `author` parola con `publish`. Ad esempio:

   * **URL autore** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL di pubblicazione** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Aggiungi `/content/wknd.html` all’URL di pubblicazione in modo che l’URL finale sia simile al seguente: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Cambia `wknd.html` per corrispondere al nome del sito, se hai fornito un nome univoco durante [creazione di siti](create-site.md).

1. Passando all’URL di pubblicazione, dovresti visualizzare il sito senza alcuna delle funzionalità di creazione AEM.

   ![Sito pubblicato](assets/author-content-publish/publish-url-update.png)

1. Utilizzo di **Navigazione** clic del menu **Articolo** > **Hello World** per passare alla pagina Hello World creata in precedenza.
1. Torna a **Servizio di authoring AEM** e apportare alcune modifiche aggiuntive al contenuto nell’Editor pagina.
1. Pubblica queste modifiche direttamente dall’editor pagina facendo clic sul pulsante **Proprietà pagina** icona > **Pubblica pagina**

   ![pubblicazione diretta](assets/author-content-publish/page-editor-publish.png)

1. Torna a **Servizio di pubblicazione AEM** per visualizzare le modifiche. Molto probabilmente **non** visualizzare immediatamente gli aggiornamenti. Questo perché il **Servizio di pubblicazione AEM** include [caching tramite un server web Apache e CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Per impostazione predefinita, il contenuto di HTML viene memorizzato nella cache per circa 5 minuti.

1. Per ignorare la cache a scopo di test/debug, è sufficiente aggiungere un parametro di query come `?nocache=true`. L’URL sarà simile al seguente `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Ulteriori dettagli sulla strategia e sulle configurazioni di caching disponibili [si trova qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Puoi anche trovare l’URL del servizio di pubblicazione in Cloud Manager. Accedi a **Programma Cloud Manager** > **Ambienti** > **Ambiente**.

   ![Visualizza servizio di pubblicazione](assets/author-content-publish/view-environment-segments.png)

   Sotto **Segmenti di ambiente** è possibile trovare collegamenti a **Autore** e **Pubblica** servizi.

## Congratulazioni. {#congratulations}

Congratulazioni, hai appena creato e pubblicato le modifiche al tuo sito AEM!

### Passaggi successivi {#next-steps}

In un’implementazione reale, la pianificazione di un sito con modelli e progettazioni dell’interfaccia utente in genere precede la creazione del sito. Scopri come utilizzare i kit di interfaccia utente di Adobe XD per progettare e accelerare l’implementazione di Adobe Experience Manager Sites in [Pianificazione dell’interfaccia utente con Adobe XD](./ui-planning-adobe-xd.md).

Vuoi continuare a esplorare le funzionalità di AEM Sites? Puoi passare direttamente al capitolo su [Modelli di pagina](./page-templates.md) per comprendere la relazione tra un modello di pagina e una pagina.


