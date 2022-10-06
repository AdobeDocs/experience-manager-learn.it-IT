---
title: Guida introduttiva ad AEM Sites - Pagine e modelli
description: Scopri la relazione tra un componente della pagina di base e modelli modificabili. Scopri in che modo i componenti core vengono sottoposti a proxy nel progetto. Scopri le configurazioni avanzate dei criteri dei modelli modificabili per creare un modello di pagina articolo ben strutturato basato su un modello di Adobe XD.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '3066'
ht-degree: 1%

---

# Pagine e modelli {#pages-and-template}

In questo capitolo verrà esplorata la relazione tra un componente della pagina di base e modelli modificabili. Verrà creato un modello di articolo non formattato basato su alcuni modelli di [AdobeXD](https://www.adobe.com/products/xd.html). Durante il processo di creazione del modello, sono trattati i componenti core e le configurazioni avanzate dei modelli modificabili.

## Prerequisiti {#prerequisites}

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Progetto iniziale

>[!NOTE]
>
> Se hai completato con successo il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Consulta la sezione `tutorial/pages-templates-start` ramo [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi la variabile `classic` su qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) o controlla il codice localmente passando al ramo `tutorial/pages-templates-solution`.

## Obiettivo

1. Inspect è una progettazione di pagina creata in Adobe XD e la associa a Componenti core.
1. Comprendi i dettagli dei modelli modificabili e come possono essere utilizzati i criteri per applicare il controllo granulare del contenuto della pagina.
1. Scopri come i modelli e le pagine sono collegati

## Cosa verrà creato {#what-you-will-build}

In questa parte dell’esercitazione verrà creato un nuovo modello Pagina articolo che può essere utilizzato per creare nuove pagine di articoli e allinearsi a una struttura comune. Il modello Pagina articolo è basato sulle progettazioni e su un kit di interfaccia utente prodotto in AdobeXD. Questo capitolo si concentra solo sulla costruzione della struttura o dello scheletro del modello. Non vengono implementati stili, ma il modello e le pagine sono funzionanti.

![Progettazione pagina articolo e versione senza stile](assets/pages-templates/what-you-will-build.png)

## Pianificazione dell’interfaccia utente con Adobe XD {#adobexd}

Nella maggior parte dei casi, la pianificazione di un nuovo sito web inizia con modelli e progetti statici. [Adobe XD](https://www.adobe.com/products/xd.html) è uno strumento di progettazione che crea esperienze utente. Ora esamineremo un kit di interfaccia utente e i modelli per pianificare la struttura del modello di pagina dell’articolo.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Scarica la [File di progettazione di articoli WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Un generico [È disponibile anche AEM kit per l’interfaccia utente dei componenti core](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) come punto di partenza per i progetti personalizzati.

## Creare il modello di pagina dell’articolo

Quando crei una pagina devi selezionare un modello, che viene utilizzato come base per la creazione della nuova pagina. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti consentiti.

Ci sono 3 aree principali di [Modelli modificabili](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Struttura** - definisce i componenti che fanno parte del modello. Non sono modificabili dagli autori dei contenuti.
1. **Contenuto iniziale** - definisce i componenti con cui inizia il modello, che possono essere modificati e/o eliminati dagli autori di contenuti
1. **Criteri** - definisce le configurazioni sul comportamento dei componenti e sulle opzioni disponibili per gli autori.

Quindi, crea un nuovo modello in AEM che corrisponda alla struttura dei modelli. Questo si verificherà in un&#39;istanza locale di AEM. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

Passi di alto livello per il video precedente:

### Configurazioni struttura

1. Crea un nuovo modello utilizzando **Tipo di modello di pagina**, denominato **Pagina dell’articolo**.
1. Passa a **Struttura** modalità.
1. Aggiungi un **Frammento esperienza** come componente **Intestazione** nella parte superiore del modello.
   * Configura il componente a cui puntare `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Imposta il criterio su **Intestazione pagina** e assicurano che **Elemento predefinito** è impostato su `header`. La `header`nel capitolo successivo viene eseguito il targeting con CSS.
1. Aggiungi un **Frammento esperienza** come componente **Piè di pagina** nella parte inferiore del modello.
   * Configura il componente a cui puntare `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Imposta il criterio su **Piè di pagina** e assicurano che **Elemento predefinito** è impostato su `footer`. La `footer` nel capitolo successivo viene eseguito il targeting con CSS.
1. Blocca **principale** contenitore incluso al momento della creazione iniziale del modello.
   * Imposta il criterio su **Pagina principale** e assicurano che **Elemento predefinito** è impostato su `main`. La `main` nel capitolo successivo viene eseguito il targeting con CSS.
1. Aggiungi un **Immagine** nella **principale** contenitore.
   * Sblocca **Immagine** componente.
1. Aggiungi un **Breadcrumb** sotto il **Immagine** nel contenitore principale.
   * Crea un nuovo criterio per **Breadcrumb** componente denominato **Pagina articolo - Breadcrumb**. Imposta la **Livello iniziale navigazione** a **4**.
1. Aggiungi un **Contenitore** sotto il **Breadcrumb** e all’interno del **principale** contenitore. Questo fungerà da **Contenitore di contenuti** per il modello.
   * Sblocca **Contenuto** contenitore.
   * Imposta il criterio su **Contenuto della pagina**.
1. Aggiungi un altro **Contenitore** sotto il **Contenitore di contenuti**. Questo fungerà da **Barra laterale** contenitore per il modello.
   * Sblocca **Barra laterale** contenitore.
   * Crea un nuovo criterio denominato **Pagina articolo - Barra laterale**.
   * Configura le **Componenti consentiti** sotto **Progetto WKND Sites - Contenuto** per includere: **Pulsante**, **Scarica**, **Immagine**, **Elenco**, **Separatore**, **Condivisione social media**, **Testo** e **Titolo**.
1. Aggiorna il criterio del contenitore Directory principale pagina. Questo è il contenitore più esterno del modello. Imposta il criterio su **Radice pagina**.
   * Sotto **Impostazioni dei contenitori**, imposta le **Layout** a **Griglia reattiva**.
1. Modalità di layout coinvolgimento per **Contenitore di contenuti**. Trascinare la maniglia da destra a sinistra e ridurre il contenitore in modo che sia largo 8 colonne.
1. Modalità di layout coinvolgimento per **Contenitore Barra laterale**. Trascinare la maniglia da destra a sinistra e ridurre il contenitore in modo che sia largo 4 colonne. Quindi trascina la maniglia sinistra da sinistra a destra 1 colonna per rendere il contenitore largo 3 colonne e lasciare uno spazio di 1 colonna tra **Contenitore di contenuti**.
1. Apri l’emulatore mobile e passa a un punto di interruzione mobile. Attiva nuovamente la modalità di layout e imposta la **Contenitore di contenuti** e **Contenitore Barra laterale** la larghezza intera della pagina. I contenitori verranno sovrapposti verticalmente nel punto di interruzione mobile.
1. Aggiorna il criterio **Testo** nel **Contenitore di contenuti**.
   * Imposta il criterio su **Testo contenuto**.
   * Sotto **Plug-in** > **Stili paragrafo**, controlla **Abilitare gli stili di paragrafo** e assicurano che **Blocco preventivo** è abilitato.

### Configurazioni del contenuto iniziale

1. Passa a **Contenuto iniziale** modalità.
1. Aggiungi un **Titolo** nella **Contenitore di contenuti**. Questo agisce come titolo dell&#39;articolo. Quando viene lasciato vuoto, viene visualizzato automaticamente il Titolo della pagina corrente.
1. Aggiungi un secondo **Titolo** sotto il primo componente Titolo .
   * Configura il componente con il testo: &quot;Per autore&quot;. Segnaposto per il testo.
   * Imposta il tipo da `H4`.
1. Aggiungi un **Testo** sotto il **Per autore** Componente titolo.
1. Aggiungi un **Titolo** nella **Contenitore della barra laterale**.
   * Configura il componente con il testo: &quot;Condividi questa storia&quot;.
   * Imposta il tipo da `H5`.
1. Aggiungi un **Condivisione social media** sotto il **Condividi questa storia** Componente titolo.
1. Aggiungi un **Separatore** sotto il **Condivisione social media** componente.
1. Aggiungi un **Scarica** sotto il **Separatore** componente.
1. Aggiungi un **Elenco** sotto il **Scarica** componente.
1. Aggiorna **Proprietà pagina iniziale** per il modello.
   * Sotto **Social media** > **Condivisione social media**, controlla **Facebook** e **Pinterest**

### Abilita il modello e aggiungi una miniatura

1. Per visualizzare il modello nella console Modelli , vai a [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Abilita** il modello Pagina articolo .
1. Modifica le proprietà del modello Pagina articolo e carica la seguente miniatura per identificare rapidamente le pagine create utilizzando il modello Pagina articolo :

   ![Articolo Miniatura del modello di pagina](assets/pages-templates/article-page-template-thumbnail.png)

## Aggiornare intestazione e piè di pagina con frammenti esperienza {#experience-fragments}

Una pratica comune nella creazione di contenuto globale, ad esempio intestazione o piè di pagina, consiste nell’utilizzare un’ [Frammento esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Frammenti esperienza consente agli utenti di combinare più componenti per creare un singolo componente utilizzabile come riferimento. I frammenti esperienza hanno il vantaggio di supportare la gestione multisito e [localizzazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

L’Archetipo di progetto AEM ha generato un’intestazione e un piè di pagina. Quindi, aggiorna i frammenti esperienza in modo che corrispondano ai pattern. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Passi di alto livello per il video precedente:

1. Scarica il pacchetto di contenuti di esempio **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Carica e installa il pacchetto di contenuti utilizzando Gestione pacchetti in [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Aggiorna il modello Variazione web , che è il modello utilizzato per Frammenti esperienza in [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Aggiorna il criterio **Contenitore** nel modello.
   * Imposta il criterio su **Radice XF**.
   * Sotto **Componenti consentiti** seleziona il gruppo di componenti **Progetto WKND Sites - Struttura** per includere **Navigazione lingua**, **Navigazione** e **Ricerca rapida** componenti.

### Aggiorna frammento esperienza intestazione

1. Apri il frammento esperienza che esegue il rendering dell’intestazione in [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configurare la radice **Contenitore** del frammento. Questa è la più esterna **Contenitore**.
   * Imposta la **Layout** a **Griglia reattiva**
1. Aggiungi il **Logo WKND Dark** come immagine nella parte superiore della **Contenitore**. Il logo è stato incluso nel pacchetto installato in un passaggio precedente.
   * Modificare il layout del **Logo WKND Dark** essere **2** largo. Trascinare le maniglie da destra a sinistra.
   * Configura il logo con **Testo alternativo** di &quot;WKND Logo&quot;.
   * Configurare il logo **Collegamento** a `/content/wknd/us/en` la home page.
1. Configura le **Navigazione** che è già inserito nella pagina.
   * Imposta la **Escludere i livelli della radice** a **1**.
   * Imposta la **Profondità struttura di navigazione** a **1**.
   * Modificare il layout del **Navigazione** componente **8** largo. Trascinare le maniglie da destra a sinistra.
1. Rimuovi **Navigazione lingua** componente.
1. Modificare il layout del **Ricerca** componente **2** largo. Trascinare le maniglie da destra a sinistra. Ora tutti i componenti devono essere allineati orizzontalmente su una singola riga.

### Aggiorna frammento esperienza piè di pagina

1. Apri il frammento esperienza che esegue il rendering del piè di pagina in [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configurare la radice **Contenitore** del frammento. Questa è la più esterna **Contenitore**.
   * Imposta la **Layout** a **Griglia reattiva**
1. Aggiungi il **Logo WKND Light** come immagine nella parte superiore della **Contenitore**. Il logo è stato incluso nel pacchetto installato in un passaggio precedente.
   * Modificare il layout del **Logo WKND Light** essere **2** largo. Trascinare le maniglie da destra a sinistra.
   * Configura il logo con **Testo alternativo** di &quot;WKND Logo Light&quot;.
   * Configurare il logo **Collegamento** a `/content/wknd/us/en` la home page.
1. Aggiungi un **Navigazione** sotto il logo. Configura le **Navigazione** componente:
   * Imposta la **Escludere i livelli della radice** a **1**.
   * Deseleziona **Raccogli tutte le pagine figlie**.
   * Imposta la **Profondità struttura di navigazione** a **1**.
   * Modificare il layout del **Navigazione** componente **8** largo. Trascinare le maniglie da destra a sinistra.

## Creare una pagina di articolo

Quindi, crea una nuova pagina utilizzando il modello Pagina articolo . Crea il contenuto della pagina in modo che corrisponda ai modelli di sito. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

Passi di alto livello per il video precedente:

1. Passa alla console Sites all’indirizzo [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Crea una nuova pagina sotto **WKND** > **US** > **IT** > **Rivista**.
   * Scegli la **Pagina dell’articolo** modello.
   * Sotto **Proprietà** imposta **Titolo** alla &quot;Guida definitiva a LA Skatepark&quot;
   * Imposta la **Nome** a &quot;guide-la-skatepark&quot;
1. Sostituisci **Per autore** Titolo con il testo &quot;By Stacey Roswells&quot;.
1. Aggiorna **Testo** per includere un paragrafo per compilare l’articolo. È possibile utilizzare il seguente file di testo come copia: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Aggiungi un altro **Testo** componente.
   * Aggiorna il componente per includere il preventivo: &quot;Non c&#39;è posto migliore per distruggere allora Los Angeles.&quot;
   * Modifica l’Editor Rich Text in modalità a schermo intero e modifica le virgolette di cui sopra per utilizzare il **Blocco preventivo** elemento.
1. Continua a popolare il corpo dell’articolo per adattarlo ai modelli.
1. Configura le **Scarica** per utilizzare una versione PDF dell’articolo.
   * Sotto **Scarica** > **Proprietà**, fai clic sulla casella di controllo per **Ottenere il titolo dalla risorsa DAM**.
   * Imposta la **Descrizione** a: &quot;Ottieni la storia completa&quot;.
   * Imposta la **Testo azione** a: &quot;Scarica PDF&quot;.
1. Configura le **Elenco** componente.
   * Sotto **Impostazioni elenco** > **Crea elenco tramite**, seleziona **Pagine figlie**.
   * Imposta la **Pagina padre** a `/content/wknd/us/en/magazine`.
   * Sotto **Impostazioni elemento** check **Collega elementi** e controlla **Mostra data**.

## Inspect la struttura del nodo {#node-structure}

A questo punto la pagina dell&#39;articolo è chiaramente senza stile. Tuttavia, la struttura di base è in atto. Quindi, controlla la struttura del nodo della pagina dell’articolo per acquisire una migliore comprensione del ruolo del modello, della pagina e dei componenti.

Utilizza lo strumento CRXDE-Lite su un&#39;istanza AEM locale per visualizzare la struttura del nodo sottostante.

1. Apri [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e utilizza la navigazione ad albero per navigare `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Fai clic sul pulsante `jcr:content` sotto il nodo `la-skateparks` e visualizza le proprietà:

   ![Proprietà contenuto JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Osserva il valore per `cq:template`, che indica `/conf/wknd/settings/wcm/templates/article-page`, Modello pagina articolo creato in precedenza.

   Osserva anche il valore di `sling:resourceType`, che indica `wknd/components/page`. Si tratta del componente pagina creato dall’archetipo di progetto AEM ed è responsabile del rendering della pagina in base al modello.

1. Espandi la `jcr:content` nodo sotto `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e visualizza la gerarchia dei nodi:

   ![JCR Content LA Skatepark](assets/pages-templates/page-jcr-structure.png)

   Dovresti essere in grado di mappare liberamente ciascuno dei nodi ai componenti creati. Vedi se puoi identificare i diversi Contenitori di layout utilizzati dall’ispezione dei nodi con prefisso `container`.

1. Successivamente, controlla il componente della pagina in `/apps/wknd/components/page`. Visualizza le proprietà del componente in CRXDE Lite:

   ![Proprietà dei componenti pagina](assets/pages-templates/page-component-properties.png)

   Si noti che esistono solo 2 script HTL, `customfooterlibs.html` e `customheaderlibs.html` sotto il componente pagina. *Quindi, in che modo questo componente esegue il rendering della pagina?*

   La `sling:resourceSuperType` fa riferimento a `core/wcm/components/page/v2/page`. Questa proprietà consente al componente pagina di WKND di ereditare **tutto** della funzionalità del componente della pagina Componenti core. Questo è il primo esempio di una cosa chiamata [Pattern componente proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Ulteriori informazioni sono disponibili [qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Un altro componente all’interno dei componenti WKND, il `Breadcrumb` componente situato in: `/apps/wknd/components/breadcrumb`. Notate che lo stesso `sling:resourceSuperType` è possibile trovare la proprietà , ma questa volta fa riferimento a `core/wcm/components/breadcrumb/v2/breadcrumb`. Questo è un altro esempio di utilizzo del pattern del componente Proxy per includere un componente core. Infatti, tutti i componenti della base di codice WKND sono proxy dei componenti core AEM (ad eccezione del nostro famoso componente HelloWorld). È consigliabile provare e riutilizzare il più possibile la funzionalità dei componenti core *prima* scrittura di codice personalizzato.

1. Quindi controlla la pagina del componente core in `/libs/core/wcm/components/page/v2/page` con CRXDE Lite:

   >[!NOTE]
   >
   > In AEM 6.5/6.4, i componenti core si trovano in `/apps/core/wcm/components`. In AEM as a Cloud Service, i componenti core si trovano in `/libs` e vengono aggiornati automaticamente.

   ![Pagina dei componenti core](assets/pages-templates/core-page-component-properties.png)

   Molti altri script sono inclusi in questa pagina. La pagina dei componenti core contiene molte funzionalità. Questa funzionalità è suddivisa in più script per facilitarne la manutenzione e la leggibilità. Puoi tracciare l’inclusione degli script HTL aprendo la `page.html` e alla ricerca `data-sly-include`:

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

   L’altro motivo per suddividere l’HTL in più script è consentire ai componenti proxy di sostituire i singoli script per implementare una logica di business personalizzata. Gli script HTL, `customfooterlibs.html` e `customheaderlibs.html`, sono creati per lo scopo esplicito di essere sostituiti dall’implementazione di progetti.

   Per ulteriori informazioni sui fattori del modello modificabile nel rendering del [pagina del contenuto leggendo questo articolo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect è l’altro componente core, come la Breadcrumb in `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualizza la `breadcrumb.html` script per comprendere in che modo viene generato il markup per il componente Breadcrumb.

## Salvataggio delle configurazioni nel controllo del codice sorgente {#configuration-persistence}

In molti casi, specialmente all’inizio di un progetto AEM, è utile mantenere le configurazioni, come i modelli e i relativi criteri dei contenuti, al controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano con lo stesso set di contenuti e configurazioni e possono garantire un’ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la gestione dei modelli può essere affidata a un gruppo speciale di utenti.

Per il momento i modelli verranno trattati come altri pezzi di codice e sincronizzeremo il **Modello pagina articolo** nell&#39;ambito del progetto. Finora abbiamo **spinto** dal nostro progetto AEM a un&#39;istanza locale di AEM. La **Modello pagina articolo** è stato creato direttamente su un&#39;istanza locale di AEM, quindi dobbiamo **importare** il modello nel nostro progetto AEM. La **ui.content** Il modulo è incluso nel progetto AEM per questo scopo specifico.

I passaggi successivi si svolgeranno utilizzando l’IDE VSCode utilizzando [Sincronizzazione AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) plug-in, ma potrebbe utilizzare qualsiasi IDE configurato per **importare** o importa contenuto da un’istanza locale di AEM.

1. In VSCode apri la `aem-guides-wknd` progetto.

1. Espandi la **ui.content** in Project explorer. Espandi la `src` e passa a `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Clic con il pulsante destro] la `templates` e seleziona **Importa da AEM server**:

   ![Modello di importazione VSCode](assets/pages-templates/vscode-import-templates.png)

   La `article-page` devono essere importati e `page-content`, `xf-web-variation` È inoltre necessario aggiornare i modelli.

   ![Modelli aggiornati](assets/pages-templates/updated-templates.png)

1. Ripeti i passaggi per importare il contenuto, ma seleziona la **politiche** cartella situata in `/conf/wknd/settings/wcm/policies`.

   ![Criteri di importazione VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect `filter.xml` file situato in `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   La `filter.xml` Il file è responsabile dell&#39;identificazione dei percorsi dei nodi installati con il pacchetto. Osserva che `mode="merge"` in ciascuno dei filtri che indica che il contenuto esistente non verrà modificato, viene aggiunto solo un nuovo contenuto. Poiché gli autori dei contenuti possono aggiornare questi percorsi, è importante che la distribuzione del codice lo faccia **not** sovrascrivi il contenuto. Consulta la sezione [Documentazione FileVault](https://jackrabbit.apache.org/filevault/filter.html) per ulteriori informazioni sull’utilizzo degli elementi filtro.

   Confronto `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.

   >[!WARNING]
   >
   > Per garantire distribuzioni coerenti per il sito WKND Reference, alcuni rami del progetto sono configurati in modo che `ui.content` sovrascrive eventuali modifiche nel JCR. Questo è di progettazione, ovvero per i rami della soluzione, poiché il codice/gli stili sono scritti per criteri specifici.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato un nuovo modello e una nuova pagina con Adobe Experience Manager Sites.

### Passaggi successivi {#next-steps}

A questo punto la pagina dell&#39;articolo è chiaramente senza stile. Segui [Librerie lato client e flusso di lavoro front-end](client-side-libraries.md) esercitazione per scoprire le best practice per includere CSS e JavaScript per applicare stili globali al sito e integrare una build front-end dedicata.

Visualizza il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) o rivedi e distribuisci il codice localmente in sul blocco Git `tutorial/pages-templates-solution`.

1. Clona il [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) archivio.
1. Consulta la sezione `tutorial/pages-templates-solution` ramo.
