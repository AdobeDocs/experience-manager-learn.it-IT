---
title: 'Guida introduttiva ad AEM Sites: pagine e modelli'
description: Scopri la relazione tra un componente della pagina base e i modelli modificabili. Scopri in che modo i Componenti core vengono inseriti come proxy nel progetto. Scopri le configurazioni avanzate dei criteri dei modelli modificabili per creare un modello di pagina di articolo ben strutturato basato su un modello di Adobe XD.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '3040'
ht-degree: 1%

---

# Pagine e modelli {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

In questo capitolo, esaminiamo la relazione tra un componente della pagina base e i modelli modificabili. Scopri come creare un modello di articolo senza stili basato su alcuni modelli di [Adobe XD](https://helpx.adobe.com/support/xd.html). Durante la creazione del modello, sono trattati i Componenti core e le configurazioni avanzate dei criteri dei modelli modificabili.

## Prerequisiti {#prerequisites}

Esaminare gli strumenti e le istruzioni necessari per l&#39;impostazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Progetto iniziale

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per estrarre il progetto iniziale.

Consulta il codice della riga di base su cui si basa l’esercitazione:

1. Consulta la sezione `tutorial/pages-templates-start` ramo da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Implementa la base di codice in un’istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se si utilizza AEM 6.5 o 6.4, aggiungere `classic` profilo a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) oppure estrarre il codice localmente passando al ramo `tutorial/pages-templates-solution`.

## Obiettivo

1. Inspect crea una progettazione di pagina in Adobe XD e la mappa ai Componenti core.
1. Scopri i dettagli dei modelli modificabili e come utilizzare i criteri per applicare un controllo granulare del contenuto della pagina.
1. Scopri come vengono collegati modelli e pagine

## Cosa intendi creare {#what-build}

In questa parte dell&#39;esercitazione verrà creato un nuovo Modello per pagina articolo che può essere utilizzato per creare pagine di articoli e allinearle a una struttura comune. Il modello per pagina dell’articolo si basa sulle progettazioni e su un kit di interfaccia utente prodotto in Adobe XD. Questo capitolo si concentra solo sulla creazione della struttura o dell&#39;ossatura del modello. Non vengono implementati stili, ma il modello e le pagine funzionano.

![Progettazione pagina articolo e versione non formattata](assets/pages-templates/what-you-will-build.png)

## Pianificazione dell’interfaccia utente con Adobe XD {#adobexd}

Di solito, la pianificazione di un nuovo sito web inizia con modelli e progetti statici. [Adobe XD](https://helpx.adobe.com/support/xd.html) è uno strumento di progettazione che crea un’esperienza utente. Ora esaminiamo un kit di interfaccia utente e modelli per aiutare a pianificare la struttura del modello della pagina dell’articolo.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Scarica il file [File progettazione articolo WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Un generico [È disponibile anche il kit dell’interfaccia utente dei Componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) come punto di partenza per i progetti personalizzati.

## Creare il modello per la pagina dell’articolo

Quando crei una pagina, devi selezionare un modello, che viene utilizzato come base per la creazione della pagina. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti consentiti.

Sono disponibili tre aree principali [Modelli modificabili](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=it):

1. **Struttura** - definisce i componenti che fanno parte del modello. Non sono modificabili dagli autori di contenuti.
1. **Contenuto iniziale** : definisce i componenti con cui inizia il modello, che possono essere modificati e/o eliminati dagli autori di contenuto
1. **Criteri** : definisce le configurazioni sul comportamento dei componenti e sulle opzioni disponibili per gli autori.

Quindi, crea un modello in AEM che corrisponda alla struttura dei modelli. Ciò si verifica in un’istanza locale dell’AEM. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Passaggi di alto livello per il video precedente:

### Configurazioni struttura

1. Creare un modello utilizzando **Tipo di modello pagina**, denominato **Pagina articolo**.
1. Passa a **Struttura** modalità.
1. Aggiungi un **Frammento esperienza** componente che funge da **Intestazione** nella parte superiore del modello.
   * Configura il componente a cui puntare `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Imposta il criterio su **Intestazione pagina** e garantire che **Elemento predefinito** è impostato su `header`. Il `header`L&#39;elemento di destinazione è CSS nel capitolo successivo.
1. Aggiungi un **Frammento esperienza** componente che funge da **Piè di pagina** nella parte inferiore del modello.
   * Configura il componente a cui puntare `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Imposta il criterio su **Piè di pagina** e garantire che **Elemento predefinito** è impostato su `footer`. Il `footer` L&#39;elemento di destinazione è CSS nel capitolo successivo.
1. Blocca **principale** contenitore incluso al momento della creazione iniziale del modello.
   * Imposta il criterio su **Pagina principale** e garantire che **Elemento predefinito** è impostato su `main`. Il `main` L&#39;elemento di destinazione è CSS nel capitolo successivo.
1. Aggiungi un **Immagine** componente per **principale** contenitore.
   * Sblocca **Immagine** componente.
1. Aggiungi un **Breadcrumb** componente sotto il **Immagine** nel contenitore principale.
   * Creare un criterio per **Breadcrumb** componente denominato **Pagina articolo - Breadcrumb**. Imposta il **Livello di navigazione iniziale** a **4**.
1. Aggiungi un **Contenitore** componente sotto il **Breadcrumb** e all&#39;interno del **principale** contenitore. Questo funge da **Contenitore di contenuti** per il modello.
   * Sblocca **Contenuto** contenitore.
   * Imposta il criterio su **Contenuto pagina**.
1. Aggiungi un altro **Contenitore** componente sotto il **Contenitore di contenuti**. Questo funge da **Barra laterale** contenitore per il modello.
   * Sblocca **Barra laterale** contenitore.
   * Creare un criterio denominato **Pagina articolo - Barra laterale**.
   * Configurare **Componenti consentiti** in **Progetto WKND Sites - Contenuto** per includere: **Pulsante**, **Scarica**, **Immagine**, **Elenco**, **Separatore**, **Condivisione social media**, **Testo**, e **Titolo**.
1. Aggiorna il criterio del contenitore Pagina principale. Questo è il contenitore più esterno del modello. Imposta il criterio su **Pagina principale**.
   * Sotto **Impostazioni contenitore**, imposta **Layout** a **Griglia reattiva**.
1. Coinvolgi modalità di layout per **Contenitore di contenuti**. Trascina la maniglia da destra a sinistra e riduci il contenitore a otto colonne di larghezza.
1. Coinvolgi modalità di layout per **Contenitore barra laterale**. Trascina la maniglia da destra a sinistra e riduci il contenitore in modo che abbia una larghezza di quattro colonne. Quindi trascina la maniglia sinistra da sinistra a destra di una colonna per rendere il contenitore largo 3 colonne e lasciare un intervallo di 1 colonna tra **Contenitore di contenuti**.
1. Apri l’emulatore mobile e passa a un punto di interruzione mobile. Rivolgiti alla modalità di layout e imposta **Contenitore di contenuti** e **Contenitore barra laterale** l’intera larghezza della pagina. In questo modo i contenitori vengono impilati verticalmente nel punto di interruzione mobile.
1. Aggiorna il criterio di **Testo** componente in **Contenitore di contenuti**.
   * Imposta il criterio su **Testo contenuto**.
   * Sotto **Plug-in** > **Stili paragrafo**, spunta **Abilita stili di paragrafo** e garantire che **Blocco offerta** è abilitato.

### Configurazioni del contenuto iniziale

1. Passa a **Contenuto iniziale** modalità.
1. Aggiungi un **Titolo** componente per **Contenitore di contenuti**. Questo funge da titolo dell’articolo. Quando viene lasciato vuoto, visualizza automaticamente il Titolo della pagina corrente.
1. Aggiungi un secondo **Titolo** al di sotto del primo componente Titolo.
   * Configura il componente con il testo: &quot;Per autore&quot;. Segnaposto di testo.
   * Imposta il tipo su `H4`.
1. Aggiungi un **Testo** componente sotto il **Per autore** Componente titolo.
1. Aggiungi un **Titolo** componente per **Contenitore barra laterale**.
   * Configura il componente con il testo: &quot;Condividi questa storia&quot;.
   * Imposta il tipo su `H5`.
1. Aggiungi un **Condivisione social media** componente sotto il **Condividi questa storia** Componente titolo.
1. Aggiungi un **Separatore** componente sotto il **Condivisione social media** componente.
1. Aggiungi un **Scarica** componente sotto il **Separatore** componente.
1. Aggiungi un **Elenco** componente sotto il **Scarica** componente.
1. Aggiornare il **Proprietà pagina iniziale** per il modello.
   * Sotto **Social media** > **Condivisione social media**, spunta **Facebook** e **Pinterest**

### Abilitare il modello e aggiungere una miniatura

1. Visualizzare il modello nella console Modelli passando a [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Abilita** il modello Pagina articolo.
1. Modifica le proprietà del modello Pagina articolo e carica la miniatura seguente per identificare rapidamente le pagine create utilizzando il modello Pagina articolo:

   ![Miniatura modello pagina articolo](assets/pages-templates/article-page-template-thumbnail.png)

## Aggiornare intestazione e piè di pagina con frammenti esperienza {#experience-fragments}

Una pratica comune durante la creazione di contenuti globali, ad esempio un’intestazione o un piè di pagina, consiste nell’utilizzare un’ [Frammento esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Frammenti di esperienza, consente agli utenti di combinare più componenti per creare un singolo componente di riferimento. I frammenti di esperienza hanno il vantaggio di supportare la gestione multisito e [localizzazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en).

L’Archetipo progetto AEM ha generato un’intestazione e un piè di pagina. Quindi, aggiorna i Frammenti esperienza in modo che corrispondano ai modelli. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Passaggi di alto livello per il video precedente:

1. Scaricare il pacchetto di contenuti di esempio **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Caricare e installare il pacchetto di contenuti utilizzando Gestione pacchetti all’indirizzo [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Aggiorna il modello Variante web, che è il modello utilizzato per i frammenti esperienza in [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Aggiornare il criterio **Contenitore** nel modello.
   * Imposta il criterio su **Directory principale XF**.
   * Alla voce, il **Componenti consentiti** seleziona il gruppo di componenti **Progetto WKND Sites - Struttura** da includere **Navigazione lingua**, **Navigazione**, e **Ricerca rapida** componenti.

### Aggiorna frammento esperienza intestazione

1. Apri il frammento di esperienza in cui viene eseguito il rendering dell’intestazione [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configurare la directory principale **Contenitore** del frammento. Questo è il più esterno **Contenitore**.
   * Imposta il **Layout** a **Griglia reattiva**
1. Aggiungi il **Logo WKND scuro** come immagine nella parte superiore della **Contenitore**. Il logo è stato incluso nel pacchetto installato in un passaggio precedente.
   * Modificare il layout del **Logo WKND scuro** essere **due** colonne. Trascinare le maniglie da destra a sinistra.
   * Configurare il logo con **Testo alternativo** del &quot;Logo WKND&quot;.
   * Configura il logo in **Collegamento** a `/content/wknd/us/en` la home page.
1. Configurare **Navigazione** componente già inserito nella pagina.
   * Imposta il **Escludi livelli di navigazione principali** a **1**.
   * Imposta il **Annidamento struttura di navigazione** a **1**.
   * Modificare il layout del **Navigazione** componente da **otto** colonne. Trascinare le maniglie da destra a sinistra.
1. Rimuovi il **Navigazione lingua** componente.
1. Modificare il layout del **Ricerca** componente da **due** colonne. Trascinare le maniglie da destra a sinistra. Ora tutti i componenti devono essere allineati orizzontalmente su un’unica riga.

### Aggiorna frammento esperienza piè di pagina

1. Apri il frammento di esperienza per il rendering del piè di pagina in [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configurare la directory principale **Contenitore** del frammento. Questo è il più esterno **Contenitore**.
   * Imposta il **Layout** a **Griglia reattiva**
1. Aggiungi il **Logo WKND Light** come immagine nella parte superiore della **Contenitore**. Il logo è stato incluso nel pacchetto installato in un passaggio precedente.
   * Modificare il layout del **Logo WKND Light** essere **due** colonne. Trascinare le maniglie da destra a sinistra.
   * Configurare il logo con **Testo alternativo** di &quot;WKND Logo Light&quot;.
   * Configura il logo in **Collegamento** a `/content/wknd/us/en` la home page.
1. Aggiungi un **Navigazione** sotto il logo. Configurare **Navigazione** componente:
   * Imposta il **Escludi livelli di navigazione principali** a **1**.
   * Deseleziona **Raccogli tutte le pagine figlie**.
   * Imposta il **Annidamento struttura di navigazione** a **1**.
   * Modificare il layout del **Navigazione** componente da **otto** colonne. Trascinare le maniglie da destra a sinistra.

## Creare una pagina di articolo

Quindi, crea una pagina utilizzando il modello Pagina articolo. Crea il contenuto della pagina in modo che corrisponda ai modelli del sito. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Passaggi di alto livello per il video precedente:

1. Passa alla console Sites all’indirizzo [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Crea una pagina sotto **WKND** > **STATI UNITI** > **IT** > **Rivista**.
   * Scegli la **Pagina articolo** modello.
   * Sotto **Proprietà** imposta **Titolo** to &quot;Guida di ultima generazione a LA Skateparks&quot;
   * Imposta il **Nome** a &quot;guide-la-skateparks&quot;
1. Sostituisci **Per autore** Titolo con il testo &quot;di Stacey Roswells&quot;.
1. Aggiornare il **Testo** per includere un paragrafo per compilare l&#39;articolo. È possibile utilizzare il seguente file di testo come copia: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Aggiungi un altro **Testo** componente.
   * Aggiorna il componente per includere la citazione: &quot;Non c&#39;è posto migliore da condividere di Los Angeles&quot;.
   * Modifica l’Editor Rich Text in modalità a schermo intero e modifica le virgolette per utilizzare **Blocco offerta** elemento.
1. Continua a popolare il corpo dell’articolo in modo che corrisponda ai modelli.
1. Configurare **Scarica** per utilizzare una versione PDF dell’articolo.
   * Sotto **Scarica** > **Proprietà**, fare clic sulla casella di controllo per **Ottieni il titolo dalla risorsa DAM**.
   * Imposta il **Descrizione** to: &quot;Leggi la storia completa&quot;.
   * Imposta il **Testo azione** to: &quot;Download PDF&quot;.
1. Configurare **Elenco** componente.
   * Sotto **Impostazioni elenco** > **Genera elenco con**, seleziona **Pagine figlie**.
   * Imposta il **Pagina padre** a `/content/wknd/us/en/magazine`.
   * Alla voce, il **Impostazioni elemento** spunta **Collega elementi** e verifica **Mostra data**.

## Inspect la struttura dei nodi {#node-structure}

A questo punto, la pagina dell’articolo è chiaramente sformattata. Tuttavia la struttura di base è in atto. Quindi, esamina la struttura dei nodi della pagina dell’articolo per comprendere meglio il ruolo del modello, della pagina e dei componenti.

Utilizza lo strumento CRXDE-Lite su un’istanza AEM locale per visualizzare la struttura del nodo sottostante.

1. Apri [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e utilizza la struttura di navigazione per passare a `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Fai clic sul pulsante `jcr:content` nodo sotto `la-skateparks` e visualizzare le proprietà:

   ![Proprietà contenuto JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Osserva il valore per `cq:template`, che fa riferimento a `/conf/wknd/settings/wcm/templates/article-page`, il modello per pagina di articolo creato in precedenza.

   Nota anche il valore di `sling:resourceType`, che fa riferimento a `wknd/components/page`. Questo è il componente pagina creato dall’archetipo di progetto AEM ed è responsabile del rendering della pagina basata sul modello.

1. Espandi `jcr:content` nodo sotto `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e visualizzare la gerarchia dei nodi:

   ![JCR Content LA Skatepark](assets/pages-templates/page-jcr-structure.png)

   Dovresti essere in grado di mappare liberamente ciascuno dei nodi ai componenti creati. Scopri se è possibile identificare i diversi Contenitori di layout utilizzati esaminando i nodi con il prefisso `container`.

1. Quindi, controlla il componente Pagina in `/apps/wknd/components/page`. Visualizza le proprietà del componente in CRXDE Liti:

   ![Proprietà del componente Pagina](assets/pages-templates/page-component-properties.png)

   Esistono solo due script HTL, `customfooterlibs.html` e `customheaderlibs.html` sotto il componente page. *Come avviene il rendering della pagina da parte di questo componente?*

   Il `sling:resourceSuperType` la proprietà punta a `core/wcm/components/page/v2/page`. Questa proprietà consente al componente page del WKND di ereditare **tutto** la funzionalità del componente core Pagina. Questo è il primo esempio di qualcosa chiamato [Modello di componente proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Ulteriori informazioni sono disponibili [qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect è un altro componente dei componenti WKND, il `Breadcrumb` componente da: `/apps/wknd/components/breadcrumb`. Notare che lo stesso `sling:resourceSuperType` , ma questa volta punta a `core/wcm/components/breadcrumb/v2/breadcrumb`. Questo è un altro esempio di utilizzo del modello di componente Proxy per includere un Componente core. Infatti, tutti i componenti nella base di codice WKND sono proxy dei Componenti core AEM (ad eccezione del componente demo personalizzato HelloWorld). È consigliabile riutilizzare quante più funzionalità possibile dei Componenti core *prima di* scrittura di codice personalizzato.

1. Quindi, controlla la pagina dei componenti core all’indirizzo `/libs/core/wcm/components/page/v2/page` utilizzo di CRXDE Liti:

   >[!NOTE]
   >
   > In, AEM 6.5/6.4 i Componenti core si trovano sotto `/apps/core/wcm/components`. In, AEM as a Cloud Service, i Componenti core si trovano in `/libs` e vengono aggiornati automaticamente.

   ![Pagina Componente core](assets/pages-templates/core-page-component-properties.png)

   Tieni presente che molti file di script sono inclusi sotto questa pagina. La pagina dei componenti core contiene numerose funzionalità. Questa funzionalità è suddivisa in più script per facilitarne la manutenzione e la leggibilità. Per tracciare l’inclusione degli script HTL, apri la sezione `page.html` e alla ricerca `data-sly-include`:

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   L’altro motivo per suddividere HTL in più script è quello di consentire ai componenti proxy di ignorare singoli script per implementare una logica di business personalizzata. Script HTL `customfooterlibs.html`, e `customheaderlibs.html`, vengono create allo scopo esplicito di essere sostituite dall&#39;implementazione dei progetti.

   Puoi saperne di più su come il modello modificabile influisce sul rendering del [pagina del contenuto leggendo questo articolo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=it).

1. Inspect contiene un altro componente core, come il Breadcrumb in `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualizza `breadcrumb.html` script per comprendere come viene generato il markup per il componente Breadcrumb.

## Salvataggio delle configurazioni nel controllo del codice sorgente {#configuration-persistence}

Spesso, soprattutto all’inizio di un progetto AEM, è utile mantenere le configurazioni, come i modelli e le relative policy di contenuto, per il controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano sullo stesso set di contenuti e configurazioni e possono garantire ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la pratica di gestione dei modelli può essere affidata a uno speciale gruppo di utenti esperti.


Per il momento, i modelli vengono trattati come altre parti di codice e sincronizzano **Modello pagina articolo** come parte del progetto.
Finora il codice viene inviato dal progetto AEM a un&#39;istanza locale dell&#39;AEM. Il **Modello pagina articolo** è stato creato direttamente su un’istanza locale dell’AEM, pertanto deve **importa** il modello nel progetto AEM. Il **ui.content** è incluso nel progetto AEM per questo scopo specifico.

I passaggi successivi vengono eseguiti nell’IDE VSCode utilizzando [Sincronizzazione AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) plugin. Tuttavia, potrebbe utilizzare qualsiasi IDE configurato per **importa** o importare contenuti da un’istanza locale dell’AEM.

1. In, VSCode apre la `aem-guides-wknd` progetto.

1. Espandi **ui.content** in Esplora progetti. Espandi `src` cartella e passare a `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Clic con il pulsante destro] il `templates` cartella e seleziona **Importa da server AEM**:

   ![Modello di importazione VSCode](assets/pages-templates/vscode-import-templates.png)

   Il `article-page` devono essere importati e `page-content`, `xf-web-variation` Anche i modelli devono essere aggiornati.

   ![Modelli aggiornati](assets/pages-templates/updated-templates.png)

1. Ripeti i passaggi per importare il contenuto, ma seleziona la **criteri** cartella da `/conf/wknd/settings/wcm/policies`.

   ![Criteri di importazione VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect `filter.xml` file da `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   Il `filter.xml` Il file è responsabile dell’identificazione dei percorsi dei nodi installati con il pacchetto. Osserva `mode="merge"` in ciascuno dei filtri che indica che il contenuto esistente non deve essere modificato, viene aggiunto solo il nuovo contenuto. Poiché gli autori dei contenuti potrebbero aggiornare questi percorsi, è importante che una distribuzione del codice **non** sovrascrivi il contenuto. Consulta la [Documentazione di FileVault](https://jackrabbit.apache.org/filevault/filter.html) per ulteriori dettagli sull’utilizzo degli elementi filtro.

   Confronta `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.

   >[!WARNING]
   >
   > Per garantire distribuzioni coerenti per il sito di riferimento WKND, alcuni rami del progetto sono configurati in modo tale che `ui.content` sovrascrive eventuali modifiche nel JCR. Questo è per progettazione, ovvero per i rami della soluzione, in quanto il codice e gli stili sono scritti per criteri specifici.

## Congratulazioni. {#congratulations}

Congratulazioni, hai creato un modello e una pagina con Adobe Experience Manager Sites.

### Passaggi successivi {#next-steps}

A questo punto, la pagina dell’articolo è chiaramente sformattata. Segui le [Librerie lato client e workflow front-end](client-side-libraries.md) tutorial per scoprire le best practice per includere CSS e JavaScript per applicare stili globali al sito e integrare una build front-end dedicata.

Visualizza il codice finito il [GitHub](https://github.com/adobe/aem-guides-wknd) oppure controlla e distribuisci il codice localmente in sul ramo Git `tutorial/pages-templates-solution`.

1. Clona il [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) archivio.
1. Consulta la sezione `tutorial/pages-templates-solution` filiale.
